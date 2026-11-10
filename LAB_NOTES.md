# Lab notes: Agents I (governing actions)

Name: Carlos Herrera 
GitHub username: carloshe

Write one to three sentences per lab, with numbers or names from YOUR run. Commit and push this file to your fork by 9:25, then open a pull request titled "Lab notes: Firstname Lastname".

## Lab 0: the loop
What are the five things the loop does on every step, and which two of them are governance rather than intelligence?

The loop observes, plans, checks the proposed action against policy, acts by running the tool, and logs what happened. The governance steps are check and log because policy controls whether an action may run and the audit log records evidence of the decision.

## Lab 1: the model as a tool
P003 and P006: their probabilities and bands from your run. Which feature drove P006's risk most? Who authored the model card the agent relies on?

P003 scored 0.128 and was low risk, while P006 scored 0.909 and was high risk. P006’s top drivers were prior_admissions_12m, er_visits_6m, and lives_alone. The federated model card was authored by the Week 6 coordinator; the agent only consumes the model and its provenance.

## Lab 2: capability is not permission
Three refusals: the tool or patient, and the policy line that caused each. After your edit (max_patients_per_task, max_tool_calls_per_task), what finished that did not before, and who should be allowed to make that edit?

The agent refused export_records because it was not on the allowed list, refused P011 because the policy scope only allows northgate patients and stopped after P004 because max_patients_per_task was 1. After changing the policy to allow 3 patients and 12 tool calls, the task completed for P004, P006, and P008. A governance owner or AI committee should control this kind of policy change, and the change should be recorded through the policy version and Git history.

## Lab 3: the approval gate
What did the log record when you approved and when you rejected? Name one action in your final project that belongs in approval_required, and one that does not, with a reason.

when i approved schedule_followup for p007, the log recorded an approval for the tool and its arguments, and the follow-up was booked. When I rejected it, the log recorded the rejection and the action did not run. In a final project, an action that changes a person’s care, access, money, or status should require approval, while low-stakes analysis or read-only actions should not because they do not directly change the real-world outcome.

## Lab 4: evidence
From your audit summary: tasks, allowed calls, refusals, approvals. Who should read this log, how often, and what would make them pull the agent?

My audit log shows 11 tasks, 34 allowed tool calls, 3 refusals, 5 approvals, and 1 rejection. The clinical AI committee should review the log regularly, with the privacy office reviewing any attempted data exports or scope violations. The agent should be paused or pulled if refusal rates rise, approvals become a rubber stamp, or the model falls below its required performance floor.

## Lab 5: a language model in the planner's seat (if you ran it)
Did the model take the same route as the scripted planner? With --inject on P009: did it try export_records, and what stopped it?

My OpenAI run could not complete because the API account had no remaining credits, so I used the scripted comparison. P009 scored 0.686 HIGH, and the scripted planner looked up the patient, predicted risk, requested approval for schedule_followup, and then wrote a care note. It did not attempt export_records because the scripted planner does not read and follow free-text instructions from the discharge note. The governance lesson is that an LLM might follow a malicious instruction, but the policy should still refuse a forbidden tool such as export_records

## Declaration
I ran every command above in my own Codespace and the numbers are from my own run.  Initials:
