# Privacy Policy for Another Diff

**Effective Date:** November 29, 2025

**Developer:** Wilson

## Introduction

Another Diff is a Chrome browser extension that allows users to compare JSON, XML and HTML data from two URLs with a VSCode-style side-by-side diff viewer. This privacy policy explains how the Extension handles data.

## Data Collection and Usage

### What Data We Collect

The Extension collects and processes the following data **locally in your browser only**:

1. **URLs**: The two URLs you select for comparison
2. **Fetched Content**: The JSON/XML/text content retrieved from your selected URLs
3. **Temporary State**: Your current comparison state (which URLs are selected, comparison results)

### How We Use This Data

- **URLs and Content**: Used solely to perform the comparison you requested and display results
- **Temporary State**: Stored in Chrome's session storage to maintain your workflow across popup opens/closes

### What We DO NOT Collect

- We **do not** collect, store, or transmit any personal information
- We **do not** track your browsing history
- We **do not** send any data to external servers or third parties
- We **do not** use analytics or tracking tools
- We **do not** store your data permanently

## Data Storage

All data is stored **locally in your browser** using Chrome's session storage API:

- **Storage Location**: Your browser's session storage (temporary)
- **Storage Duration**: Data is automatically cleared when you close your browser
- **Encryption**: Chrome handles encryption of session storage according to browser security standards

## Data Transmission

The Extension fetches content from URLs you explicitly select:

- **Transmission**: HTTPS/HTTP requests are made directly from your browser to the URLs you choose
- **Security**: We recommend using HTTPS URLs for secure data transmission
- **No Third Parties**: No data is sent to the Extension developer or any third-party servers

## Permissions Explanation

The Extension requires the following permissions:

- **storage**: To temporarily save selected URLs and comparison state in session storage
- **tabs**: To create a new tab for the comparison view and read the current tab's URL
- **activeTab**: To automatically capture the URL of the active tab when you click the extension icon
- **host_permissions (http://*/* and https://*/*)**: To fetch content from any URLs you choose to compare
- **notifications**: To show notifications when you reset the extension or encounter errors
- **contextMenus**: To provide a right-click "Start Over" option on the extension icon

## Data Retention

- **Session Duration**: Data is retained only during your browser session
- **Automatic Deletion**: All data is automatically deleted when you close your browser
- **Manual Deletion**: You can clear data anytime by clicking "Start Over" in the extension

## User Rights

You have complete control over your data:

- **Access**: All data is stored locally and accessible only to you
- **Deletion**: Close your browser or click "Start Over" to delete all data
- **Control**: You choose which URLs to compare; the Extension never acts without your explicit action

## Third-Party Services

The Extension **does not use any third-party services**, analytics, or tracking tools. The only external communication is fetching content from URLs that **you explicitly select**.

## Children's Privacy

The Extension does not knowingly collect data from children under 13. The Extension is intended for developers and professionals comparing API responses and structured data.

## Changes to This Policy

We may update this privacy policy from time to time. The "Effective Date" at the top indicates when the policy was last updated. Continued use of the Extension after changes constitutes acceptance of the updated policy.


## Contact

If you have questions about this privacy policy or the Extension's data practices, please contact:

**Developer:** Wilson

**Contact:** wangwscn@gmail.com

---

## Summary

**In short:** Another Diff processes data entirely within your browser. No data is collected, tracked, or transmitted to external servers. All comparison data is temporary and automatically deleted when you close your browser.
