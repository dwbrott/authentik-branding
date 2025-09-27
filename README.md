# authentik-branding
A set of README providing hints on how to manage brands on your Authentik instance

- Works with Authentik version 2025.8.3
- Replace _brand_ in these instructions with a unique string for each brand you create

## Goal of this document
* Provide a basic cheat sheet for setting up one or more Authentik brands
* Customize look and feel of all elements, including e-mail messages
* _Note: This documentation is very basic and is not intended to provide every option available in Authentik_

## Assumptions
* Authentik instance and e-mail server are both available for customization

# Create your brand in Authentik
This section will show you how to create a new brand in Authentik.  You will need to first make sure you have created an Application that will be used to autheticate your users as your flows will link to that application.

## System > Brands

### Brand Settings
* **Domain** : Enter your domain name here
  * Leave Default unchecked if you're managing multiple brands
* **Brand Settings** : 
  * **Title** : Enter your brand title here
  * **Logo** : path to logo file
    * Images are stored in _media_ folder of your docker instance
  * **Favicon** : path to favicon file
  * **Default flow background** : Default background image for brand flows
  * **Custom CSS** : Optional custom CSS for your pages

### External user settings
* **Default application** : Choose application for your brand

### Default flows
* **Enrollment flow** : [brand-enrollment-flow](flows/Enrollment.md)
  * _This is not listed in the brand settings section but is an important piece of the puzzle_
* **Authentication flow** : [brand-authentication-flow](flows/Authentication.md)
* **Invalidation flow** : [brand-authentication-flow](flows/Invalidation.md)
* **Recovery flow** : [brand-recovery-flow](flows/Recovery.md)
* **Unenrollment flow** : _Not used in this example_
* **Device code flow** : _Not used in this example_

### Other global settings
* _No changes in this example_

# Mimedefang
Install / Configure [mimedefang](Mimedefang.md) to rewrite the sender address of your messages to match you brand.  This will make the experience more enjoyable for everyone.
