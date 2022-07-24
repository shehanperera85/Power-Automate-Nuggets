**Goal of this automation task**

Microsoft very cleverly introduced the **group based licensing** feature some time ago that helps the IT admins to assign licenses to Azure AD groups and the users who are in them will get those licenses assigned. Pretty neat way to standardize the license assignment workflows and helps to manage users in a better way.

The problem however is the notification. Well I’m still thinking Microsoft could have added a “notify” section with an option to enter an email address, but that’s not the case here.

Systems Admin or the 1st level support admin added the user to the AD group that is synced with Azure AD which has the Group Based Licensing feature is using. OR admin adds the user to the cloud group.
User complains they still can’t use the desired service that the license should be able to provision.

I will be using MS Graph and Power Automate for this scenario and will be using Microsoft Teams Flow Bot to notify the admin.

Spoiler Alert. Your end result will look like this.

![image](https://user-images.githubusercontent.com/98259062/180668973-19b799bc-3f42-4dc5-a611-29b26f1184bc.png)

## What do you need?

### [MS Graph API Permissions](https://github.com/shehanperera85/Power-Automate-Nuggets/blob/main/1.%20Setting%20up%20MS%20Graph%20Permissions.md)

### Permissions Required
