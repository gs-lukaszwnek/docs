---
url: >-
  https://developer-portal.gainsight.com/docs/custom-widgets/v2/admin-approval.md
description: >-
  Request and complete GitHub connection when your organization requires
  administrator approval before installing external apps
---

# Admin Approval

When connecting your GitHub repositories to the platform, some GitHub Enterprise organizations enforce security policies that require administrator approval before external applications can be installed. This guide explains what happens when admin approval is required and how to complete the setup once approved.

## Prerequisites

* A GitHub account with access to your organization
* Knowledge that your organization uses GitHub Enterprise with approval policies
* (Optional) Contact information for your GitHub organization admin

**This guide is for you if:**

* Your organization uses GitHub Enterprise with strict security policies
* You see a "pending approval" message after attempting to connect GitHub
* Your GitHub organization admin needs to approve external app installations

## Which Flow Will You See

When you click , you'll experience one of two flows depending on whether the app is already installed on your organization:

| Scenario | What You'll See | This Guide? |
|----------|-----------------|-------------|
| **App not installed on your org** | GitHub's installation wizard with "Install" or "Request" button | Yes |
| **App already installed** (by you or another community member) | Picker showing existing installations to connect | No approval needed |

**How to tell which scenario you're in:**

* If you see a list of organizations with "Install" or "Request" buttons → **New installation flow** (this guide)
* If you see a picker listing existing installations to select → **Already installed** (just pick and connect)

## New Installation Flow (Admin Approval Required)

This section covers what happens when you're installing the app on an organization for the first time and that organization requires admin approval.

### Step 1: Initiate GitHub Connection

Go to **Integrations** → **Developer Studio** → **Sources** and click :

![Screenshot placeholder: Sources page](../screenshots/widget-sources-page.png)

You'll be redirected to GitHub to authorize the application.

### Step 2: Request Installation

On GitHub, you'll see the installation page where you select your organization and repositories:

![Screenshot placeholder: GitHub installation request](../screenshots/github-install-authorize-request.png)

**What you'll see on this page:**

* **Organization selector**: Choose which organization to install on
* **Repository access**: Select "All repositories" or "Only select repositories"
* **Request tag**: Repositories requiring admin approval show a `request` tag
* **Permissions**: The app requests "Read access to code and metadata"

**To request installation:**

1. Select your organization from the dropdown
2. Choose which repositories to grant access to
3. Notice the `request` tag next to repositories requiring approval
4. Click **Install, Authorize, & Request** to submit your request

### Step 3: Approval Pending Page

After submitting your request, GitHub redirects you back to a confirmation page in the platform:

![Screenshot placeholder: Approval pending](../screenshots/installation-request-submitted.png)

**This page shows:**

* **"Installation Request Submitted"** — confirming your request reached your organization admin
* **What happens next** — three numbered steps explaining the approval and reconnection process
* **"Return to Sources"** link — takes you back to your Sources settings page

**While waiting for approval:**

* You can close the browser window — no action is needed until approval
* If you have the Sources page open in another tab, it will **automatically detect** when approval is granted
* The approval request does not expire, but your browser session may — you'll need to click  again after approval

> **Note**: If you've been waiting for more than a few days, follow up with your organization admin to check the status of your request.

### Step 4: Admin Approves the Request

Your GitHub organization admin will:

1. Receive a notification about the pending installation request
2. Review the app's requested permissions
3. Approve or deny the installation

> **Note**: This process may take hours or days depending on your organization's approval workflow.

### Step 5: Complete the Connection

Once your admin approves the installation:

1. Return to your Sources settings page
2. Click  again
3. Complete the standard OAuth flow
4. Select your installation from the picker

![Screenshot placeholder: Select installation](../screenshots/select-github-installation.png)

5. You'll see the success confirmation screen

![Screenshot placeholder: Connection successful](../screenshots/connection-success.png)

Your GitHub repositories are now connected and ready to use.

## What If Approval Is Denied

If your organization admin denies the installation request:

1. **You won't receive a notification** — the request won't be approved
2. **Contact your admin** to understand why it was denied
3. **Address any concerns** they may have about the application
4. **Submit a new request** once issues are resolved

Common reasons for denial:

* Security policy restrictions on third-party apps
* Need for additional review or documentation
* Temporary freeze on new app installations

## What Happens Behind the Scenes

For a new installation, your request waits with GitHub until your organization admin approves it — then you return and reconnect. If the app is already installed on your organization, you skip the wait entirely and connect through the picker. If you're the GitHub organization administrator approving these requests, see [Approve GitHub Access](approve-github-access).

## Troubleshooting

### "I don't see my organization in the list"

**Possible causes:**

* You're not a member of that GitHub organization
* The organization has restricted app installations to admins only
* You need to re-authenticate with GitHub

**Solution:**

1. Verify your GitHub organization membership
2. Contact your GitHub admin to check organization settings
3. Try signing out and back into GitHub

### "My admin approved but I still can't connect"

**Solution:**

1. Make sure you're clicking  again after approval
2. You must complete the OAuth flow again — approval alone doesn't connect you
3. Select your organization's installation from the picker

### "The approval is taking too long"

**What to do:**

1. Check with your GitHub organization admin
2. Ask them to check **Settings** → **Third-party access** → **Pending requests**
3. The request may be in a queue or require additional review

### "I see 'Request' instead of 'Install' for my organization"

**This is expected.** It means your organization requires admin approval. Follow the steps in this guide to request and complete the installation.

For more troubleshooting help, see [Common Issues](common-issues).

## Frequently Asked Questions

### How long does approval take?

Approval time depends on your organization's policies. It can range from minutes to several days. Contact your GitHub admin for your organization's typical approval timeline.

### Do I need to request approval every time?

No. Once the app is approved and installed, you won't need approval again unless:

* The app is uninstalled and needs to be reinstalled
* The app requests additional permissions

### Can I use a different organization while waiting?

Yes. If you have access to other GitHub organizations that don't require approval (or are already approved), you can connect those while waiting.

### Someone else in my org already installed the app. Do I need approval?

No. If the app is already installed on your organization (by another community member or team member), you won't see the "Request" button. Instead, you'll go through the simple OAuth flow and see a picker to select the existing installation. No admin approval is needed in this case.

### What permissions does the app request?

The application requests:

* **Repository access**: To read widget, script, and stylesheet files from your repositories
* **Webhooks**: To receive notifications when you push changes
* **Metadata**: To list available repositories

### Is my code safe?

See [Content Security](content-security) for how the platform handles your repository content.

## Getting Help

If you continue to experience issues:

1. **Check this guide** for troubleshooting steps
2. **Contact your GitHub admin** for approval-related issues
3. **Talk to your Gainsight team** if the issue persists after admin approval

## Next Steps

* [Connect Your GitHub Account](connect-github) — Standard connection flow
* [Repository & Branch Settings](repository-settings) — Configure repositories after connection
* [Common Issues](common-issues) — More troubleshooting help
