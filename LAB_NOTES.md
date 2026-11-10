# Lab notes: Agents I (governing actions)

Name: Joseph Galvez
GitHub username: josephg753

Write one to three sentences per lab, with numbers or names from YOUR run. Commit and push this file to your fork by 9:25, then open a pull request titled "Lab notes: Firstname Lastname".

## Lab 0: the loop
What are the five things the loop does on every step, and which two of them are governance rather than intelligence?

The five things that the loop does are plan (look up patient), review policy is in place, create an event, assign a task or tool and print the final result. Plan and policy are governance because they require the agent to review its processes to make sure it is doing the correct steps. 

## Lab 1: the model as a tool
P003 and P006: their probabilities and bands from your run. Which feature drove P006's risk most? Who authored the model card the agent relies on?

The features that drove P006's risk are prior_admissions_12m, er_visits_6m, and lives_alone. A human authored the model card, it was done by a coordinator who manually approved the action to schedule a followup with P006.

## Lab 2: capability is not permission
Three refusals: the tool or patient, and the policy line that caused each. After your edit (max_patients_per_task, max_tool_calls_per_task), what finished that did not before, and who should be allowed to make that edit?

Before the changes, the model only reviewed P004 and stopped because the max patients was 1. Once it finished, it reviewed P006 and 008 as well. This should be determined by what the controls are by the hospital, since it is for Northgate - the coordinator for them should make that decision to edit.

## Lab 3: the approval gate
What did the log record when you approved and when you rejected? Name one action in your final project that belongs in approval_required, and one that does not, with a reason.

When approved, the log recorded the approval and continued to run the analysis with following steps. Once rejected, nothing happened and the log recorded this too. One action that should be in approval_required is an <action> as that would require the model to make a change and <summarize_member> would not need to be in there as it is only reading the data.

## Lab 4: evidence
From your audit summary: tasks, allowed calls, refusals, approvals. Who should read this log, how often, and what would make them pull the agent?

The person reading the log should be Northgate's coordinator and it should review the log at least once a day to make sure no changes have been made. Readmission would make the coordinator pull from the agent.

## Lab 5: a language model in the planner's seat (if you ran it)
Did the model take the same route as the scripted planner? With --inject on P009: did it try export_records, and what stopped it?

The model did the same thing with or without --inject as it stopped the request from c

## Declaration
I ran every command above in my own Codespace and the numbers are from my own run.  Initials: JG
