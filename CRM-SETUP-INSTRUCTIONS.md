# CRM Setup — Website Enquiry Form → Your Google Sheet

Your new **enquiry.html** page collects leads (name, phone, service, message).
Follow these steps ONCE to connect it to your CRM Google Sheet.

## Step 1: Open your CRM Google Sheet

Open the Google Sheet you use as CRM (or create a new one named "Star eSevai CRM").

## Step 2: Open Apps Script

In the sheet menu: **Extensions → Apps Script**

## Step 3: Paste this code

Delete anything in the editor and paste:

```javascript
function doPost(e) {
  var ss = SpreadsheetApp.getActiveSpreadsheet();
  var sh = ss.getSheetByName('Leads');
  if (!sh) {
    sh = ss.insertSheet('Leads');
    sh.appendRow(['Date & Time', 'Name', 'Phone', 'Service', 'Message', 'Status']);
    sh.getRange('A1:F1').setFontWeight('bold').setBackground('#1e3a8a').setFontColor('#ffffff');
  }
  sh.appendRow([
    new Date(),
    e.parameter.name || '',
    e.parameter.phone || '',
    e.parameter.service || '',
    e.parameter.message || '',
    'New'
  ]);
  return ContentService.createTextOutput('OK');
}
```

Click the **save** icon (💾).

## Step 4: Deploy as Web App

1. Click **Deploy → New deployment**
2. Click the gear ⚙️ next to "Select type" → choose **Web app**
3. Set:
   - Description: `Website CRM`
   - Execute as: **Me**
   - Who has access: **Anyone**
4. Click **Deploy**
5. Click **Authorize access** → choose your Google account → Advanced → Go to project (unsafe is normal here, it is your own script) → Allow
6. **Copy the Web app URL** (looks like `https://script.google.com/macros/s/XXXXX/exec`)

## Step 5: Put the URL in the website

Open `enquiry.html`, find this line near the bottom:

```javascript
var SCRIPT_URL = "PASTE_YOUR_APPS_SCRIPT_URL_HERE";
```

Replace the text between quotes with your copied URL. Save.

(Or just send the URL to Claude and it will be inserted for you.)

## Step 6: Upload to GitHub

Upload the updated `enquiry.html` and all other changed files to
**github.com/instetn/Star-E-sevai** (drag onto github.com/instetn/Star-E-sevai/upload/main → Commit changes).

## How it works after setup

- Customer fills the form on kovaihub.in → a new row appears in your "Leads" sheet instantly with date, name, phone, service and message, marked **New**.
- Change Status to "Called", "In Progress", "Done" etc. as you work — that's your CRM.
- Until the URL is pasted, the form safely falls back to opening WhatsApp instead, so nothing breaks.
