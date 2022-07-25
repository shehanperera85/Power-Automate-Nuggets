![image](https://user-images.githubusercontent.com/98259062/180681115-057cf0eb-0e2a-4185-b8cf-861544b15520.png)


# Send Reminders to Teams

**Goal of this flow:** My team mates have this practice of making our own Flat White coffees in the morning because that can save some money in the long run. Also re-usable coffee mug. So yeah why not?
We all chipped in for and have everyone’s round to bring a bag of coffee beans and in the morning we grind it and make coffee in this semi professional espresso maker.

Now the problem was because everyone is so busy, no one wants to keep a track of whos round was the last one and even though we tried many different manual methods, either the next person would not bring the beans at the right time or can’t remember whos round is next.

## My Solution
My solution is a very simple Power Automate flow that takes the names from a SharePoint List, make sure user is in the Team and if not add the user and post a message in the Team I’ve specified in the flow steps and once everyone’s round is completed, it sends a mail to everyone saying the round has completed which will restart the process again.
This has the opportunity to add new mates to the list and the flow will make sure they are in the Team and send them the message once the current mates’ round is finished.

## Creating the List
This is where I maintain my mates who are in the Make your own Coffee clique and add any new people

![image](https://user-images.githubusercontent.com/98259062/180681285-93ff8606-1793-4dfc-affd-26fa89b7a7c7.png)

## What did I create?
* For this I used a service account called CoffeeBot - You can similaly use the **Flow Bot** without creating an additional account
* Team called TEAM IT and Channel called CoffeeChannel with CoffeeBot as an Owner in that Team
* A proper M365 License for the CoffeeBot user

## My Steps
The very 1st trigger will be a manual one as it is waiting for the mail to arrive to trigger the flow

![image](https://user-images.githubusercontent.com/98259062/180681354-2777eae1-3f44-4a8b-b629-9043529e6427.png)

It get the items from my SPO list

![image](https://user-images.githubusercontent.com/98259062/180681385-f9c8d6d6-634c-4c71-aeba-77015b5c8637.png)


Sorting process as I was not 100% accurate with the ODATA Filter in the previous step

![image](https://user-images.githubusercontent.com/98259062/180681425-b59a641b-d479-4c3f-8d75-c7bc9970af1d.png)


For each value derived from the previous step, get the @mention, send the email wait for the given number of minutes/ hours/ days and do until the the current value is not equal the next in line

![image](https://user-images.githubusercontent.com/98259062/180681451-9fedcd05-c789-4bd0-b2b0-f9931259718a.png)

Once the do until breaks at the end of the list, send a mail to everyone in the list with the specific Subject from the specific account and terminate the flow for that round

![image](https://user-images.githubusercontent.com/98259062/180681472-b24cbb0f-132d-47aa-9028-655dc14a3975.png)

The Teams Message that spits out according to the provided delay

## The Result

My delay was 1 minute. That is to test the flow, but for this use case I would take the average time to finish a 1kg of coffee beans.

The specific person will get the @mention so they will be notified by Teams.

![image](https://user-images.githubusercontent.com/98259062/180681527-9475b827-1a34-40da-aa69-fa51f53792cd.png)

**Coffee round completion email**
As the trigger to start the flow is waiting in arrival of this email, it will start the flow of next round.

![image](https://user-images.githubusercontent.com/98259062/180681585-c4f67756-8614-4536-b0e2-b4b1e85dbf0d.png)







