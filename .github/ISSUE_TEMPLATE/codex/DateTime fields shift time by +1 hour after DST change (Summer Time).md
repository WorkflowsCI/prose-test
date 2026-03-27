### Shopware Version

6.7.8.1

### Affected area / extension

Platform(Default)

### Actual behaviour

When entering a date including a time value in a DateTime field (e.g. “Release date”) after the switch to daylight saving time (DST), the saved/displayed time is automatically increased by one hour. The time is shifted by +1 hour (e.g. selecting 14:00 becomes 15:00 after closing the DateTimePicker).

This leads to inconsistent and confusing behavior for users. In order to reproduce this, switch to `(UTC +01:00) Europe/Berlin` in your profile settings.

<img width="1150" height="817" alt="Image" src="https://github.com/user-attachments/assets/c964dd12-5464-4f70-af8f-629031b12e1f" />

<img width="1151" height="753" alt="Image" src="https://github.com/user-attachments/assets/cd11a60f-d08e-44ff-9201-dc46b99bcfd6" />

### Expected behaviour

The selected time should remain unchanged (e.g. 14:00 stays 14:00).

### How to reproduce

1. Open profile settings, set timezone = (UTC +01:00) Europe/Berlin
2. Go to any entity with a DateTime field (e.g. product “Release date”)
3. Select a date before DST change (e.g. 2026-03-20 14:00) → works as expected (meaning 14:00 stays at 14:00)
4. Select a date after DST change (e.g. 2026-04-03 14:00)
5. Save or close the DateTimePicker

Result: Selected time (14:00) shifts to 15:00.
