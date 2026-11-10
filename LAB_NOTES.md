# Lab notes: Agents I (governing actions)

Name:Kelly A
GitHub username:kellyaltamira892

Write one to three sentences per lab, with numbers or names from YOUR run. Commit and push this file to your fork by 9:25, then open a pull request titled "Lab notes: Firstname Lastname".

## Lab 0: the loop
What are the five things the loop does on every step, and which two of them are governance rather than intelligence? The five things the loop went through is observe, plan , check, act and log. Check and log are the governance portions.

## Lab 1: the model as a tool
P003 and P006: their probabilities and bands from your run. Which feature drove P006's risk most? Who authored the model card the agent relies on? Priya's probabilty was .12 and Hector's was .90. Priya's band was low and Hectors was high. The features prior admissions and er visits. 

## Lab 2: capability is not permission
Three refusals: the tool or patient, and the policy line that caused each. After your edit (max_patients_per_task, max_tool_calls_per_task), what finished that did not before, and who should be allowed to make that edit? . The refusal came before the tool ran, and the log holds the reason.


## Lab 3: the approval gate
What did the log record when you approved and when you rejected? Name one action in your final project that belongs in approval_required, and one that does not, with a reason.It was never approved as it was rejected. what was rejected was the drivers. The luukop and predict  stopped at the first action.

## Lab 4: evidence
From your audit summary: tasks, allowed calls, refusals, approvals. Who should read this log, how often, and what would make them pull the agent? It should be read often after every action. Any flag that is crossed and tries to cross should be pulled.

## Lab 5: a language model in the planner's seat (if you ran it)
Did the model take the same route as the scripted planner? With --inject on P009: did it try export_records, and what stopped it? The plan did change however nothing else did.

## Declaration
I ran every command above in my own Codespace and the numbers are from my own run.  Initials:KA
