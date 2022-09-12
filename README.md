## Here I will explain what I do and how I do this challenge

<b>Technical Design:</b>

Go to Setup and search for queue and create one named 'Agent' for lead object.
Go to Setup and search for Lead Assignment Rules and create one. Put priority as 1.
Go to Setup and search for Flows and create one Record Trigger Flow and choose:
-Lead as an object. 
-Trigger the Flow When: 'A record is created'
Then add an element: Update Triggering record and at Set Field Values for the Lead Record select Status and the value: Working.
go to Setup -> object manager -> serch for lead and create a new validation rule that blocks if status is closed/completed and source is blank.
Go to one lead record -> pres the wheel -> press edit page -> press on path and hide path update button
Go to Setup and search for Flows and create one Record Trigger Flow and choose:
-Lead as an object. 
-Trigger the Flow When: 'A record is updated'
Then add an element: Update Triggering record and Set Field Values for the Lead Record select RecordTypeID and as value put the recordtypeID value (you can find it if you open object manager -> lead -> recordtypes and select the Acquired record type and copy the id from link)
Go to Setup -> object manager -> lead -> create a validation rule that blocks for one of next 2 scenarios:
1. a new record of type Acquired is created
2. record type is changed and current record type is Acquired.


<b>Steps for deployment of my solution:</b>

1.Create a new Sandbox

2.Install Visual Studio Code

3.Install Salesforce CLI

4.Open this project in VSC

5.Go on manifest->package.xml-> right click and press deploy.

<b>Pre Release Steps: N/A </b>

<b>Post Release Steps: Go to Lead object -> open Acquired Record Type and copy the id from link.
                    Go to Flows -> open 'Change Record Type to Acquired'. Go to Resources -> constants and open AcquiredRecordTypeID -> past the Id that you copied in the value section. Save the flow as a new version.

So in this challenge i create one queue Agent to be owner of lead records when a new lead is created. I use on assignment rule for this to be done. Then I create a record trigger flow to update the status. This could be handled with a before insert flow, but it is a simple update on record so I choose this type of flow, as salesforce recommend.
I create one validation rule on lead to block one lead to be closed if the Source is not filled.
The path is displayed by default so i just hide the button that changes the status.
I create one process called 'Sales' because every record type needs one process. One process can have multiple record types so this process will hold both record type that i created.
Again, i prefered to use another record trigger flow insted of before update trigger because is one simple update on a record. The disadvantage of this is that we need a manual steps, as the id changes in every org for the record type.
And then i create one validation rule that runs in one of 2 scenarios:
-When a record of type Acquired is create (as I assume this record type is used only when a lead is closed.)
-When trying to edit a record of type Acquired
