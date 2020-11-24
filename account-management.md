---
title: Account Management
---

## The basics

An account in Reaction represents a single person who uses the system, either as a shop operator or as a customer.  

*Accounts* in Reaction are different than *users*, although they are linked to one another. A user is represented by a login e-mail and password, which are managed by a system external to Reaction. Each user maps onto a single account, which includes all Reaction-specific information, such as carts, orders, and profiles.

## Using the Accounts dashboard

The Accounts dashboard is the place to manage user groups and invite shop owners or managers. Here, you can add members to specific groups, thus giving them privileges to perform actions. By default, the Accounts dashboard shows the administrator groups (i.e., Owner and Store Managers) and the users in those groups.

💡 **Note**: When developers first start the main Reaction app with an empty database, it will automatically create an initial administrator account.

[tk screenshot]

## Add a store manager or owner

Before getting started, make sure you are logged into `reaction-admin` (on [localhost:4080](http://localhost:4080) if you're running it locally), and click on **Accounts** in the left sidebar.

To add a new member to your store, click the **Invite Staff Member** button at the top of the **Staff Members** tab on the Accounts dashboard. Enter the user's name and email address and choose their role (Shop Manager or Owner). An invitation will be sent to the new user via email.

## Default admin groups

There are two default admin groups that come with Reaction: Shop Manager and Owner.

### Owner group

This is a special group for the "owner" of a store. This group has all the permissions available in the app. There can only be one Owner per store.

Store owners can:

- Move a user from one group to another, except for the Owner group.
- Assign another user as the owner of a store. This will remove the current owner from the group, because there can only be one store owner.

### Shop manager group

This group has almost all of the permissions of an owner. A user in the store manager group can act on behalf of the owner in most cases.

## Managing group permissions

Store Owners and users with permission to access the Accounts dashboard have the ability to:

- Create new groups under the **Staff Members** section
- Add existing users to groups
- Add or remove permissions from groups
- Invite new users to groups (with the same or lesser permissions than the inviting user)