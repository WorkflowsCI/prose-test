### Shopware Version

6.7.8.0

### Affected area / extension

Platform(Default)

### Actual behaviour

When a video element with config "Show controls" disabled exists on a page, that element tries to display a playbutton in the storefront, but the button image-url returns a 404 error.

<img width="1525" height="955" alt="Image" src="https://github.com/user-attachments/assets/dc0a921a-5123-485f-968f-c73c8545e592" />

### Expected behaviour

Playbutton image gets loaded and does not display an error image.

### How to reproduce

1. Create a CMS Page
2. Add a video Block with a video
3. Set config "Show controls" disabled
4. Open CMS Page in Storefront
