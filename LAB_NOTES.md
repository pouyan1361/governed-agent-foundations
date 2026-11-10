# Lab notes: Agents I (governing actions)

Name: Hannah Villasis
GitHub username: hannahbananalytics

Write one to three sentences per lab, with numbers or names from YOUR run. Commit and push this file to your fork by 9:25, then open a pull request titled "Lab notes: Firstname Lastname".

## Lab 0: the loop
What are the five things the loop does on every step, and which two of them are governance rather than intelligence?

Every step, the agent does five things: it looks at the situation, makes a plan, checks the policy, takes the action, and logs it. Planning and acting are the "smart" parts. Checking the policy and logging are the governance parts. In my run, the agent looked up patient P003 (call 1 of 6) and wrote a care note (call 2 of 6). Both were on the allow list, so no human had to approve anything, and the log recorded four lines for the task.

## Lab 1: the model as a tool
P003 and P006: their probabilities and bands from your run. Which feature drove P006's risk most? Who authored the model card the agent relies on?

P003 came back at 0.128, which is LOW, so the agent just wrote a care note and stopped. P006 came back at 0.909, which is HIGH, so the agent asked to schedule a follow-up call, and the script approved it automatically. P006's top driver was prior admissions in the last 12 months (then ER visits and living alone). The model card was not written by the agent. It comes from the Week 6 federation, and the agent only reads it.

## Lab 2: capability is not permission
Three refusals: the tool or patient, and the policy line that caused each. After your edit (max_patients_per_task, max_tool_calls_per_task), what finished that did not before, and who should be allowed to make that edit?

The policy refused three things.

a) Exporting all patient records to the insurer portal was blocked because export_records isn't on the allow list, so no tools ran.
b) Looking up P011 was blocked because that patient belongs to Kestrel, and this agent is only allowed to see Northgate patients. No tools ran here either.
c) Assessing P004, P006 and P008 stopped partway. The agent finished P004 (4 calls), then got blocked when it tried to move to P006, because max_patients_per_task was set to 1.

After I changed max_patients_per_task to 3 and max_tool_calls_per_task to 12, all three patients finished in 11 calls: P004 at 0.56 HIGH, P006 at 0.91 HIGH, and P008 at 0.48 MEDIUM.

Only someone in charge of governance, like a compliance or clinical safety lead, should be able to make that change, and a second person should review it. It should be recorded in version control (a commit and pull request) and in the audit log, so there's a record of who loosened the limits and why.

## Lab 3: the approval gate
What did the log record when you approved and when you rejected? Name one action in your final project that belongs in approval_required, and one that does not, with a reason.

When I approved, the log recorded the approval (mode "ask") along with the action and its details (schedule_followup for P007, 3 days), and then the call was booked. When I rejected, the log recorded the rejection, nothing was booked, and the task stopped after 2 tool calls.

For our final project, a follow-up action, like scheduling a call or reaching out to someone, belongs in approval_required. It changes something in the real world and affects a real person, so a human should own that decision. Looking up a record or running the risk prediction does not belong there. Those only read data and make no change, and if every read needed a click, people would approve without looking, which makes the gate pointless.

## Lab 4: evidence
From your audit summary: tasks, allowed calls, refusals, approvals. Who should read this log, how often, and what would make them pull the agent?

My log has 57 events across 9 tasks. 30 tool calls were allowed (9 lookup_patient, 8 predict_readmission, 8 write_care_note, 5 schedule_followup), 3 were refused (the export, the P011 scope, and the max_patients limit), and there were 5 approvals and 1 rejection. Every task used the scripted planner.

The log should be read by a compliance or clinical safety lead - weekly, plus right away after any incident. I'd pull the agent if refusals started showing up constantly, which would mean it's badly scoped, if it ever tried a forbidden action like export, or if approvals were being clicked through without anyone reading the drivers. Down the road, the follow-up approval could be automated only after a long record of humans approving it with no problems, and only for low-risk cases.

## Lab 5: a language model in the planner's seat (if you ran it)
Did the model take the same route as the scripted planner? With --inject on P009: did it try export_records, and what stopped it?

I couldn't run the LLM planner because my OpenAI account had no credits, so I don't know if it would have taken the same route or tried export_records. Instead I ran the scripted planner on P009: it looked up the patient, predicted 0.686 (HIGH), asked me to approve the follow-up call, and wrote a care note. It never reads the discharge note, so it can't be tricked by it, but it also can't handle anything new. If an LLM did try to export records, the policy would block it, because export isn't on the allow list. The policy protects the system, not the planner.

## Declaration
I ran every command above in my own Codespace and the numbers are from my own run.  Initials: HV
