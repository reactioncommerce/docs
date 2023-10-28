## The basics

The central function of an Open Commerce store is to handle orders. The first part of this process is done by the shopper: adding items to a shopping cart and checking out. The second part is done by a store manager: accepting payment and fulfilling items by delivering them to the shopper. 

When a shopper chooses an item for purchase, a cart is created; as they continue shopping and eventually check out, Open Commerce collects information from the shopper and saves it to the cart. At the end of the checkout process, an order is created and the cart is deleted. 

An order consists of all the items that a shopper has chosen to purchase; the methods for fulfilling the items and completing the associated payments; and information on returns, taxes, and invoicing. Orders are always organized into one or more fulfillment groups, each of which contains items that can be delivered in the same way by a single shop. The delivery choices that shoppers make at checkout determine what items go into which fulfillment group. 

This documentation outlines what information is tracked in fulfillment groups, how to process orders, and how to accept and refund payments.

## Fulfillment

Every product variant has a list of available fulfillment types. The built-in fulfillment types are “shipping,” “pickup,” and “digital,” but you can customize them or create additional types with plugins. If a variant can be fulfilled in more than one way, then the shopper can choose among the available fulfillment types during checkout, and that choice determines what other fields appear on the checkout form—for example, a shipping address form for items with the “shipping” type, or a pickup time and location form for items with the “pickup” type.

### Fulfillment methods

Each fulfillment type has at least one fulfillment method, which describes how to deliver an item in greater detail. For example, you might have two methods for the “shipping” type, “Free Shipping” and “Flat Rate USPS,” which a shopper can choose between at checkout. Some fulfillment types may only have a single fulfillment method, in which case the shopper would see the method already selected during checkout. 

### Shipping restrictions

Shipping options can be restricted based on nearly any aspect of a cart, including item attribute (e.g., products with the tag “Hazardous” are for pickup only), destination (e.g., no international shipping), a price limit (e.g., shipping only for orders above $100), or a combination of these (e.g., no live plants can be sent to California). By default, shipping methods can be allowed or restricted based on the following item data: 

* `attributes`
* `weight`, `height`, `width`, `length`
* `price.amount`
* `productVendor`
* `quantity`
* `subtotal.amount`
* `tags`
* `title`, `variantTitle`
* `description`
* `attributeLabel`
* `isTaxable`
* `originCountry`
* `postal` (ZIP or postal code), `region`, `country`

Based on these criteria, the restriction can be set to either `allow` or `deny`. Shipping restrictions can be applied universally to all shipping methods or only to select methods.
### Fulfillment quotes

A fulfillment quote represents the current price of a single fulfillment method for a specific cart. The price may be a fixed amount or may be calculated based on the “from” and “to” addresses, the parcel dimensions, discounts, or other factors. As the items in a fulfillment group change, the fulfillment quotes for that group are updated and can also change.

> **Note**: The total price of a fulfillment group may be comprised of multiple prices, such as item prices and shipping fees. These prices can be separated for tax or accounting purposes, but may or may not be shown separately to the shopper.
    
## Processing an order

To view and process orders, go to the **Orders** page, which shows a sortable table of all the orders your shop has received or that you’ve imported from an external system. Clicking on an order will show you its details, including the order summary, fulfillment groups, payments, and refunds. 

From here, you can perform various actions on a single fulfillment group:
 
 - **Update group status** lets you change a fulfillment group’s status to any other available status. 
- **Tracking number** lets you add an optional tracking number to a fulfillment group at any time. Once a tracking number has been added, click on the number to edit it.
- **Cancel group** will cancel the fulfillment group.

Clicking **Cancel order** at the top right of the screen will cancel all fulfillment groups in the order, which in turn cancels the order itself. When you cancel an entire order, the full amount is refunded (for every payment method that supports refunds). 

## Payments

Orders have one or more payment methods, which represent a type of payment or the API used to process a payment. Open Commerce does not have any built-in payment methods—all payment methods have to be added via plugins. We include an example plugin that supports processing payments using Stripe.

Each payment method specifies what kinds of payment input it requires. For example, the `stripe_card` payment method requires a billing address and a Stripe card token as input. 

During checkout, the shopper chooses a payment method (if more than one is available) and enters any required information. Some payment methods allow the shopper to pay a portion of the amount due; the shopper can then add additional payments until they have covered the full amount.

Each payment must be authorized by the payment method handler in order to create an order. The payment plugin will then create one or more payments (also referred to as “charges”) for the order. After that, a shop operator can individually accept, refund, or partially refund each payment.

### Capturing payments

The process of accepting authorized payments is called “capturing” the payment. The practical effect of capturing a payment differs based on the payment method—it may mean charging a credit card, sending an invoice, or nothing at all (like if a shopper “purchases” a free item).

To capture payments, navigate to the **Payments** page. You can capture all payments at once by clicking **Capture all payments**, or individually by clicking **Capture payment** next to each payment line. Payment providers can also flag payments as risky—you will see a notification in the payments information area. 

### Refunds

Payment methods may or may not support refunding.  If the payment method allows refunds, you will be able to refund any amount of an already-captured payment up to the full amount charged. 

To refund a payment, go to the **Refunds** tab on the order details page and enter the amount you’d like to refund. If you’re refunding more than one payment, you’ll see the total amount reflected in the **Refund** button. You can optionally enter a reason for giving the refund for your own records.

If a payment has been fully refunded, that will be reflected on the order page; if a payment has been partially refunded, you will see the refunded amount and the amount that is still available to refund under each payment. All previous refunds are listed in a card at the bottom of the order details page.
