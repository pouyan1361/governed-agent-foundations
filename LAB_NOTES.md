# Lab notes: Agents I (governing actions)

Name: Anahit Grigoryan
GitHub username:  

Write one to three sentences per lab, with numbers or names from YOUR run. Commit and push this file to your fork by 9:25, then open a pull request titled "Lab notes: Firstname Lastname".

## Lab 0: the loop
What are the five things the loop does on every step, and which two of them are governance rather than intelligence?
The loop observes, plans, checks policy, acts, and logs. In P003 (`e9ef2bbb`), the policy check and logging were the governance steps.

## Lab 1: the model as a tool
P003 and P006: their probabilities and bands from your run. Which feature drove P006's risk most? Who authored the model card the agent relies on?
P003 was 0.128 (LOW) and P006 was 0.909 (HIGH); `prior_admissions_12m` mattered most for P006. The Week 6 federation/model team made the model card; it doesn’t name an individual.

## Lab 2: capability is not permission
Three refusals: the tool or patient, and the policy line that caused each. After your edit (max_patients_per_task, max_tool_calls_per_task), what finished that did not before, and who should be allowed to make that edit?
It refused `export_records` because it wasn’t allowed, P011 because Kestrel is outside Northgate’s scope, and P006 because the patient limit was 1. Raising the patient and tool-call limits to 3 and 12 let task (c) finish for P004, P006, and P008 in 11 calls. Northgate Health IT should make policy changes with the Clinical AI Committee’s approval.

## Lab 3: the approval gate
What did the log record when you approved and when you rejected? Name one action in your final project that belongs in approval_required, and one that does not, with a reason.
When I approved, P007’s follow-up was booked after 4 calls. When I rejected, the task stopped after 2 and nothing was booked. I’d require approval for `schedule_followup` because it changes care, but not for `predict_readmission`, which only gives decision support.

## Lab 4: evidence
From your audit summary: tasks, allowed calls, refusals, approvals. Who should read this log, how often, and what would make them pull the agent?
My run had 18 tasks, 59 allowed calls, 5 refusals, 9 approvals, 3 rejections, and 18 scripted planners. Health IT could check the log daily, with a weekly review by the Clinical AI Committee. I’d stop the agent for a policy breach, a skipped approval, or Northgate accuracy below 0.78.

## Lab 5: a language model in the planner's seat (if you ran it)
Did the model take the same route as the scripted planner? With --inject on P009: did it try export_records, and what stopped it?
I didn’t have an API key, so I couldn’t run the LLM.
## Declaration
I ran every command above in my own Codespace and the numbers are from my own run.  Initials: AG
