### Shopware Version

6.7.8.1

### Affected area / extension

Extension:Commercial

### Actual behaviour

If the store has a discount promotion that checks for a customer with a Tag, an offer will no longer be generated once the customer has logged in to the frontend at least once since the promotion was created.

### Expected behaviour

The quote can be prepared regardless of any ongoing discount promotions.

### How to reproduce

1. Create a discount promotion
2. Set up the discount promotion without a promo code
3. Assign a customer rule to the discount promotion
4. The rule's condition is as follows:  
   “Customer with tag / Is one of / XY”
5. Set the discount to, for example, 10%.
6. Add the tag to a customer
7. Go to “Log in as customer” for that customer and log in with the customer on the front end
8. Go to the admin panel and try to create an offer for the customer.
9. An error occurs
