# Enrollment Flow
Register as a new user for an Authentik OAuth2 application

Entry point is URL / button from your website 
e.g. https://auth.domain.com/if/flow/brand-enrollment/?next=/

########################################################################
#  - Flows (under Flows and Stages) -

Create Entry Point 

brand-enrollment
----------------
Name: Register (or whatever you deem appropriate)
Title: Register for Brand
Slug: brand-enrollment (obviously, name it how it works for you)
Designation: Enrollment
Authentication: No Requirement
Behavior & Appearance to your liking

########################################################################
#  - Stage Bindings for brand-enrollment -

10 brand-source-enrollment-prompt
20 brand-source-enrollment-write
30 brand-email-account-confirmation

  - Stages (under Flows and Stages) -

brand-source-enrollment-prompt
------------------------------
Name: brand-source-enrollment-prompt

Stage-specific settings
(Choose what fits your need here -- My choices below)
* default-password-change-field-password
* default-password-change-field-password-repeat
* first-name ("attributes.givenName", of type text)
* initial-setup-field-email ("email", of type email)
* last-name ("attributes.familyName", of type text)
* username ("username", of type hidden)

Validation Policies
(You may want to add more validations -- My choices below)
set username to brand-Email (Expression Policy) <-- See below

brand-source-enrollment-write
------------------------------
Name: brand-source-enrollment-write
Stage-specific-settings
  User path template: brand
(Optional) Group: Brand Group Name

brand-email-account-confirmation
--------------------------------
Name: brand-email-account-confirmation
Subject: [BRAND STRING] Account Confirmation 
(Note: see the brackets are relevant for mimedefang-filter)
Template: Choose default template, or create your own here (it's a dropdown)
(Note: Templates are stored in docker "media" folder under customer_templates)

########################################################################
# - Policies (under Customization) -

brand-Email
-----------
Name: set Username to brand-Email
(Needed so your brand Usernames are unique while not requiring username entry)
Expression:
if regex_match(context['prompt_data'].get('email'), '^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'):
  context['prompt_data']['username'] = "brand/" + context['prompt_data'].get('email')
  context['prompt_data']['name'] = context['prompt_data']['attributes']['givenName'] + " " + context['prompt_data']['attributes']['familyName']
  return True
else:
  return False
-- END (do not include this line) --

-- Enrollment --
