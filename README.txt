CHAOS INSPECT - HOSTING GUIDE (for IT and the Jira admin)

WHAT THIS IS
A static web app (no server code). 6 files. Works in any phone browser; installs to the home screen.

FOR IT
1. Upload all files in this folder to any static web host, in one folder, over HTTPS
   (for example inspect.chaosinc.com behind company single sign-on). HTTPS is required for home-screen install and offline use.
2. Put it behind company login so only CHAOS staff can open it.
3. Confirm Security is happy with where it is hosted. It stores inspection answers and photos on each phone only.
   Photos and notes are sent to Jira only as text; photos are NOT sent.
4. Do not put controlled-area details, safe room or duress locations, or any CUI in checklists.

FOR THE JIRA ADMIN
1. In project BOB: Project settings > Automation > Create rule > trigger "Incoming webhook".
   Work item data: "Provided in the webhook request". Save and copy the webhook URL.
2. Add action "Create work item": project BOB, issue type Task, then
     Summary = {{webhookData.summary}}
     Description = {{webhookData.description}}
     Priority = {{webhookData.priority}}
     Office Location = {{webhookData.site}}
     What's the problem? = {{webhookData.problem}}
3. The app sends this JSON: summary, description, priority (Critical, High, Medium, Low), site (an Office Location value),
   problem (Restock issue, Cleanliness issue, Plumbing issue, or empty), trade, inspection, area, inspector, ref.
4. Turn the rule on. Paste the webhook URL into config.js (webhook: '...') and re-upload that one file.
5. TEST: send one ticket from the app and check it in BOB. Not yet tested against your Jira:
   - the app posts the body as text/plain without waiting for a reply (browsers block a normal cross-site call to Jira), and
   - Office Location is a checkbox field; confirm Automation accepts a single value.
   If the rule does not read the body, tell Paul; a small proxy is the fallback.

KNOWN LIMITS
- The webhook URL is visible to anyone who can load the app. Keep the app behind company login, and rotate the URL if it leaks.
- Jira does not return the new ticket number to the app; the app shows a local ref (REF-####) and "Sent to Jira".
- Each phone keeps its own history. Jira is the shared record.
