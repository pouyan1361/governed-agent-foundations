# Lab notes: Agents I (governing actions)

Name: Denise Anciola
GitHub username: @denisecanciola99

Write one to three sentences per lab, with numbers or names from YOUR run. Commit and push this file to your fork by 9:25, then open a pull request titled "Lab notes: Firstname Lastname".

## Lab 0: the loop
What are the five things the loop does on every step, and which two of them are governance rather than intelligence?
The loop does five things each step: observe, plan, check, act, log. Check and log are governance. Plan and act are intelligence.


## Lab 1: the model as a tool
P003 and P006: their probabilities and bands from your run. Which feature drove P006's risk most? Who authored the model card the agent relies on?
P003 scored 0.128 (LOW). P006 scored 0.909 (HIGH). P006's top driver was prior_admissions_12m (3). The Week 6 federation coordinator authored the model card.


## Lab 2: capability is not permission
Three refusals: the tool or patient, and the policy line that caused each. After your edit (max_patients_per_task, max_tool_calls_per_task), what finished that did not before, and who should be allowed to make that edit?
export_records was refused by allowed_tools. P011 was refused by data_scope (Kestrel, not Northgate). P006 was refused by max_patients_per_task = 1. After my edit, all three patients finished in 11 calls. Only the Clinical AI Committee should make that edit, recorded in policy_version and git history.


## Lab 3: the approval gate
What did the log record when you approved and when you rejected? Name one action in your final project that belongs in approval_required, and one that does not, with a reason.
Approve logged outcome "approved", mode "ask", and the follow-up was booked. Reject logged outcome "rejected", mode "deny", and nothing ran. A home visit belongs in approval_required because it changes care. Descriptive KPIs do not because they change nothing.



## Lab 4: evidence
From your audit summary: tasks, allowed calls, refusals, approvals. Who should read this log, how often, and what would make them pull the agent?
10 tasks, 32 allowed calls, 3 refusals, 5 approved and 1 rejected. The Clinical AI Committee should review it monthly. They should pull the agent for rising refusals, zero rejections, or accuracy below 0.78.


## Lab 5: a language model in the planner's seat (if you ran it)
Did the model take the same route as the scripted planner? With --inject on P009: did it try export_records, and what stopped it?
My key connected, but both runs stopped with a 429 "no credits remaining" error, so the LLM never planned a step. With no LLM output, I can't say whether it would have tried export_records. The allow list would still have refused it, because the policy checks every call no matter which planner proposes it.


## Declaration
I ran every command above in my own Codespace and the numbers are from my own run.  Initials: DA
