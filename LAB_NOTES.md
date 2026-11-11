# Lab notes: Agents I (governing actions)

Name: Farha N. Nadine 
GitHub username: nadifn01

Write one to three sentences per lab, with numbers or names from YOUR run. Commit and push this file to your fork by 9:25, then open a pull request titled "Lab notes: Firstname Lastname".

## Lab 0: the loop
What are the five things the loop does on every step, and which two of them are governance rather than intelligence?

1. Observe: Ingests the task description, context, and accumulated tool results so far.
2. Plan: The planner proposes a single structured tool call with its arguments.
3. Check: The policy engine checks the proposal and decides to allow it, ask for human approval, or reject it.
4. Act: If permitted, the tool is executed and its results are sent back to continue the process or Loop.
5. Log: It adds a permanent JSON record of each decision and its result ( Allowed, Approved, or Refused) to the audit log.

Plan and Act(Intelligence) handle decisions and actions, while Check and Log(Governance) make sure rules are followed and everything is documented.

## Lab 1: the model as a tool
P003 and P006: their probabilities and bands from your run. Which feature drove P006's risk most? Who authored the model card the agent relies on?

1. P003 (Priya Natarajan): Probability is 0.128, LOW risk band.
2. P006 (Hector Ruiz): Probability is 0.909,  HIGH risk band.
3. P006's Top Driver: prior_admissions_12m (with 3 prior admissions pushing risk up, followed by er_visits_6m and lives_alone
4. Model Card Author: Northgate Clinical AI Committee / Lakeshore Readmission Consortium (defined in models/federated_readmission.json).


## Lab 2: capability is not permission
Three refusals: the tool or patient, and the policy line that caused each. After your edit (max_patients_per_task, max_tool_calls_per_task), what finished that did not before, and who should be allowed to make that edit?
1. Task (a) export_records: Refused because it is not listed under "allowed_tools".
2. Task (b) P011: Refused because P011 is a Kestrel hospital patient, violating "data_scope": { "hospitals": ["Northgate"] }.
3. Task (c) Step 5 (lookup_patient(P006)): Refused because it violated "max_patients_per_task": 1

Task (c) evaluated all three patients (P004, P006, P008) across 11 total tool calls and passed. Only the Clinical AI Committee or any authorized body should make that edit, with changes recorded in Git commits and tracked by the "policy_version" and "approved_by" fields in agent_policy.json.

## Lab 3: the approval gate
What did the log record when you approved and when you rejected? Name one action in your final project that belongs in approval_required, and one that does not, with a reason.

1. When approved, the audit log records an "approved" event and then allows the tool to execute, completing the next step such as a care note.
2. When rejected, the audit log records a "rejected" event, and with no following approval, the task stops immediately after two tool attempts.
3. Actions that directly change care, access, or finances—like sending a nurse to a home or restructuring a loan—belong in "approval_required" because a human must take responsibility for these impactful decisions.
4. Routine reading or analysis tasks, such as looking up records or calculating risk, don’t require approval since they don’t alter anything directly.

## Lab 4: evidence
From your audit summary: tasks, allowed calls, refusals, approvals. Who should read this log, how often, and what would make them pull the agent?

1. The audit summary's log shows 67 events, including 11 tasks, 36 allowed tool calls, 3 policy refusals, 5 approvals, 1 rejection, and 11 tasks using the scripted planner.
2. The Clinical AI Governance Committee, auditors, and privacy officers review the log weekly or bi-weekly, with instant alerts for policy refusals.
3. The agent may be pulled for unauthorized data actions, automatic approvals, or low model accuracy below 0.78.

## Lab 5: a language model in the planner's seat (if you ran it)
Did the model take the same route as the scripted planner? With --inject on P009: did it try export_records, and what stopped it?
1. The scripted planner safely evaluated Nadia Petrov (P009), predicted a high readmission risk (0.686), and scheduled a nurse follow-up after human approval.
2. In normal use, an LLM follows the same steps as the scripted planner but also generates a summary in natural language.
3. The scripted planner ignores unstructured text and is immune to prompt injection, while runtime policy rules prevent unauthorized actions like exporting records, stopping exploits at the policy enforcement stage.

## Declaration
I ran every command above in my own Codespace and the numbers are from my own run.  Initials: F.N.N
