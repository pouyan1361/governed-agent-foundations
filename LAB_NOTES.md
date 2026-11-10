# Lab notes: Agents I (governing actions)

Name: Riley Phipps
GitHub username: rileyphipps23

Write one to three sentences per lab, with numbers or names from YOUR run. Commit and push this file to your fork by 9:25, then open a pull request titled "Lab notes: Firstname Lastname".

## Lab 0: the loop
What are the five things the loop does on every step, and which two of them are governance rather than intelligence?
The five things the loop does on every step are observes the current information or result, chooses the next action or tool to use, carries out the approved tool action, checks the action against the policy, and records the action and result in the audit log. The last two are the ones that are governance rather than intelligence.

## Lab 1: the model as a tool
P003 and P006: their probabilities and bands from your run. Which feature drove P006's risk most? Who authored the model card the agent relies on?
P003: probability = 0.128, band low
P006: probability = 0.909, band high
The feature that drove P006’s risk the most was prior_admissions_12m. The model card was authored by federated-learning/model team that trained the week 6 model.

## Lab 2: capability is not permission
Three refusals: the tool or patient, and the policy line that caused each. After your edit (max_patients_per_task, max_tool_calls_per_task), what finished that did not before, and who should be allowed to make that edit?
Export_records:tool refused because it was not in the policy’s "allowed_tools" list. in policy line "allowed_tools": [
  "lookup_patient",
  "predict_readmission",
  "write_care_note",
  "schedule_followup"
]
P011: Patient refused because it belonged to Kestrel, while "data_scope" only allowed Northgate. policy line "data_scope": {
  "hospitals": ["Northgate"]
}
P006: Patient refused because "max_patients_per_task": 1 had already been reached. policy line "max_patients_per_task": 1
What finished that did not before was the assessment of P006 and P008, including follow-ups for P006 and a care note for P008. Only a policy administrator or Northgate Clinical AI Committee should be allowed to make that edit. 

## Lab 3: the approval gate
What did the log record when you approved and when you rejected? Name one action in your final project that belongs in approval_required, and one that does not, with a reason.
When I rejected the action, the log showed that the approval was denied and that the follow-up was not scheduled. In my final project, schedule_followup should require approval because it directly affects patient care. lookup_patient does not need approval because it only retrieves information.

## Lab 4: evidence
From your audit summary: tasks, allowed calls, refusals, approvals. Who should read this log, how often, and what would make them pull the agent?
The Northgate Clinical AI Committee, compliance staff, and nurse supervisor should review the audit log. It should be checked once a week and immediately whenever an unusual refusal, approval, or activity appears. The agent should be stopped if it tries to use forbidden tools, accesses the wrong hospital’s records, exceeds its limits, skips required approval, writes inaccurate notes, or begins producing many errors or refusals.

## Lab 5: a language model in the planner's seat (if you ran it)
Did the model take the same route as the scripted planner? With --inject on P009: did it try export_records, and what stopped it?
Yes. The scripted planner followed the route of looked up P009, predicted readmission risk, requested approval to schedule a follow-up, and wrote a care note. The --inject test could not run because the OpenAI API account had no credits. Therefore, we do not know whether the LLM attempted to use export_records. If it had, the policy would have blocked it because that tool is not approved, and the event would have been added to the audit log.

## Declaration
I ran every command above in my own Codespace and the numbers are from my own run.  Initials: RP
