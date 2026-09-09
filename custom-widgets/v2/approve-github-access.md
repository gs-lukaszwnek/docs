---
url: >-
  https://developer-portal.gainsight.com/docs/custom-widgets/v2/approve-github-access.md
description: >-
  Review and approve a pending GitHub app installation request as your
  organization's GitHub administrator
---

# Approve GitHub Access

If a member of your community requested to connect the platform's GitHub app and your organization requires administrator approval, use this guide to review and approve that request.

## Prerequisites

* GitHub organization administrator access

## Step 1: Access Pending Requests

1. Go to your GitHub organization's settings
2. Navigate to **Settings** → **Third-party access** → **Pending requests**

## Step 2: Review the Request

Review the application's requested permissions:

* **Repository access**: Which repositories the app can access
* **Permissions**: What actions the app can perform (read, write, etc.)

## Step 3: Approve or Deny

* Click **Approve** to allow the installation
* Click **Deny** to reject the request

> **Note**: After approval, the requesting user will need to return to the Sources settings and click  to complete the setup.

## What the Requesting User Sees

After you approve the request:

* The user does **not** receive an automatic notification
* They must return to the Sources settings and click  again
* If they had the page open, it may automatically detect the approval
* The installation will appear in their picker, ready to connect

## What You See After Approving

When you approve via the email link or GitHub settings:

* You'll see a confirmation page indicating the installation was approved
* This page is for your confirmation only — it doesn't complete the user's connection
* The requesting user must complete their connection separately

## Next Steps

* [Admin Approval](admin-approval) — The requesting user's side of this flow
* [Common Issues](common-issues) — Troubleshooting for connection problems
