# Permission Justifications for Another Diff

This document provides detailed justifications for each permission requested by the Another Diff Chrome extension. This is required for Chrome Web Store submission.

---

## Permissions Summary

```json
"permissions": [
  "storage",
  "tabs",
  "activeTab",
  "notifications",
  "contextMenus"
],
"host_permissions": [
  "https://*/*",
  "http://*/*"
]
```

---

## Detailed Justifications

### 1. `storage` Permission

**Why we need it:**
The extension must temporarily save the two URLs selected by the user and their comparison state across popup opens/closes.

**Specific usage:**
- Save URL 1 and URL 2 selected by the user
- Store fetched JSON/XML data for the comparison view
- Maintain extension state (idle, url1-saved, url2-saved, ready, comparing)
- Store comparison tab ID to track the active comparison window

**Code reference:**
- `src/utils/storage.ts` - Storage helper functions
- `src/background/service-worker.ts` - Stores URLs and fetched data using `chrome.storage.session`
- `src/popup/PopupApp.tsx` - Reads stored state to display current selection

**Storage type:**
We use `chrome.storage.session` (temporary storage cleared when browser closes), not `chrome.storage.local` (persistent).

**User benefit:**
Users can close the popup and return to it without losing their selected URLs. This enables a smooth, interrupted workflow.

---

### 2. `tabs` Permission

**Why we need it:**
The extension needs to:
1. Get the current tab's URL to auto-save it when user clicks the extension icon
2. Create a new tab to display the comparison results

**Specific usage:**
- **Get current URL**: When user opens the popup, we read the active tab's URL using `chrome.tabs.query({ active: true, currentWindow: true })`
- **Create comparison tab**: When both URLs are selected, we open a new tab with `chrome.tabs.create({ url: comparePageUrl })`

**Code reference:**
- `src/popup/PopupApp.tsx` line 55-66 - Gets current tab URL
- `src/background/service-worker.ts` line 309 - Creates comparison tab

**User benefit:**
Users don't have to manually copy-paste URLs. The extension automatically captures the current page URL and opens results in a dedicated tab.

---

### 3. `activeTab` Permission

**Why we need it:**
To automatically read the URL of the currently active tab when the user clicks the extension icon, enabling the one-click URL capture workflow.

**Specific usage:**
- Read `tab.url` property of the active tab
- Only when user explicitly clicks the extension icon (not background activity)

**Code reference:**
- `src/popup/PopupApp.tsx` line 57 - `await chrome.tabs.query({ active: true, currentWindow: true })`

**User benefit:**
Enables the core "one-click capture" feature. Users simply navigate to a URL and click the extension icon to select it, rather than manually entering URLs.

---

### 4. `notifications` Permission

**Why we need it:**
To provide user feedback for important actions like resetting the extension or error conditions.

**Specific usage:**
- Show notification when user right-clicks and selects "Start Over" from context menu
- Notify user of successful reset: "Reset Complete - Ready to compare new URLs"

**Code reference:**
- `src/background/service-worker.ts` lines 54-60 - Creates notification after context menu reset

**User benefit:**
Clear visual feedback confirms actions completed successfully, especially when the popup is closed.

---

### 5. `contextMenus` Permission

**Why we need it:**
To add a convenient "Start Over" option in the right-click menu on the extension icon.

**Specific usage:**
- Create a single context menu item: "Start Over"
- Only appears when right-clicking the extension icon (not on web pages)

**Code reference:**
- `src/background/service-worker.ts` lines 39-44 - Creates context menu on installation
- `src/background/service-worker.ts` lines 48-61 - Handles context menu click

**User benefit:**
Provides a quick way to reset the extension and start a new comparison without opening the popup.

---

### 6. `host_permissions` - `https://*/*` and `http://*/*`

**Why we need it:**
The core functionality of the extension is to fetch JSON/XML content from any two URLs the user wants to compare. We must be able to make HTTP requests to any URL the user explicitly selects.

**Specific usage:**
- Fetch content from the first URL selected by the user
- Fetch content from the second URL selected by the user
- Only URLs explicitly chosen by the user are fetched (never automatic or background fetching)

**Code reference:**
- `src/background/service-worker.ts` lines 72-116 - `fetchContent()` function makes fetch requests to user-selected URLs

**How it works:**
1. User navigates to URL 1 and clicks extension icon → URL 1 saved
2. User navigates to URL 2 and clicks extension icon → URL 2 saved
3. Extension fetches both URLs using standard `fetch()` API
4. Content is compared and displayed locally in the browser

**Security measures:**
- Only http:// and https:// protocols allowed (validated in code)
- No arbitrary code execution
- Content is processed entirely locally (never sent to external servers)
- User must explicitly select each URL (no automatic fetching)

**Why broad permissions:**
We need access to any HTTP/HTTPS URL because:
- Users may want to compare any two APIs, services, or endpoints
- We cannot predict which domains users need to access
- Limiting to specific domains would break the extension's core functionality

**Data handling:**
- Fetched data is stored temporarily in session storage
- Automatically cleared when browser closes
- Never transmitted to third parties
- User has full control via "Start Over" button

**User benefit:**
Users can compare responses from any APIs, endpoints, or services they have access to, making the extension universally useful for API testing and debugging.

---

## Summary Table

| Permission | Purpose | User Interaction | Data Handling |
|------------|---------|------------------|---------------|
| `storage` | Save selected URLs & comparison state | Automatic (session only) | Temporary, cleared on browser close |
| `tabs` | Get current URL & open comparison tab | User clicks icon | Read-only URL access |
| `activeTab` | Read active tab URL | User clicks icon | Read-only URL access |
| `notifications` | Show reset confirmation | User triggers reset | No data stored |
| `contextMenus` | Provide "Start Over" option | User right-clicks icon | No data stored |
| `host_permissions` | Fetch user-selected URLs | User explicitly selects URLs | Temporary storage, local processing only |

---

## Privacy Guarantees

1. **No Background Activity**: Extension only acts when user explicitly clicks the icon
2. **No Tracking**: No analytics, no telemetry, no user behavior tracking
3. **No Third-Party Servers**: All data processing happens locally in the browser
4. **Temporary Storage**: All data automatically deleted when browser closes
5. **User Control**: User can clear all data instantly via "Start Over"
6. **Open Source**: Full source code available for inspection

---

## Chrome Web Store Reviewer Notes

**Minimal Permissions**: We request only the permissions absolutely necessary for core functionality. We do not request permissions for future features or "nice to have" functionality.

**User Transparency**: Permission usage is clearly documented in the privacy policy and visible in the extension's behavior (only acts when user clicks).

**Security**: No remote code execution, no eval(), no external scripts. All logic is self-contained in the extension package.

**Manifest V3 Compliant**: Extension uses service workers (not background pages) and follows all Manifest V3 security requirements.
