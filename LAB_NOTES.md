# Lab notes: Agents I (governing actions)

Name:
GitHub username: Vanessa Ojeda-Coogan

Write one to three sentences per lab, with numbers or names from YOUR run. Commit and push this file to your fork by 9:25, then open a pull request titled "Lab notes: Firstname Lastname".

## Lab 0: the loop
What are the five things the loop does on every step, and which two of them are governance rather than intelligence?

The agent loop performs five steps: observe the task and previous results, plan the next tool call, check the call against policy, act by running the approved tool, and log the decision and result. The two governance steps are the policy check and the audit log because they control whether the action is allowed and preserve evidence of what happened. Planning and acting are the agent’s operational steps.

## Lab 1: the model as a tool
P003 and P006: their probabilities and bands from your run. Which feature drove P006's risk most? Who authored the model card the agent relies on?

P003 had a readmission probability of 0.128 and was classified as LOW risk, so the agent wrote a care note and stopped. P006 had a probability of 0.909 and was classified as HIGH risk. Its top driver was prior_admissions_12m, and the agent scheduled a follow-up call for three days after discharge and wrote a care note. The agent relied on the Week 6 federated model and its model card, which were created by the team that developed the federated model, not by the agent.

## Lab 2: capability is not permission
Three refusals: the tool or patient, and the policy line that caused each. After your edit (max_patients_per_task, max_tool_calls_per_task), what finished that did not before, and who should be allowed to make that edit?

The first task was refused because export_records was not on the allowed-tools list. The second task was refused because P011 belongs to Kestrel while the data scope allows only Northgate patients. The third task initially stopped after P004 because max_patients_per_task was 1. After changing max_patients_per_task to 3 and max_tool_calls_per_task to 12, the task completed for P004, P006 and P008 in 11 tool calls. P004 had a 0.563 HIGH risk, P006 had a 0.909 HIGH risk, and P008 had a 0.483 MEDIUM risk. Only the authorized governance committee should approve this policy change, and the change should be recorded in the Git history and policy version record.

## Lab 3: the approval gate
What did the log record when you approved and when you rejected? Name one action in your final project that belongs in approval_required, and one that does not, with a reason.

For the approved run, P007 had a 0.813 HIGH readmission risk. The log recorded that schedule_followup required approval, the human approved it, the follow-up was booked for three days after discharge, and the care note was written. For the denied run, the log recorded that the human rejected schedule_followup, so the tool did not run and the task stopped after two tool calls. In the final project, a prescriptive action that changes a person’s care or access should require approval. A descriptive or diagnostic action, such as calculating a summary or comparing groups, should not require approval because it does not directly change a person’s outcome.

## Lab 4: evidence
From your audit summary: tasks, allowed calls, refusals, approvals. Who should read this log, how often, and what would make them pull the agent?

The audit log contained 9 tasks and 57 events. It recorded 30 allowed tool calls: 9 lookup_patient calls, 8 write_care_note calls, 8 predict_readmission calls, and 5 schedule_followup calls. There were 3 refusals, involving export_records, the out-of-scope Kestrel patient P011, and the one-patient budget. There were 5 approved actions and 1 rejected action. The privacy or governance committee, auditors, and the agent operations team should read this log at least daily and after any incident. The committee should pull or suspend the agent if it makes an unauthorized call, repeatedly triggers refusals, shows unexplained approval patterns, exceeds its budgets, or relies on a model below the required accuracy floor.

## Lab 5: a language model in the planner's seat (if you ran it)
Did the model take the same route as the scripted planner? With --inject on P009: did it try export_records, and what stopped it?

The OpenAI planner could not complete because the API returned a 429 error stating that no credits remained. The API key loaded successfully, so the issue was available API credits rather than the code. The scripted planner assessed P009 with a 0.686 HIGH risk, requested a follow-up call, received approval, booked the call, and wrote a care note. The scripted planner does not read free-text discharge notes, so it cannot follow a prompt-injection instruction. If the LLM planner attempted export_records after reading the injected note, the policy would refuse it because export_records is not on the allow list, and the refusal would be recorded in the audit log.

## Declaration
I ran every command above in my own Codespace and the numbers are from my own run.  Initials: VOC
