# Lab notes: Agents I (governing actions)

Name:
GitHub username:

Write one to three sentences per lab, with numbers or names from YOUR run. Commit and push this file to your fork by 9:25, then open a pull request titled "Lab notes: Firstname Lastname".

## Lab 0: the loop
On every step, the loop asks the planner for the next action, checks the action against the policy, decides whether to allow, approve, or refuse it, runs the tool if allowed, and writes an audit log entry. The governance steps are policy.check() and logging; the planner's choice is the intelligence step.

## Lab 1: the model as a tool
In my run, P003 scored 0.13 (LOW) and P006 scored 0.91 (HIGH). The biggest driver for P006 was prior_admissions_12m, with a push of 2.404 in the model output, and the agent relies on the Week 6 federated model card stored in models/federated_readmission.json rather than on any model code it writes itself.

## Lab 2: capability is not permission
The default policy refused export_records because it was not in allowed_tools, refused patient P011 because data_scope was only Northgate, and stopped the multi-patient task after one patient because max_patients_per_task was 1. After editing governance/agent_policy.json to max_patients_per_task = 3 and max_tool_calls_per_task = 12, the task finished for P004, P006, and P008 in one run. Only a policy owner or the Northgate Clinical AI Committee should be allowed to make that change, and it should be recorded in the policy history and audit log.

## Lab 3: the approval gate
For patient P007, the agent allowed lookup_patient and predict_readmission, then stopped at schedule_followup because it is in approval_required. In the approved run, the audit log recorded an approval event for schedule_followup({"patient_id": "P007", "days": 3}) with outcome "approved", and the task finished with risk 0.81 (HIGH), follow-up booked. In the rejected run, the audit log recorded outcome "rejected" and the final task ended after 2 tool calls with "human rejected the action." In the final project, schedule_followup belongs in approval_required because it changes care by booking a nurse callback; write_care_note does not, because it only documents the decision and is already allowed under the policy.

## Lab 4: evidence
The audit log for this run contains 10 tasks, 32 allowed tool calls, 3 refusals, and 5 approvals with 1 rejection. The refusals were export_records (not on the allow list), P011 outside Northgate scope, and the multi-patient task ending after one patient when max_patients_per_task was 1; the approvals show schedule_followup booked for high-risk patients while the rejection stopped the P007 follow-up before any action ran. The log should be reviewed by the Northgate Clinical AI Committee, the nurse manager or operational clinical lead, and the IT governance owner at least weekly, with immediate review if any approval is outside policy, a patient is outside scope, or a task exceeds budget or touches a protected action.

## Lab 5: a language model in the planner's seat (if you ran it)
 The Lab 5 request reached the OpenAI API, but the run stopped with a 429 error because the account had no remaining credits. The model did not plan or act; the same policy, approval gate, and audit log remain in place, and the P009 injection case still needs a successful run to verify its behavior.

## Declaration
I ran every command above in my own Codespace and the numbers are from my own run.  Initials:
