# Creating and managing a shop

## The basics
Congratulations on a successful installation of Open Commerce. Now, you can access the Admin Portal at http://localhost:4080/
![Admin](_assets/01-admin-clean.png). The Open Commerce Admin is the user interface used by administrators and shop managers to set up the required shop settings and manage operations of the shop.

## Create a user
To create a user, click the profile icon in the top right corner of the screen and select **Create Account**.


Click **Create Account** to bring up the sign-up window.

You'll need a valid email to use as your ID. Open Commerce does not enforce any password requirements but we recommend you use a strong password. The first user you create is automatically added to the *account-manager*, *system-manager*, and *owner* groups. The shop owner can assign roles to any subsequent users, but they are not added to any groups automatically.

## Create a shop

Once you've created your first user, you're prompted to create a shop. Enter the name of the shop in the text field and click **Create Shop**. The first shop you create is automatically assigned to the primary shop type. Each Open Commerce installation can have only one *primary shop*. Any subsequent shops you may create is assigned a *merchant shop* type.

You can create multiple shops in Open Commerce. This is referred to as *multitenancy*. To create additional shops, click your shop name in the upper left corner of the screen and choose **New Shop** from the drop-down menu. Enter the name of the new shop and click **Create Shop**. This new shop is automatically assigned to the merchant shop type.
![Create second shop](_assets/02-admin-create-shop-2.png)

You can switch between shops using the admin portal. Click the name of the current shop in the upper left corner and then choose the shop you want to switch to.
![Switch shop](_assets/02-admin-switch-shop.png)

Once you have created a user and set up a shop, the admin portal will look like this.
![Admin](_assets/02-admin-user-shop-setup.png)

## Accounts

You'll find account management and settings options by clicking Accounts in the side menu of the Open Commerce Admin portal.

Within Accounts you can access the following options::
- Staff Members: View and modify the accounts involved with shop management and the different permission groups to which they belong. The first account created is populated under Owner. You can also create additional groups and invite users here.
- Customers: View customers of the shop..
- Invitations: View the outstanding sent invitations. Once the user accepts an invitation it is no longer shown here.

### Create Group

To create a new group, click Create Group under Staff Members, and add the following information in the pop-up modal:
- Name: Group name. Required.
- Slug: A URL-friendly slug. Optional. If you don't provide this, the group name is used as the slug.
- Description: A brief description of the group. Optional.
- Roles: Select the appropriate permissions for the group from the dropdown menu. You can create, modify and delete permissions for different features of the app here.

Don't forget to save your changes!

### Invite Staff Member

You can invite people to be a part of your shop management by clicking Invite Staff Member. You'll need to enter their Name and Email ID, and then you can select the groups to which you want to add this member.

Once you've successfully sent the invitation, the recipient should receive an email.

> Note: You'll need to set up an email service to send out invitations. See [Email Configuration](75-email-configuration.md) for more information.

## Payments

To set up Payments, go to Settings > Payments in the admin dashboard. Click to enable or disable a payment method. Enabled payment methods are visible to the customer at checkout.
You can use the IOU Example payment method to test your store before enabling your actual payment method, like Stripe or another supported payment processor.
Open Commerce includes a [Stripe](https://stripe.com/) plugin for payments. The new plugin is [SCA complient](https://stripe.com/docs/strong-customer-authentication).

## Taxes

To set up Taxes, go to **Settings > Taxes** in the admin dashboard.
Select whether to add tax charges to products. If you do choose to add tax charges, select Custom Rates to define the tax rate you'll be using in the Custom Tax Rates table. Note that the default tax code for products applies to all products. The tax codes can be created in the Custom Tax Rates table below. Finally, click Save changes to save the settings.

To create a Custom Tax Rate, click New Tax Rates and fill in the fields on the pop-up modal.
- **Rate**: The percentage tax that needs to be applied.
- **Country**: The origin or destination country for which the tax rates apply. Leave this blank if you want it to apply to any Country.
- **Region**: The origin or destination region for which the tax rates apply. Leave this blank if you want it to apply to any Region.
- **Postal code**: The origin or destination postal code for which the tax rates apply. Leave this blank if you want it to apply to any Postal code.
- **Tax code**: An identifier or name for the tax rate.
- **Sourcing**: You can pick between Origin and Destination. This allows different tax rates to be calculated based on the country, state or even ZIP code of the sender or receiver.

Finally, click **Save changes** to save your new settings.

>Note: Leaving all fields (Country, Region, Postal code and Sourcing) blank applies this rate to every product and every destination address. Generally, we recommend creating a tax rates for one specific country or geographic region per rate.

## Shipping method

To set up a shipping method, go to **Settings > Shipping** in the admin dashboard and then scroll to click the **plus icon (+)** under **Flat Rate**. Fill in the following information.
- **Method Name**: A unique name to identify the shipping method. This name is not visible to the public. The admin can use detailed names to identify a method among many.
- **Public Label**: The shipping method name seen on the storefront by the customer during checkout.
- **Group**: Select from a dropdown list consisting of Free, Ground, One day, and Priority to help you organize your various shipping methods.
- **Cost**: Your cost, as a shop owner, to ship this item.
- **Handling**: The handling price you'll charge for this shipping method at checkout.
- **Rate**: The shipping price you'll charge for this shipping method at checkout.
- **Enabled**: Select this to make the option visible on the storefront.

Finally, click **Save changes** to save your new settings.

Shipping methods enabled at storefront on checkout.
![Storefront Shipping at Checkout](_assets/74-storefront-shipping-checkout.png)

## Address validation

You can find settings for the for Address Validation under **Settings > Address Validation** on the admin dashboard.

To add a new address validation service, click **Add Address Validation Services** and fill in the information on the pop-up modal.

- **Validation service**: The list of address validation services that have been set up with the application, including a default Test Validation for testing.
- **Country filter**: The countries to which the address validation should apply.
