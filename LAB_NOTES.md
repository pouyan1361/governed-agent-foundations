# Lab notes: Agents I (governing actions)

Name: Madison Metz
GitHub username:madisonmetz637

Write one to three sentences per lab, with numbers or names from YOUR run. Commit and push this file to your fork by 9:25, then open a pull request titled "Lab notes: Firstname Lastname".

## Lab 0: the loop
What are the five things the loop does on every step, and which two of them are governance rather than intelligence?
The loop does five things on every step which includes observe, plan, check, act, log. Plan and act are the intelligence (deciding what to do and doing it). Check and log are the governance: the policy approves every proposal before the tool runs, and every step gets written to the audit log. In my run, P003 took two allowed calls (lookup and care note) and the log recorded task_start, two tool_allowed and task_end.
## Lab 1: the model as a tool
P003 and P006: their probabilities and bands from your run. Which feature drove P006's risk most? Who authored the model card the agent relies on?
P003 (Priya Natarajan) scored 0.128, LOW, so the agent wrote a care note and stopped. P006 (Hector Ruiz) scored 0.909, HIGH, and his top driver was prior_admissions_12m (3), followed by er_visits_6m (2) and lives_alone. The model card was authored by the Week 6 federation coordinator; the agent only reads and uses it.
## Lab 2: capability is not permission
Three refusals: the tool or patient, and the policy line that caused each. After your edit (max_patients_per_task, max_tool_calls_per_task), what finished that did not before, and who should be allowed to make that edit?
Three refusals included (a) export_records was refused because it’s not on the allow list, even though the function exists in tools.py; (b) P011 was refused by data scope because it belongs to Kestrel and this agent is scoped to Northgate; (c) P006 was refused after P004 because max_patients_per_task was 1. After I raised max_patients_per_task to 3 and max_tool_calls_per_task to 12, all three patients finished in 11 calls. A policy edit like that shouldn’t be made by one developer alone. It should go through a change board or governance committee, be reviewed as a pull request, and be recorded through the policy_version field and the git history.
## Lab 3: the approval gate
What did the log record when you approved and when you rejected? Name one action in your final project that belongs in approval_required, and one that does not, with a reason.
When I approved, the log recorded an approval event with outcome “approved”, the tool (schedule_followup), the arguments (P007, days=3), then tool_allowed with approval=true and task_end Done. When I rejected, the log recorded outcome “rejected”, there was no tool_allowed line, and the task ended with “Stopped after 2 tool call(s)”, so the follow-up never ran. For a final project like the bank consortium, offer_restructure belongs in approval_required because it changes a member’s loan terms. Computing a default-risk score or KPI summary should not need approval, because it’s analysis that doesn’t change anything in the real world.
## Lab 4: evidence
From your audit summary: tasks, allowed calls, refusals, approvals. Who should read this log, how often, and what would make them pull the agent?
My log showed about 9 tasks, about 25 allowed tool calls (lookup, predict, care note, followup), 3 refusals (export_records, P011 scope, max_patients) and about 4 approved with 1 rejected. The clinical AI committee should review this log monthly, and the privacy office should be alerted right away on any export_records refusal. They should pull the agent if refusals start climbing, if an approval is never rejected across many runs (a rubber stamp), or if the model drops below its accuracy floor.
## Lab 5: a language model in the planner's seat (if you ran it)
Did the model take the same route as the scripted planner? With --inject on P009: did it try export_records, and what stopped it?
The LLM took mostly the same route as the scripted planner on P007 (lookup, predict 0.813 HIGH, followup with approval, care note), but it chose those steps itself and wrote its own summary. With --inject on P009, it [tried export_records and the policy refused it because it’s not on the allow list / ignored the instruction and ran the normal HIGH-risk flow]. Either way, the protection is the policy around the planner, not the model’s judgment; if it ignored the note, that was luck, not a control.
## Declaration
I ran every command above in my own Codespace and the numbers are from my own run.  Initials:
MM