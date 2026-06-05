# Google Sheets RSVP Setup (One-Time, ~5 mins)

## Step 1 — Create the Sheet
1. Go to https://sheets.google.com and create a new spreadsheet
2. Name it "Akhil & Abhini — RSVPs"
3. In **Row 1**, add these headers in columns A–G:
   `Timestamp | First Name | Last Name | Email | Attending | Guests | Message`

## Step 2 — Open Apps Script
1. In the sheet, click **Extensions → Apps Script**
2. Delete any code in the editor and paste this:

```javascript
function doPost(e) {
  try {
    const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    const data  = JSON.parse(e.postData.contents);
    sheet.appendRow([
      new Date().toLocaleString('en-IN'),
      data.fname,
      data.lname,
      data.email,
      data.attending,
      data.guests,
      data.message || ''
    ]);
    return ContentService
      .createTextOutput(JSON.stringify({ result: 'success' }))
      .setMimeType(ContentService.MimeType.JSON);
  } catch (err) {
    return ContentService
      .createTextOutput(JSON.stringify({ result: 'error', error: err.toString() }))
      .setMimeType(ContentService.MimeType.JSON);
  }
}
```

3. Click **Save** (Ctrl+S)

## Step 3 — Deploy as Web App
1. Click **Deploy → New deployment**
2. Click the gear icon next to "Type" → choose **Web app**
3. Set:
   - **Execute as:** Me
   - **Who has access:** Anyone
4. Click **Deploy**
5. Click **Authorize access** → choose your Google account → Allow
6. Copy the **Web app URL** (looks like: `https://script.google.com/macros/s/AKfyc.../exec`)

## Step 4 — Paste URL into index.html
In `index.html`, find this line near the top of the `<script>` tag:

```javascript
const RSVP_SHEET_URL = 'PASTE_YOUR_APPS_SCRIPT_URL_HERE';
```

Replace the placeholder with your copied URL. Done! 🎉
