### Shopware Version

6.7.8.1

### Affected area / extension

Platform(Default)

### Actual behaviour

I called support about this today, and they confirmed it over the phone.

Example:

I exported a profile and then immediately tried to import it again, using the original file.

The error message was:

`Die gewählte Datei "product_Technische-Felder-Produkte.csv" hat ein nicht unterstütztes Format. Bitte benutze eins der folgenden Formate: text/csv.`

It's also strange that this profile was only a CSV file with just one header row and without the database field that's needed.

The documentation states that it should be a JSON file.

### Expected behaviour

I can re-import a profile after editing it.

### How to reproduce

Create a profile in the admin panel or use one of the default profiles.

Download the template file and create a new profile immediately. Import this file in step 2.
