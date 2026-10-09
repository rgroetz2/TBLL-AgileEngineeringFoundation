# G118 Login Prompts and Outputs V1-V3

## Prompt V1

Generate four high-quality negative test cases for the ToolShop login feature.

**Website:** [https://practicesoftwaretesting.com/auth/login](https://practicesoftwaretesting.com/auth/login)

---

## Result V1

These four test cases cover common negative login scenarios. Expected results are proposed expectations to confirm against the requirements; the tests have not been executed. The linked page did not expose readable form details through retrieval, so exact error messages remain unverified.

**Common preconditions:** The user is logged out and the ToolShop login page is open.

### TC-LOGIN-NEG-01 — Incorrect password

**Additional precondition:** An active, registered test account exists.

#### Test data

- Email: Registered test account’s email.
- Password: A nonempty password known to be incorrect.

#### Steps

1. Enter the registered email address.
2. Enter the incorrect password.
3. Click the login button.

#### Expected result

- Login is rejected.
- An authentication error is displayed.
- The user remains logged out and cannot access protected account information.

### TC-LOGIN-NEG-02 — Invalid email format

#### Test data

- Email: tester.example.com — missing @.
- Password: A nonempty test password.

#### Steps

1. Enter the invalid email address.
2. Enter the password.
3. Attempt to submit the login form.

#### Expected result

- Login does not succeed.
- Validation indicates that the email format is invalid.
- The user remains logged out.

**To confirm:** Whether validation occurs when leaving the email field or when submitting the form.

### TC-LOGIN-NEG-03 — Unregistered email address

**Additional precondition:** Confirm that the test email is not registered.

#### Test data

- Email: A syntactically valid, unregistered test email.
- Password: A nonempty test password.

#### Steps

1. Enter the unregistered email address.
2. Enter the password.
3. Click the login button.

#### Expected result

- Login is rejected.
- An authentication error is displayed.
- The user remains logged out.

**To confirm:** The required error wording. This test does not assume that the application should reveal whether an account exists.

### TC-LOGIN-NEG-04 — Empty mandatory fields

**Test data:** Email and password are both empty.

#### Steps

1. Ensure that both fields are empty.
2. Attempt to submit the login form.

#### Expected result

- Login does not succeed.
- Required-field validation identifies both missing values.
- The user remains logged out.

**Assumption to confirm:** Both email and password are mandatory.

---

## Prompt V2

Generate four high-quality negative test cases for the ToolShop login feature.

**Website:** [https://practicesoftwaretesting.com/auth/login](https://practicesoftwaretesting.com/auth/login)

Use the user story and its acceptance criteria below as the test basis. For each test case, identify the relevant scenario from the user story. Clearly label any expected behavior that is not supported by the user story as an assumption requiring confirmation.

### Title: Login

### User Story:

As a customer,

I want to be able to log in

so that I can track my orders and administer my personal information.

### Acceptance Criteria:

#### Scenario: User logs in using valid credentials

**Given** I'm a logged-out user and on the login page

**When** I log in with valid credentials

**Then** I am logged into my account

### Alternatives:

N/A

### Errors:

#### Scenario: Email and password are mandatory

**Given** I'm a logged-out user and on the login page

**When** I just hit the Login button

**Then** the system notifies me that email and password are mandatory.

#### Scenario: Email must be in correct format

**Given** I'm a logged-out user and on the login page

**When** I fill in an email in the wrong format

**Then** the system notifies me that the email-address is written in the wrong format.

#### Scenario: Customer must exist in the database

**Given** I'm a logged-out user and on the login page

**When** I try to login with an email-address in the correct format and some random password

**Then** the system notifies me that there is something wrong with the given email-address or password.

---

## Result V2

**Test basis:** The supplied user story and acceptance criteria. These test cases have not been executed. Exact message wording and validation timing are not specified.

**Common preconditions:** The user is logged out and the ToolShop login page is open.

### TC-LOGIN-NEG-01 — Incorrect password for an existing customer

**Relevant scenario:** “Customer must exist in the database.”

**Traceability note:** This is the closest relevant scenario because it describes an email/password error. An existing customer entering an incorrect password is not explicitly covered.

#### Test data

- Email: Email address of an existing test customer.
- Password: A nonempty password confirmed to differ from that customer’s correct password.

#### Steps

1. Enter the existing customer’s email address.
2. Enter the incorrect password.
3. Click the Login button.

#### Expected results

- Assumption requiring confirmation: The system notifies the user that there is something wrong with the email address or password. This extends the referenced scenario to an existing customer with an incorrect password.
- Assumption requiring confirmation: Login is rejected and the user remains logged out.

### TC-LOGIN-NEG-02 — Invalid email format

**Relevant scenario:** “Email must be in correct format.”

#### Test data

- Email: tester.example.com — missing the @ symbol.
- Password: A nonempty test password.

#### Steps

1. Enter the invalid email address.
2. Enter the test password.
3. Click the Login button if submission is available.

#### Expected results

- Supported by the user story: The system notifies the user that the email address is in the wrong format.
- Assumption requiring confirmation: Login does not succeed and the user remains logged out.

### TC-LOGIN-NEG-03 — Unregistered email address

**Relevant scenario:** “Customer must exist in the database.”

**Additional precondition:** Confirm that the test email address does not exist in the customer database.

#### Test data

- Email: A correctly formatted email address confirmed to be unregistered.
- Password: A nonempty random test password.

#### Steps

1. Enter the unregistered email address.
2. Enter the random password.
3. Click the Login button.

#### Expected results

- Supported by the user story: The system notifies the user that there is something wrong with the given email address or password.
- Assumption requiring confirmation: Login is rejected and the user remains logged out.

### TC-LOGIN-NEG-04 — Both mandatory fields empty

**Relevant scenario:** “Email and password are mandatory.”

#### Test data

- Email: Empty.
- Password: Empty.

#### Steps

1. Clear the email and password fields, including any autofilled values.
2. Click the Login button.

#### Expected results

- Supported by the user story: The system notifies the user that email and password are mandatory. The notification must communicate that both values are required; separate messages are not explicitly required.
- Assumption requiring confirmation: Login does not succeed and the user remains logged out.

---

## Prompt V3

### Supplementary manual observations

The following observations describe the current system behavior. They supplement the user story but do not replace its requirements.

Common precondition: The user is logged out and on the login page. The interface language is German.

| ID | Input and action | Observed result |
| --- | --- | --- |
| M02 | Enter a registered email address and an incorrect password. Click “Einloggen”. | “Invalid email or password” is displayed. |
| M03 | Enter `sara.d.hotmail.com` and a nonempty password. Click “Einloggen”. | “Email Format ist ungültig” is displayed. |
| M04 | Enter `customer5@practicesoftwaretesting.com`, confirmed to be unregistered, and a nonempty password. Click “Einloggen”. | “Invalid email or password” is displayed. |
| M05 | Leave both fields empty. Click “Einloggen”. | “Email ist verpflichtend” and “Passwort ist verpflichtend” are displayed. |

Use these observations to make the four test cases more precise. Reference the relevant user-story scenario and manual observation ID.

Write concrete actions and concise, observable expected results. Preserve the observed error messages in their original language. Distinguish requirement-based expectations from observed behavior.

Avoid repeated explanations. State shared assumptions or uncertainties once, and identify the affected test cases. Do not infer validation timing from these observations.

---

## Result V3

**Common preconditions:** The user is logged out, the ToolShop login page is open, and the interface language is German.

#### Shared notes

- Exact error messages below come from the manual observations; the user story specifies their meaning.
- The incorrect-password case (TC-01) is not explicitly covered by the user story. Its expected message is supported by M02; treating it as a requirement needs confirmation.
- Validation timing is unspecified. Check the displayed messages after the listed actions without asserting when validation occurs.
- Suggested passwords below are test data, not values recorded in the observations.

### TC-01 — Incorrect password

**User-story reference:** “Customer must exist in the database” — related scenario; see shared note.

**Manual observation:** M02.

**Test data:** A registered test customer’s email and WrongPassword123!, confirmed to differ from that customer’s correct password.

#### Actions

1. Enter the registered email address in the email field.
2. Enter WrongPassword123! in the password field.
3. Click “Einloggen”.

#### Expected result — observed behavior

- The message “Invalid email or password” is displayed.

### TC-02 — Invalid email format

**User-story reference:** “Email must be in correct format”.

**Manual observation:** M03.

**Test data:** Email sara.d.hotmail.com; password TestPassword123!.

#### Actions

1. Enter sara.d.hotmail.com in the email field.
2. Enter TestPassword123! in the password field.
3. Click “Einloggen”.

#### Expected result — requirement-based

- A notification indicates that the email format is invalid.

#### Expected result — observed behavior

- The message “Email Format ist ungültig” is displayed.

### TC-03 — Unregistered email address

**User-story reference:** “Customer must exist in the database”.

**Manual observation:** M04.

**Additional precondition:** Confirm that customer5@practicesoftwaretesting.com remains unregistered in the test environment.

**Test data:** Email customer5@practicesoftwaretesting.com; password TestPassword123!.

#### Actions

1. Enter customer5@practicesoftwaretesting.com in the email field.
2. Enter TestPassword123! in the password field.
3. Click “Einloggen”.

#### Expected result — requirement-based

- A notification indicates that something is wrong with the supplied email address or password.

#### Expected result — observed behavior

- The message “Invalid email or password” is displayed.

### TC-04 — Both mandatory fields empty

**User-story reference:** “Email and password are mandatory”.

**Manual observation:** M05.

**Test data:** Email and password both empty.

#### Actions

1. Clear the email field, including any autofilled value.
2. Clear the password field, including any autofilled value.
3. Click “Einloggen”.

#### Expected result — requirement-based

- The system notifies the user that both email and password are mandatory.

#### Expected result — observed behavior

- Both messages are displayed:
- “Email ist verpflichtend”
- “Passwort ist verpflichtend”
