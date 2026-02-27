```skill
---
name: "gh-issue-management"
description: "Create, link, and manage GitHub issues (Initiative > Epic > Story > Task) with types, labels, project association, and bulk operations using the gh CLI"
metadata:
  domain: "github-project-management"
  confidence: "high"
  source: "earned"
---

# gh-issue-management

Create, link, and manage GitHub issues in a GitHub Organization repository using the `gh` CLI. Covers creating a full hierarchy (Initiative > Epic > Story > Task) with issue types, labels, sub-issue relationships, and project board association, as well as bulk operations like permanent deletion.

## Prerequisites

- **gh CLI** (v2.x+) authenticated with `project` scope
- Repository must belong to a **GitHub Organization** (issue types are an org-level feature)
- Organization must have the required **issue types** enabled (Initiative, Epic, Story, Task)
- Target **project** must exist under the org

### Verify auth scopes

```bash
gh auth status
```

If the token is missing the `project` scope, refresh it:

```bash
gh auth refresh -h github.com -s project
```

### Verify issue types exist

```bash
curl -s -H "Authorization: token $(gh auth token)" \
  -H "Accept: application/vnd.github+json" \
  https://api.github.com/orgs/{ORG}/issue-types | cat
```

Confirm that `Initiative`, `Epic`, `Story`, and `Task` types are present and `is_enabled: true`.

### Verify the target label exists

```bash
gh label list --repo {ORG}/{REPO} --json name | cat
```

If the label does not exist, create it:

```bash
gh label create "{LABEL}" --color "1D76DB" --description "Newly created issue" --repo {ORG}/{REPO}
```

## Step-by-Step Process

### 1. Prepare issue bodies as temp files

Write each issue body to a temp file to avoid shell escaping problems. Use the `create_file` tool (preferred) or Python to write to `/tmp/{type}-body.md`.

Follow the repo's issue templates in `.github/ISSUE_TEMPLATE/` for body structure. The templates define the sections and fields for each issue type:

| Type | Template | Key sections |
|------|----------|-------------|
| Initiative | `initiative.md` | Vision, Business Objective, Success Metrics, Epics, Stakeholders, Risks & Assumptions, Out of Scope, Dependencies, DT Source |
| Epic | `epic.md` | Summary, Problem Statement, Goals & Desired Outcomes, Features/Stories, Acceptance Criteria, Out of Scope, Dependencies, DT Source |
| Story | `story.md` | Description (As a / I want / So that), Acceptance Criteria, Dependencies, Additional Information |
| Task | `task.md` | Description, TODOs, Dependencies |

### 2. Create issues top-down using the REST API

The `gh issue create` command does **not** support a `--type` flag. Use `gh api` to POST directly to the issues endpoint with the `type` field.

```bash
gh api repos/{ORG}/{REPO}/issues \
  -f title="{TITLE}" \
  -f body="$(cat /tmp/{type}-body.md)" \
  -f type="{TYPE_NAME}" \
  --jq '.number,.html_url' 2>&1 | cat
```

**Create in order:** Initiative first, then Epic, then Story, then Task. Capture each issue number for linking in subsequent steps.

**Important:** Always pipe to `| cat` to prevent `gh api` from opening a pager on large responses.

#### Title conventions

The templates define title prefixes for Initiative and Epic types:

- Initiative: `[Initiative] {descriptive title}`
- Epic: `[Epic] {descriptive title}`
- Story: No bracket prefix (plain descriptive title)
- Task: No bracket prefix (plain descriptive title)

### 3. Apply labels

The REST API `POST /issues` does not reliably accept the `labels` field through `gh api -f`. Add labels in a separate call after creation:

```bash
gh api repos/{ORG}/{REPO}/issues/{NUMBER}/labels \
  -f "labels[]={LABEL}" \
  --jq '.[].name' 2>&1 | cat
```

Run this for each issue immediately after creation.

### 4. Link sub-issue hierarchy

Use the GitHub sub-issues API to establish parent-child relationships. You need the **numeric issue ID** (not the issue number).

#### Get issue IDs

```bash
gh api repos/{ORG}/{REPO}/issues/{NUMBER} --jq '{number: .number, id: .id}' 2>&1 | cat
```

#### Create sub-issue relationships

```bash
gh api repos/{ORG}/{REPO}/issues/{PARENT_NUMBER}/sub_issues \
  -F sub_issue_id={CHILD_ISSUE_ID} \
  --jq '.id,.title' 2>&1 | cat
```

Link in order:
1. Epic as sub-issue of Initiative
2. Story as sub-issue of Epic
3. Task as sub-issue of Story

### 5. Add issues to a project

```bash
gh project item-add {PROJECT_NUMBER} \
  --owner {ORG} \
  --url "https://github.com/{ORG}/{REPO}/issues/{NUMBER}" 2>&1 | cat
```

Run for each issue. Can be done in a loop:

```bash
for issue in {N1} {N2} {N3} {N4}; do
  gh project item-add {PROJECT_NUMBER} --owner {ORG} \
    --url "https://github.com/{ORG}/{REPO}/issues/$issue" 2>&1 | cat
done
```

### 6. Update parent bodies with child references

After all issues exist, update parent issue bodies to replace placeholder links with actual issue numbers:

```bash
gh api repos/{ORG}/{REPO}/issues/{PARENT_NUMBER} -X PATCH \
  -f body="$(sed 's/- \[ \] _(to be linked)_/- [ ] #{CHILD_NUMBER}/' /tmp/{parent-type}-body.md)" \
  --jq '.number' 2>&1 | cat
```

## Variable Reference

| Variable | Example | Description |
|----------|---------|-------------|
| `{ORG}` | `clomonico` | GitHub organization login |
| `{REPO}` | `fluffyturtle` | Repository name |
| `{PROJECT_NUMBER}` | `1` | Project board number (visible in the project URL) |
| `{LABEL}` | `New` | Label to apply to all created issues |
| `{TYPE_NAME}` | `Initiative` | Must match an enabled org issue type exactly |
| `{PARENT_NUMBER}` | `19` | Issue number of the parent issue |
| `{CHILD_ISSUE_ID}` | `4001077927` | Numeric ID (not number) of the child issue |

## Edge Cases and Gotchas

1. **`gh issue create --type` does not exist.** You must use `gh api` to hit the REST endpoint directly with `-f type="{TYPE}"`.
2. **Shell escaping in issue bodies.** Never pass multi-line markdown with apostrophes or special characters as inline `-f body='...'` strings. Always write to a temp file first and use `-f body="$(cat /tmp/file.md)"`.
3. **Pager hijacking.** `gh api` opens a pager for large JSON responses. Always pipe to `| cat` to avoid hanging.
4. **Project scope.** The `gh project` commands require the `project` OAuth scope. If missing, run `gh auth refresh -h github.com -s project`.
5. **Sub-issue API uses issue ID, not number.** The `sub_issue_id` parameter takes the numeric `.id` field from the API response, not the human-readable issue `#number`.
6. **Issue types are org-only.** Personal (user-owned) repositories do not support issue types. The `type` field is silently ignored or errors on user repos.
7. **Label must exist before applying.** The label add endpoint returns an error if the label does not exist on the repo. Create it first with `gh label create`.
8. **EMU (Enterprise Managed User) accounts** may not have access to org repos through the MCP GitHub tools. Use the `gh` CLI with the locally authenticated account instead.

## Permanently Deleting Issues

GitHub does not support permanent issue deletion through the REST API. Use the GraphQL `deleteIssue` mutation instead. This is irreversible — deleted issues cannot be recovered and their numbers will not be reused.

### 1. List all issues and capture node IDs

```bash
gh api graphql -f query='
  query {
    repository(owner: "{ORG}", name: "{REPO}") {
      issues(first: 100, states: [OPEN, CLOSED]) {
        totalCount
        nodes { id number title }
      }
    }
  }' 2>&1 | cat
```

Each issue's `id` field (e.g. `I_kwDORaJ-6c7ue5PW`) is the global node ID needed for deletion.

### 2. Delete each issue

```bash
gh api graphql -f query='
  mutation {
    deleteIssue(input: {issueId: "{ISSUE_NODE_ID}"}) {
      clientMutationId
    }
  }' 2>&1 | cat
```

A successful response looks like: `{"data":{"deleteIssue":{"clientMutationId":null}}}`

To delete all issues in a loop:

```bash
ids=$(gh api graphql -f query='query { repository(owner: "{ORG}", name: "{REPO}") { issues(first: 100, states: [OPEN, CLOSED]) { nodes { id } } } }' --jq '.data.repository.issues.nodes[].id' 2>&1)

for id in $ids; do
  gh api graphql -f query="mutation { deleteIssue(input: {issueId: \"$id\"}) { clientMutationId } }" 2>&1 | cat
done
```

### 3. Verify clean slate

```bash
gh api graphql -f query='query { repository(owner: "{ORG}", name: "{REPO}") { issues(first: 1, states: [OPEN, CLOSED]) { totalCount } } }' --jq '.data.repository.issues.totalCount' 2>&1 | cat
```

Should return `0`.

### Gotchas

- **Requires admin or owner permissions** on the repository. Collaborators with write access alone cannot delete issues.
- **Irreversible.** There is no undo or trash — the issue and all its comments, reactions, and timeline events are gone.
- **GraphQL pagination.** The query above fetches the first 100 issues. If the repo has more than 100 issues, use cursor-based pagination with `after` and `pageInfo { hasNextPage endCursor }`.
- **Node ID format.** Use the `id` field from GraphQL (e.g. `I_kwDORaJ-6c7ue5PW`), not the numeric `.id` from the REST API — they are different.

## Cleanup

Remove temp body files after all issues are created:

```bash
rm -f /tmp/initiative-body.md /tmp/epic-body.md /tmp/story-body.md /tmp/task-body.md
```
```
