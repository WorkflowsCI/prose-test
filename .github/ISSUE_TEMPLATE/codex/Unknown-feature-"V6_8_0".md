### Shopware Version

6.6.10.10

### Affected area / extension

Platform(Default)

### Actual behaviour

On systems not in prod mode, a warning is thrown when `OrderConversionContext::setIncludeOrderDate()` is called:

```
User Warning: Unknown feature "V6_8_0"
```

This is because there is a call to `Feature::triggerDeprecationOrThrow()` with `'v6.8.0'` as the feature. This gets converted to `V_6_8_0`, when the existing feature flag is actually `V_6_8_0_0`, so it should be 'v6.8.0.0'.

### Expected behaviour

The existing feature flag should be used so that no warning is thrown.

### How to reproduce

Call the referenced core function.
