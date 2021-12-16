# Accounts

The accounts management and settings can be found by clicking on Accounts in the side bar of the Open Commerce Admin. 
![Accounts](_assets/41-admin-accounts-settings.png)

Accounts has 3 tabs, namely:
- Staff Members: To view and modify the accounts involved with shop management and the different permission groups to which they belong. The first account created is populated under Owner. This tab has additional provisions to create more groups and invite new staff members.
- Customers: To view list of customers.
- Invitations: To view list of sent out invites. Once an invite is accepted it is cleared from the list.

## Create Group

To create a new group click on the Create Group button in the the Staff Members tab. A pop up dialog will appear with the following fields.
- Name: Group name. Thia is the only mandatory field.
- Slug: A URL friendly slug. If not provided the group name is used as slug.
- Description: A short description of the group.
- Roles: A dropdown with create, modify and delete permissions for different features of the app.

Don't forget to save changes at the end.

## Invite Staff Member

You can invite people to be a part of your shop management by clicking on the Invite Staff Member button. You will require the staff members Name and Email ID. Select the groups to which this member is to be added. 
![Invite Staff Member](_assets/41-admin-accounts-invite-staff.png)

Once an invite has been successfully sent the recipient should receive an email invitation.

> Note: You will need to set up an email service to send out invites. Refer to [Email Configuration](75-email-configuration.md) for more information.