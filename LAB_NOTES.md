# Lab notes: Agents I (governing actions)

Name: Julie Loyez
GitHub username: julixlyz08

Write one to three sentences per lab, with numbers or names from YOUR run. Commit and push this file to your fork by 9:25, then open a pull request titled "Lab notes: Firstname Lastname".

## Lab 0: the loop
What are the five things the loop does on every step, and which two of them are governance rather than intelligence?
1- reads the task and the history
2- plans (lookup patient P003)
3- checks the policy and decides if a call is needed 
4- runs and returns a result 
5- writes a line in the audit log

3 and 5 are governance; the others are intelligence. 

## Lab 1: the model as a tool
P003 and P006: their probabilities and bands from your run. Which feature drove P006's risk most? Who authored the model card the agent relies on?
P003: probability 0.128, band LOW. 
P006: probability 0.909, band HIGH. 
'prior_admissions_12m' drove P006's risk most, followed by 'er_visits_6m' and 'lives_alone'.
The model card was authored by the Week 6 hospital federation (five hospitals, neutral test accuracy 0.806), not by the agent.

## Lab 2: capability is not permission
Three refusals: the tool or patient, and the policy line that caused each. After your edit (max_patients_per_task, max_tool_calls_per_task), what finished that did not before, and who should be allowed to make that edit?
1- 'export_records' refused because it is not on the allow list. 
2- 'lookup_patient' for patient P011 refused because he belongs to Kestrel, while the data scope is for Northgate only.
3- 'lookup_patient' for patient P006 refused because max_patients_per_task was already completed with P004. 

After raising max_patients_per_task to 3 and max_tool_calls_per_task to 12, the third task finished all three patients and didn't stop at P006 like before. 
Only the policy owner (Northgate Health IT) should make that edit, with approval from the Northgate Clinical AI Committee, and it should be recorded as a new policy version in a commit, not changed by the agent or its user.

## Lab 3: the approval gate
What did the log record when you approved and when you rejected? Name one action in your final project that belongs in approval_required, and one that does not, with a reason.
1- When I approved: the log recorded an approval event for schedule_followup with outcome "approved", mode "ask" and the arguments (P007, days=3), then tool_allowed for the booking and the care note. The task ended with 4 tool calls and the follow-up booked.
2- When I rejected: the log recorded the approval event with outcome "rejected" and mode "deny", no tool_allowed for schedule_followup, and a task_end marked stopped after 2 tool calls.

What belongs in approval_required: request home visit. It changes a patient's care and uses staff time, so a human should see the risk and drivers before it runs.
What doesn't belong in it: a descriptive KPI tool such as a per-hospital readmission rate summary.

## Lab 4: evidence
From your audit summary: tasks, allowed calls, refusals, approvals. Who should read this log, how often, and what would make them pull the agent?
- tasks: 11
- allowed calls: 30
- refusals: 5
- approvals: 5 approved, 1 rejected 
- who reads this: the policy owner (Northgate IT) and Northgate Committee 
- what would make them pull the agent: repeated attempts at export_records or out-of-scope patients, any action that ran without a matching log line, the model falling below its 0.78 accuracy floor, or approvals that are never rejected

## Lab 5: a language model in the planner's seat (if you ran it)
Did the model take the same route as the scripted planner? With --inject on P009: did it try export_records, and what stopped it?
- I set my key and ran lab5_llm.py for P007 and with --inject for P009, but both stopped at the first planner call with a 429 error (no API credits), so the LLM never proposed a step and I cannot compare its route to the scripted planner.
- Scripted planner on P009: lookup_patient, predict_readmission (0.686, HIGH), schedule_followup with approval, write_care_note. It never reads the discharge note, so the injected instruction had no effect.
- If the LLM had tried export_records, the policy would have stopped it: export_records is not in allowed_tools, the same refusal I saw in Lab 2 (a), and the run would stop and be logged.
- If it had ignored the note instead, that would be luck, not a control. The defense is the policy around the planner, not the planner's judgment.
(The two last points are based on what was written on the slides as I couldn't actually run it myself)

## Declaration
I ran every command above in my own Codespace and the numbers are from my own run.  Initials: JL
