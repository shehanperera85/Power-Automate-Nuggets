# AzureAD Group Based Licensing Notifications

**Goal of this automation task**

Microsoft very cleverly introduced the **group based licensing** feature some time ago that helps the IT admins to assign licenses to Azure AD groups and the users who are in them will get those licenses assigned. Pretty neat way to standardize the license assignment workflows and helps to manage users in a better way.

The problem however is the notification. Well I’m still thinking Microsoft could have added a “notify” section with an option to enter an email address, but that’s not the case here.

Systems Admin or the 1st level support admin added the user to the AD group that is synced with Azure AD which has the Group Based Licensing feature is using. OR admin adds the user to the cloud group.
User complains they still can’t use the desired service that the license should be able to provision.

I will be using MS Graph and Power Automate for this scenario and will be using Microsoft Teams Flow Bot to notify the admin.

Spoiler Alert. Your end result will look like this.

![image](https://user-images.githubusercontent.com/98259062/180668973-19b799bc-3f42-4dc5-a611-29b26f1184bc.png)

## What do you need?

### [Registering an app](https://github.com/shehanperera85/Power-Automate-Nuggets/blob/main/1.%20Setting%20up%20MS%20Graph%20Permissions.md)

### Permissions Required
Group.Read.All

### Setup Power Automate

Lets go to Power Automate now. We will be creating a simple flow and connect Teams into it to get the results. This is my flow at a glance.
![image](https://user-images.githubusercontent.com/98259062/180682465-622fea19-4ac1-49a4-aa2e-d0bed8d45afb.png)

Start the flow with a trigger. I gave a recurrence. Maybe 3 times a day as a start?

![image](https://user-images.githubusercontent.com/98259062/180682496-aac1e4a2-216c-48f1-b9ed-533a2c40cead.png)

Then I move to my 1st action. That is to run the GET request to find out the groups with issues

![image](https://user-images.githubusercontent.com/98259062/180682525-32285adf-b047-423e-8fcd-417e3985f868.png)

Click on **Show advanced options feature** and set the parameters we noted from the Azure AD app registration part

![image](https://user-images.githubusercontent.com/98259062/180682582-050f7c78-4088-4b1f-9497-30d499c826c5.png)

Now, the result that the GET request gives is in the JSON format. We need to find a way to get the information we only require for the next steps. For this, I will be using the JSON Parse action which will cleanup and gives actionable parameters. To get the Generate from sample, go to Graph Explorer, run the same request and get the results and paste it here so it will create the Schema for you.

As an example, I got the below reply from the Graph Explorer.

![image](https://user-images.githubusercontent.com/98259062/180682618-28b09776-efa2-4ce4-9490-1d941c759028.png)


![image](https://user-images.githubusercontent.com/98259062/180682632-e65a75df-33f2-489a-b81a-228714d83505.png)

Now that we have actionable parameters parsed, let’s take the displayName parameter from top and create the next section. This action is basically allowing Teams to Post message in a chat or channel.

![image](https://user-images.githubusercontent.com/98259062/180682651-9e87a0ba-bfac-4213-bdd0-d490f75a517e.png)

The Teams section would look like below. I’m using the Flow Bot to post messages without using another user account. I’m just posting a simple message. You can add instructions to check the next steps if you need.

![image](https://user-images.githubusercontent.com/98259062/180682675-af8da8fe-302b-42fe-a95f-cfd37b93471a.png)

## Testing the Solution

You can run the Flow to see if there are any errors and fix them 1st.

If you have a subscription you can start testing the behavior. If you don’t have any issues, there will be no messages. This will only spit out the messages if there are issues in the groups.

![image](https://user-images.githubusercontent.com/98259062/180682720-71fefaa9-1cd7-4840-b34c-373758296d55.png)

If you have issues in more than 1 group. The results will be as below

![image](https://user-images.githubusercontent.com/98259062/180682742-e23cf970-03e3-4a11-b665-61bf8e45762e.png)

## Check the Group for Issues

So now that you have been notified. Next thing is to go and fix the issues.

For this, go to the Azure AD group > Licenses blade and you will see as below.
![image](https://user-images.githubusercontent.com/98259062/180682801-71e70e87-57b0-4977-a0a7-c60cac9e381b.png)


Click on the Warning and it will take you to the users with errors. It also shows the reason for failure.
![image](https://user-images.githubusercontent.com/98259062/180682812-54ed3ad0-eb12-421b-bd64-33bfb899a4c4.png)


To find out which License SKU is in error, click on the user and it will take you to user’s license blade and will show you which licenses has issues.
![image](https://user-images.githubusercontent.com/98259062/180682830-d7d90604-8df0-4590-8e8b-09638371729e.png)


You can start troubleshooting the license issues now starting with adjusting your license seats, removing licenses from users who are not using the services anymore etc.



