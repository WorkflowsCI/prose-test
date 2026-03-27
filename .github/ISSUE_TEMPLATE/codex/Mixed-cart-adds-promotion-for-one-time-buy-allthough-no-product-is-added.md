### Shopware Version

6.7.X

### Affected area / extension

Platform(Default)

### Actual behaviour

If you define a promotion for the cart and add a subscription for a product, the promotion is aded with the amount of the subscribed product.

This promotion value is added to every one time product.

It should be possible to add a restriction, that promotion eligible products in a subscription do not trigger a promotion for one time buys in a mixed cart.

<img width="1503" height="1030" alt="Image" src="https://github.com/user-attachments/assets/77ef5428-0f45-488a-806b-e1a0b9089bec" />

### Expected behaviour

A promotion for one time buys should only be used for one time buys.

### How to reproduce

- Define a simple promotion of 10% for all products without any restriction.
- define a subscription plan an add it to a product.
- activate mixed carts
- go to the frontend and add a subscription in the cart.

You should see the promotion in the one time buy frame allthough no one time buy product is added.
