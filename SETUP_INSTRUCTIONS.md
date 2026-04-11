# VLA Sales App — Google Sheets Setup (One-Time, 10 minutes)

## Step 1: Create the Google Sheet
1. Go to sheets.google.com (signed in as absotechhelp@gmail.com)
2. Create a new spreadsheet
3. Name it: "VLA Sales App — Quote Log"
4. Add these headers in Row 1 (copy exactly):
   Timestamp | Agent | QuoteRef | ClientName | ClientEmail | ClientPhone | Gender | Occupation | OccClass | DOB | ANB | QuoteDate | Term | Frequency | SumAssured | BasicPremium | DABPremium | PolicyFee | TotalPremium | ProjectedMaturity | DABIncluded | Mode

## Step 2: Create the Apps Script webhook
1. In the Sheet, click Extensions → Apps Script
2. Delete all existing code
3. Paste this code:

```javascript
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    var data = JSON.parse(e.postData.contents);
    sheet.appendRow([
      data.Timestamp, data.Agent, data.QuoteRef,
      data.ClientName, data.ClientEmail, data.ClientPhone,
      data.Gender, data.Occupation, data.OccClass,
      data.DOB, data.ANB, data.QuoteDate,
      data.Term, data.Frequency, data.SumAssured,
      data.BasicPremium, data.DABPremium, data.PolicyFee,
      data.TotalPremium, data.ProjectedMaturity,
      data.DABIncluded, data.Mode
    ]);
    return ContentService.createTextOutput(JSON.stringify({status:'ok'}))
      .setMimeType(ContentService.MimeType.JSON);
  } catch(err) {
    return ContentService.createTextOutput(JSON.stringify({status:'error',msg:err.toString()}))
      .setMimeType(ContentService.MimeType.JSON);
  }
}

function doGet(e) {
  return ContentService.createTextOutput(JSON.stringify({status:'VLA Sales App webhook active'}))
    .setMimeType(ContentService.MimeType.JSON);
}
```

4. Click Save (💾)
5. Click Deploy → New Deployment
6. Type: Web app
7. Description: VLA Sales App v1
8. Execute as: Me
9. Who has access: Anyone
10. Click Deploy → Copy the Web App URL

## Step 3: Paste the URL into the app
- Open vla_app.html
- Find the line: const SHEETS_URL = 'PASTE_YOUR_URL_HERE';
- Replace PASTE_YOUR_URL_HERE with the URL you copied
- Save the file

## Data Protection Notes
- Data is stored in YOUR Google account only
- Share the Sheet with the Sales Manager's Google account (View only)
- No third party has access
- To revoke access: delete the deployment in Apps Script
