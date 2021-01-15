<!-- # Sending Emails -->

## The basics

Mailchimp Open Commerce can send transactional emails for a variety of reasons, such as new user signups, password resets, order receipts, and shipping notifications. Open Commerce comes preloaded with templates that specify the subject and body content of these emails.

You need to configure an SMTP email provider in order to send transactional emails from Open Commerce.

## Email configuration and logs

You can configure the address that Open Commerce will send emails from by setting the `MAIL_URL` [environment variable](tk) to an SMTP URL.

```
MAIL_URL="smtp://username:password@example.com:465"
```

You can use any SMTP server, including a self-hosted server.

All emails sent by Open Commerce are logged. Go to **Settings > Email** in the admin dashboard to view log entries for each email.

If an email fails to send, you will see a button that will allow you to attempt to resend the failed email. Emails are scheduled with a job queue, so failed emails will automatically attempt to resend up to 5 times, with 3 minutes between each retry.

## Editing email templates

The following email templates are included in Mailchimp Open Commerce:

<!--
- Accounts – Invite Shop Member – Existing User Account
- Accounts – Invite Shop Member – New User Account
- Accounts – Invite Shop Owner
- Accounts – Reset Password
- Accounts – Verify Account (via LaunchDock)
- Accounts – Verify Updated Email Address
- Accounts – Welcome Email
- Orders – New Order Placed
- Orders – Order Item Refunded
- Orders – Order Refunded
- Orders – Order Shipped
 -->

- Accounts
  - Invite Shop Member
    - Existing User Account
    - New User Account
  - Invite Shop Owner
  - Reset Password
  - Verify Account (via LaunchDock)
  - Verify Updated Email Address
  - Welcome Email
- Orders
  - New Order Placed
  - Order Item Refunded
  - Order Refunded
  - Order Shipped

The Open Commerce email templates are loaded into the `Templates` collection on startup. To customize the loaded templates, go to **Settings > Email Templates** in the admin dashboard.

You can edit several aspects of each email template:

- `Title`: A user-friendly name for the type of email
- `Subject`: The email subject line. [tk check this: Blaze variables are allowed, granted they are passed to the server-side rendering function.]
- `HTML`: The body of the email. [tk check this: Blaze variables are allowed, granted they are passed to the server-side rendering function.]

Two non-editable fields also appear in this view:

- `Name`: The name of the function that is used to trigger the email
- `Language`: The language that is set for the shop under **Settings > Shop Localization**. The language can't be changed on a per-template basis.
