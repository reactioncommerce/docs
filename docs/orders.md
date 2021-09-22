# Order

Orders plugin deals with order creation, fetching orders by order ID, reference ID or payment ID, cancelling order, refunds. 

## Types

- _id: ID! - The Order ID

- account: Account - The account that placed the order. Some orders are created for anonymous users. Anonymous orders have a null account.
  
- billingName: String - Full name(s) involved with payment. Payment can be made by one or more than one person
  

- cartId: ID - The ID of the cart that created this order. Carts are deleted after becoming orders, so this is just a reference.

- createdAt: DateTime! -"The date and time at which the cart was created, which is when the first item was added to it.

- displayStatus: String! - The order status for display in UI"
  

- email: String - An email address that has been associated with the cart
  

- fulfillmentGroups: [OrderFulfillmentGroup]! -"One or more fulfillment groups. Each of these are fulfilled and charged as separate orders.
  

- notes: [OrderNote]! - Notes about the order. This will always return an array but it may be empty
  
- payments: [Payment] - Payments that collectively have paid or will pay for the total amount due for this order.
  
- referenceId: String! - An ID by which the customer can reference this order when enquiring about it. A storefront user interface may show this to customers. Do not display other IDs (`_id`) to customers.
  
- refunds: [Refund] - Refunds that have been applied to the payments on this order.
  
- shop: Shop! - The shop through which the order was placed
  
- status: String! - The machine-readable order status.
  
- summary: OrderSummary! - A summary of the totals for all fulfillment groups for this order
  
- totalItemQuantity: Int! - Total quantity of all items in the order
  
- updatedAt: DateTime! - The date and time at which this order was last updated
  
- surcharges: [AppliedSurcharge]!


## Query
1. **orders** : Query the Orders collection for one or more shops. Accepts a list of shopIds as query parameter. An order has much of the same information that its source cart had, but additionally tracks the process of fulfilling the order and completing the associated payments, as well as returns, taxes, and invoice information.

2. **orderById**  
Query the Orders collection for an order. It requires both orderId (as id) and shopId as query parameters. A token string can be used as a parameter for annonymous orders not linked with an account.


3. **orderByReferenceId**   
Query the Orders collection for an order. It requires both referenceId (as id) and shopId as query parameters. A token string can be used as a parameter for annonymous orders not linked with an account. ReferenceId id the one displayed to the customer. 

4. **ordersByAccountId** : Query the Orders collection for orders for a single account. Requires both accountId and shopIds.

5. **refunds** : Query the Orders collection for an order, and returns refunds applied to payments associated with this order. Requires both orderId and shopId. [More on refunds](https://github.com/reactioncommerce/docs/blob/3860ad31862c515dc9a9bcf8d9f86d781ad85f17/docs/fulfilling-orders.md#refunds).

6. **refundsByPaymentId** : An order can have one or more payment method. This query looks up the Orders collection for an order, and returns refunds applied to a specific payment associated with this order. [More on payments](https://github.com/reactioncommerce/docs/blob/3860ad31862c515dc9a9bcf8d9f86d781ad85f17/docs/fulfilling-orders.md#payments).

## Mutation
1. **addOrderFulfillmentGroup** : Use this mutation to add a new order fulfillment group to an order. It must have at least one item, which can be provided or moved from another existing group. The built-in fulfillment types are Shipping, Pickup, and Digital. Each fulfillment type has at least one fulfillment method. For example, “shipping” type can have methods like Free Shipping and Flat Rate USPS. [More on fulfilment](https://github.com/reactioncommerce/docs/blob/trunk/docs/fulfilling-orders.md#fulfillment)

2. **cancelOrderItem** : Use this mutation to cancel one item of an order, either for the full ordered quantity or for a partial quantity. If partial, the item will be split into two items and the original item will have a lower quantity and will be canceled. If this results in all items in a fulfillment group being canceled, the group will also be canceled. If this results in all fulfillment groups being canceled, the full order will also be canceled.

3. **createRefund** : Use this mutation to create a refund on an order payment.

4. moveOrderItems : Use this mutation to move one or more items between existing order fulfillment groups.
5. placeOrder : Create all authorized payments for a potential order
6. sendOrderEmail : A mutation that compiles and server-side renders the email template with order data, and sends the email
7. splitOrderItem : Use this mutation to reduce the quantity of one item of an order and create a new item for the remaining quantity in the same fulfillment group, and with the same item status. You may want to do this if you are only able to partially fulfill the item order right now.
8. updateOrder : Use this mutation to update order status, email, and other properties
9. updateOrderFulfillmentGroup : Use this mutation to update an order fulfillment group status and tracking information