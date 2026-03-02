# Approval Data Instrumentation Specification

**Issue:** #40 — Instrument approval data collection
**Initiative:** Glass Floor (Initiative 2)
**Epic:** 14 — Process Analytics
**Author:** Lambert (Implementation Planner)
**Date:** 2026-03-02
**Status:** Draft

---

## Problem Statement

Nobody has real data on process performance today. Lumbergh guesses cycle time is "2-3 weeks" but no actual measurement exists. The Bobs see an audit trail but not operational data. Peter knows where things stall but doesn't document it. Without instrumentation, the cycle time dashboard (#41), quarterly reviews (#42), and every improvement hypothesis remain unverifiable.

This spec defines what data to capture, how to capture it automatically, where to store it, and how to handle privacy and compliance so the approval system becomes a measurable process instead of a black box.

---

## 1. Data Points to Capture

### 1.1 Request Lifecycle Events

| # | Data Point | Field Name | Type | Description |
|---|---|---|---|---|
| 1 | Request ID | `request_id` | UUID | Unique identifier assigned at submission |
| 2 | Request type | `request_type` | Enum | Procurement, vendor onboarding, contract renewal, facilities, credit card, other |
| 3 | Submission timestamp | `submitted_at` | ISO 8601 | When the requestor clicked submit |
| 4 | Requestor ID | `requestor_id` | String | Employee ID of the person who submitted |
| 5 | Requestor department | `requestor_dept` | String | Department at time of submission |
| 6 | Dollar amount | `dollar_amount` | Decimal | Requested spend amount |
| 7 | Vendor ID | `vendor_id` | String | Vendor identifier (null if not applicable) |
| 8 | Request completion timestamp | `completed_at` | ISO 8601 | When final disposition was reached |
| 9 | Final disposition | `disposition` | Enum | Approved, rejected, withdrawn, expired |
| 10 | Total cycle time | `cycle_time_hours` | Decimal | Calculated: `completed_at` minus `submitted_at` in hours |

### 1.2 Approval Step Events

| # | Data Point | Field Name | Type | Description |
|---|---|---|---|---|
| 11 | Step ID | `step_id` | UUID | Unique identifier for this approval step |
| 12 | Step sequence | `step_sequence` | Integer | Order in the approval chain (1, 2, 3...) |
| 13 | Step name | `step_name` | String | Human-readable step label (e.g. "Budget verification") |
| 14 | Step tier | `step_tier` | Integer | Tier number per control matrix (Epic 7) |
| 15 | Step owner ID | `step_owner_id` | String | Employee ID of the assigned approver |
| 16 | Step owner role | `step_owner_role` | String | Role or title at time of assignment |
| 17 | Step assigned timestamp | `step_assigned_at` | ISO 8601 | When the step was assigned to the owner |
| 18 | Step started timestamp | `step_started_at` | ISO 8601 | When the owner first opened or viewed the request |
| 19 | Step completed timestamp | `step_completed_at` | ISO 8601 | When the owner submitted their decision |
| 20 | Step decision | `step_decision` | Enum | Approved, rejected, returned-for-info, escalated |
| 21 | Step duration (queue) | `queue_time_hours` | Decimal | Calculated: `step_started_at` minus `step_assigned_at` |
| 22 | Step duration (work) | `work_time_hours` | Decimal | Calculated: `step_completed_at` minus `step_started_at` |
| 23 | Step duration (total) | `step_time_hours` | Decimal | Calculated: `step_completed_at` minus `step_assigned_at` |
| 24 | Rejection reason | `rejection_reason` | Text | Free text when decision is rejected or returned |

### 1.3 Routing and Parallel Processing Events

| # | Data Point | Field Name | Type | Description |
|---|---|---|---|---|
| 25 | Route type | `route_type` | Enum | Sequential, parallel, hybrid |
| 26 | Parallel fork timestamp | `fork_at` | ISO 8601 | When parallel branches were spawned (Epic 12) |
| 27 | Parallel join timestamp | `join_at` | ISO 8601 | When all parallel branches completed |
| 28 | Parallel branch ID | `branch_id` | UUID | Identifier for each parallel branch |
| 29 | Parallel wait time | `parallel_wait_hours` | Decimal | Time the fastest branch waited for the slowest |

### 1.4 Escalation Events

| # | Data Point | Field Name | Type | Description |
|---|---|---|---|---|
| 30 | Escalation event ID | `escalation_id` | UUID | Unique identifier for the escalation |
| 31 | Escalation trigger | `escalation_trigger` | Enum | SLA breach, manual, auto-48h (Epic 13) |
| 32 | Escalation timestamp | `escalated_at` | ISO 8601 | When escalation fired |
| 33 | Escalation source step | `escalation_from_step` | UUID | Step that triggered the escalation |
| 34 | Escalation target owner | `escalation_to_owner` | String | Employee ID of the escalation recipient |
| 35 | Time before escalation | `pre_escalation_hours` | Decimal | How long the step sat before escalation fired |

### 1.5 Verification and Compliance Events

| # | Data Point | Field Name | Type | Description |
|---|---|---|---|---|
| 36 | Verification receipt ID | `receipt_id` | UUID | Links to verification receipt (Epic 11) |
| 37 | Verification items checked | `items_verified` | JSON Array | List of items the approver confirmed (budget, vendor, compliance) |
| 38 | Downstream skip flag | `downstream_skip` | Boolean | Whether a downstream step was skipped because upstream already verified |
| 39 | Decision log entry ID | `decision_log_id` | UUID | Link to the decision log record |

### 1.6 System and Metadata

| # | Data Point | Field Name | Type | Description |
|---|---|---|---|---|
| 40 | Event type | `event_type` | Enum | submit, assign, open, decide, escalate, fork, join, complete, withdraw |
| 41 | Event timestamp | `event_at` | ISO 8601 | Canonical timestamp for any event |
| 42 | Source system | `source_system` | String | Which system generated the event |
| 43 | Data version | `schema_version` | Semver | Schema version for forward compatibility |

---

## 2. Collection Methodology

### 2.1 Design Principle

All data collection is automatic. No approver or requestor manually enters instrumentation data. The system captures events as side effects of normal workflow actions.

### 2.2 Collection Architecture

```
┌─────────────────┐     ┌──────────────┐     ┌───────────────┐
│  Approval        │     │  Event        │     │  Analytics     │
│  Platform        │────▶│  Pipeline     │────▶│  Data Store    │
│  (source of      │     │  (transform   │     │  (queryable    │
│   truth)         │     │   + enrich)   │     │   warehouse)   │
└─────────────────┘     └──────────────┘     └───────────────┘
        │                       │
        │  Webhook / CDC        │  Validation
        │  on every state       │  + Schema check
        │  change               │
        ▼                       ▼
   Platform audit         Dead-letter queue
   log (backup)           (failed events)
```

### 2.3 Instrumentation Points

| Workflow Action | Events Emitted | Trigger Mechanism |
|---|---|---|
| Requestor submits a request | `submit` | Form submission webhook |
| Step assigned to approver | `assign` | Routing engine callback |
| Approver opens request | `open` | UI view event (first access only) |
| Approver submits decision | `decide` | Decision form submission |
| Parallel branches created | `fork` | Routing engine callback (Epic 12) |
| All parallel branches complete | `join` | Join-gate callback (Epic 12) |
| Escalation fires | `escalate` | Auto-escalation timer (Epic 13) |
| Request reaches final state | `complete` | State machine transition |
| Requestor withdraws | `withdraw` | Withdrawal action webhook |

### 2.4 Event Payload Standard

Every event follows a common envelope:

```json
{
  "event_id": "uuid",
  "event_type": "decide",
  "event_at": "2026-03-02T14:30:00Z",
  "schema_version": "1.0.0",
  "source_system": "approval-platform",
  "request_id": "uuid",
  "payload": {
    "step_id": "uuid",
    "step_owner_id": "EMP-123",
    "step_decision": "approved",
    "items_verified": ["budget", "vendor_status"],
    "receipt_id": "uuid"
  }
}
```

### 2.5 Failure Handling

- Events that fail schema validation go to a dead-letter queue for manual review
- The pipeline retries transient failures 3 times with exponential backoff
- If the analytics store is unavailable, events buffer in the pipeline for up to 24 hours
- The platform audit log serves as a backup source; events can be replayed from it

---

## 3. Data Model and Storage Design

### 3.1 Core Tables

**`fact_request`** — One row per approval request

| Column | Type | Notes |
|---|---|---|
| request_id | UUID PK | |
| request_type | VARCHAR | |
| requestor_id | VARCHAR | |
| requestor_dept | VARCHAR | |
| dollar_amount | DECIMAL(12,2) | |
| vendor_id | VARCHAR | Nullable |
| submitted_at | TIMESTAMP | |
| completed_at | TIMESTAMP | Nullable until complete |
| disposition | VARCHAR | Nullable until complete |
| cycle_time_hours | DECIMAL(8,2) | Calculated on completion |
| route_type | VARCHAR | sequential, parallel, hybrid |

**`fact_step`** — One row per approval step

| Column | Type | Notes |
|---|---|---|
| step_id | UUID PK | |
| request_id | UUID FK | |
| step_sequence | INT | |
| step_name | VARCHAR | |
| step_tier | INT | Per control matrix |
| step_owner_id | VARCHAR | |
| step_owner_role | VARCHAR | |
| step_assigned_at | TIMESTAMP | |
| step_started_at | TIMESTAMP | Nullable until opened |
| step_completed_at | TIMESTAMP | Nullable until decided |
| step_decision | VARCHAR | |
| queue_time_hours | DECIMAL(8,2) | Calculated |
| work_time_hours | DECIMAL(8,2) | Calculated |
| step_time_hours | DECIMAL(8,2) | Calculated |
| rejection_reason | TEXT | Nullable |
| branch_id | UUID | Nullable, for parallel routing |
| receipt_id | UUID | Nullable, links to Epic 11 |
| decision_log_id | UUID | Nullable |
| downstream_skip | BOOLEAN | Default false |

**`fact_escalation`** — One row per escalation event

| Column | Type | Notes |
|---|---|---|
| escalation_id | UUID PK | |
| request_id | UUID FK | |
| step_id | UUID FK | |
| escalation_trigger | VARCHAR | |
| escalated_at | TIMESTAMP | |
| escalation_from_step | UUID | |
| escalation_to_owner | VARCHAR | |
| pre_escalation_hours | DECIMAL(8,2) | |

**`fact_event_log`** — Append-only event stream (raw)

| Column | Type | Notes |
|---|---|---|
| event_id | UUID PK | |
| event_type | VARCHAR | |
| event_at | TIMESTAMP | |
| request_id | UUID FK | |
| schema_version | VARCHAR | |
| source_system | VARCHAR | |
| payload | JSONB | Full event payload |

### 3.2 Dimension Tables

**`dim_approver`** — Approver reference data (updated nightly)

| Column | Type |
|---|---|
| employee_id | VARCHAR PK |
| name | VARCHAR |
| role | VARCHAR |
| department | VARCHAR |
| is_active | BOOLEAN |

**`dim_request_type`** — Request type reference

| Column | Type |
|---|---|
| type_code | VARCHAR PK |
| type_label | VARCHAR |
| default_route | VARCHAR |
| tier_count | INT |

### 3.3 Storage Requirements

- **Analytics store:** PostgreSQL or cloud-equivalent columnar store (Redshift, BigQuery, Synapse)
- **Event stream:** Append-only `fact_event_log` table retained for replay and debugging
- **Indexing:** Composite indexes on `(request_id, step_sequence)`, `(step_owner_id, step_assigned_at)`, `(submitted_at)`
- **Partitioning:** `fact_event_log` partitioned by month on `event_at`
- **Estimated volume:** ~500 requests/month × ~5 steps/request × ~3 events/step = ~7,500 events/month. Storage is not a scaling concern for the foreseeable future.

---

## 4. Integration with Existing Systems

### 4.1 Verification Receipts (Epic 11)

- Each `decide` event includes the `receipt_id` from the verification receipt system
- The `items_verified` array copies what the approver confirmed, enabling analytics on verification patterns
- The `downstream_skip` flag tracks when a step was shortened because upstream verification was visible

### 4.2 Decision Log (Epic 11)

- Each `decide` event includes the `decision_log_id` linking to the canonical decision record
- The analytics store does not duplicate the decision log content; it links to it

### 4.3 Parallel Routing (Epic 12)

- `fork` and `join` events bracket parallel execution windows
- `parallel_wait_hours` measures how much time was added by the slowest parallel branch
- Each parallel branch gets a `branch_id`; steps within a branch share this ID
- Route type (`sequential`, `parallel`, `hybrid`) is recorded at the request level

### 4.4 Auto-Escalation (Epic 13)

- Every escalation event from the auto-escalation engine emits an `escalate` event
- The `escalation_trigger` field distinguishes automatic (48h SLA breach) from manual escalations
- `pre_escalation_hours` measures idle time before escalation, feeding bottleneck analysis
- Deadline notification data (#39) integrates here — warning/urgent/breach timestamps correlate with escalation events

### 4.5 Dashboard Tooling (#41)

- The analytics store exposes a read-only connection or API for dashboard consumption
- Pre-computed materialized views for common dashboard queries:
  - Average cycle time by request type (rolling 30 days)
  - Step duration distribution by tier
  - Bottleneck heatmap (which steps have the longest queue time)
  - Escalation frequency by approver
  - Rejection rate by step and request type

---

## 5. Historical Data Backfill Strategy

### 5.1 Available Sources

| Source | Data Available | Quality | Coverage |
|---|---|---|---|
| Platform audit log | Submission date, final decision, decision date | Medium — timestamps exist but step-level detail is sparse | ~18 months |
| Email notifications | Assignment timestamps (sent date) | Low — requires parsing, not all notifications retained | ~12 months |
| Peter's routing spreadsheet | Request type, routing path, informal status notes | Low — manual data, inconsistent formatting | ~24 months |
| The Bobs' audit exports | Compliance check results, control point outcomes | Medium — structured but audit-focused, not operational | ~36 months |

### 5.2 Backfill Approach

**Phase 1: Platform audit log (priority)**
- Extract submission and completion timestamps for all requests in the past 18 months
- Calculate total cycle time for each request
- Populate `fact_request` with available fields; leave step-level data null
- Estimated effort: 2 days of Michael Bolton's time

**Phase 2: Step-level reconstruction (best effort)**
- Cross-reference audit log entries with email notification timestamps to approximate step timing
- Flag all backfilled records with `source_system = "backfill"` and `schema_version = "0.1.0"` so downstream consumers can filter them
- Estimated effort: 3 days, with expectation that only ~60% of steps can be reconstructed

**Phase 3: Skip**
- Peter's spreadsheet and the Bobs' audit exports are supplementary but not worth the parsing effort for instrumentation purposes
- They may be useful for validating backfill accuracy as spot-checks

### 5.3 Backfill Constraints

- Backfilled data will not have: `step_started_at` (queue vs. work time split), `items_verified`, `receipt_id`, `branch_id`, or escalation events
- All backfilled records are clearly tagged so they are never mixed with live-instrumented data in accuracy-sensitive analyses
- Backfill is a one-time operation, not an ongoing process

---

## 6. Data Retention Policy

| Data Category | Retention Period | Archive Strategy |
|---|---|---|
| `fact_request` | 7 years | Active for 2 years, then move to cold storage |
| `fact_step` | 7 years | Active for 2 years, then move to cold storage |
| `fact_escalation` | 7 years | Active for 2 years, then move to cold storage |
| `fact_event_log` (raw) | 2 years | Purge after 2 years; fact tables retain aggregated data |
| `dim_approver` | Indefinite | Soft-delete with `is_active = false` for departed employees |
| Backfilled data | 7 years | Same as live data, tagged with `source_system = "backfill"` |

### 6.1 Rationale

- 7-year retention aligns with standard financial audit retention requirements
- 2-year active window covers trend analysis needs (quarterly reviews require at minimum 4 quarters of comparison data)
- Raw event log purge after 2 years reduces storage costs without losing analytical value

### 6.2 Archival Format

- Archived data exported as Parquet files to object storage (S3/Azure Blob/GCS)
- Archived data remains queryable via external table definitions if needed for audit requests

---

## 7. Privacy and Compliance Review Items

### 7.1 Individual Performance Visibility

**The core concern:** Instrumentation data reveals individual approver performance — how long each person takes, how often they're escalated, their rejection patterns. This creates a surveillance risk if misused.

**Recommended controls:**

| Control | Description |
|---|---|
| Role-based access | Only process owners and analytics team see individual-level data. Approvers see aggregate team metrics only. |
| Aggregation for reporting | Dashboard (#41) defaults to team-level and tier-level views. Individual drill-down requires explicit permission. |
| No performance scoring | Data is used for process improvement, not individual performance evaluation. This must be stated in the data governance policy. |
| Notification | Approvers are informed that step timing is recorded as part of process analytics. No hidden tracking. |
| Manager access limits | Managers see their team's aggregate metrics. Individual approver data requires a documented business justification. |

### 7.2 Compliance Checkpoints

| Item | Owner | Status |
|---|---|---|
| Confirm data collection complies with employee monitoring policies | HR / Legal | Pending review |
| Confirm retention periods meet regulatory requirements | Compliance (The Bobs) | Pending review |
| Review data access controls with IT security | Michael Bolton's team | Pending review |
| Confirm GDPR/privacy applicability if any international approvers | Legal | Pending review |
| Document data processing purpose in privacy impact assessment | Data governance team | Pending review |

### 7.3 Data Anonymization

- For external benchmarking or reporting beyond operational use, approver IDs must be pseudonymized
- Aggregated data (averages, medians, percentiles) can be shared freely
- Raw individual-level data never leaves the analytics platform without explicit approval

---

## 8. Performance Impact Assessment

### 8.1 Design for Zero Perceptible Impact

The instrumentation must not slow the approval workflow. Approvers should not notice any latency change.

### 8.2 Mitigation Strategies

| Concern | Mitigation |
|---|---|
| Event emission adds latency to user actions | Events are emitted asynchronously. The approval action completes immediately; the event is queued for processing. Fire-and-forget from the user's perspective. |
| Pipeline backpressure slows the platform | Pipeline is decoupled from the platform via a message queue. Pipeline slowdowns don't affect the approval system. |
| Database writes compete with approval reads | Analytics store is a separate database from the operational approval database. No shared resources. |
| Backfill load impacts live system | Backfill runs during off-hours with rate limiting. Reads from audit log use read replicas where available. |
| Event payload size | Payload size is bounded. JSON payloads average <2KB per event. At 7,500 events/month, total monthly payload is under 15MB. |

### 8.3 Monitoring

- Alert if event processing latency exceeds 5 minutes (events should typically process in <30 seconds)
- Alert if dead-letter queue depth exceeds 50 events
- Weekly report on event volume vs. expected volume (detect missing instrumentation)

---

## 9. Data Quality and Validation Rules

### 9.1 Schema Validation

Every event is validated against the schema before entering the analytics store. Invalid events go to the dead-letter queue.

| Rule | Check |
|---|---|
| Required fields present | `event_id`, `event_type`, `event_at`, `request_id`, `schema_version` must be non-null |
| Timestamp ordering | `step_started_at` >= `step_assigned_at`; `step_completed_at` >= `step_started_at` |
| Valid enums | `event_type`, `step_decision`, `disposition`, `route_type`, `escalation_trigger` must be from allowed values |
| Referential integrity | `request_id` must exist in `fact_request` before step or escalation events are recorded |
| Duration sanity | Calculated durations must be >= 0 and < 8,760 hours (1 year) |
| Amount sanity | `dollar_amount` must be >= 0 and < 10,000,000 |

### 9.2 Completeness Checks

| Check | Frequency | Action |
|---|---|---|
| Every `submit` event has a matching `complete` or `withdraw` within 90 days | Weekly | Flag orphaned requests for investigation |
| Every `assign` event has a matching `decide` or `escalate` | Weekly | Flag stuck steps |
| Step count per request matches routing configuration | Daily | Alert on step count mismatches |
| Event count per request matches expected pattern | Daily | Alert on requests with missing events |

### 9.3 Data Reconciliation

- Monthly reconciliation: compare `fact_request` count against platform's request count
- Acceptable variance: <1%
- Reconciliation is automated and produces a report; discrepancies trigger investigation

---

## 10. Michael Bolton Implementation Requirements

### 10.1 What Michael's Team Needs to Build

| # | Requirement | Priority | Estimated Effort |
|---|---|---|---|
| 1 | Configure webhook/CDC events on all approval state changes | P0 | 3 days |
| 2 | Create the analytics database schema (tables, indexes, partitions) | P0 | 2 days |
| 3 | Build event pipeline (queue + transform + load) | P0 | 5 days |
| 4 | Add `open` event tracking (first-view detection) | P1 | 1 day |
| 5 | Integrate verification receipt IDs into event payload (Epic 11 dependency) | P1 | 1 day |
| 6 | Integrate parallel routing events (fork/join) (Epic 12 dependency) | P1 | 2 days |
| 7 | Integrate escalation events (Epic 13 dependency) | P1 | 1 day |
| 8 | Build backfill scripts for historical data (Phase 1 + 2) | P2 | 5 days |
| 9 | Create materialized views for dashboard consumption (#41) | P2 | 2 days |
| 10 | Set up monitoring and alerting for pipeline health | P2 | 1 day |
| 11 | Implement dead-letter queue and retry logic | P1 | 2 days |
| 12 | Configure role-based access controls on analytics store | P1 | 1 day |

### 10.2 Dependencies and Sequencing

```
Phase 1 (Foundation) — No dependencies
  #1 Configure webhooks/CDC
  #2 Create schema
  #3 Build event pipeline
  #11 Dead-letter queue

Phase 2 (Enrichment) — After Epic 11, 12, 13 deliver
  #4 Open event tracking
  #5 Verification receipt integration
  #6 Parallel routing integration
  #7 Escalation integration
  #12 Access controls

Phase 3 (Analytics Ready) — After Phase 2
  #8 Historical backfill
  #9 Materialized views
  #10 Monitoring and alerting
```

### 10.3 Platform Concerns

Michael has flagged the platform as fragile and poorly integrated. To mitigate:

- All instrumentation is read-only from the platform's perspective (webhooks, CDC, audit log reads). No writes back to the approval system.
- The event pipeline is fully decoupled. If it fails, the approval system is unaffected.
- Backfill reads from the audit log use read replicas or off-hours scheduling. No production load.
- New webhook configurations should be tested in staging before production deployment.

### 10.4 Estimated Total Effort

- **P0 items:** 10 days (foundation, can start immediately)
- **P1 items:** 8 days (after Epic 11/12/13 dependencies)
- **P2 items:** 8 days (after Phase 2)
- **Total:** ~26 days of Michael Bolton's team effort, spread across the initiative timeline

---

## Open Questions

1. Does the existing platform support CDC (change data capture) natively, or do we need to rely on polling the audit log?
2. What is the exact schema of the current audit log? Michael needs to confirm field availability for backfill.
3. Are there any union or labor agreement constraints on tracking individual step timing?
4. Should the analytics store live in the same cloud account as the approval platform, or in a separate analytics account?
5. What BI/dashboard tool will consume this data? (#41 decision affects materialized view design)

---

## DT Source

- **Session:** HVE Design Thinking — Approval Process
- **Scenario:** Glass Floor
- **Route:** Process Analytics — make approval performance measurable
- **Date:** 2026-03-02
- **Key findings:** No operational data exists today. Lumbergh's "2-3 weeks" estimate is unverified. The Bobs have audit data but not process timing. Instrumentation is the prerequisite for every analytics deliverable in Epic 14.
