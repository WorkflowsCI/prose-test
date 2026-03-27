### Shopware Version

6.6.10.10 & 6.7.5.1

### Affected area / extension

Platform(Default)

### Actual behaviour

When a custom field is attached to a product (in this example a checkbox), and that checkbox is set in various languages, the evaluation both via dynamic product groups and rules does not work correctly in the storefront. The following behaviour is observed when the field is evaluated in a promotion, assuming DE as the default shop language:

Dynamic product group evaluated in rule:

| Discount applied in: | DE sales channel | EN sales channel |
| -------------------- | ---------------- | ---------------- |
| Checkbox set in DE   | ✅               | ✅               |
| Checkbox set in EN   | ❌               | ❌               |

Rule directly evaluating custom field of product:

| Discount applied in: | DE sales channel | EN sales channel |
| -------------------- | ---------------- | ---------------- |
| Checkbox set in DE   | ✅               | ✅               |
| Checkbox set in EN   | ❌               | ✅               |

### Expected behaviour

Expected result:

| Discount applied in: | DE sales channel | EN sales channel |
| -------------------- | ---------------- | ---------------- |
| Checkbox set in DE   | ✅               | ❌               |
| Checkbox set in EN   | ❌               | ✅               |

### How to reproduce

- Set up a shop with at least two products
- Set up two sales channels, one in the default language, one in another language, make sure all products are available in both
- Set up a custom field with a checkbox, make sure the checkbox is available in the cart
- Go to each product. In one product, set the language while editing the product to the default language and check the checkbox, on the other product do it in the other language
- Create a dynamic product group that checks for the checkbox being set:

<img width="1568" height="201" alt="Image" src="https://github.com/user-attachments/assets/c4d6f3f3-0bfc-4813-922b-d7e63837ef23" />

- Create two rules:
  - Check if an item in dynamic product group is part of the previously created group:
  - <img width="1628" height="223" alt="Image" src="https://github.com/user-attachments/assets/46c1550c-f971-47d1-9d30-4d64a4147f2b" />

  - Check if an item with custom field has the checkbox checked:

  - <img width="1629" height="191" alt="Image" src="https://github.com/user-attachments/assets/ffcd0117-a141-4c00-8ba1-d1430f1472e2" />

- Create a promotion that is valid for both sales channel and insert one of the above rules as the cart rule. Add a discount as well
- Navigate to each sales channel and add the products to the cart. Observe that the discount only applies to the product that has the checkbox in the main language set
- Exchange the rule in the promotion for the other one
- Check each sales channel again, the behaviour will have slightly changed, yet in the non-default sales channel it's still not correct, since the product that has the checkbox set in the default language will be discounted
