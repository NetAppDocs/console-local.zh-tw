# 1 Active directory integration

This section gives admins and operators a practical guide to configuring Directory Services, adding remote users from the Members page, and remote user sign-in. It focuses on workflow steps, validation points, and troubleshooting.

Only one directory service can be integrated at a time. To add another, the user must delete the first.

## 1.1 Directory Services Workflow

### 1.1.1 Open Directory Services

Go to **Settings** > **Identity & Access Management** > **Directory Services**.

Use **Add Active Directory integration** to start configuration.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=4936c9977a87&id=66d8ad32-188a-4fbd-9077-b96615080679&&collection=contentId-668081349&height=955&occurrenceKey=null&width=1916&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
### 1.1.2 Add a Directory Service

This flow is split into three required steps.

#### 1.1.2.1 Step 1: Server Connection

Enter host name, IP, connection timeout (in ms), and port.

**Default values used by the form:**

* Port: **636**
* Connection timeout: **1000** ms
* Use SSL/TLS: **enabled**

By default, secure LDAP configuration is selected, which uses port number 636 and SSL/TLS enabled. If the user wants to configure LDAP without security, they can uncheck the Use SSL/TLS checkbox and the port will automatically change to 389.

If the user wants to use a specific port, they can change it.

**LDAP vs LDAPS:**

* If **Use SSL/TLS = enabled**, protocol is **ldaps://** and default port is **636**.
* If **Use SSL/TLS = disabled**, protocol is **ldap://** and port is set to **389**.

When all required fields are filled, Test connection will be enabled.

Click **Test connection**.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=0b94a8010987&id=22db13be-e6ef-40dc-be7a-37d827af0953&&collection=contentId-668081349&height=963&occurrenceKey=null&width=1713&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
**Important LDAPS behavior:** When SSL/TLS is enabled, test-connection API fetches the certificate and updates a secret in Keycloak. The Keycloak pod is then restarted, which takes about 2 minutes, and SSL handshake is done after that. Due to this, we show a progress modal. The modal indicates testing is in progress and asks the user not to navigate away until complete.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=055d74364830&id=57c25c8f-ac31-4483-aabe-1584499cd882&&collection=contentId-668081349&height=961&occurrenceKey=null&width=1727&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
If the certificate is expired for an LDAPS connection, then we show a specific error message as shown below.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=b599d9d9a39c&id=f377e943-1475-418f-8a8b-98f8138a3544&&collection=contentId-668081349&height=1117&occurrenceKey=null&width=1728&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
After success, the user will see an inline success message and Step 2 becomes available. If the test fails, stay in Step 1 and fix host/port/SSL/timeout.

#### 1.1.2.2 Step 2: Binding Credentials

Enter Bind DN and Bind credentials.

Click **Authenticate**.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=fa0b6dee6d70&id=182468e1-7ba7-47d1-8639-b216e2b93bc4&&collection=contentId-668081349&height=965&occurrenceKey=null&width=1723&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
After success, Step 3 becomes available. If authentication fails, correct credentials and retry.

#### 1.1.2.3 Step 3: User DN Configuration

Enter **Users DN** (directory container to search users).

When all required checks are complete:

* Connection test passed
* Authentication test passed
* Users DN entered

Click **Add** to save.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=a9f5003b371e&id=495b2958-b92d-4f56-85df-b30cda743ec1&&collection=contentId-668081349&height=962&occurrenceKey=null&width=1727&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
Once directory service is integrated successfully, the integrated service will be shown on the page with all the configured parameters as shown below.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=6bbaee809b82&id=ad5f7bba-ca13-4d38-adf9-628b2c3e21db&&collection=contentId-668081349&height=965&occurrenceKey=null&width=1728&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
Now the Add Active Directory integration button will be disabled with the tooltip since only one Active Directory can be integrated at a time.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=18a296e03660&id=aa0f360a-790b-4cb9-b085-a08fb2a5cc82&&collection=contentId-668081349&height=1117&occurrenceKey=null&width=1728&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
### 1.1.3 Disable a Service

1. Open service row actions.
2. Select **Disable**.
3. Confirm **Disable**.
4. Save.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=90b544ee8c6a&id=731234de-6fe4-4c48-b92d-bbfc0462a552&&collection=contentId-668081349&height=1117&occurrenceKey=null&width=1728&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
Once service is disabled, the status will be changed to "Disabled" as shown below.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=cb778342ab0b&id=9bb40ebf-c3fa-4b76-aa40-d6afa1126e6a&&collection=contentId-668081349&height=974&occurrenceKey=null&width=1724&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
If service is disabled, then role assignment in the Members page for remote user will not work and will fail with the following error.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=d23777888aee&id=dba025a1-0fcd-4364-8b5a-b9e4d852f364&&collection=contentId-668081349&height=966&occurrenceKey=null&width=1722&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
Make sure service is enabled to assign roles to remote user.

### 1.1.4 Enable a Service

1. Open service row actions.
2. Select **Enable**.
3. Click on Enable button.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=1844db9ef33b&id=43e1840b-1203-4d1e-8de8-342c1b46e709&&collection=contentId-668081349&height=961&occurrenceKey=null&width=1728&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
### 1.1.5 Delete a Service

Service can be deleted only if it is disabled first. Enabled services cannot be deleted directly. If service is enabled, the delete button will be disabled with the following tooltip.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=491b7f8cdc15&id=4b0d7f59-8f9f-4bec-9e73-45d300ca31c4&&collection=contentId-668081349&height=962&occurrenceKey=null&width=1728&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
1. Open service row actions.
2. Select **Delete**.
3. Review warnings.
4. Confirm delete.

Before showing final confirmation, the page checks for linked remote users and shows warning text when applicable.

**Important LDAPS behavior:** For deleting LDAPS services, it might take a minute before it gets deleted since the imported certificate associated with this LDAPS connection will also be removed.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=e4e016311853&id=70077f56-306d-4d49-bd54-ba8847e01f7c&&collection=contentId-668081349&height=967&occurrenceKey=null&width=1726&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
If user deletes the Active Directory before deleting the directory users from the directory, then in that case they will see some ghost entries of those users in the Members page.

Ideally before deleting directory integration, it is expected that user will first delete them from directory.

## 1.2 Adding remote user

### 1.2.1 Open Members

After directory service integration is complete and status of service is enabled, admin can add Active Directory users to the console and assign roles to them.

Go to **Identity & Access Management** > **Members**.

### 1.2.2 Add a Remote User

1. Click **Add member**.
2. Select **Remote user**.
3. Enter username or email.
4. Assign role.
5. Click Add.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=0c8af5f4aa94&id=9dd59b36-261a-4fd4-87bc-693a9703a5e9&&collection=contentId-668081349&height=1117&occurrenceKey=null&width=1728&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
If directory service is not configured, Remote user option is disabled and points user to Directory Services.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=d7aaf3666607&id=b6e2c2bd-48f7-41eb-9f7c-fe53142b168b&&collection=contentId-668081349&height=972&occurrenceKey=null&width=1728&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
Once directory user is added, then we will show them in Members page as user type labelled as Remote.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=5087df029cf2&id=c73222da-962f-4e36-ac71-fb2f4c91c1cb&&collection=contentId-668081349&height=966&occurrenceKey=null&width=1727&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
## 1.3 Remote User Login Workflow

Once remote users are assigned roles and added to console, they can login and use console. They can login either via username or password.

### 1.3.1 Login with email/username and password

1. Enter email or username.
2. Enter password.
3. Click **Login**.

## 1.4 Troubleshooting

### 1.4.1 Directory Services workflow

| **Serial Number** | **Error Scenario** | **Likely Cause** | **Troubleshooting Steps** |
| --- | --- | --- | --- |
| 1 | Connection test fails | Incorrect host, port. For LDAPS, if SSL is not enabled in the AD server. | Verify the host name or IP address, port number. For LDAPS, confirm the certificate is valid and not expired. Allow approximately 2 minutes for the certificate update and Keycloak restart to complete. Retry the connection test after the restart. |
| 2 | Authentication fails | The Bind DN or bind credentials are incorrect, or the bind account does not have the required directory read access. |  Confirm the bind credentials  is correct. Correct the credentials and click **Authenticate** again. |
| 3 | Cannot delete AD federation | The AD integation is currently in an enabled state. Enabled federation cannot be deleted directly. | Open the federation row actions and select **Disable**. Confirm the disable action and wait for the status to change to **Disabled**. Open the row actions again and select **Delete**. |
| 4 | Disable/enable affects remote user role assignment | When  federation is disabled, role assignment for remote users from the Members page fails. | Navigate to **Directory Services** and verify the federation status Enable the federation before attempting to assign roles to remote users. Retry the role assignment from the Members page. |
| 5 | Ghost entries appear in Members page after deleting directory integration | The Active Directory integration was deleted before removing the associated remote users from the Members page. | Before deleting the directory integration, first remove all remote directory users from the Members page. If ghost entries already exist, delete the directory integration and then manually remove the ghost user entries from the Members page. |
| 6 | User add throws 5xx error | If AD federation is configured in NCL and LDAP connection with default timeout 1000ms is timing out. | If AD federation is configured in NCL and during user add if we are hitting an intermittent error with status code 5xx then we can reconfigure federation by increasing the LDAP connection timeout, which is in milliseconds, to 10000ms+ and verify whether the intermittent issue is resolved. |

### 1.4.2 Members Page workflow

| **Serial Number** | **Error Scenario** | **Likely Cause** | **Troubleshooting Steps** |
| --- | --- | --- | --- |
| 1 | Remote user option is disabled when adding a member | No directory service has been configured and enabled. The **Remote user** radio button is disabled until a valid directory service is active. | Navigate to **Settings > Identity & Access Management > Directory Services**. Complete the Directory Services workflow: Server Connection, Binding Credentials, and User DN Configuration. Ensure the service status is **Enabled**. Return to the Members page and retry adding a remote user. |
| 2 | Cannot add remote user | The Users DN is incorrect, the user does not exist in the specified directory container, or the Members page data is stale. |  Verify that the Users DN configured in Directory Services points to the correct container where the user resides. Confirm the user exists in that directory container via the Active Directory management tool. Refresh the Members page and retry the search. |

### 1.4.3 Remote User Login workflow

| **Serial Number** | **Error Scenario** | **Likely Cause** | **Troubleshooting Steps** |
| --- | --- | --- | --- |
| 1 | Cannot log in with username or email | The remote user has not been added from the Members page, or no roles have been assigned to the user. | Navigate to **Identity & Access Management > Members** and confirm the user is listed. If the user is not listed, add them as a remote user. Verify that at least one role has been assigned to the user. Ask the user to retry login after the role is assigned. |
| 2 | Password or authentication issues during login | The user's directory password is incorrect, or the federation is disabled or unreachable. | Ask the user to verify their Active Directory password by attempting to authenticate against another directory-integrated application. Navigate to **Directory Services** and confirm the service is **Enabled**. Run a connection test to verify the directory service is reachable. If the service is unreachable, check the host, port, and SSL/TLS settings and retry. |

# 2 MFA

## 2.1 Overview

Multi-factor authentication (MFA) in NCL adds a second verification step on top of the user's password. In the current NCL implementation, MFA is based on TOTP codes from an authenticator application, with recovery codes available as a fallback when the authenticator app is not accessible.

This document describes the NCL-specific MFA experience for local users who sign in through the NCL login page with email or username and password. It also covers the user-settings flow, recovery-code handling, sign-in behavior, and the available admin actions.

**Note:** This MFA capability is valid only for local users and not for directory users. If a directory user is logged in, MFA-related options are not shown in User Settings or Identity & Access Management. It also covers the user-settings flow, recovery-code handling, sign-in behavior, and the available admin actions.

## 2.2 Scope

This TOI is for the NCL MFA flow implemented in the UI under:

* User Settings > MFA
* NCL sign-in MFA challenge flow
* Identity & Access Management user-level MFA management

## 2.3 Supported MFA capability in NCL

NCL currently supports:

* TOTP-based MFA using an authenticator application
* Recovery-code based fallback sign-in
* User self-service MFA configuration
* User self-service recovery code generation
* User self-service MFA deletion
* Admin-driven MFA removal and disable actions from IAM

## 2.4 Key differences from SaaS MFA

Compared to the existing SaaS MFA flow, NCL behaves differently in a few important ways:

| Area | SaaS | NCL |
| --- | --- | --- |
| Enroll flow backend | Auth0-centered enroll/verify flow | NCL identity backend creates and registers the TOTP device |
| Recovery codes | Single recovery code shown/downloaded in the common flow | NCL uses a list of recovery codes and downloads them as a set |
| Recovery-code sign-in UX | Separate post-password choice flow | NCL shows OTP first, with an inline switch to recovery code |

## 2.5 User Workflows and Screenshots

### 2.5.1 Configure MFA

A logged-in NCL user can configure MFA from the User Settings panel.

1. Open the user menu.
2. Open User Settings.
3. Locate the MFA section.
4. If MFA is not configured, the status shows as `Not configured`.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=5a126d1d547e&id=fb285d23-c6b8-4610-8372-541736e8f1b6&&collection=contentId-668081349&height=964&occurrenceKey=null&width=1723&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
‌

5. Click `Configure`.

The configuration modal walks the user through four steps.

#### 2.5.1.1 Step 1: Re-authenticate with password

The user must confirm the current account password before enrollment begins.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=ed653c892698&id=3f6a0f0b-5051-49b9-b2db-8f106474d605&&collection=contentId-668081349&height=963&occurrenceKey=null&width=1718&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
‌

Behavior in NCL:

* The password is submitted with a password grant request.
* If the password is invalid, the flow remains on step 1 and an error is shown.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=c86b61c8bd14&id=c9ebaf6f-4fd3-44e4-9770-c53e8e323d0b&&collection=contentId-668081349&height=963&occurrenceKey=null&width=1716&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
‌

* After successful authentication, NCL requests MFA device creation from the identity backend.

‌

#### 2.5.1.2 Step 2: Install and link authenticator app

The user must use a TOTP-compatible authenticator app such as Microsoft Authenticator or Google Authenticator.

NCL supports two linking options:

* Scan QR code

![](blob:https://media.staging.atl-paas.net/?type=file&localId=56e8ccd65242&id=d39ab6a1-e42b-44ea-922c-183d405f9b9c&&collection=contentId-668081349&height=968&occurrenceKey=null&width=1717&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
‌

* Type a secret key manually

![](blob:https://media.staging.atl-paas.net/?type=file&localId=672f5067c27e&id=d0a37599-f49b-4174-86b6-23753f7e4758&&collection=contentId-668081349&height=962&occurrenceKey=null&width=1716&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
‌

Behavior in NCL:

* The user can switch between QR-code view and secret-key view before continuing.
* User can use any authenticator app for the MFA.

#### 2.5.1.3 Step 3: Verify the authenticator code

After linking the authenticator app, the user enters the one-time code generated by the app.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=f222e1b6d2b7&id=0e704610-1706-4a9e-897c-442b7113091f&&collection=contentId-668081349&height=961&occurrenceKey=null&width=1715&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
‌

Behavior in NCL:

* The entered OTP is sent to the NCL identity backend by calling the MFA register endpoint.
* Registration payload includes the encoded secret, a device name, overwrite flag, and the initial OTP code.
* If verification succeeds, the flow moves to the final step.
* If verification fails, the user stays on the same step and sees the backend error.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=7194403aa583&id=f27172b8-dc21-4a6f-8723-1662f8379300&&collection=contentId-668081349&height=961&occurrenceKey=null&width=1728&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
**Note:** If the error "Failed to verify the application code." appears, verify the following:

1. The user entered the correct TOTP.
2. The NCL server time is synchronized with UTC through NTP.

TOTP verification typically allows an acceptable time drift of ±30 seconds. If the server time is out of sync beyond that range, even a valid TOTP can fail with the same error. Synchronize the NTP server and try again.

Sample Error Screenshot:

![](blob:https://media.staging.atl-paas.net/?type=file&localId=d6243e1f66b1&id=17deb069-142d-4bdb-9401-5d741eae2e2f&&collection=contentId-668081349&height=812&occurrenceKey=null&width=1496&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
#### 2.5.1.4 Step 4: Recovery codes and completion

When setup completes, the user is shown recovery codes.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=6e5f3901cf52&id=0eba7746-27fc-4888-bbf0-cd9e1fbdde5b&&collection=contentId-668081349&height=969&occurrenceKey=null&width=1718&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
‌

NCL-specific behavior:

* NCL works with a set of 12 recovery codes instead of presenting only a single code.
* Recovery codes are intended to be used in sequence. If first recovery code is used during login then next time second recovery code needs to be used. User can’t use recovery code in any sequence he wishes.
* The user must confirm that the codes have been saved before the modal can be closed.
* Recovery codes can be downloaded from the UI.

Expected user action:

* Download the recovery codes.
* Store them securely.
* Confirm that they have been saved.

After completion, the MFA status in User Settings changes to `Configured`.

- [ ] 

![](blob:https://media.staging.atl-paas.net/?type=file&localId=2a593e8615a6&id=b6dc7a42-42b6-4902-9871-4b22d648e9db&&collection=contentId-668081349&height=965&occurrenceKey=null&width=1722&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
‌

‌

### 2.5.2 Login flow after MFA is configured

Once MFA is configured, the next NCL login requires a second factor after the user enters email or username and password.

Behavior in NCL:

1. The user submits email or username and password from the NCL sign-in page.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=acc0c162b547&id=9e977fa8-ba9c-407a-8a89-8cd7f8db7685&&collection=contentId-668081349&height=962&occurrenceKey=null&width=1720&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
‌

2. If MFA is required, the backend returns the `mfa_required` error.
3. The sign-in experience switches to the MFA pane.
4. By default, the user is prompted to enter the code from the authenticator application.

#### 2.5.2.1 Authenticator-app sign-in

Default MFA sign-in path:

* User enters the TOTP code from the authenticator app.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=a49788a97e09&id=75b9a02f-d18f-40c2-acd0-eb6e41e1d59a&&collection=contentId-668081349&height=958&occurrenceKey=null&width=1723&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
* The login request is retried with the `totp` field included.
* On success, the user is signed in.

#### 2.5.2.2 Recovery-code sign-in

If the user does not have access to the authenticator app, NCL provides an inline `Use recovery code` option.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=995ad62c886a&id=c5de319f-38c9-482d-b8ad-5b5ffad4c7b7&&collection=contentId-668081349&height=963&occurrenceKey=null&width=1717&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
Behavior in NCL:

* The UI fetches recovery-code status before the user enters a recovery code.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=4514a4d4e249&id=b5e28ea8-80cf-4f72-81ee-096439f41104&&collection=contentId-668081349&height=1117&occurrenceKey=null&width=1728&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
‌

* The UI displays the next recovery-code number when available.
* If recovery-code status cannot be fetched, sign-in with recovery code is blocked and an error is shown. In this case user needs to sign using authenticator app.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=b94943395bbb&id=3e3f8f92-3960-4f07-9e44-6e10b1d8f2c6&&collection=contentId-668081349&height=1001&occurrenceKey=null&width=1722&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
‌

* If all recovery codes are exhausted, the login button is disabled and the UI instructs the user to contact an administrator.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=01184af23358&id=e2b8070a-e1a8-4618-87ef-cbeb8bbe1a9b&&collection=contentId-668081349&height=997&occurrenceKey=null&width=1714&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
### 2.5.3 Generate recovery codes

A logged-in user can generate a new set of recovery codes from the MFA action menu in User Settings.

Steps:

1. Open the user menu.
2. Open User Settings.
3. In the MFA section, open the action menu.
4. Click `Generate recovery code`.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=165b28a155ee&id=3a42a60c-136c-4dd6-a477-da3dd932ea9a&&collection=contentId-668081349&height=963&occurrenceKey=null&width=1719&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
5. Download the returned recovery-code set.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=11a8ae54f798&id=3cef06f9-7be7-4794-b2c5-d08dc52f2a68&&collection=contentId-668081349&height=963&occurrenceKey=null&width=1714&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
‌

6. Confirm that the codes have been saved. Once new set of recovery codes are downloaded, old ones automatically becomes invalid.

NCL-specific behavior:

* The recovery-code response contains a list of recovery codes.
* The UI renders the codes in a tabular/list format.
* The user downloads them as a CSV file.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=bb4db49554e9&id=fc5943d7-dd9e-45f6-888b-a19f4b2c482d&&collection=contentId-668081349&height=1117&occurrenceKey=null&width=1463&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
‌

### 2.5.4 Delete MFA

A logged-in user can remove MFA from the same MFA action menu.

Steps:

1. Open the user menu.
2. Open User Settings.
3. In the MFA section, open the action menu.
4. Click `Delete`.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=926e6eaaf07f&id=887e474a-26ae-4bdc-bc35-470ae7ad4f6d&&collection=contentId-668081349&height=965&occurrenceKey=null&width=1720&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
‌

5. Confirm the deletion.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=b37dbc8946ae&id=1a252e90-feb5-462d-9195-465ba7286fd5&&collection=contentId-668081349&height=961&occurrenceKey=null&width=1718&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
‌

NCL-specific behavior:

* After successful deletion, the MFA status returns to `Not configured`.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=e10165c314cb&id=a63283fd-162e-442f-9971-d0101c9da064&&collection=contentId-668081349&height=958&occurrenceKey=null&width=1717&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
## 2.6 Admin Workflows

### 2.6.1 Manage MFA for another user

NCL also exposes admin-level MFA actions from Identity & Access Management.

High-level flow:

1. Open Identity & Access Management.
2. Open the Members view.
3. Locate the user.
4. Open the user action menu.
5. Select `Manage multi-factor authentication`.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=2fc7a824138a&id=0d56f171-7573-49a7-a04d-5dd5b5ed4f6f&&collection=contentId-668081349&height=969&occurrenceKey=null&width=1714&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
From the modal, the admin can perform:

* Remove MFA
* Disable MFA

### 2.6.2 Remove MFA

If the target user has MFA configured, the admin can remove it. This clears the user's configured MFA and allows sign-in without the second factor until MFA is configured again.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=cd84a865caed&id=582a11af-310f-49b5-97f4-9c49416a73c1&&collection=contentId-668081349&height=959&occurrenceKey=null&width=1725&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
### 2.6.3 Disable MFA

If the target user has MFA configured and enabled, the admin can disable MFA for 8 hours using the disable endpoint exposed by the identity backend.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=b7a64fcc58e1&id=0af81991-08ee-470c-8c7b-3529ef5022e1&&collection=contentId-668081349&height=962&occurrenceKey=null&width=1725&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
Note:

* The NCL UI supports both `Remove` and `Disable` in the manage-MFA dialog.
* If the user's MFA is not configured, the dialog shows the not-configured message instead of management options.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=6c39eccb805a&id=1cc52665-9a11-4feb-b9f5-ae8d13d3c15f&&collection=contentId-668081349&height=963&occurrenceKey=null&width=1728&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
## 2.7 Troubleshooting

| **Serial number** | **Error scenario** | **Likely cause** | **Troubleshooting steps** |
| --- | --- | --- | --- |
| 1 | Wrong password during MFA setup | The user entered an incorrect current account password during the re-authentication step. | Confirm that the user is entering the current local account password. Retry the password-confirmation step. If the password is forgotten, use the password reset flow before configuring MFA again. |
| 2 | User is not seeing MFA-related options | The logged-in account is not eligible for NCL MFA. MFA options are available only for local users; directory users do not see MFA settings or MFA actions. | Verify whether the logged-in account is a local user or directory user. If the account is a directory user, this is expected behavior; MFA-related options are not shown. If the account is a local user and options are still missing, refresh the page and verify the user details in Identity & Access Management. |
| 3 | OTP verification fails during setup | The authenticator app code is expired, mistyped, or not synchronized with the MFA secret registered during setup. | Wait for the authenticator app to rotate to a fresh code. Enter the latest code exactly as shown in the authenticator app. If the issue persists, restart MFA setup and re-scan the QR code or re-enter the secret key. |
| 4 | Failed to verify the application code. | The user entered an incorrect TOTP, or the NCL server time is not synchronized with UTC through NTP. TOTP verification typically allows an acceptable time drift of ±30 seconds. If the server time is out of sync beyond that range, even a valid TOTP can fail with the same error. | Verify that the user entered the correct TOTP from their authenticator app. Confirm that the NCL server time is synchronized with UTC through NTP. If the server time is out of sync beyond the ±30-second acceptable drift range, synchronize the NTP server and try again. |
| 5 | Recovery codes exhausted | All available recovery codes for the user have already been consumed. | Ask the user to sign in with the authenticator app if available. If the user is already signed in elsewhere, generate a new set of recovery codes from User Settings > MFA. If the user cannot sign in, contact an administrator to remove or temporarily disable MFA for the account. |
| 6 | Lost authenticator access | The authenticator device is unavailable, replaced, or no longer has the registered TOTP account. | If recovery codes are still available, use the recovery-code sign-in path. After signing in, generate new recovery codes and reconfigure MFA if required. If both authenticator access and recovery codes are unavailable, ask an administrator to remove or disable MFA for the account. |

# 3. SMTP Configuration and Local user

This section gives admins and operators a practical guide to adding local users from the Members page. It covers the prerequisite SMTP configuration required to deliver the invitation email, the member-addition workflow, and the end-to-end steps a new local user follows to set their password and sign in to the NCL console.

**Prerequisite:** SMTP must be configured before adding a local user. The invitation email that allows the user to set their password is delivered via the configured SMTP server. If SMTP is not set up, the email will not be delivered and the user will not be able to activate their account.

## 3.1 Add a Local User from the Members Page

An admin can invite a new local user from the **Identity & Access Management > Members** page. Before proceeding, ensure SMTP is configured (see section 1.2.2 below).

### 3.1.1 Open Members

Go to **Settings > Identity & Access Management > Members**.

### 3.1.2 Configure SMTP

Before inviting a local user, a valid SMTP server must be configured. If SMTP is already set up, skip to section 1.2.3.

1. Click **Add User**. If SMTP is not configured, the **Local user** radio button will be disabled and a prompt to configure the email server will appear.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=c6912b9a09c8&id=5d99782b-0088-458c-bbc1-25ea9c81214d&&collection=contentId-668081349&height=1117&occurrenceKey=null&width=1728&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
2. Click **Configure an email server** in the prompt. This redirects to the Email Server configuration page.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=5e1e6c2e63c8&id=1ccd9a42-a20a-483e-ab9d-e3d27f750643&&collection=contentId-668081349&height=1117&occurrenceKey=null&width=1728&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
3. Enter the SMTP server host, port, sender name, and sender email address.
4. Provide the SMTP username, password, and self-signed certificate (if applicable).
5. Click **Test Connection** to validate the settings. The **Save** button is enabled only after a successful test.
6. Click **Save** to apply the configuration.

### 3.1.3 Invite a Local User

Once SMTP is configured, return to **Settings > Identity & Access Management > Members** and complete the following steps to invite a local user.

1. Click **Add User**.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=1aff9d3bc4cc&id=e57e24bd-4ace-4c1a-892d-73d46bba8184&&collection=contentId-668081349&height=1117&occurrenceKey=null&width=1728&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
2. Select **Local user** as the user type.
3. Enter the user's **full name**, **email address**, and **role**.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=f6a3626f7d5a&id=f8808966-35c8-4e3e-b16d-c32cc7f4eb87&&collection=contentId-668081349&height=1117&occurrenceKey=null&width=1728&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
4. Click **Add**.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=4064a4bb8446&id=3c1c51ac-eed1-4bdc-8506-5319cfbfb53d&&collection=contentId-668081349&height=1117&occurrenceKey=null&width=1728&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
**Behavior:**

* The system sends an invitation email to the provided address via the configured SMTP server.
* If SMTP is not configured, the **Local user** radio button will be disabled.

## 3.2 New User — Set Password Workflow

After the admin adds the local user, the invited user receives an email and follows the steps below to activate their account.

### 3.2.1 Receive the Invitation Email

The user receives an email at the registered address. The email contains a secure link to set the account password.

‌

![](blob:https://media.staging.atl-paas.net/?type=file&localId=c494b36f6429&id=2dbafe09-3693-496b-80f8-fa3d8d715f5e&&collection=contentId-668081349&height=828&occurrenceKey=null&width=880&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
‌

### 3.2.2 Set the Password

1. Click the **Access NetApp Console** link in the invitation email. The link opens a **Set New Password** page in the browser.
2. Enter a new password in the **Password** field.
3. Re-enter the same password in the **Confirm Password** field. Passwords must match and meet the complexity requirements.
4. Click **Submit**.

![](blob:https://media.staging.atl-paas.net/?type=file&localId=9d3c83506c11&id=950bf5a5-3508-4239-8a5c-2ed6993c25b6&&collection=contentId-668081349&height=1117&occurrenceKey=null&width=1728&__contextId=null&__displayType=null&__external=false&__fileMimeType=null&__fileName=null&__fileSize=null&__mediaTraceId=null&url=null)
‌

**Behavior:**

* The invitation link is single-use and expires after **7 days**. If the link has expired, the user can initiate the Forgot Password flow to regain access.
* The password must meet the system's complexity requirements (minimum length, character types).
* On success, the user is automatically redirected to the Sign In page.

### 3.2.3 Sign In and Access the Console

1. On the Sign In page, enter the registered email address and the newly set password.
2. Click **Log In**.
3. On successful authentication, the user lands on the NCL home dashboard where they can manage storage and access all features available for their assigned role.

**Behavior:**

* If credentials are incorrect, an inline error is displayed.
* If the user forgets the newly set password, they can use the **Forgot Password?** link on the Sign In page — this also requires SMTP to be configured.
* Local users authenticate using email and password only. They do not use directory-based (LDAP/AD) credentials.

‌

### **3.2.4 Troubleshooting** Local User workflow

* **Local user option is disabled when adding a member:** SMTP is not configured. The **Local user** radio button is disabled until a valid SMTP server is set up. Go to **Administration > Console > Email Server**, complete the SMTP configuration, run **Test Connection**, and save before retrying.
* **Invitation email not received:** Check the spam or junk folder — the invitation email may have been filtered by the recipient's mail client. Also verify that the correct email address was entered when adding the user in the Members page.
* **Invitation email still not received after checking junk:** The configured SMTP settings may be incorrect. Go to **Administration > Console > Email Server > Edit**, review the host, port, sender credentials, and SSL/TLS settings, then click **Test Connection** to confirm the configuration is valid before saving.
* **Invitation link is expired or already used:** The invitation link is single-use and expires after **7 days**. If the link has expired, the admin must remove the user from the Members page and add them again to trigger a new invitation email.

