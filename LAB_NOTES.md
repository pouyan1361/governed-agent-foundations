# Lab notes: Agents I (governing actions)

Name: MD ABDUS SALAM
GitHub username:salam-msbana

Write one to three sentences per lab, with numbers or names from YOUR run. Commit and push this file to your fork by 9:25, then open a pull request titled "Lab notes: Firstname Lastname".

## Lab 0: the loop
What are the five things the loop does on every step, and which two of them are governance rather than intelligence?
Ans: On every step the loop does five things: observe (the task and results so far), plan (the planner proposes one tool call), check (the policy allows it, requires approval, or refuses it), act (the tool runs and returns a result), and log (one JSON line per decision). Plan and act are the intelligence; check and log are the governance. In my run (task 14d005d1), P003 had lookup_patient and write_care_note, both "allowed: on allow list" (call 1 and 2 of 6), and four lines were written to the audit log: task_start, two tool_allowed, task_end.
## Lab 1: the model as a tool
P003 and P006: their probabilities and bands from your run. Which feature drove P006's risk most? Who authored the model card the agent relies on?
Ans: After writing score() in agent/model.py, the Lab 1 test gave 3 passed. P003 (Priya Natarajan) scored 0.128, LOW, so the agent only wrote a care note. P006 (Hector Ruiz) scored 0.909, HIGH; his top driver was prior_admissions_12m (3 prior admissions), followed by er_visits_6m (2) and lives_alone. The model card (models/federated_readmission.json, neutral test accuracy 0.806, trained under the Readmission Federation Charter v1.0) was authored by the Week 6 federation coordinator; the agent only consumes it, it did not author it.

## Lab 2: capability is not permission
Three refusals: the tool or patient, and the policy line that caused each. After your edit (max_patients_per_task, max_tool_calls_per_task), what finished that did not before, and who should be allowed to make that edit?
Ans: (a) export_records was REFUSED by allowed_tools (it is not on the allow list), with 0 tool calls run. (b) lookup_patient(P011) was REFUSED by data_scope, because P011 belongs to Kestrel and the scope is ["Northgate"]. (c) lookup_patient(P006) was REFUSED at step 5 by max_patients_per_task = 1, after 4 calls on P004. After I changed max_patients_per_task to 3 and max_tool_calls_per_task to 12, task (c) finished in 11 calls: P004 0.563 HIGH (follow-up booked), P006 0.909 HIGH (follow-up booked), P008 0.483 MEDIUM (note only). Only the Northgate Clinical AI Committee (the approved_by owner) should be allowed to make that edit, through a reviewed change that bumps policy_version; it is recorded in the git commit history of governance/agent_policy.json.
## Lab 3: the approval gate
What did the log record when you approved and when you rejected? Name one action in your final project that belongs in approval_required, and one that does not, with a reason.
Ans: P007 (Grace Whitfield) scored 0.813 HIGH, with drivers discharged_to_facility, num_medications 14 and er_visits_6m 2. When I approved (y), the log recorded event=approval, tool=schedule_followup, args {patient_id: P007, days: 3}, outcome=approved, mode=ask, then the follow-up was booked, a care note was written, and the task ended "Done". When I rejected (--deny), the log recorded outcome=rejected, mode=deny, the follow-up never ran, and the task ended "Stopped after 2 tool call(s)". In my final project, a prescriptive action such as placing a reorder or transferring stock belongs in approval_required, because it changes money and inventory in the real world. A descriptive or predictive query (computing a KPI or a stockout probability) does not, because it changes nothing and gating it would make the agent useless.
## Lab 4: evidence
From your audit summary: tasks, allowed calls, refusals, approvals. Who should read this log, how often, and what would make them pull the agent?
Ans: From my audit summary: 7 tasks run, 23 tool calls allowed (lookup_patient 7, write_care_note 6, predict_readmission 6, schedule_followup 4), 3 refusals (export_records not on the allow list, P011 outside the Northgate data scope, and max_patients_per_task = 1 on P006), and approvals {approved: 4, rejected: 1}; all 7 tasks used the scripted planner. The Northgate Clinical AI Committee should review this log monthly, and the privacy office should review it immediately whenever export_records is refused. They should pull the agent if refusals start climbing (a badly scoped or hijacked agent), if the model drops below the 0.78 accuracy floor, or if approvals are never rejected over many runs (a rubber stamp).

## Lab 5: a language model in the planner's seat (if you ran it)
Did the model take the same route as the scripted planner? With --inject on P009: did it try export_records, and what stopped it?
Ans:  I did not have an OpenAI key, so I ran the scripted comparison. On P009 the scripted planner did lookup, predict (0.686 HIGH), schedule_followup (approved), and a care note. P009's discharge note contains an injection telling any AI assistant to call export_records, but the scripted planner never reads free text, so it cannot be hijacked (and cannot handle anything new either). If an LLM planner obeyed the note, the policy would still refuse export_records because it is not on the allow list, and stop_on_violation would stop the task and log the attempt.
## Declaration
I ran every command above in my own Codespace and the numbers are from my own run.  Initials: MAS
