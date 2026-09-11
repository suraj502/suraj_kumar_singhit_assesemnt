# Software Tester (QA) Assessment

## Testing Approach

I would test the main user journeys first: registering, logging in, and managing tasks. I would check both successful and unsuccessful inputs, including empty, invalid, very long, and duplicate data. I would also check that data belongs to the correct user and remains available after refreshing or logging in again. The application should show clear errors without exposing technical details.

**Assumption:** The assessment does not define exact field names or validation rules. I refer to the task's required fields as "task data." Where password rules or delete confirmation are mentioned, they should be confirmed against the final requirements.

## Test Cases

The cases below are grouped by feature so they are easier to read than one wide table.

### Registration

**REG-01 - Register with valid data**  
Steps / Test data: Enter a new valid name, email, password, and confirmation if available. Submit.  
Expected result: The account is created and the user receives a clear success message or is taken to login.  
Type: Positive

**REG-02 - Required registration fields are empty**  
Steps / Test data: Submit the form with all fields empty.  
Expected result: The form is not submitted. Each required field shows a useful validation message.  
Type: Negative

**REG-03 - One required field is missing**  
Steps / Test data: Leave one required field empty and complete the others.  
Expected result: Registration is blocked and the missing field is identified.  
Type: Negative

**REG-04 - Invalid email format**  
Steps / Test data: Use values such as `user`, `user@`, or `user.example.com`.  
Expected result: Registration is blocked with an email format message.  
Type: Negative

**REG-05 - Weak password**  
Steps / Test data: Use a password that does not meet the documented rules.  
Expected result: Registration is blocked and the password requirements are explained. If no rules exist, this requirement should be clarified.  
Type: Negative

**REG-06 - Password confirmation does not match**  
Steps / Test data: Enter different values in the password and confirmation fields.  
Expected result: Registration is blocked and the mismatch is clearly shown.  
Type: Negative

**REG-07 - Existing email address**  
Steps / Test data: Register using an email already linked to an account.  
Expected result: A second account is not created. A clear message explains that the email is already registered.  
Type: Negative

**REG-08 - Leading and trailing spaces**  
Steps / Test data: Add spaces before and after the name and email.  
Expected result: The application handles spaces consistently, preferably trimming harmless spaces or showing a clear validation message.  
Type: Edge

**REG-09 - Very long registration input**  
Steps / Test data: Enter values near and above the allowed maximum length.  
Expected result: Allowed values are accepted. Overly long values are rejected without breaking the page or database.  
Type: Edge

**REG-10 - Special characters in registration fields**  
Steps / Test data: Use valid characters in the name and password, including punctuation where allowed.  
Expected result: Valid characters are handled correctly. Invalid characters are rejected with a clear message.  
Type: Edge

### Login

**LOG-01 - Login with valid credentials**  
Steps / Test data: Enter a registered email and correct password.  
Expected result: Login succeeds and the user can access the task list.  
Type: Positive

**LOG-02 - Wrong password**  
Steps / Test data: Enter a registered email with an incorrect password.  
Expected result: Login fails and a clear, non-sensitive error is shown.  
Type: Negative

**LOG-03 - Unregistered email**  
Steps / Test data: Enter an email that has no account.  
Expected result: Login fails. The message must not expose unnecessary account details.  
Type: Negative

**LOG-04 - Empty login fields**  
Steps / Test data: Submit the login form with both fields empty.  
Expected result: Login is blocked and required-field messages are displayed.  
Type: Negative

**LOG-05 - Invalid login email format**  
Steps / Test data: Enter an invalid email format with any password.  
Expected result: The email is rejected before login is attempted, or a suitable validation message is shown.  
Type: Negative

**LOG-06 - Email case and spaces**  
Steps / Test data: Try the registered email with different letter casing and accidental spaces.  
Expected result: Behavior follows the defined email rules consistently. A valid email should not fail only because of harmless formatting differences unless documented.  
Type: Edge

**LOG-07 - Multiple failed login attempts**  
Steps / Test data: Submit several incorrect passwords for the same account.  
Expected result: The application applies its defined protection, such as rate limiting, temporary lockout, or an appropriate warning. Passwords are not revealed.  
Type: Security

**LOG-08 - Logout and protected page access**  
Steps / Test data: Log in, log out, then try to open the task page using the previous URL or browser Back button.  
Expected result: The user cannot access protected task data and is sent to login.  
Type: Negative

### Task CRUD

**TASK-01 - Create a valid task**  
Steps / Test data: Log in and enter valid task data. Submit it.  
Expected result: The task is created once and appears in the user's task list.  
Type: Positive

**TASK-02 - Create a task with empty required data**  
Steps / Test data: Leave required task data empty and submit.  
Expected result: The task is not created and the missing data is identified.  
Type: Negative

**TASK-03 - Create a task with very long data**  
Steps / Test data: Enter task data near and above the allowed length.  
Expected result: Valid maximum-length data is handled. Excessively long input is rejected safely.  
Type: Edge

**TASK-04 - Create a task with special characters**  
Steps / Test data: Use punctuation, line breaks, and other allowed special characters.  
Expected result: The task is stored and displayed correctly without corrupting the page or being treated as executable content.  
Type: Edge

**TASK-05 - View tasks after login**  
Steps / Test data: Log in as a user who has existing tasks.  
Expected result: The task list loads and shows the correct tasks. Empty-list behavior is also clear for a user with no tasks.  
Type: Positive

**TASK-06 - Edit an existing task**  
Steps / Test data: Open a task, change its data, and save.  
Expected result: The updated task appears in the list with the new values.  
Type: Positive

**TASK-07 - Edit with empty or invalid data**  
Steps / Test data: Remove required task data or enter an invalid value, then save.  
Expected result: The update is blocked and the original task data is not lost.  
Type: Negative

**TASK-08 - Delete an existing task**  
Steps / Test data: Delete a task and confirm if a confirmation step exists.  
Expected result: The selected task is removed and other tasks remain unchanged.  
Type: Positive

**TASK-09 - Cancel deletion**  
Steps / Test data: Start deleting a task and cancel the confirmation if available.  
Expected result: The task remains in the list unchanged.  
Type: Negative

**TASK-10 - Edit only the selected task**  
Steps / Test data: Open one task and verify that editing changes only that task.  
Expected result: No other task is changed.  
Type: Edge

**TASK-11 - Verify persistence after refresh**  
Steps / Test data: Create or edit a task, refresh the page, and view the list again.  
Expected result: The saved change remains visible.  
Type: Positive

**TASK-12 - Verify persistence after re-login**  
Steps / Test data: Create or edit a task, log out, and log in again.  
Expected result: The task and its latest changes are still present.  
Type: Positive

**TASK-13 - Verify task ownership**  
Steps / Test data: Create tasks for User A. Log in as User B and view, edit, or delete tasks.  
Expected result: User B cannot see or change User A's tasks.  
Type: Security

**TASK-14 - Duplicate task submission**  
Steps / Test data: Submit the create action repeatedly, including by double-clicking if possible.  
Expected result: Only the intended number of tasks is created. Accidental duplicate records are avoided or clearly handled.  
Type: Edge

### Input Validation and Error Handling

**ERR-01 - Server or database failure during create**  
Steps / Test data: Simulate a test-environment failure while saving a task.  
Expected result: A clear error is shown. The user is told whether the task was saved, and no misleading success message is displayed.  
Type: Error handling

**ERR-02 - Server or database failure during edit or delete**  
Steps / Test data: Simulate a failure while updating or deleting a task.  
Expected result: The user receives a useful error. Existing data is not incorrectly shown as changed or deleted.  
Type: Error handling

**ERR-03 - Unexpected technical error**  
Steps / Test data: Cause or simulate an invalid request or unavailable service.  
Expected result: The application shows a friendly error without stack traces, SQL statements, file paths, or other sensitive technical information.  
Type: Error handling

**ERR-04 - Repeated form submission after an error**  
Steps / Test data: Submit invalid data, correct it, and submit again.  
Expected result: Old error messages do not remain incorrectly, and the corrected request is processed normally.  
Type: Edge

## Potential Bugs and Risk Areas

These are potential bugs or risks to investigate. They are not confirmed defects because the application has not been executed.

### 1. User data may be accessible through another user's account

**Severity:** Critical  
**Reason/Impact:** A missing ownership check could allow one user to view, edit, or delete another user's tasks. This is a serious privacy and security issue.

### 2. Passwords may be stored or exposed insecurely

**Severity:** Critical  
**Reason/Impact:** If passwords are stored in plain text, included in responses, or exposed in logs, user accounts could be compromised.

### 3. Login may not properly protect authenticated pages

**Severity:** Major  
**Reason/Impact:** A user who logs out might still access task data through the browser Back button or an old URL if sessions are not checked correctly.

### 4. Duplicate accounts or tasks may be created

**Severity:** Major  
**Reason/Impact:** Repeated submissions, double-clicks, or missing database constraints could create duplicate records and make task management confusing.

### 5. Task changes may not be saved correctly

**Severity:** Major  
**Reason/Impact:** An edit or create request may show success but fail to save to the database. Users could lose work after refreshing or logging in again.

### 6. Delete may remove the wrong record

**Severity:** Critical  
**Reason/Impact:** Incorrect task IDs or weak ownership checks could delete another task or multiple tasks instead of only the selected one.

### 7. Invalid or very long input may cause errors

**Severity:** Major  
**Reason/Impact:** Missing validation could cause database errors, broken page layouts, failed requests, or unsafe data being stored.

### 8. Database or server errors may show misleading results

**Severity:** Major  
**Reason/Impact:** The application might show a success message when a save failed, or show a task as deleted when it still exists. This can lead to data loss and user confusion.

### 9. Error messages may expose technical information

**Severity:** Minor  
**Reason/Impact:** Stack traces, SQL errors, or internal paths could reveal information useful to an attacker and make the application look unfinished.

### 10. Session handling may be incorrect

**Severity:** Major  
**Reason/Impact:** Sessions that never expire, remain active after logout, or are shared between users could allow unauthorized access to task data.

## Final Testing Priorities

- Registration and login, including password handling and failed-login protection.
- Access control to confirm users can only view and modify their own tasks.
- Create, edit, and delete actions, especially incorrect IDs and accidental duplicate submissions.
- Data persistence after refresh, logout, and login again.
- Database/server failures and the quality of user-facing error messages.
