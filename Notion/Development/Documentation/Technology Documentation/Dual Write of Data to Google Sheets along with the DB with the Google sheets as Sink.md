---

---

Overview:

The idea is to persist the same data in two independent storage systems during a single request lifecycle.  The system guarantees that data is always persisted in the primary database.

Google Sheets acts as a best-effort sink for secondary visibility and reporting. A failure in the sink does not affect the correctness or availability of the system.

This ensures:

Flow 

`Client Request`

`Controller (bridgecontactform.controller.js)
           |`

`My SQL Write (Primary)
           |`

`Google Sheets Append (Sink)`

Google Sheets Sink Service

googleSheet.service.js this is the main logic which encapsulates all Google Sheets logic, acting as sink writer.

Responsibilities:


services/googleSheet.service.js (code)

```javascript
const { google } = require("googleapis");
const { appKeys } = require("../config/init");
const LOGGER = require("../utils/logger.utils");


const auth = new google.auth.GoogleAuth({
    credentials: {
        client_email: appKeys.apiKeys.google_service_account,
        private_key: appKeys.apiKeys.google_private_key.replace(/\\n/g, "\n"),
    },
    scopes: ["https://www.googleapis.com/auth/spreadsheets"],
});

const sheets = google.sheets({ version: "v4", auth });

async function appendContactForm(data) {
    try {
            await sheets.spreadsheets.values.append({
            spreadsheetId: appKeys.apiKeys.google_sheet_id,
            range: "Sheet1!A:E",
            valueInputOption: "USER_ENTERED",
            requestBody: {
                values:[
                    [
                        data.name,
                        data.email || " ",
                        data.phone || " ",
                        data.description || " ",
                        new Date().toISOString(),
                    ]
                ],
            },
        });
        LOGGER.info("Google sheet updated successfully");
    } catch (error) {
        LOGGER.error("Failed to update Google Sheet: ", error);
    }
}

module.exports = { appendContactForm };

```

The code uses the GoogleAuth (service account authentication to authenticate the service account)

The code writes to the sheet based on the Sheet Id, provided in the environment, (note: The service account (client_email) should be given the Editor access in the Google Sheets) to enable the writing to it, it only works with the specified scope provided from the [Sheets API](https://developers.google.com/workspace/sheets/api/scopes) for our requirement we utilized the following scope: [**https://www.googleapis.com/auth/spreadsheets**](https://www.googleapis.com/auth/spreadsheets)** **which as the following abilities: see, edit, create & delete. 

appendContactForm(data) function

The appendContactForm function is responsible for writing the data (parameter) into the Google Sheets (Sink) which is the implementation of Dual Write.

The function: appends a new row to Google Sheet, Does not affect the primary database transaction.

The function is made asynchronus as it performs an external network request, interacts with the Google Sheets API, so it must wait for google response before logging success or failure.

Then the passed parameter data({name, email, phone, description}) is appended to the sheets using the sheets.spreadsheets.values.append() method this calls the Google Sheets API, uses the authenticated sheets using the GoogleAuth.  The data is written only to the specific sheet which is identified using the Sheet ID which is provided in the environment.

In the end we Logged the info to convey the successful write to sink. and the specific error log if anything happened.
