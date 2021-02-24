<!-- # Account Management -->

## The basics

An account in Mailchimp Open Commerce represents a single person who uses the system, either as a shop operator or as a customer.

Open Commerce _accounts_ are different than _users_, although they are linked to one another. A user is represented by a login email and password, which are managed by an external system. Each user maps onto a single account, which includes all information specific to Open Commerce, such as carts, orders, and profiles.

The **Accounts** page of the admin dashboard is the place to manage user groups and invite shop owners or managers. There, you can add members to specific groups, thus giving them privileges to perform actions. By default, the Accounts page shows the administrator groups (i.e., Owner and Shop Managers) and the users in those groups.

> Note: An account is generally created by registering using one of the Open Commerce UI clients. However, when a developer first starts the Open Commerce app with an empty database, it will automatically create an initial administrator account.

## Default account groups

There are two default admin groups that come with Mailchimp Open Commerce: Shop Manager and Owner.

### Owner group

This is a special group for the "owner" of a store. This group has all the permissions available in the app. There can only be one Owner per store.

Store Owners can:

- Move a user from one group to another, except for the Owner group.
- Assign another user as the Owner of a store. This will remove the current Owner from the group, because there can only be one store Owner.

### Shop manager group

This group has almost all of the permissions of an owner. A user in the Shop Manager group can act on behalf of the Owner in most cases.

## Add a shop manager or owner

Before getting started, make sure you are logged into `reaction-admin` (on [localhost:4080](http://localhost:4080) if you're running it locally), and click on **Accounts** in the left sidebar.

To add a new member to your store, click the **Invite Staff Member** button at the top of the **Staff Members** tab on the Accounts page. Enter the user's name and email address and choose their role (Shop Manager or Owner). An invitation will be sent to the new user via email. To customize the content of these emails, see [Sending Emails](tk).

## Managing group permissions

Store Owners and users with permission to access the Accounts dashboard have the ability to:

- Create new groups under the **Staff Members** section
- Add existing users to groups
- Add or remove permissions from groups [tk flesh this out?]
- Invite new users to groups (with the same or lesser permissions than the inviting user)
