# Software Tester (QA) Assessment

## Testing Approach

I would test the main user journeys first: registering, logging in, and managing tasks. I would check both successful and unsuccessful inputs, including empty, invalid, very long, and duplicate data. I would also check that data belongs to the correct user and remains available after refreshing or logging in again. The application should show clear errors without exposing technical details.

**Assumption:** The assessment does not define exact field names or validation rules. I refer to the task's required fields as "task data." Where password rules or delete confirmation are mentioned, they should be confirmed against the final requirements.

## Test Cases

| Test Case ID | Scenario | Steps / Test Data | Expected Result | Type |
|---|---|---|---|---|
| REG-01 | Register with valid data | Enter a new valid name, email, password, and confirmation if available. Submit. | Account is created and the user receives a clear success message or is taken to login. | Positive |
| REG-02 | Required registration fields are empty | Submit the form with all fields empty. | The form is not submitted. Each required field shows a useful validation message. | Negative |
| REG-03 | One required field is missing | Leave one required field empty and complete the others. | Registration is blocked and the missing field is identified. | Negative |
| REG-04 | Invalid email format | Use values such as `user`, `user@`, or `user.example.com`. | Registration is blocked with an email format message. | Negative |
| REG-05 | Weak password | Use a password that does not meet the documented rules. | Registration is blocked and the password requirements are explained. If no rules exist, this requirement should be clarified. | Negative |
| REG-06 | Password confirmation does not match | Enter different values in the password and confirmation fields. | Registration is blocked and the mismatch is clearly shown. | Negative |
| REG-07 | Existing email address | Register using an email already linked to an account. | A second account is not created. A clear message explains that the email is already registered. | Negative |
| REG-08 | Leading and trailing spaces | Add spaces before and after the name and email. | The application handles spaces consistently, preferably trimming harmless spaces or showing a clear validation message. | Edge |
| REG-09 | Very long registration input | Enter values near and above the allowed maximum length. | Allowed values are accepted. Overly long values are rejected without breaking the page or database. | Edge |
| REG-10 | Special characters in registration fields | Use valid characters in the name and password, including punctuation where allowed. | Valid characters are handled correctly. Invalid characters are rejected with a clear message. | Edge |
| LOG-01 | Login with valid credentials | Enter a registered email and correct password. | Login succeeds and the user can access the task list. | Positive |
| LOG-02 | Wrong password | Enter a registered email with an incorrect password. | Login fails and a clear, non-sensitive error is shown. | Negative |
| LOG-03 | Unregistered email | Enter an email that has no account. | Login fails. The message must not expose unnecessary account details. | Negative |
| LOG-04 | Empty login fields | Submit the login form with both fields empty. | Login is blocked and required-field messages are displayed. | Negative |
| LOG-05 | Invalid login email format | Enter an invalid email format with any password. | The email is rejected before login is attempted, or a suitable validation message is shown. | Negative |
| LOG-06 | Email case and spaces | Try the registered email with different letter casing and with accidental spaces. | Behavior follows the defined email rules consistently. A valid email should not fail only because of harmless formatting differences unless documented. | Edge |
| LOG-07 | Multiple failed login attempts | Submit several incorrect passwords for the same account. | The application applies its defined protection, such as rate limiting, temporary lockout, or an appropriate warning. Passwords are not revealed. | Security |
| LOG-08 | Logout and protected page access | Log in, log out, then try to open the task page using the previous URL or browser Back button. | The user cannot access protected task data and is sent to login. | Negative |
| TASK-01 | Create a valid task | Log in and enter valid task data. Submit it. | The task is created once and appears in the user's task list. | Positive |
| TASK-02 | Create a task with empty required data | Leave required task data empty and submit. | The task is not created and the missing data is identified. | Negative |
| TASK-03 | Create a task with very long data | Enter task data near and above the allowed length. | Valid maximum-length data is handled. Excessively long input is rejected safely. | Edge |
| TASK-04 | Create a task with special characters | Use punctuation, line breaks, and other allowed special characters. | The task is stored and displayed correctly without corrupting the page or being treated as executable content. | Edge |
| TASK-05 | View tasks after login | Log in as a user who has existing tasks. | The task list loads and shows the correct tasks. Empty-list behavior is also clear for a user with no tasks. | Positive |
| TASK-06 | Edit an existing task | Open a task, change its data, and save. | The updated task appears in the list with the new values. | Positive |
| TASK-07 | Edit with empty or invalid data | Remove required task data or enter an invalid value, then save. | The update is blocked and the original task data is not lost. | Negative |
| TASK-08 | Delete an existing task | Delete a task and confirm if a confirmation step exists. | The selected task is removed and other tasks remain unchanged. | Positive |
| TASK-09 | Cancel deletion | Start deleting a task and cancel the confirmation if available. | The task remains in the list unchanged. | Negative |
| TASK-10 | Edit only the selected task | Open one task and verify that editing changes only that task. | No other task is changed. | Edge |
| TASK-11 | Verify persistence after refresh | Create or edit a task, refresh the page, and view the list again. | The saved change remains visible. | Positive |
| TASK-12 | Verify persistence after re-login | Create or edit a task, log out, and log in again. | The task and its latest changes are still present. | Positive |
| TASK-13 | Verify task ownership | Create tasks for User A. Log in as User B and view, edit, or delete tasks. | User B cannot see or change User A's tasks. | Security |
| TASK-14 | Duplicate task submission | Submit the create action repeatedly, including by double-clicking if possible. | Only the intended number of tasks is created. Accidental duplicate records are avoided or clearly handled. | Edge |
| ERR-01 | Server or database failure during create | Simulate a test-environment failure while saving a task. | A clear error is shown. The user is told whether the task was saved, and no misleading success message is displayed. | Error handling |
| ERR-02 | Server or database failure during edit/delete | Simulate a failure while updating or deleting a task. | The user receives a useful error. Existing data is not incorrectly shown as changed or deleted. | Error handling |
| ERR-03 | Unexpected technical error | Cause or simulate an invalid request or unavailable service. | The application shows a friendly error without stack traces, SQL statements, file paths, or other sensitive technical information. | Error handling |
| ERR-04 | Repeated form submission after an error | Submit invalid data, correct it, and submit again. | Old error messages do not remain incorrectly, and the corrected request is processed normally. | Edge |

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
