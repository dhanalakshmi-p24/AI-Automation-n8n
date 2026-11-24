# AI-Automation-n8n
https://app.slack.com/app
# n8n Automation Projects

## Overview

This repository contains n8n automation workflows built as part of hands-on learning and assignments.

### Workflows Completed

1. Slack Recipe Assistant
2. Google Sheets to Gmail Email Automation

---

# Project 1: Slack Recipe Assistant

## Objective

Build an n8n workflow that:

- Listens for a recipe name in Slack
- Sends the recipe name to an AI model
- Generates a list of ingredients and quantities
- Posts the response back to the same Slack channel

## Workflow

```text
Slack Trigger
      ↓
AI Agent
      ↓
Slack Send Message

AI Agent
      │
      ▼
OpenAI Chat Model
```

## Example

User:

@Recipe AI Assistant Paneer Butter Masala

Bot Response:

- Paneer – 250g
- Onion – 2
- Tomato – 3
- Butter – 2 tbsp
- Cream – 1/2 cup
- Garam Masala – 1 tsp

---

## Issues Faced and Solutions

### Issue 1: Slack Webhook Verification Failed

Error:

```text
Your URL didn't respond with the value of the challenge parameter
```

Cause:

- Workflow was not active
- Slack could not verify the webhook URL

Solution:

- Published the workflow
- Used the correct production webhook URL
- Added the URL in Slack Event Subscriptions

---

### Issue 2: Socket Mode Enabled

Error:

```text
Socket Mode is enabled.
You won't need to specify a Request URL.
```

Cause:

- Socket Mode conflicted with Slack Event Subscriptions

Solution:

- Disabled Socket Mode
- Used Event Subscription Webhooks

---

### Issue 3: Chat Model Not Connected

Error:

```text
A Chat Model sub-node must be connected and enabled
```

Cause:

- OpenAI Chat Model was not connected to AI Agent

Solution:

- Added OpenAI Chat Model
- Connected it to AI Agent

---

### Issue 4: Could Not Get Parameter "user"

Error:

```text
Could not get parameter "user"
```

Cause:

- Slack Send Message node was configured for User instead of Channel

Solution:

- Changed destination to Channel
- Used Slack channel ID

---

### Issue 5: AI Asked for Recipe Name Again

Error:

```text
Please provide the name of the recipe...
```

Cause:

- Slack message text was not passed to the AI Agent

Solution:

Used:

```javascript
{{$node["Slack Trigger"].json["text"]}}
```

inside the AI prompt.

---

# Project 2: Google Sheets to Gmail Automation

## Objective

Read student information from Google Sheets, generate a professional email using AI, and send it through Gmail automatically.

## Workflow

```text
Manual Trigger
      ↓
Google Sheets
      ↓
AI Agent
      ↓
Gmail
```

---

## Sample Input

| Student_ID | Name | Course | Status |
|-----------|------|---------|---------|
| STU1001 | Manoj | Python | Paid |

---

## Generated Email

Subject:

```text
Course Enrollment Confirmation: Python
```

Body:

```text
Dear Manoj,

I hope this message finds you well.

I am writing to confirm your enrollment in the Python course.

Your payment has been successfully processed.

Thank you.

Best regards,
Training Team
```

---

## Issues Faced and Solutions

### Issue 1: Google Drive API Disabled

Error:

```text
Google Drive API has not been used in project
```

Solution:

Enabled:

- Google Drive API
- Google Sheets API

from Google Cloud Console.

---

### Issue 2: Invalid Client

Error:

```text
invalid_client
```

Cause:

- Incorrect OAuth Client ID
- Incorrect Client Secret

Solution:

- Created a new OAuth Client
- Added correct redirect URI
- Updated credentials in n8n

---

### Issue 3: No Prompt Specified

Error:

```text
Expected to find the prompt in an input field called chatInput
```

Cause:

- AI Agent expected Chat Trigger data

Solution:

Changed:

```text
Source for Prompt
```

to:

```text
Define Below
```

and created a custom prompt.

---

### Issue 4: Subject and Email Body Combined

Problem:

AI returned:

```text
Subject: ...
Body: ...
```

in one field.

Solution:

Used JavaScript Code Node to separate Subject and Body.

Example:

```javascript
for (const item of $input.all()) {

  const output = item.json.output;

  item.json.emailBody = output.replace(
    /^Subject:.*?(\\n\\n|\n\n)/s,
*   ''
  );

}

return $*nput.all();
```

---

##*Technologies Used

- n8n
- Slack A*I
-*OpenAI
* Google Sheets API
- Gmail API
-*JavaScript

---

## Learning*Outcomes

Through these projects*I learned:

- Creating*workflows in n8n
* Slack Trigger configuration
- Ope*AI integration
* Webhook setup*and troubleshooting
- Google Sheet**integration
- Gmail automation
* OAuth authentication*- JavaScript data transformation
-*AI-powered*workflow automation

---

## Autho*

**Dhanalakshmi**

*NET Developer | AI & Automation Le*rner

*itHub:
https://github.com/dhanalak*hmi-p24
