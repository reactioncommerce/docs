---
title: Orders and Fulfillment
---

## The basics

Orders are created from carts at the completion of the checkout process. While shopping and checking out, Reaction collects information from the shopper and saves it to the cart. When the shopper checks out, an order is created and the cart is deleted. The order has much of the same information that its source cart had, but additionally it tracks the process of fulfilling the order and completing the associated payments, as well as returns, taxes, and invoice information.

Orders can also be created directly from another system.

Fulfillment refers to the way in which shoppers choose to receive the items they order, and the process of completing (fulfilling) that order. The built-in fulfillment types are “shipping”, “pickup”, and “digital”.

Every product variant (catalog item) has a list of available fulfillment types. If a catalog item can be fulfilled in more than one way (e.g., shipping or pickup), then a shopper is shown a selector during checkout, allowing them to choose the preferred fulfillment type.

The fulfillment type also determines which user interface components are shown to a shopper during checkout. For example, a client may show a shipping address form for items with the “shipping” type, and a pickup time and location form for items with the “pickup” type.

## Processing an order

To view and process orders, log in to the Reaction admin dashboard and click on **Orders** in the sidebar. You'll be taken to the Orders view, which is a sortable table showing a list of all orders your shop has received.

<!-- 
![](/assets/operator-guide-orders-table.png "Reaction Admin orders table")
 -->

Clicking on an order will take you to the single order processing screen. This will show you all of the order details, including the order summary, fulfillment groups, payments, and refunds. From here you can fulfill your order and act upon all of this data.

<!-- 
![](/assets/operator-guide-single-order.png "Reaction Admin single order page")
 -->

## Fulfillment groups

### Fulfillment groups

Items in a cart, and ultimately in an order, are divided into fulfillment groups. As soon as items are added to a cart, they are divided into groups by shop and fulfillment type. Items from the same shop may be moved between fulfillment groups if they support multiple fulfillment types.

### Fulfillment quotes

A fulfillment quote represents the current price of a single fulfillment method for a specific cart. The price may be a fixed amount or may be calculated based on the “from” and “to” addresses, the parcel dimensions, discounts, and other factors. Thus, as the items in a fulfillment group change, the fulfillment quotes for that group are updated and can also change.

💡 **Note**: The price may be comprised of multiple prices, for example, shipping and handling. These prices are separated for tax or accounting purposes, but may or may not be shown separated to the shopper.

### Fulfillment methods

A fulfillment method describes how a shop will deliver an item in greater detail than a fulfillment type. For example, there might be two methods for the “shipping” type, “Free Shipping” and “Flat Rate USPS”. In this case, the shopper is able to choose between these methods at checkout. Some fulfillment types may only have a single fulfillment method associated with them, in which case the shopper would not see any choice during checkout.

Shipping options can be restricted based on nearly any aspect of a cart, including item attribute (e.g., products with the tag "Hazardous" are pick-up only), destination (e.g., no shipping to Hawaii), a price limit (e.g., shipping only for orders above $100), or a combination of these (e.g., no live plants can be sent to California). By default, shipping methods can be allowed or restricted based on the following item data: `attributes`, `height`, `weight`, `width`, `length`, `price`, `productVendor`, `quantity`, `subtotal`, `tags`, `title`, and `variantTitle`. They can also be restricted by these shipping destination fields: `postal` code, `region`, and `country`.

Based on these criteria, the restriction can be set to either `allow` or `deny`. Shipping restrictions can be applied universally to all shipping methods or only to select methods.
    
### Acting on fulfillment groups

- **Update group status** lets you change a fulfillment group's status to any other available status. 
- **Tracking** lets you add an optional tracking number to a fulfillment group at any time. Once a tracking number has been added, click on the number to edit it.
- **Cancel group** will cancel the fulfillment group. A dialog will require you to confirm your desire to cancel the group.

All of these actions will only affect the group you’re working on.

<!-- 
![](/assets/operator-guide-update-group-status.png "Update fulfillment group status")

![](/assets/operator-guide-cancel-group.png "Cancel group")
 -->


### Canceling an entire order

Clicking the **Cancel order** button at the top right of the screen will cancel _*all*_ fulfillment groups in the order, which in turn cancels the order itself. When you cancel an entire order, the system automatically refunds the full amount of every payment that supports refunds. A dialog will require you to confirm your desire to cancel the order.

<!-- 
![](/assets/operator-guide-single-order-cancel-order.png "Cancel an entire order")
 -->

## Payments

A *payment method* is the type of payment, or which API will be used to process it. Reaction Commerce itself does not have any payment methods; all payment methods are added by plugins. One of the [Reaction core plugins](https://mailchimp.com/developer/commerce/docs/core-plugins) supports processing payments using [Stripe](https://stripe.com).

*Payment input* is the information that is necessary to create a charge, dependent on the payment method. For example, the “stripe_card” payment method requires a billing address and a Stripe card token as input.

During checkout, the shopper chooses a payment method (if there are more than one available) and enters any information required by that method. That payment input is then saved temporarily and used to create the order at the end of the checkout flow.

If the checkout UI component for a payment method supports entering a specific amount lower than the full amount due, then a shopper has an opportunity to add additional payments until the full amount due is covered.

When a client creates an order, collected payment input is passed along with the order. The payment plugin will then create one or more *payments* (also referred to as *charges*) to pay for the order. While placing the order, each payment must be "authorized" by the payment method handler, but each payment method can decide what "authorized" means. If all payments for an order are authorized, then the order is created. After that, a shop operator can individually capture, refund, or partially refund each payment.

### Capturing payments

Each payment method can decide what it means to "capture" a payment. For a credit card payment, it means actually charging the card. For an invoice payment method, it might mean sending the invoice. It might not mean anything, in which case the payment method will mark the payment captured without doing anything.

All payments can be captured in a single click by using the **Capture all payments** button inside the Payments card.

<!-- 
![](/assets/operator-guide-capture-all-payments.png "Capture all payments")
 -->

Each payment can be captured independently by using the **Capture payment** button next to each payment line.

<!-- 
![](/assets/operator-guide-capture-single-payment.png "Capture single payment")
 -->

If your payment provider flags a payment as having an elevated risk, you will be notified by a message in the payments information area. You can still capture this payment independently, or alongside other payments with the **Capture all payments** button. Either way, you will be required to confirm your desire to capture the payment in a dialog.

<!-- 
![](/assets/operator-guide-single-order-elevated-risk-payment.png "Capturing a payment with elevated risk")
 -->

### Refunds

Payment methods may or may not support refunding. Each payment has a current status, such as "authorized", "captured", "refunded", or "partially refunded". If your payment method allows refunds, you will be able to refund any amount, up to the full amount charged. 

💡 **Note**: Payments must already be captured to be refunded.

<!-- 
![](/assets/operator-guide-single-order-refunds.png "Refunds UI")
 -->

To refund one or more payments, type the amount you'd like to refund in the appropriate input box. If more than one payment is to be refunded, you'll see the total refund amount updated in the submit button <!-- IN the button? -->. You many enter an optional refund reason.

<!-- 
![](/assets/operator-guide-refunds.png "Refunds")
 -->

Once a payment has been partially refunded, you will see the refunded amount, as well as the amount that is still available to refund, under each payment.

<!-- 
![](/assets/operator-guide-partial-refunds.png "Partial refunded")
 -->

If your payments have been fully refunded, you will see a message that states this. All previous refunds are listed in a card at the bottom of the page.

<!-- ![](/assets/operator-guide-fully-refunded.png "Fully refunded") -->