### Shopware Version

6.6.10.6

### Affected area / extension

Platform(Default)

### Actual behaviour

When creating a rule in the Rule Builder, the condition “Item quantity” combined with “All” and e.g. “Are lower than / equal to” 1 does not behave as expected.

It seems to always expect one product.

To me, this looks like a logical bug in how the “All” option is evaluated.

<img width="1135" height="342" alt="Image" src="https://github.com/user-attachments/assets/b44d4552-9bfa-4e36-a0a4-39364f528015" />

### Expected behaviour

It should be possible to validate whether a line item is in the cart, e.g., less than or equal to 1 time.

### How to reproduce

See screenshot.
