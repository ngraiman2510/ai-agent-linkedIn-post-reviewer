# LinkedIn Post Approval Agent

This n8n workflow helps you draft and review a LinkedIn post before you decide
whether to use it. Enter a topic in a simple form, let AI write and improve the
draft, then approve or reject the final version from an email.

> **You stay in control:** approval does not publish the post to LinkedIn. The
> workflow only emails the draft and waits for your decision.

## What happens when I submit a topic?

```text
Topic form
   ↓
Writer Agent creates a draft
   ↓
Reviewer Agent checks and improves it
   ↓
Gmail sends you the final draft
   ↓
You approve or reject it
```

The agents use OpenAI's `gpt-4o-mini` model. The writer is asked for a
120–150-word post with a hook in the first line, exactly one call to action,
and no more than three hashtags. The reviewer evaluates the hook, clarity, and
call to action, then rewrites the weakest part. The final email contains the
post for your review.

## See it in action

### The workflow in n8n

![LinkedIn post review workflow](LinkedIn%20Post%20Reviewer%20Agent.png)

### The topic form

![Form for entering a LinkedIn post topic](Linked%20Post%20Topic%20On%20Form%20Submission.png)

## What you need

- An n8n instance
- An OpenAI API credential
- A Gmail OAuth2 credential that can send approval emails
- Access to the email address where you want to receive drafts

OpenAI usage may incur charges. Email topic data and generated post content are
sent to the connected OpenAI and Gmail accounts.

## Set it up

### 1. Import the workflow

1. Download or open `LinkedIn Post Approval Agent.json`.
2. In n8n, choose **Import from File** and select that JSON file.
3. Open the imported workflow.

### 2. Connect OpenAI

1. Open each **OpenAI Chat Model** node.
2. Select your OpenAI credential.
3. The workflow uses `gpt-4o-mini` by default. Change the model if needed.

### 3. Connect Gmail and choose who receives the approval email

1. Open **Approval Email**.
2. Select your Gmail OAuth2 credential.
3. Change **Send To** to your email address.
4. Confirm the email subject and approve/reject buttons are suitable.
5. Check the node's wait-time setting. The exported workflow has a wait limit
   configured with a value of `24`; confirm the unit shown in your n8n version.

> The imported workflow may refer to credentials from another n8n account.
> Select your own credentials; credentials are not included in the exported
> workflow.

### 4. Try it out

1. Save the workflow.
2. In n8n, run it in test mode and open the form-test URL, or activate/publish
   the workflow and use its production form URL.
3. Enter a focused topic, such as `How small teams can use AI responsibly`.
4. Submit the form and wait for the workflow to finish writing and reviewing.
5. Check your inbox for **LinkedIn post Approval**.
6. Read the draft, then choose **Approve** or **Reject** in the email.

If you're using a production form, the workflow must be active or published in
your n8n version. Test submissions may use a temporary form URL.

## Files in this folder

- `LinkedIn Post Approval Agent.json` - workflow to import into n8n
- `LinkedIn Post Reviewer Agent.png` - workflow screenshot
- `Linked Post Topic On Form Submission.png` - topic form screenshot
- `Gmail AI Agent n8n workflow.png` - Gmail AI Agent screenshot (not part of this
  LinkedIn approval workflow)

## Change the writing style

You can edit the prompts in **Writer Agent** and **Reviewer Agent** to change
the post requirements. For example, adjust the word count, tone, hashtag limit,
or call-to-action rule. Keep the two prompts consistent so the reviewer
preserves the style you want.

You can also change the form title and description in **On form submission**,
and the recipient and email subject in **Approval Email**.

## What this workflow does not do

- It does not publish or schedule posts on LinkedIn.
- It does not automatically revise a rejected post or send a second draft.
- It does not guarantee that AI-generated facts are correct; review the draft
  before using it.

## If something goes wrong

- **The form does not open:** use the test URL while executing in test mode, or
  activate/publish the workflow before using its production URL.
- **The AI nodes fail:** check the OpenAI credential, API access, and available
  usage balance.
- **No approval email arrives:** check the Gmail credential, recipient address,
  spam folder, and the **Approval Email** node's execution details.
- **The workflow is waiting:** open the execution in n8n; the Gmail approval
  node pauses until a decision is submitted or its configured wait limit is
  reached.
