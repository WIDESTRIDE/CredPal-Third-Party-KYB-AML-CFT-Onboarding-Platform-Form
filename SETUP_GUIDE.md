# CredPal Partner Onboarding – setup guide

You have two files:

- **CredPal_Partner_Onboarding_Form.html**: the form (logo already embedded)
- **Code.gs**: the Google Apps Script backend

## 1. Create the Google Sheet and script

1. Create a new Google Sheet, e.g. "CredPal Third-Party KYB Register".
2. Go to **Extensions > Apps Script**.
3. Delete what's in `Code.gs` and paste in the contents of the **Code.gs** file.
4. At the top, in `CONFIG`, set `COMPLIANCE_EMAILS` to the people who should get new-submission alerts (comma-separated).
5. Save, choose **setup** in the function dropdown and click **Run**. Approve the permissions when Google asks.

`setup()` does the following:
- creates the tabs: Applications, Shareholders, UBOs, Directors, Subcontractors, Licences, Documents, Audit Log, Risk Rules
- creates a Drive folder called "CredPal Partner Onboarding – Documents" (each application gets its own subfolder)
- turns on a daily 8am licence-expiry check (alerts at 90, 60, 30 and 0 days)

## 2. Deploy as a Web App

1. **Deploy > New deployment > Web app**
2. Execute as: **Me**. Who has access: **Anyone**.
3. Copy the Web App URL.

## 3. Host the form (pick one)

**Option A: host it anywhere (GitHub Pages, your website)**
Open the HTML file, find `const SCRIPT_URL = 'PASTE_YOUR_WEB_APP_URL_HERE';` and paste your Web App URL there.

**Option B: let Apps Script serve it**
In the Apps Script editor, click **+ > HTML**, name it `Index` (capital I), and paste in the whole HTML file. Redeploy (Deploy > Manage deployments > Edit > New version). The Web App URL now opens the form itself, and SCRIPT_URL can stay as it is.

## How it behaves

- **Status on arrival:** Low risk → Submitted, Medium → Compliance Review, High → EDD Required, Critical → Compliance Review.
- **Risk rules** live in the "Risk Rules" tab. Compliance can change a factor's level (Low/Medium/High/Critical) or set Active to "No" without touching code. The rating is the highest level triggered; the score is the sum of points, useful for sorting.
- **High-risk countries** are listed in `CONFIG.HIGH_RISK_COUNTRIES`. Keep it in line with the current FATF lists.
- **Screening status** for UBOs, directors and shareholders starts as "Pending Review". The form doesn't run sanctions/PEP/adverse-media checks itself; record those results in the sheet or connect a screening provider later.
- **Audit log:** every submission is logged. Any manual edit on the Applications tab (status, risk rating, notes) is logged with who changed it, the old value and the new value. Setting Status to Approved / Conditionally Approved / Rejected stamps the Decision Date.
- **Files:** PDF, JPG or PNG, 5 MB each, 30 MB in total per application. Documents are private to the script owner's Drive. Share the main folder with the Compliance team.

## If you change the script later

After any edit to Code.gs: **Deploy > Manage deployments > Edit > Version: New version > Deploy**. The URL stays the same.
