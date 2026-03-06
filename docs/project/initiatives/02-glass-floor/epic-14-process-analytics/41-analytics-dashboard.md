# Process Analytics Dashboard Specification

**Issue:** #41 — Build cycle time and bottleneck dashboard
**Initiative:** Glass Floor (Initiative 2)
**Epic:** 14 — Process Analytics
**Author:** Ripley (Squad Lead)
**Date:** 2026-03-02
**Status:** Draft
**Depends on:** #40 (Data Instrumentation)

---

## 1. Purpose and Audience

### 1.1 What This Dashboard Does

This dashboard answers the question everyone asks but nobody can verify: **how is the approval process actually performing?**

Lumbergh guesses cycle time is "2-3 weeks." The Bobs see audit trail gaps. Peter knows where things stall but has no data to prove it. Joanna waits and has no idea why. This dashboard makes the invisible visible by surfacing aggregate process performance to everyone who touches the system.

### 1.2 How This Differs from the Executive Visibility Dashboard (Epic 10, Issue #28)

Two dashboards serve two different questions. They share data infrastructure but do not overlap in purpose.

| | Executive Visibility Dashboard (#28) | Process Analytics Dashboard (this) |
|---|---|---|
| **Core question** | "Where is MY request right now?" | "How is the PROCESS performing overall?" |
| **View type** | Operational — individual request tracking | Analytical — aggregate patterns and trends |
| **Primary audience** | Lumbergh (executive viewer) | All stakeholders: requestors, approvers, managers, the Bobs, process owners |
| **Time focus** | Current state of in-flight requests | Historical trends, comparisons, and projections |
| **Granularity** | Single request drill-down with SLA progress bars | Aggregated metrics: averages, distributions, rates |
| **Update model** | Near-real-time (5-min auto-refresh) | Near-real-time for live metrics; hourly for trend recalculation |
| **Access** | Role-restricted (5 defined roles) | Open to all stakeholders — no restriction |
| **Action capability** | Read-only (by design, to prevent gate re-creation) | Read-only (analytical, not operational) |

### 1.3 Design Principle

The Executive Dashboard tells Lumbergh "Request REQ-2041 is sitting with the Bobs on Day 2 of 3." This dashboard tells everyone "The average request spends 18 hours waiting in queue at Compliance / COI Check, and that number has increased 40% over the last quarter."

One tracks items. The other tracks the system.

### 1.4 Audience

| Audience | What they get from this dashboard |
|---|---|
| **Requestors** (Joanna, anyone) | Visibility into typical processing times so they can plan ahead and understand delays |
| **Approvers** (tier owners) | Team-level performance benchmarks — how their step compares to others |
| **Process owners** (Peter) | Bottleneck identification, trend analysis, evidence for process improvement proposals |
| **Compliance** (the Bobs) | Audit-ready performance data, rejection patterns, escalation rates |
| **Management** (Lumbergh, VP) | High-level process health metrics for reporting up the chain |
| **IT** (Michael Bolton) | System performance correlation — is platform health affecting process performance? |

---

## 2. Key Metrics and Visualizations

### 2.1 Primary Metrics

Every metric is derived from the 43 data points defined in the instrumentation spec (#40). No manual data entry. No surveys. No guesswork.

| # | Metric | Source Fields (#40) | Visualization | Why It Matters |
|---|---|---|---|---|
| 1 | **Average cycle time (overall)** | `cycle_time_hours` from `fact_request` | Large number with trend arrow + sparkline | The headline number. Today nobody knows it. |
| 2 | **Average cycle time by step** | `step_time_hours` from `fact_step`, grouped by `step_name` | Horizontal bar chart | Shows which steps contribute most to total time |
| 3 | **Queue time vs. work time split** | `queue_time_hours`, `work_time_hours` from `fact_step` | Stacked bar per step | Reveals whether delays are waiting (queue) or processing (work) |
| 4 | **Top bottleneck steps** | `step_time_hours` from `fact_step`, ranked by median duration | Ranked list with median, P75, P95 values | Pinpoints exactly where requests stall |
| 5 | **Rejection rate by step** | `step_decision = 'rejected'` count / total decisions per `step_name` | Bar chart with percentage labels | High rejection at one step may indicate unclear requirements or duplicate checks |
| 6 | **Escalation frequency** | `fact_escalation` count, grouped by `escalation_trigger` | Donut chart (auto vs. manual) + trend line | High escalation = SLA breaches = systemic delay |
| 7 | **Throughput** | `fact_request` count where `disposition = 'approved'`, per time period | Line chart (weekly/monthly) | Volume of work the system is processing |
| 8 | **Request volume by type** | `request_type` from `fact_request` | Pie or bar chart | Shows workload distribution across request categories |
| 9 | **Parallel routing efficiency** | `parallel_wait_hours` from routing events | Box plot | Measures wasted wait time in parallel branches (Epic 12) |
| 10 | **Verification skip rate** | `downstream_skip = true` count from `fact_step` | Percentage with trend | Measures whether upstream verification is reducing downstream redundancy (Epic 11) |

### 2.2 Derived Indicators

| Indicator | Calculation | Display |
|---|---|---|
| **Process health score** | Composite: cycle time vs. target + rejection rate + escalation rate | Green/Yellow/Red badge with score 0-100 |
| **SLA compliance rate** | % of steps completed within their tier SLA | Percentage with trend arrow |
| **First-pass approval rate** | % of requests approved without any rejection or return-for-info | Percentage — higher is better |
| **Withdrawal rate** | `disposition = 'withdrawn'` / total requests | Percentage — rising rate may signal requestor frustration |

---

## 3. Dashboard Layout

### 3.1 Wireframe

```
┌──────────────────────────────────────────────────────────────────────────────┐
│  PROCESS ANALYTICS DASHBOARD                    [Filters ▼] [Compare] [⟳]  │
├──────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐ ┌──────────────┐       │
│  │ AVG CYCLE    │ │ SLA          │ │ REJECTION    │ │ ESCALATION   │       │
│  │ TIME         │ │ COMPLIANCE   │ │ RATE         │ │ RATE         │       │
│  │  3.8 days ▼  │ │  87% ▲       │ │  12% ▼       │ │  8% ▼        │       │
│  │ target: 5d   │ │ target: 95%  │ │ target: <10% │ │ target: <5%  │       │
│  └──────────────┘ └──────────────┘ └──────────────┘ └──────────────┘       │
│                                                                              │
├──────────────────────────────────────────────────────────────────────────────┤
│  FILTER BAR                                                                  │
│  ┌──────────┐ ┌───────────────┐ ┌──────────────┐ ┌────────────┐ ┌───────┐  │
│  │Period  ▼ │ │ Req Type ▼    │ │ Department ▼ │ │ Amount ▼   │ │ Tier ▼│  │
│  │Last 30d  │ │ All           │ │ All          │ │ All        │ │ All   │  │
│  └──────────┘ └───────────────┘ └──────────────┘ └────────────┘ └───────┘  │
│                                                                              │
├─────────────────────────────────┬────────────────────────────────────────────┤
│                                 │                                            │
│  CYCLE TIME BY STEP             │  BOTTLENECK RANKING                        │
│  ┌─────────────────────────┐    │  ┌──────────────────────────────────────┐  │
│  │ Budget Authority        │    │  │ 1. Compliance / COI    18.2h (med) │  │
│  │ ████████░░  12.4h       │    │  │ 2. Requestor Verif.    14.6h (med) │  │
│  │  [queue ██ 8.1h]        │    │  │ 3. Budget Authority     12.4h (med) │  │
│  │  [work  ██ 4.3h]        │    │  │                                      │  │
│  │                         │    │  │ Queue Time is 65% of total step      │  │
│  │ Compliance / COI       │    │  │ duration across all steps             │  │
│  │ █████████████░  18.2h   │    │  └──────────────────────────────────────┘  │
│  │  [queue ██████ 13.1h]   │    │                                            │
│  │  [work  ██ 5.1h]        │    │  REJECTION RATE BY STEP                    │
│  │                         │    │  ┌──────────────────────────────────────┐  │
│  │ Requestor Verif.       │    │  │ Budget Authority      ███░░  8%     │  │
│  │ ███████████░  14.6h     │    │  │ Compliance / COI     █████░ 15%    │  │
│  │  [queue █████ 10.2h]    │    │  │ Requestor Verif.     ████░░ 11%    │  │
│  │  [work  ██ 4.4h]        │    │  └──────────────────────────────────────┘  │
│  └─────────────────────────┘    │                                            │
│                                 │                                            │
├─────────────────────────────────┴────────────────────────────────────────────┤
│                                                                              │
│  TREND: CYCLE TIME (30-DAY ROLLING AVERAGE)                                  │
│  ┌────────────────────────────────────────────────────────────────────────┐  │
│  │  6d ┤                                                                  │  │
│  │  5d ┤  ╭─╮      TARGET LINE ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─           │  │
│  │  4d ┤ ╭╯ ╰─╮  ╭╮                                                     │  │
│  │  3d ┤╭╯    ╰──╯╰──╮╭──╮                                              │  │
│  │  2d ┤╯             ╰╯  ╰──────                                       │  │
│  │  1d ┤                                                                  │  │
│  │     └──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬─  │  │
│  │       Jan  Feb  Mar  Apr  May  Jun  Jul  Aug  Sep  Oct  Nov  Dec      │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
│  ESCALATION BREAKDOWN                     THROUGHPUT (WEEKLY)                │
│  ┌────────────────────────┐              ┌─────────────────────────────┐    │
│  │   ╭───╮                │              │  30 ┤     ╭─╮               │    │
│  │  ╭│   │╮   Auto: 62%  │              │  25 ┤  ╭──╯ ╰──╮           │    │
│  │  │ SLA ││  Manual: 38% │              │  20 ┤╭─╯       ╰──╮       │    │
│  │  │Brch ││              │              │  15 ┤╯             ╰──     │    │
│  │  ╰│   │╯              │              │     └──┬──┬──┬──┬──┬──┬──  │    │
│  │   ╰───╯               │              │       W1  W2  W3  W4  W5   │    │
│  └────────────────────────┘              └─────────────────────────────┘    │
│                                                                              │
└──────────────────────────────────────────────────────────────────────────────┘
```

### 3.2 Layout Sections

| Section | Purpose | Update Frequency |
|---|---|---|
| **Summary Cards** (top row) | At-a-glance process health: cycle time, SLA compliance, rejection rate, escalation rate. Each with trend arrow and target comparison. | Near-real-time (recomputed on page load + auto-refresh) |
| **Filter Bar** | Narrow all views by time period, request type, department, dollar amount range, and tier. All filters apply globally to every chart and metric on the page. | Instant (client-side filtering against cached dataset) |
| **Cycle Time by Step** | Stacked horizontal bars showing queue vs. work time per step. Immediately reveals whether slow steps are slow because of waiting or actual review work. | Hourly recalculation |
| **Bottleneck Ranking** | Ordered list of steps by median total duration. Highlights the single biggest contributor to cycle time. | Hourly recalculation |
| **Rejection Rate by Step** | Which steps reject most often? A step with 30% rejection rate may be catching real problems or may be enforcing unclear requirements. | Hourly recalculation |
| **Trend: Cycle Time** | Rolling 30-day average cycle time over the past 12 months. Target line overlaid. Shows whether process improvements are working. | Hourly recalculation |
| **Escalation Breakdown** | Donut showing auto (SLA breach) vs. manual escalations. High auto-escalation rate = people aren't meeting SLAs. | Hourly recalculation |
| **Throughput** | Weekly count of completed requests. Shows capacity and whether volume changes correlate with performance changes. | Daily recalculation |

---

## 4. Filter and Drill-Down Capabilities

### 4.1 Global Filters

All filters apply to every visualization on the page simultaneously. Selecting a filter instantly recalculates summary cards, charts, and rankings.

| Filter | Options | Default |
|---|---|---|
| **Time period** | Last 7 days, Last 30 days, Last 90 days, Last 12 months, Custom range | Last 30 days |
| **Request type** | Procurement, Vendor onboarding, Contract renewal, Facilities, Credit card, Other | All |
| **Department** | Populated dynamically from `dim_approver` and `requestor_dept` | All |
| **Dollar amount** | Under $5K, $5K–$25K, $25K–$50K, $50K–$100K, Over $100K, Custom range | All |
| **Tier** | Requestor Verification, Budget Authority Confirmation, Compliance / COI Check | All |

### 4.2 Drill-Down Behavior

Clicking any chart element opens a detail panel with the underlying data.

| Click Target | Drill-Down View |
|---|---|
| A step bar in Cycle Time chart | Distribution histogram of that step's duration: median, P25, P75, P95, with outlier callouts |
| A step in Bottleneck Ranking | Time series of that step's median duration over the selected period. Shows whether it's improving or declining. |
| A step in Rejection Rate chart | List of top rejection reasons (from `rejection_reason` field), grouped and counted |
| Escalation donut segment | Table of recent escalations: request ID, step, trigger type, time before escalation fired |
| Any point on the Trend line | That period's breakdown: volume, cycle time by step, rejection rate, escalation count |
| Throughput bar | List of completed requests for that week with cycle time and disposition |

### 4.3 Comparison Mode

The **Compare** button splits every chart into side-by-side views for two selected periods or two selected segments (e.g., "Procurement vs. Facilities" or "Q1 vs. Q2"). Useful for measuring whether a process change had an effect.

---

## 5. Trend Analysis and Comparison Views

### 5.1 Trend Lines

Every primary metric supports a trend view showing its value over time.

| Trend | Granularity | Lookback | Use Case |
|---|---|---|---|
| Cycle time trend | Daily average, 7-day rolling, 30-day rolling | Up to 18 months (backfill data available) | Detect gradual improvement or regression |
| Rejection rate trend | Weekly | Up to 18 months | Identify if new controls are increasing rejection unexpectedly |
| Escalation rate trend | Weekly | Up to 18 months | Measure whether SLA configuration is appropriate |
| Throughput trend | Weekly | Up to 18 months | Capacity planning, seasonal pattern detection |
| Queue time ratio trend | Weekly | Up to 18 months | Is the waiting problem getting better or worse? |

### 5.2 Comparison Views

| Comparison | How It Works |
|---|---|
| **Period vs. Period** | Select two date ranges. All metrics display side-by-side with delta (absolute + percentage change). |
| **Segment vs. Segment** | Choose two values for any filter dimension (e.g., Engineering vs. Marketing departments). All charts split into paired views. |
| **Pre/Post analysis** | Select a "change date" (when a process improvement was deployed). Dashboard automatically compares the period before and after. |

### 5.3 Annotations

Process owners can add date-stamped annotations to the trend timeline (e.g., "Deployed parallel routing — Epic 12" or "Added verification receipts — Epic 11"). These appear as labeled markers on trend charts, making it possible to correlate process changes with performance shifts.

---

## 6. Data Source Integration

### 6.1 Data Pipeline

This dashboard consumes data from the analytics data store populated by the instrumentation pipeline (#40). It does not read from the operational approval platform directly.

```
┌────────────────┐     ┌────────────────────┐     ┌─────────────────────┐
│  Approval       │     │  Event Pipeline     │     │  Analytics Data     │
│  Platform       │────▶│  (#40)              │────▶│  Store              │
│                 │     │                     │     │                     │
└────────────────┘     └────────────────────┘     └─────────┬───────────┘
                                                            │
                                                   ┌───────┴────────┐
                                                   │                │
                                              ┌────▼────┐    ┌─────▼──────┐
                                              │ This     │    │ Executive  │
                                              │ Dashboard│    │ Dashboard  │
                                              │ (#41)    │    │ (#28)      │
                                              └─────────┘    └────────────┘
```

### 6.2 Materialized Views

The instrumentation spec (#40, Section 4.5) defines pre-computed materialized views for dashboard consumption. This dashboard requires the following views:

| Materialized View | Source Tables | Refresh Rate | Dashboard Section |
|---|---|---|---|
| `mv_cycle_time_by_type` | `fact_request` | Hourly | Summary cards, Trend chart |
| `mv_step_duration_distribution` | `fact_step` | Hourly | Cycle Time by Step, Bottleneck Ranking |
| `mv_bottleneck_heatmap` | `fact_step` (queue time ranking) | Hourly | Bottleneck Ranking |
| `mv_escalation_frequency` | `fact_escalation` | Hourly | Escalation Breakdown, Summary card |
| `mv_rejection_rate_by_step` | `fact_step` (decision = rejected) | Hourly | Rejection Rate chart |
| `mv_throughput_weekly` | `fact_request` (completed, grouped by week) | Daily | Throughput chart |
| `mv_sla_compliance` | `fact_step` (step_time vs. tier SLA) | Hourly | Summary card |
| `mv_parallel_efficiency` | Routing events (parallel_wait_hours) | Daily | Parallel routing section |

### 6.3 Query Performance Target

- Dashboard page load (all summary cards + default charts): **< 3 seconds**
- Filter change (recalculate all visible charts): **< 2 seconds**
- Drill-down expansion: **< 1 second**
- Comparison mode activation (side-by-side generation): **< 3 seconds**

These targets assume materialized views are pre-computed. The dashboard frontend queries views, not raw tables.

---

## 7. Access Control

### 7.1 Design Principle: Open by Default

The Executive Dashboard (#28) restricts access by role because it shows individual request details (requestor names, dollar amounts, vendor information). This dashboard shows aggregate process metrics. There is no reason to restrict it.

**Everyone who participates in the approval process has a right to see how the process performs.**

### 7.2 Access Model

| Role | Access Level | Rationale |
|---|---|---|
| **All employees** | Full read access to all aggregate views | Transparency is the point. Requestors deserve to know average cycle times. Approvers deserve to see how their step compares. |
| **Process owners** (Peter) | Full read access + annotation capability + comparison mode | Need to mark process changes and analyze before/after impact |
| **Compliance** (the Bobs) | Full read access + data export | Audit requirements may need raw metric exports |
| **IT / Analytics** (Michael Bolton) | Full read access + admin (view definitions, refresh schedules) | Maintain the technical infrastructure |

### 7.3 What Is NOT Shown

Individual approver performance data is **not** displayed on this dashboard. The instrumentation spec (#40, Section 7.1) explicitly limits individual-level data to process owners with documented business justification.

| Visible | Not Visible |
|---|---|
| "Compliance / COI Check averages 18 hours" | "Bob Slydell averages 22 hours" |
| "Budget Authority Confirmation has 8% rejection rate" | "R. Kumar rejected 15 requests this quarter" |
| "3 escalations last week at Compliance / COI Check" | "S. Park was escalated twice" |

Aggregate by step, tier, department, request type. Never by individual.

---

## 8. Refresh Rate and Performance

### 8.1 Update Cadence

| Component | Refresh Strategy | Latency |
|---|---|---|
| **Summary cards** | Recomputed on page load + auto-refresh every 5 minutes | < 5 minutes from event occurrence to card update |
| **Charts and rankings** | Materialized views refreshed hourly | < 60 minutes for new data to appear in charts |
| **Trend lines** | Recalculated hourly, but granularity is daily/weekly | Minutes don't matter for trend analysis |
| **Drill-down data** | Queried on demand from materialized views | Near-real-time (view refresh boundary) |
| **Comparison mode** | Computed on demand from historical data | Seconds (all historical data pre-aggregated) |

### 8.2 Why Near-Real-Time, Not Batch

The instrumentation spec (#40) uses an async event pipeline that processes events in under 30 seconds typically. By the time an approver completes a step, the event is in the analytics store within a minute. The dashboard reflects this through its materialized view refresh cycle.

Daily batch would mean a morning snapshot that goes stale by noon. Monthly reports are what the Bobs already produce. This dashboard exists because those cadences are too slow to drive process improvement.

### 8.3 Performance Budget

| Metric | Target | Mitigation if exceeded |
|---|---|---|
| Page load time | < 3 seconds | Pre-compute all summary card values; lazy-load below-fold charts |
| Filter application | < 2 seconds | Client-side filtering on cached dataset (summary data fits in memory) |
| Chart render | < 1 second per chart | Limit data points to 365 per trend line; aggregate older data |
| Data freshness (summary cards) | < 5 minutes | If materialized view refresh fails, show stale data with "last updated" timestamp |
| Concurrent users | 50+ simultaneous | Read-only queries against materialized views; no contention with write pipeline |

---

## 9. Alert and Threshold Integration

### 9.1 Automated Anomaly Detection

The dashboard surfaces anomalies without requiring someone to stare at charts all day.

| Alert | Trigger Condition | Who Sees It | Delivery |
|---|---|---|---|
| **Cycle time spike** | Rolling 7-day average exceeds target by 50% | Process owners, management | Dashboard banner + email |
| **Rejection rate surge** | Any step's weekly rejection rate exceeds 25% | Process owners | Dashboard badge on affected step |
| **Escalation cluster** | More than 5 escalations in a single week at one step | Process owners, step owner's manager | Dashboard badge + email |
| **Throughput drop** | Weekly completed requests drop below 70% of 4-week rolling average | Process owners, management | Dashboard banner |
| **Queue time outlier** | Any single request's queue time at any step exceeds 3x the step median | Process owners | Highlighted in drill-down view |
| **SLA compliance drop** | SLA compliance falls below 80% for any tier over a 7-day window | Process owners, management | Dashboard banner + email |

### 9.2 Threshold Configuration

Process owners can configure alert thresholds through an admin panel. Defaults are set based on initial target metrics, but they should be adjusted as baseline data accumulates.

| Threshold | Default | Configurable By |
|---|---|---|
| Cycle time target | 5 business days | Process owner |
| SLA compliance target | 95% | Process owner |
| Rejection rate warning | 25% per step | Process owner |
| Escalation frequency warning | 5 per week per step | Process owner |
| Throughput drop trigger | 70% of rolling average | Process owner |

### 9.3 Alert History

All triggered alerts are logged with timestamp, metric value at trigger, and threshold at the time. This log is accessible in the dashboard under an "Alert History" tab, enabling process owners to review patterns in anomaly occurrence.

---

## 10. Mobile and Responsive Considerations

### 10.1 Responsive Design Tiers

| Viewport | Layout | Priority Content |
|---|---|---|
| **Desktop** (1200px+) | Full layout as wireframed: 2-column charts, full filter bar | Everything visible |
| **Tablet** (768px–1199px) | Single-column charts, collapsible filter bar | Summary cards + one chart visible; others scroll |
| **Mobile** (< 768px) | Summary cards only + swipeable chart cards | Cycle time, SLA compliance, rejection rate, escalation rate |

### 10.2 Mobile-First Design Rationale

The Executive Dashboard (#28) deferred mobile as a nice-to-have because Lumbergh works from his office. This dashboard serves everyone, including approvers and requestors who may check it on the go. Joanna checking "is the process getting faster?" from her phone is a legitimate use case.

### 10.3 Progressive Enhancement

- **Core experience** (all devices): Summary cards with trend arrows load instantly
- **Enhanced experience** (tablet+): Interactive charts with drill-down
- **Full experience** (desktop): Comparison mode, annotations, full filter bar, side-by-side charts

---

## 11. Relationship to Executive Visibility Dashboard

### 11.1 Shared Infrastructure

Both dashboards read from the same analytics data store. The data pipeline (#40) feeds both. Neither dashboard writes data.

```
Analytics Data Store
       │
       ├──▶ Executive Dashboard (#28) — shows individual requests, SLA tracking
       │       Audience: Lumbergh, role-restricted
       │       Question: "Where is this request?"
       │
       └──▶ Process Analytics Dashboard (#41) — shows aggregate performance
               Audience: Everyone
               Question: "How is the process doing?"
```

### 11.2 No Feature Duplication

The Executive Dashboard (#28, Section 9) already committed to this boundary. This dashboard honors it:

| Feature | Which dashboard owns it |
|---|---|
| Individual request tracking | Executive (#28) |
| Per-request SLA progress bars | Executive (#28) |
| Request detail drill-down (documents, approval trail) | Executive (#28) |
| Aggregate cycle time metrics | Analytics (#41, this) |
| Step-level bottleneck analysis | Analytics (#41, this) |
| Trend analysis over time | Analytics (#41, this) |
| Rejection and escalation rates | Analytics (#41, this) |
| Process health scoring | Analytics (#41, this) |

### 11.3 Cross-Linking

- The Executive Dashboard's "Avg Cycle Time" summary card can link to this dashboard's trend view for historical context
- This dashboard's drill-down on throughput (list of completed requests) can link to the Executive Dashboard's request detail view
- Both share filter vocabulary (request type, department, dollar amount) so users experience consistent terminology

### 11.4 Build Sequencing

The Executive Dashboard (#28) recommended starting with the platform's built-in reporting (Option A) and deferring custom build to Initiative 2/3. This analytics dashboard is that custom build. It should use the shared data aggregation layer designed in #28 Section 5, extended with the materialized views from #40.

---

## 12. Implementation Approach

### 12.1 Technology Considerations

| Option | Description | Fit |
|---|---|---|
| **BI tool (Metabase, Superset, Looker)** | Connect to analytics data store; build dashboards visually. Low code. | Good for rapid delivery. Supports all chart types, filters, drill-downs. |
| **Custom web app** | Build frontend against analytics API. Full control. | Best long-term flexibility but higher build cost. |
| **Platform built-in** | Use the approval platform's reporting features. | Already used for Executive Dashboard; limited for analytics-grade visualization. |

**Recommendation:** Start with a BI tool connected to the analytics data store. The materialized views (#40) are already designed for dashboard consumption. A BI tool gets interactive charts, filters, drill-downs, and mobile responsiveness without custom frontend development. If requirements outgrow the BI tool, migrate to custom build using the same data layer.

### 12.2 Implementation Steps

| # | Step | Owner | Dependency | Estimated Effort |
|---|---|---|---|---|
| 1 | Confirm materialized view definitions with Michael Bolton | Ripley + Michael | #40 Phase 3 complete | 1 day |
| 2 | Select and provision BI tool | Michael | Analytics data store live | 2 days |
| 3 | Build summary card views | Michael + Peter | Materialized views available | 1 day |
| 4 | Build cycle time and bottleneck charts | Michael | Summary cards working | 2 days |
| 5 | Build rejection and escalation charts | Michael | Summary cards working | 1 day |
| 6 | Build trend line views | Michael | Historical data loaded (backfill) | 2 days |
| 7 | Implement filter bar and drill-downs | Michael | All charts built | 2 days |
| 8 | Implement comparison mode | Michael | Filters working | 2 days |
| 9 | Configure alert thresholds | Peter | Charts and trend lines live | 1 day |
| 10 | Responsive/mobile testing | Michael + Peter | All features complete | 1 day |
| 11 | Stakeholder walkthrough (Lumbergh, the Bobs, Joanna, Peter) | Ripley | Dashboard functional | 1 day |
| 12 | Adjust based on feedback | Michael + Peter | Walkthrough complete | 2 days |

**Total estimated effort:** ~18 days of team time (not sequential; steps 4-6 can run in parallel)

### 12.3 Dependencies

| Dependency | What's needed | Status |
|---|---|---|
| #40 Data instrumentation | Analytics data store with populated fact tables and materialized views | Draft (PR #76) |
| #28 Executive Dashboard | Shared data aggregation layer and filter vocabulary | Draft |
| Epic 11 (Verification) | `receipt_id` and `items_verified` fields in step events | In progress |
| Epic 12 (Parallel routing) | `fork`/`join` events and `parallel_wait_hours` | In progress |
| Epic 13 (Auto-escalation) | Escalation events with trigger types | In progress |

---

## Open Questions

1. Which BI tool does the organization standardize on, if any? Michael Bolton should confirm platform compatibility.
2. Should trend lines include backfilled data (flagged as lower-confidence) or start clean from the instrumentation go-live date?
3. What is the initial cycle time target? The Executive Dashboard uses 7 business days; this dashboard should align or set its own based on baseline measurement.
4. Do the Bobs need a scheduled export of analytics data, or is on-demand export from the dashboard sufficient?
5. Should the process health composite score use equal weighting across its components, or should cycle time be weighted more heavily?

---

## DT Source

- **Session:** HVE Design Thinking — Approval Process
- **Scenario:** Glass Floor
- **Route:** Process Analytics — make approval performance visible and measurable to everyone
- **Date:** 2026-03-02
- **Key findings:** Nobody has real performance data today. Lumbergh's "2-3 weeks" guess is unverifiable. This dashboard turns guesswork into measurement. Unlike the Executive Dashboard (which serves Lumbergh's operational visibility), this dashboard serves every stakeholder's need to understand and improve the process.
