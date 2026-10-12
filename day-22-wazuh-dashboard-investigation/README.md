# Day 22: Wazuh Dashboard Investigation

## Task
Use Wazuh Dashboard filters and search to locate events from one specific agent. Filter by time, rule level, or event category, and save the steps used to narrow the results.

**Tools:** Wazuh Dashboard (Discover module)

> **Status:** Theoretical walkthrough. No Wazuh manager or dashboard is deployed in my lab yet (see Days 17 to 20), so nothing here was executed and there are no dashboard screenshots. The procedure is based on how the Discover module is documented to work, and the report will be re-submitted with real screenshots once a dashboard is available.

## What I planned
- **Open Discover** and start from the default `wazuh-alerts-*` index pattern, which shows alerts from every enrolled agent.
- **Isolate one agent** with a field filter or the query `agent.name: "DESKTOP-AUHDFVM"`.
- **Filter by time** with the global time range picker (Absolute mode), set to 29 September 2026, 09:50 to 10:30, which covers both events from Day 21.
- **Filter by category or severity:** `rule.groups` ("is one of") for event category, or `rule.level` ("is between") for a severity floor.
- **Save the steps** as a named Saved Search, which stores the query, filters, time range, and column layout as one reusable object.

## Worked example (planned, using Day 21's events)

| Step | Mechanism | Value |
|---|---|---|
| 1. Agent | Field filter on `agent.name` | DESKTOP-AUHDFVM |
| 2. Time | Global time range picker (Absolute) | 29 Sept 2026, 09:50 to 10:30 |
| 3. Category | Field filter on `rule.groups` | authentication_failed |
| 3b. Severity (alternative) | Field filter on `rule.level` | level 5 and above for the failed logon |
| 4. Save | Discover "Save" control | Saved Search: DESKTOP-AUHDFVM - Authentication Failures - 29 Sept |

## Key findings
- Agent, time, and category/severity filters stack on the same dataset. They are one narrowed search, not separate searches.
- Time is set with the global picker, while agent and category are set with field filters or the query bar.
- "Save the steps" means a Saved Search object in Wazuh, which can reopen the exact view later or be embedded in a dashboard.
- Day 21 showed that a service startup-type change (Event 7040) has no default Wazuh rule, so a severity filter alone would not surface it. It would only appear if a custom rule such as 100050 were deployed.

## Key takeaway
Good investigation starts with narrowing: one host, one time window, one category. Knowing which field does which job (`agent.name`, `rule.level`, `rule.groups`, and the time picker) means I can reproduce and share a search instead of rebuilding it each time. Knowing what the ruleset does not alert on matters just as much as knowing the filters.

## Evidence
📄 [Full report (theoretical walkthrough)](./Day-22-Wazuh-Dashboard-Investigation.pdf)
