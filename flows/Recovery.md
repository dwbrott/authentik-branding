# Recovery Flow
Forgot password in an Authentik instance

- Works with Authentik version 2025.8.3
- Replace _brand_ in these instructions with a unique string for each brand you create

## Flows and Stages > Flows

### brand-recovery-flow

<ins>Edit</ins>
* **Name** : Forgot Password
* **Title** : Recover Password of Brand
* **Slug** : brand-recovery-flow
* **Designation** : Recovery
* **Authentication** : No Requirement
* **Behavior Settings & Appearance Settings** : to your liking

<ins>Stage Bindings</ins>
* 10 brand-recovery-authentication-identification
* 20 brand-recovery-email
* 30 default-password-change-prompt
* 40 default-password-change-write

## Flows and Stages > Stages
Create or modify each stage for the _brand-recovery-flow_ Flow


1. ### brand-recovery-authentication-identification
* **Name** : brand-recovery-authentication-identification
* **Type** : Identification Stage 
* **Stage-specific-settings** :
  * **User Fields** : Email
  * **Password stage** : _I left this blank_
  * **Captcha stage** : _I left this blank_
* **Source Settings** : _I used the Authentik defaults here_
* **Flow Settings** : _I used the Authentik defaults here_

2. ### brand-recovery-email
* **Name** : brand-recovery-email
* **Type** : Email Stage
* **Stage-specific-settings** :
  * **Activate pending user on success** : Enabled
  * **Use global settings** : Enabled
  * **Token expiration** : minutes=30
  * **Subject** : [BRAND STRING] Account Confirmation 
    * _Note: the brackets around 'BRAND STRING' are relevant for mimedefang-filter_
  * **Template** : Choose default template, or create your own
    * Templates are stored _custom\_templates_ folder of your docker instance
  * **Account Recovery Max Attempts** : 5
  * **Account Recovery Cache Timeout** : minutes=5

3. ### default-password-change-prompt
* **Name** : default-password-change-prompt
* **Type** : Prompt Stage
* **Stage-specific-settings** :
  * **Fields** : 
    * default-password-change-field-password
    * default-password-change-field-password-repeat
  * **Validation Policies** : _I left this blank_

4. ### default-password-change-write
* **Name** : default-password-change-write
* **Type** : User Write Stage
* **Stage-specific-settings** : _I used the Authentik defaults here_
