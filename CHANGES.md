# Changes — Personal spaces and cross-device plan access

## Implemented
- Added first-run personal-space gate for direct app visits, plus access through name + private access code.
- Added creation flow that uses `crypto.getRandomValues()` to generate a unique 20-character code from an unambiguous 32-character alphabet, then shows a copyable, grouped code.
- Added Firestore personal-space document storage and merge-based sync for plan lists, local-only plans, history, and travel-mate presets.
- Existing shared plans remain in `/shares/{shareCode}`; personal spaces store references to those shared plans, not duplicate plan content. On load, Plyt fetches the latest shared document.
- Added code display/copy and switching spaces from the existing Your name dialog.
- Pre-filled the Firebase project ID and Web API key provided for the existing Plyt Firebase project, so fresh devices can use personal-space access without manually configuring Firebase first.
- Added `personalSpaces/{accessCode}` access rules in the included Firestore Rules.

## Verification performed
- JavaScript syntax check passed for the desktop and mobile HTML.
- Browser smoke test passed for: first-run gate, generating/saving a code, creating a plan, syncing it into the Firestore profile document, and signing out/switching back into the same space with name + code to see the plan again.
- Real-device Firebase reads/writes were not executed in this development environment. Publish the included Rules in the actual Firebase Console and perform the acceptance test on Vercel before relying on it.

## Important notes
- Access code is the credential; the name is checked for consistency and is not a security secret.
- The code is shown at creation and remains viewable/copyable from the Your name dialog while signed in. Keep it in a password manager. No email-based recovery is implemented.
- One personal-space document is capped at 900 KB by app logic and Firestore Rules. Shared plan details remain in separate share documents.
