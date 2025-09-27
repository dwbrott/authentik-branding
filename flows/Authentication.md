# Authentication Flow
Login via an Authentik application

- Works with Authentik version 2025.8.3
- Replace _brand_ in these instructions with a unique string for each brand you create

## Flows and Stages > Flows

### brand-authentication-flow

<ins>Edit</ins>
* **Name** : Login
* **Title** : Welcome to Brand
* **Slug** : brand-authentication-flow
* **Designation** : Authentication
* **Authentication** : No Requirement
* **Behavior Settings & Appearance Settings** : to your liking

<ins>Stage Bindings</ins>
* 10 brand-authentication-identification
* 20 default-authentication-mfa-validation
* 30 default-authentication-login

## Flows and Stages > Stages
Create or modify each stage for the _brand-authentication-flow_ Flow


1. ### brand-authentication-identification
* **Name** : brand-authentication-identification
* **Type** : Identification Stage 
* **Stage-specific settings** : _(Choose the fields that best fit your needs -- My choices below)_ 
  * **User fields** : E-mail
  * **Password Stage** : default-authentication-password
  * **Captcha Stage** : (Optional)

2. ### default-authentication-mfa-validation
* **Name** : default-authentication-mfa-validation
* **Type** : Authenticator Validation Stage
* **Stage-specific-settings** : _I used the Authentik defaults here_
* **WebAuthn-specific settings** : _I used the Authentik defaults here_

3. ### default-authentication-login
* **Name** : default-authentication-login
* **Type** : User Login Stage
* **Stage-specific-settings** : _I used the Authentik defaults here_
