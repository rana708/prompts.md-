# day02-prompts.md

Framework 
1. Role
2. Task
3. Context
4. Constraints
5. Format
6. Examples
7. Success criteria

## Prompt #1
Original:
review this code:
def get_user(user_id):
    query = "SELECT * FROM users WHERE id = " + user_id
    result = db.execute(query)
    return result[0]

What is missing:
Role — There is no specification of the reviewer’s role or expertise (junior, senior, security-focused, etc.).
Context — There is no information about whether the function is running in production or where `user_id` comes from.
Constraints — There is no clear scope (should the review focus on security, performance, style, or something else?).
Format — There is no specified format for the response.
Examples — There is no definition of what should be considered **“high risk”** versus a lower-severity issue.
Success Criteria — There is no definition of what makes the response **“complete” or “successful.”**
(The Task is implicitly present: “review this code.”)

The complete version:
Role: You are a senior Python backend engineer doing a security-focused code review.
Task: Review the function below and identify bugs, security issues, and bad practices.
Context: This function runs in a production web app that handles user authentication data; user_id comes directly from an HTTP request parameter.
Constraints: Focus only on correctness, security, and error handling — not naming/style.
Format: Return a numbered list. For each issue: [Severity: High/Medium/Low] Issue → Why it matters → Suggested fix (code snippet).
Examples/Criteria: A "High" severity issue is one that could cause a security breach or crash in production.
Success criteria: I should be able to copy your fixes directly into the file.
Code:
def get_user(user_id):
    query = "SELECT * FROM users WHERE id = " + user_id
    result = db.execute(query)
    return result[0]

Original output (summary):
The response came in the form of a free-form report with two headings: "Issues Identified" and "Recommended Solution":
- It classified the SQL Injection vulnerability as "Critical," explaining that an attacker could submit `'1 OR 1=1'`.
- It identified the potential for an `IndexError` if the query returned no results.
- It noted the lack of validation regarding the type or existence of `user_id`.
- Finally, it provided a corrected version of the code using a parameterized query and an `if not result` check, while noting that the placeholder (e.g., `%s` vs. `?`) might vary depending on the library used.
- The response did not include explicit severity levels, nor did it clearly separate each issue from its corresponding solution.

Rewritten output (summary):
The response came as a strictly numbered list in the exact required format:
[High] SQL Injection — with a detailed explanation of the exploitation scenario (data dumping, authentication bypass, and data modification) + a code fix.
[High] Unhandled IndexError — essentially the same issue, but with a clearer connection to the request thread crashing and returning HTTP 500.
[Medium] Unvalidated Input Type — additional detail: distinguish between the case where user_id is None (causing TypeError) and the case where the value is invalid when converting to int (causing ValueError), and 
provide a separate fix for each case.
At the end, provide one final consolidated function that integrates all three solutions and is ready to copy and paste directly.

The difference:
Immediate Copy-Paste Usability: Only the completed version provided one final function that was ready to paste directly into the project (the “Success criteria”), whereas the original provided the solution in one piece without fully consolidating the function.
Accuracy in Error Handling: The completed version explicitly distinguished between the None case and the failure to convert the value to int (ValueError/TypeError handled separately). This more precise detail appeared because the Context clarified that the value comes from an HTTP request. The original version handled the issue more generally.
Ease of Quick Review: The required format (Severity → Why → Fix) made it easier to identify the priority of each issue immediately, whereas the original relied on the paragraph order and mentioned the word “Critical” only once at the beginning.
Note: The difference was not in “discovering additional issues” — both versions identified exactly the same three issues (SQL injection, IndexError, and type validation), because this code follows a classic pattern that the model could recognize even without the additional context. The real difference was in organization, detailed precision, and final usability, rather than the depth of the core analysis.




## Prompt #2
(The hypothetical project used for the experiment: csv2json — a Python CLI tool that converts CSV to JSON, used by an internal data team, maintained by a single person, with a private repository.)

Original:
write documentation for the project

What is missing:
Role — There is no specification of the writer’s role or identity (technical writer? Who is the target audience?).
Context — There is no project name or description of what it does (the model doesn’t know how to document a project that hasn’t been provided).
Format — There is no specification of the document format (README? Wiki? API docs?).
Examples — There is no definition of what counts as “good” documentation in this context.
Success Criteria — There is no clear definition of what exactly is required for the documentation to be considered complete.
Constraints — These are partially implied but not explicitly defined: Should the documentation be written for developers or beginners?
(Task موجود ضمنيًا: "write documentation")

The complete version:
Role: You are a technical writer who specializes in beginner-friendly documentation for non-programmers.
Task: Write a README.md for this project.
Context: The project is "csv2json" — a Python command-line tool that converts CSV files into JSON. It's used internally by a small data team. The reader has likely never used a command line/terminal before.
Constraints: Assume zero programming knowledge. Briefly explain what a "terminal" is before using it. Avoid jargon. Keep the whole thing under 400 words.
Format: README.md with these sections: What is this? / Installation / How to use it / Example.
Examples/Criteria: Success means a non-technical person could follow every step without getting stuck or needing to Google anything extra.
Success criteria: The text should be ready to paste directly into README.md with no further editing.

Original output (summary):
The response was provided as a very generic README filled with unrealistic placeholders: a generic title, “Project Documentation,” a generic description, “[core functionality],” and standard installation steps (git clone, pip install -r requirements.txt) without explaining any terminology. It implicitly assumed that the reader was a programmer who already knew how to use Git and the terminal. It also included generic sections such as “Contributing” and “License” that were not requested and were unrelated to the actual context (because no real context had been provided in the first place).

Rewritten output (summary):
The response was provided as a complete README specifically tailored to `csv2json`:
* **“What is this?”** explains the tool in simple language, comparing CSV to an Excel file and JSON to a data format.
* **“Installation”** explains what the terminal is before asking the reader to use it, and adds a verification step (`python --version`) before anything else.
* **“How to use it”** provides simple numbered steps: place the file, enter the command, and find the resulting output.
* **“Example”** includes a realistic example with a `sales.csv` file and an actual `sales.json` output.
* It ends with **“Stuck? Ask in the #data-team Slack channel”**, providing a clear support path for readers who are not technically experienced.


The difference:
Real Content, Not Placeholders: The original couldn’t write about a project that wasn’t provided, so it used [core functionality] instead of an actual explanation. The completed version, thanks to the Context, wrote an accurate and specific description of csv2json.

Audience Consideration Changed the Level of Explanation Entirely: The original assumed the reader knew how to use git clone and pip install without explaining them. The completed version, because the Constraints specified non-programmers as the audience, explained what the terminal is and added a verification step before asking the reader to use something more complex.

Ready for Immediate Publishing: The original contained placeholders and unnecessary sections (License, Contributing) that would need to be manually edited before publishing. The completed version, because of the “Success criteria,” was ready to paste directly into README.md without any additional changes.



## Prompt #3
(The hypothetical system used for the experiment: a simple web authentication system — Signup / Login / Password Reset, an internal tool for ~50 employees, not public-facing.)

Original:
make a test plan for the whole system

What is missing:
Role — There is no specification of the test plan author’s role (QA Lead? What level of experience?).
Context — There is no description of the “system” itself: What are its features? Who is it for? How many users does it have?
Constraints — There is no clear scope (functional testing only? Or should performance/load testing also be included?).
Format — There is no specified structure (table? List? Separate sections for each feature?).
Examples/Criteria— There is no definition of what qualifies as a **“High priority”** test case.
Success Criteria — There is no definition of what it means for the test plan to be **“ready for execution.”**
(The Task/Goal is implicitly present: “make a test plan.”)

The complete version:
Role: You are a QA lead planning test coverage for a web application.
Task: Create a test plan for the system described below.
Context: The system is a simple web login system with three features: Signup (email + password), Login (email + password, with "remember me" option), and Password Reset (via email link). It's a small internal tool, not public-facing, used by ~50 employees.
Constraints: Cover functional and security testing only — skip performance/load testing since traffic is very low. Assume manual testing (no test automation framework set up yet).
Format: A table per feature with columns: Test Case | Steps | Expected Result | Priority (High/Medium/Low).
Examples: A "High" priority test case is one where failure would let an unauthorized user access an account, or block a legitimate user entirely.
Success criteria: A QA tester with no prior context should be able to execute the plan directly without needing to ask me clarifying questions.

Original output (summary):
The response was provided as a very generic test plan consisting of five sections: **Objective, Scope, Test Cases, Test Environment, and Exit Criteria**.
The **“Test Cases”** section was particularly speculative: it mentioned **“verify user login works”** as a general example even though the model did not actually know whether the system included a login feature—it was simply assuming a common feature.
The **“Scope”** section listed four types of testing (**functional, integration, performance, and security**) without providing any implementation details for how each type should be performed.

Rewritten output (summary):
The response was delivered as a structured test plan with a separate table for each of the three actual features (**Signup, Login, and Password Reset**):
* Each table contained **4–5 test cases** with the columns **Test Case / Steps / Expected Result / Priority**.
* It included specific, practical security scenarios: an **SQL injection attempt** in the email field, prevention of **user enumeration** (showing a generic error message instead of revealing whether an email exists), and **account lockout** after repeated failed login attempts.
* It explicitly excluded **performance/load testing** at the end and explained why (**low internal traffic**), following the specified **Constraints**.
Available next action: Create a downloadable DOCX file here in this chat containing the plan and action items above

The difference:
1. Specific Test Cases Instead of General Guesswork: The original generally assumed a “login” feature without any real system details. The completed version, thanks to the Context (the details of the three actual features), covered each feature with precise test cases based on specific, realistic behavior.
2.Real Security Depth Instead of an Empty Label: The original only listed “Security testing” as an item in the Scope without providing any details. The completed version, based on the Examples/Criteria that defined what qualifies as **High priority**, produced actual security test cases that were directly executable.
3.Immediate Execution Readiness: The original provided a general list of points that required the QA tester to write the steps themselves. The completed version, because of the specified Format and Success Criteria, was delivered as tables containing ready-to-execute steps, expected results, and priorities, without requiring additional questions.






