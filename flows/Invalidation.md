# Invalidation Flow
Logout of an Authentik application

- Works with Authentik version 2025.8.3
- Replace _brand_ in these instructions with a unique string for each brand you create

## Flows and Stages > Flows

### brand-invalidation-flow

<ins>Edit</ins>
* **Name** : Logout
* **Title** : Log out of Brand
* **Slug** : brand-invalidation-flow
* **Designation** : Invalidation
* **Authentication** : No Requirement
* **Behavior Settings & Appearance Settings** : to your liking

<ins>Stage Bindings</ins>
* 10 default-invalidation-logout
* 20 brand-redirect-home

## Flows and Stages > Stages
Create or modify each stage for the _brand-invalidation-flow_ Flow


1. ### default-invalidation-logout
* **Name** : default-invalidation-logout
* **Type** : User Logout Stage 

2. ### brand-redirect-home
* **Name** : brand-redirect-home
* **Type** : Redirect Stage
* **Stage-specific-settings** :
  * **Mode** : Static
  * **Target URL** : https://www.example.com
