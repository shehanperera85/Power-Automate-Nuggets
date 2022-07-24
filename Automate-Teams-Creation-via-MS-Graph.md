# Automate Teams Creation With Templates

**Goal of this flow:** To be able to automate the Teams creation by ingesting the preferred template.

Often times the issue with the IT Admins is with the ever growing Teams popularity, how to beat the demand and how to create Teams and specially, how to template it out and automate it.
Well, Teams templates are now in the Teams Admin Center where you can see pre-defined templates and the ability to cerate custom templates if required.
With the templates, IT admins have to potentially provide the users the ability create Teams and then advice them to create a Team with a template. This will also give them the opportunity to create more Teams without the knowledge of the IT.
Microsoft have introduced Teams templates sometime ago so the Admins can use the pre-defined ones or custom ones according to the user requirement. The idea is to bring uniformity across the board.
Template can bundle up Channels, Tabs and Apps

When Powershell fails, MS Graph comes to rescue! Why I say this is when you run the new-team command with the -template parameter, it will throw an error if you are not an Education customers (namely if you don't have EDU_Class" or "EDU_PLC licenses)
Of course you can use powershell to run MS Graph, but what I'm showcasing is a way to automate Teams creation via MS Graph using Power Automate.

# Create Teams via Microsoft Graph


# What you need?
1. Register Power Automate as an Azure AD App and provide permissions to MS Graph
2. Pre Defined Teams Templates
3. Microsoft Forms
4. Excel Online Workbook
5. Power Automate

## 1. Register Power Automate as an Azure AD App(https://github.com/shehanperera85/Power-Automate-Nuggets/blob/main/1.%20Setting%20up%20MS%20Graph%20Permissions.md)

For later activities you need to perform in Power Automate using MS Graph, you ned to make sure Power Automate is registered as an app by providing the consent.


## 2. Pre Defined Teams Templates

You can use the pre-defined templates in the Teams Admin Center
Teams Admin Center **>** Teams **>** Teams Templates

When you open the preffered Template, note down the Templete ID
eg: com.microsoft.teams.template.ManageAProject

If you create a new Template according to your requirement (custom template), then the Template ID would look something like this
eg: a8643242-c5e5-444d-813b-b191hc3adb71


## 3. Create a simple Form using Microsoft Forms

You need the below 'required' questions in the Form
1. Team Name
2. Team Owner
3. Template Name (drop down)

A bit about the Template Name.
This Template name must corralate with the previously noted templates. Ideally if you are planning on using 5 templates, you need to know the Template names and the IDs.

<img width="583" alt="image" src="https://user-images.githubusercontent.com/98259062/153369440-c9a93455-4997-4580-8099-b1963ce25746.png">


## 4. Excel Online Workbook
Create a simple Excel Online workbook and add a Table.
Name your Table and the columns
Eg: Table Name: Table1
Columns: TemplateName, TemplateID

Save the file in a OneDrive or in a Docuemnt Library

## 4.Power Automate
This is the most interesting part. To create the Power Automate flow by using the above elements.

# How to Create the Power Automate Flow

In this section, I'm discussing the steps involved in creating the Flow.

## Step 1 - Create the trigger
This should be **When a new responce is submitted** and select the previously created MS Form [Check here](https://github.com/shehanperera85/Teams-via-MS-Graph-and-Power-Automate/blob/main/IntroAndPrereqs.md#3-create-a-simple-form-using-microsoft-forms)

<img width="687" alt="image" src="https://user-images.githubusercontent.com/98259062/153371830-b05ab360-3de6-4cf5-b322-718a7a57fca8.png">

## Step 2 - (Action) Get Response Details
<img width="687" alt="image" src="https://user-images.githubusercontent.com/98259062/153371198-97cb373d-6036-4b3f-9367-1deb335ea6ea.png">

## Step 3 - (Action) Search for the user
This correlates with the Qurstion 4 of the form (Team Owner's First and Last Name or Email (please enter the correct details)
The reason is the form is unable to provide user @ mentoins or resolves usernames/ UPNs.
With the **Search for users (V2)** action in place, when you provide a part of the name (given nameor surname) email address, UPN it will search for the user from Azure AD

<img width="683" alt="image" src="https://user-images.githubusercontent.com/98259062/153372847-a9c2d59d-a38a-4317-9f38-cc18c788e867.png">

As you can see in the screenshot, it Search Term should be **Team Owner's name** from the Form.

## Step 4 - (Action) Get a row
This part correlates with the drop-down question 3 in the form **Template Nname**
<img width="682" alt="image" src="https://user-images.githubusercontent.com/98259062/153373414-9fdb3cc3-d470-4bba-872e-1c65b95909e7.png">

As per the steps mentioned in [Excel Online Workbook](https://github.com/shehanperera85/Teams-via-MS-Graph-and-Power-Automate/blob/main/IntroAndPrereqs.md#4-excel-online-workbook) you now have the workbook with the required template IDs in it.
Provide the necessary feileds as mentiond above.

## Step 5 - (Action) Use HTTP to invoke REST API

As you can see in the below screenshot, for **Select an Output from the previous steps** and select **Value** from the **Dynamic Content** box.
<img width="687" alt="image" src="https://user-images.githubusercontent.com/98259062/153374061-c37a5bb4-45eb-479f-b4e2-cafa34bb5c31.png">

In the HTTP REST API function, the below information should be filled.

**Method**  : POST

**URI**     : https://graph.microsoft.com/beta/teams

**Headers** : Content-type    application/json

**Authentication**  : Active Directory OAuth

**Tenant**          : _Azure AD Tenant ID_

**Audiance**        : https://graph.microsoft.com

**Client ID**       : Client ID from [App Registration step](https://github.com/shehanperera85/Teams-via-MS-Graph-and-Power-Automate/blob/main/IntroAndPrereqs.md#1-register-power-automate-as-an-azure-ad-app)

**Credential Type** : Secret

**Secret**          : Secret Value from [Secret](https://github.com/shehanperera85/Teams-via-MS-Graph-and-Power-Automate/blob/main/IntroAndPrereqs.md#secret)

**Body**            :
In the below code you can see **[ID]**, **[TEAM NAME] **and **[USER ID]**. They are the Dynamic Content I picked from the box acordingly. (See screenshot)

<img width="343" alt="image" src="https://user-images.githubusercontent.com/98259062/153375702-4249d8a4-bb5c-4b06-aeaa-2e6df666f15a.png">

{
  "template@odata.bind": "https://graph.microsoft.com/beta/teamsTemplates('[ID]')",
  "displayName": "'[TEAM NAME]'",
  "description": "The team for those in architecture design",
  "members": [
    {
      "@odata.type": "#microsoft.graph.aadUserConversationMember",
      "roles": [
        "owner"
      ],
      "user@odata.bind": "https://graph.microsoft.com/beta/users('[USER ID]')"
    }
  ]
}

Once it's all done, you will be able to automate the Teams creation by ingesting the preferred template

