# Software Tester (QA) Assessment

## Testing Approach

I would start with the main things a user needs to do: create an account, log in, and manage tasks. I would test normal inputs as well as empty, invalid, very long, and duplicate inputs. I would also check that each user can only see their own tasks and that changes are still there after a refresh or a new login. Error messages should be clear and understandable.

**Assumption:** The assessment does not give the exact field names or validation rules. I use "task data" for any required information used to create a task. Password rules and whether deletion needs confirmation should be checked with the requirements before testing.

## Test Cases

The cases are grouped by feature. Since the task fields are not specified, the examples use general task data rather than inventing field names.

### Registration

**REG-01 - Register with valid data**
Steps / Test data: Enter a new valid name, email, password, and confirmation if available. Submit.
Expected result: The account is created and the user sees a success message or is taken to the login page.
Type: Positive

**REG-02 - Required registration fields are empty**
Steps / Test data: Submit the form with all fields empty.
Expected result: The form is not submitted. Each required field shows an understandable message.
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
Expected result: Registration is blocked and the password rules are shown. If there are no rules, they should be agreed before testing.
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
Expected result: Spaces are handled consistently. Extra spaces are removed or the user sees a clear validation message.
Type: Edge

**REG-09 - Very long registration input**
Steps / Test data: Enter values near and above the allowed maximum length.
Expected result: Values within the limit are accepted. Values over the limit are rejected without causing an error in the page or database.
Type: Edge

**REG-10 - Special characters in registration fields**
Steps / Test data: Use valid characters in the name and password, including punctuation where allowed.
Expected result: Allowed characters work correctly. Disallowed characters are rejected with a clear message.
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
Expected result: Login fails and the message does not reveal unnecessary account details.
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
Expected result: The same email rules are used each time. A valid email should not fail because of harmless spaces or letter-case differences unless this is documented.
Type: Edge

**LOG-07 - Multiple failed login attempts**
Steps / Test data: Submit several incorrect passwords for the same account.
Expected result: The application has a reasonable response, such as a short lockout, a limit on attempts, or a warning. The password is never shown.
Type: Security

**LOG-08 - Logout and protected page access**
Steps / Test data: Log in, log out, then try to open the task page using the previous URL or browser Back button.
Expected result: The user cannot open the task data and is sent back to the login page.
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
Expected result: The task is saved and displayed as entered. The special characters must not break the page or run as code.
Type: Edge

**TASK-05 - View tasks after login**
Steps / Test data: Log in as a user who has existing tasks.
Expected result: The list loads and shows the correct tasks. A user with no tasks sees a clear empty-list message or state.
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
Expected result: Only the intended number of tasks is created. Double-clicking does not create unwanted duplicates.
Type: Edge

### Input Validation and Error Handling

**ERR-01 - Server or database failure during create**
Steps / Test data: Simulate a test-environment failure while saving a task.
Expected result: A clear error is shown. The user is not told that the task was saved if the save failed.
Type: Error handling

**ERR-02 - Server or database failure during edit or delete**
Steps / Test data: Simulate a failure while updating or deleting a task.
Expected result: The user receives a useful error. The screen does not say that data changed or was deleted when it did not.
Type: Error handling

**ERR-03 - Unexpected technical error**
Steps / Test data: Cause or simulate an invalid request or unavailable service.
Expected result: The application shows a simple error message. It does not show stack traces, SQL statements, file paths, or other internal details.
Type: Error handling

**ERR-04 - Repeated form submission after an error**
Steps / Test data: Submit invalid data, correct it, and submit again.
Expected result: Old error messages are cleared when appropriate, and the corrected request works normally.
Type: Edge

## Potential Bugs and Risk Areas

These are possible bugs or areas that need checking. They are not confirmed defects because the application has not been run.

### 1. User data may be accessible through another user's account

**Severity:** Critical
**Reason/Impact:** If ownership is not checked, one user might see, edit, or delete another user's tasks. This would be a serious privacy problem.

### 2. Passwords may be stored or exposed insecurely

**Severity:** Critical
**Reason/Impact:** If passwords are saved as plain text or appear in responses or logs, someone could use them to access user accounts.

### 3. Login may not properly protect authenticated pages

**Severity:** Major
**Reason/Impact:** A logged-out user might still see task data through the Back button or an old URL if the session is not checked properly.

### 4. Duplicate accounts or tasks may be created

**Severity:** Major
**Reason/Impact:** Repeated clicks or missing database checks could create duplicate accounts or tasks, which would make the list confusing.

### 5. Task changes may not be saved correctly

**Severity:** Major
**Reason/Impact:** The screen might show success even though the change was not saved. The user could then lose the work after refreshing or logging in again.

### 6. Delete may remove the wrong record

**Severity:** Critical
**Reason/Impact:** A wrong task ID or missing ownership check could delete the wrong task or more than one task.

### 7. Invalid or very long input may cause errors

**Severity:** Major
**Reason/Impact:** Without validation, long or invalid input could cause database errors, a broken page, failed requests, or unsafe data to be saved.

### 8. Database or server errors may show misleading results

**Severity:** Major
**Reason/Impact:** The application might show success when saving failed, or show a task as deleted when it still exists. This could cause data loss and confusion.

### 9. Error messages may expose technical information

**Severity:** Minor
**Reason/Impact:** Stack traces, SQL errors, or internal paths could give an attacker useful information and would also be confusing for users.

### 10. Session handling may be incorrect

**Severity:** Major
**Reason/Impact:** A session that never expires, stays active after logout, or is shared between users could give someone access to private task data.

## Final Testing Priorities

- Registration and login, including password handling and failed-login protection.
- Access control to confirm users can only view and modify their own tasks.
- Create, edit, and delete actions, especially incorrect IDs and accidental duplicate submissions.
- Data persistence after refresh, logout, and login again.
- Database/server failures and the quality of user-facing error messages.
