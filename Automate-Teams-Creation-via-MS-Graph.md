**Goal of this flow:** To be able to automate the Teams creation by ingesting the preferred template.

Often times the issue with the IT Admins is with the ever growing Teams popularity, how to beat the demand and how to create Teams and specially, how to template it out and automate it.
Well, Teams templates are now in the Teams Admin Center where you can see pre-defined templates and the ability to cerate custom templates if required.
With the templates, IT admins have to potentially provide the users the ability create Teams and then advice them to create a Team with a template. This will also give them the opportunity to create more Teams without the knowledge of the IT.
Microsoft have introduced Teams templates sometime ago so the Admins can use the pre-defined ones or custom ones according to the user requirement. The idea is to bring uniformity across the board.
Template can bundle up Channels, Tabs and Apps

When Powershell fails, MS Graph comes to rescue! Why I say this is when you run the new-team command with the -template parameter, it will throw an error if you are not an Education customers (namely if you don't have EDU_Class" or "EDU_PLC licenses)
Of course you can use powershell to run MS Graph, but what I'm showcasing is a way to automate Teams creation via MS Graph using Power Automate.
