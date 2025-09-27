# Enrollment Flow
Register a new user for an Authentik application

- Works with Authentik version 2025.8.3
- Replace _brand_ in these instructions with a unique string for each brand you create

## Entry point
This is the URL / Button on your website that starts the enrollment process
```
   example: https://auth.domain.com/if/flow/brand-enrollment/?next=/
```

## Flows and Stages > Flows
Create the _brand enrollment_ flow

### brand-enrollment-flow

<ins>Edit</ins>
* **Name** : Register
* **Title** : Register for Brand
* **Slug** : brand-enrollment-flow (obviously, name it how it works for you)
* **Designation** : Enrollment
* **Authentication** : No Requirement
* **Behavior Settings & Appearance Settings** : to your liking

<ins>Stage Bindings</ins>
* 10 brand-source-enrollment-prompt  
* 20 brand-source-enrollment-write  
* 30 brand-email-account-confirmation

## Flows and Stages > Stages
Create or modify each stage for the _brand-enrollment_ Flow


1. ### brand-source-enrollment-prompt
* **Name** : brand-source-enrollment-prompt
* **Type** : Prompt Stage
* **Stage-specific settings** : _(Choose the fields that best fit your needs -- My choices below)_ 
  * default-password-change-field-password
  * default-password-change-field-password-repeat
  * first-name ("attributes.givenName", of type text)
  * initial-setup-field-email ("email", of type email)
  * last-name ("attributes.familyName", of type text)
  * username ("username", of type hidden)
* **Validation Policies** _(You may want to add more validations -- My choices below)_
  * set username to brand-Email (Expression Policy) _defined below_

2. ### brand-source-enrollment-write
* **Name** : brand-source-enrollment-write
* **Type** : User Write Stage
* **Stage-specific-settings**
  * **User path template** : brand
  * **Group** : Brand Group Name _(Optional)_

3. ### brand-email-account-confirmation
* **Name** : brand-email-account-confirmation
* **Type** : Email Stage
* **Subject** : [BRAND STRING] Account Confirmation 
  * _Note: the brackets around 'BRAND STRING' are relevant for mimedefang-filter_
* **Template** : Choose default template, or create your own
  * Templates are stored _custom\_templates_ folder of your docker instance

## Customization > Policies

### brand-email
Authentik requires usernames, but if you don't want your users create their own username, you can create the username based on the email. Since you're working with brands, you should separate the email address by brand in case someone wants to register for more than one of your brands.  Mitigates co-mingling.

* **Name** : set Username to brand-Email
* **Expression** _replace brand in expression with your unique string_
```
   if regex_match(context['prompt_data'].get('email'), '^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'):
     context['prompt_data']['username'] = "brand/" + context['prompt_data'].get('email')
     context['prompt_data']['name'] = context['prompt_data']['attributes']['givenName'] + " " + context['prompt_data']['attributes']['familyName']
     return True
   else:
     return False
```
