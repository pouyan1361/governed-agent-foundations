# Lab notes: Agents I (governing actions)

Name: Manchi Hsu
GitHub username: manchihsu

Write one to three sentences per lab, with numbers or names from YOUR run. Commit and push this file to your fork by 9:25, then open a pull request titled "Lab notes: Firstname Lastname".

## Lab 0: the loop
What are the five things the loop does on every step, and which two of them are governance rather than intelligence?

The loop performs five steps: observe, plan, check policy, act, and log. Policy checking and logging are governance steps because they control premitted actions and record evidence. In my run, the agent looked up P003 and saved a care note using two allowed tool calls, with no human approval required.

## Lab 1: the model as a tool
P003 and P006: their probabilities and bands from your run. Which feature drove P006's risk most? Who authored the model card the agent relies on?

P003 socred 0.128 (LOW), while P006 scored 0.909 (HIGH). P006's top driver was prior_admissions_12m, and the agent booked a follow-up for day 3 with automatic approval. The model card comes from the Week 6 federation team; the agent consumes it rather than authoring it.

## Lab 2: capability is not permission
Three refusals: the tool or patient, and the policy line that caused each. After your edit (max_patients_per_task, max_tool_calls_per_task), what finished that did not before, and who should be allowed to make that edit?

Export was refused beucase export_records was not on the allow list; P011 was refused beucase the data scope allowed only Northfate; the multi-patient task stopped after P004 beucase max_patients_per_task was 1. Afer increasing max_patient_per_task to 3 and max_tool_calls_per_task to 12, all three patients were processed using 11 tool calls. OOnly authorized governance owners should approve policy changes, with the change, reason, and approval recorded in a reviewed GitHub pull request and commit history.

## Lab 3: the approval gate
What did the log record when you approved and when you rejected? Name one action in your final project that belongs in approval_required, and one that does not, with a reason.

When I approved schedule_followup for P007 with days=3, the log recorded approval in ask mode, and the task completed with 4 tool calls. When I used deny mode, the log recorded rejection, and the task stopped after 2 tool calls without scheduling the follow-up. As a possible final-project example, carrying out a recommended action should require human approval because it affects operations, while calculating summary statistics within the permitted data scope should not.

## Lab 4: evidence
From your audit summary: tasks, allowed calls, refusals, approvals. Who should read this log, how often, and what would make them pull the agent?

My audit log contained 57 events across 9 tasks, with 30 allowed tool calls, 3 policy refusals, 5 approvals, and 1 rejected approval; all tasks used the scripted planner. Northgate Health IT should review the log daily, with the Clinical AI Committee reviewing it weekly; unauthorized data access, actions executed without required approval, or model accuracy below 0.78 should trigger suspension and investigation. Routine follow-up scheduling could be considered for automation only after committee approval and prospective validation showing safe, appropriate scheduling with reliable monitoring and an escalation process.

## Lab 5: a language model in the planner's seat (if you ran it)
Did the model take the same route as the scripted planner? With --inject on P009: did it try export_records, and what stopped it?

I completed the no-API-key scripted comparison for P009. The agent predicted 0.686 (HIGH), requested human approval, and booked a day-3 follow-up after I approved, completing 4 tool calls. I did not run the LLM or --inject experiment, so I cannot report whether an LLM attempted export_records; the policy would refuse that tool because it is not on the allow list.

## Declaration
I ran every command above in my own Codespace and the numbers are from my own run.  Initials: MH
