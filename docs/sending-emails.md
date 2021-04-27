## The basics

Open Commerce lets you send transactional emails for a variety of reasons, such as new user signups, password resets, and shipping notifications. The development platform comes preloaded with templates that specify the subject and body content of these emails.

In this documentation, we’ll cover configuring your email server and the types of templates that are available.

>**Note**: If you need to connect to a service that sends transactional messages at scale, check out [Mailchimp Transactional](https://mailchimp.com/developer/transactional/docs/fundamentals/). 

## Email configuration and logs

To send transactional emails from Open Commerce, you need to configure an SMTP email provider. Open Commerce will send emails from the address that you set in the `MAIL_URL` [environment variable](/open-commerce/docs/fundamentals/#environment-variables):

`MAIL_URL="smtp://username:password@example.com:465"`

You can use any SMTP server, including a self-hosted server.

All emails sent by Open Commerce are logged: to view them, go to **Settings > Email** in the admin dashboard. Emails are scheduled with a job queue, so failed emails will automatically attempt to resend up to five times, with three minutes between each retry. You’ll also see a button next to failed emails that will allow you to manually attempt a resend. 

## Editing email templates

A range of email templates are preloaded into the `Templates` collection when Open Commerce starts up.

Accounts:
- Invite Shop Member – New User Account
- Reset Password
- Verify Account (via LaunchDock)
- Verify Updated Email Address
- Welcome Email

Orders:
- New Order Placed
- Order Item Refunded
- Order Refunded
- Order Shipped

To customize a template, go to **Settings > Email Templates** in the admin dashboard. There are several fields that define each template: 

| Field | Description |
|-------|-------------|
|Title|A user-friendly name for the type of email.|
|Subject|The email subject line. [Handlebars expressions](https://handlebarsjs.com/guide/) are allowed, as long as they are passed to the server-side rendering function.|
|HTML|The body of the email. [Handlebars expressions](https://handlebarsjs.com/guide/) are allowed, as long as they are passed to the server-side rendering function.|
|Name|Not editable. The name of the function that is used to trigger the email.|
|Language|Not editable. The language that is set for the shop under **Settings > Shop Localization**. The language can’t be changed on a per-template basis.|


