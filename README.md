# LinkedIn Post Approval Agent

Turn a topic into a reviewed LinkedIn post, approve it from your inbox, and
automatically save approved posts to Google Sheets.

> **You make the final call.** This workflow does not publish anything to
> LinkedIn. It drafts, reviews, emails, and (if approved) logs the post.

## Try to picture the flow

```text
You enter a topic
       ↓
Writer Agent drafts a post
       ↓
Reviewer Agent improves the draft
       ↓
Gmail asks you to approve or reject
       ↓
 Approved? ── Yes → Save the post to Google Sheets
       └───── No  → Stop; nothing is added to the sheet
```

Both AI agents use OpenAI's `gpt-4o-mini` by default. The draft instructions
request 120–150 words, a hook in the first line, exactly one call to action,
and up to three hashtags. The reviewer evaluates the hook, clarity, and call
to action, then rewrites the weakest part.

## Quick start checklist

- [ ] Import `LinkedIn Post Approval Agent Latest.json` into n8n.
- [ ] Connect your OpenAI, Gmail, and Google Sheets credentials.
- [ ] Set the email address that should receive approval requests.
- [ ] Select your own Google spreadsheet and confirm its column headers.
- [ ] Run one test with a topic you can easily review.

## Step 1: Import the workflow

1. In n8n, choose **Import from File**.
2. Select [`LinkedIn Post Approval Agent Latest.json`](./LinkedIn%20Post%20Approval%20Agent%20Latest.json).
3. Open the imported workflow and save it.

The workflow is inactive by default. Connect your accounts before activating
it or sharing the production form.

## Step 2: Connect your accounts

### OpenAI

Open both **OpenAI Chat Model** nodes and select your OpenAI credential. The
workflow uses `gpt-4o-mini`; select another available model if you prefer.

### Gmail

Open **Approval Email** and:

1. Select your Gmail OAuth2 credential.
2. Change **Send To** to the address where you want to receive approval
   requests.
3. Check the subject and approval/rejection buttons.
4. Review the wait limit in the node. The exported workflow sets a value of
   `24`; verify the time unit shown in your n8n version.

The Gmail account needs permission to send these messages and wait for your
approval response.

### Google Sheets

The **Append row in sheet** node records approved posts. Connect a Google
Sheets OAuth2 credential, select your spreadsheet, and use these column
headers in the first row:

| Date | Topic | Final Post | Status |
|---|---|---|---|
| *(approval response time)* | *(submitted topic)* | *(reviewed post)* | `Approved` |

The folder includes [`LinkedIn Posts.xlsx`](./LinkedIn%20Posts.xlsx) as a
spreadsheet template. Upload or convert it to Google Sheets, then select that
Google Sheet in n8n. If you use another sheet, make sure the four headers match
exactly.

> The exported workflow may contain references to credentials and accounts
> from another n8n instance. Re-select your own credentials and spreadsheet
> after importing. Do not put API keys or passwords in the workflow JSON.

## Step 3: Make your first post

1. In n8n, open the form trigger **On form submission**.
2. For a test, start the workflow's test listener and open the test form URL.
   For regular use, activate/publish the workflow and use its production form
   URL.
3. Enter a clear topic. For example:

   > How small teams can use AI responsibly

4. Submit the form. The writer and reviewer will prepare the post.
5. Open the email titled **LinkedIn post Approval**.
6. Read the complete post, then select **Approve** or **Reject**.
7. If approved, check your Google Sheet for the new row.

### What should happen?

- **Approved:** the workflow appends the approval time, topic, final post, and
  `Approved` status to the selected Google Sheet.
- **Rejected:** the workflow does not append a row. It does not publish or
  automatically rewrite the post.
- **No response:** the workflow waits until the configured approval wait limit
  is reached.

## See the workflow and form

### Workflow diagram

![LinkedIn post approval workflow](LinkedIn%20Post%20Multiagent%20approver.png)

### Topic form

![Form for entering a LinkedIn post topic](Linked%20Post%20Topic%20On%20Form%20Submission.png)

### Example approval email

[`Gmail - LinkedIn post Approval.pdf`](./Gmail%20-%20LinkedIn%20post%20Approval.pdf)

## Files in this project

- `LinkedIn Post Approval Agent Latest.json` - current n8n workflow export
- `LinkedIn Posts.xlsx` - spreadsheet template for approved-post logging
- `LinkedIn Post Multiagent approver.png` - workflow screenshot
- `Linked Post Topic On Form Submission.png` - form screenshot
- `Gmail - LinkedIn post Approval.pdf` - example approval email

## Want to customize it?

- **Change the post style:** edit the prompts in **Writer Agent** and
  **Reviewer Agent**. Keep the requirements consistent in both prompts.
- **Change the form:** edit the title, description, or required topic field in
  **On form submission**.
- **Change the reviewer:** adjust what it checks (hook, clarity, call to
  action) in **Reviewer Agent**.
- **Change the approver:** update **Send To** and the email subject in
  **Approval Email**.
- **Change where approvals are saved:** select another Google spreadsheet in
  **Append row in sheet**, keeping the expected headers.

## If something does not work

| What you see | What to check |
|---|---|
| Form URL does not open | Use the test URL while the test listener is running, or activate/publish the workflow for its production URL. |
| AI node fails | Confirm the OpenAI credential, model access, and API usage balance. |
| Approval email is missing | Check the Gmail credential, recipient address, spam folder, and the node's execution details. |
| Approved post is not in Sheets | Confirm the Google Sheets credential, selected document, worksheet, and exact column headers. |
| Execution is still waiting | The Gmail approval node pauses until you respond or its configured wait limit is reached. |

## A few important notes

- Approval is a human review step, not LinkedIn publishing.
- Review generated text for accuracy, tone, and claims before using it.
- The workflow sends the submitted topic and generated content to OpenAI, and
  sends the final draft to Gmail. Use accounts and data that you are authorized
  to share with those services.
- OpenAI API usage may incur charges.
