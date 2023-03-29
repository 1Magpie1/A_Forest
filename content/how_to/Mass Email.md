---
title: "Group Emails"
tags:
- Guide
---

This is a quick tutorial on using Word, Excel, and Outlook to send emails to groups of people!

This procedure will send out individual emails to each person on your list. 

> This means recipients can only respond to you, and no one else in the email list will see their response, even if the person hits "Reply All."


Good luck! If you need help, there are many resources online that can help!

## Requirements

Have Word, Excel, and Outlook installed

Make sure Outlook is your default email app

## Set up

You will need a spreadsheet with all the people you want to email, their email addresses, and any information you may need to make subgroups. 

| First_Name | Last_Name | Email Address            | Job          |
| ---------- | --------- | ------------------------ | ------------ |
| Jane       | Doe       | JaneDoe@example.email    | Mathematician |
| Jack       | Frost     | FrostyJack@example.email | Artist             |

Make sure it is clean with separate columns for name, email, and subgroup. 

> I recommend having people fill out the subgroup component as a forced-choice question to ensure consistent spelling and vocabulary. This will help out later.

## Writing and Formatting the Email

This component is relatively simple, draft the email in Word. If you want to include personal information (i.e. Names), leave that area blank.

Next, click "Mailings." In the ribbon, click "Start Mail Merge" $\rightarrow$ "Email Message." This formats the document as an email and prepares Word for the next steps.

To tell it where to find the email database, click "Select Recipients" $\rightarrow$ "Use an Existing List." This will open a dialogue box where you can select the excel document from **Set up**. 

>You will receive a dialogue asking if you trust the author of the excel document. This is an extra security measure. If you made the document, you do not have to worry. Click "Yes" to continue.

In the "Open Workbook" dialogue, you can select the specific sheet of the email lists. Leave the "Cell Range" option blank and click "OK."

### Optional 

1) Adding in people's names
If you want to include personal information from the excel document (i.e. people's names):
- Click so that your cursor is at the point where you want to insert the information into the email.
- Click "Insert Merge Field" and select the column with the information you want to insert
- To see how the final email will look when sent, use the "Preview Results" button to see what different recipients will receive.

2) Sending to only some participants
If you want to send only to a subset of your email group:
- Click "Filter Recipients" 
- In "Field," click the drop-down menu and select the column in your excel file with the information you want to use to subset your list.
- Use the "Equal to" option in the "Comparison" column and the group name in the "Compare to" column to select that specific group. 
	-  Use the "Not equal to" option in the "Comparison" column to choose a group to exclude.

## Sending Email

To send the email, click "Finish & Merge" $\rightarrow$ "Merge to Email."

After hitting "Merge to Email," you will receive the "Mail Recipient" dialogue box. 
- In the "To" field, select the column of the data sheet with people's emails.  
- In the "Subject" field, write out the email's subject line.
- In the "Send As" field, it is recommended that you use the "HTML" option. 

Finally, click "Mail Merge To Outbox." This will open Outlook, asking if you want to send the email. Click "Yes," and your email has been sent off! Good job you!