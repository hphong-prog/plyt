# Plyt — static deploy + live sharing

## Recommended free setup

- **Frontend:** Cloudflare Pages (static HTML; no server/build framework needed).
- **Shared data:** Firebase Cloud Firestore on the Spark no-cost plan.
- Supabase is not needed: Plyt already writes/reads Firestore documents through the Firestore REST API. Replacing it with Supabase would require changing the app's sync code.

## 1. Set up Firestore

1. Create a Firebase project and create a Cloud Firestore database.
2. Register a Web app in Project settings and note the **Project ID** and **Web API key**.
3. Open **Firestore Database → Rules**, paste the contents of `firestore.rules`, and Publish.
4. Do not use a blanket `allow read, write: if true` rule for the whole database.

**Sharing model:** Plyt uses a random share code in the invite link as the access token. Anyone who has an invite link can read and edit that shared trip. Do not store sensitive information in a shared plan. These rules intentionally grant access only to `/shares/{code}` and validate the document fields; they are not user-login-based access control.

## 2. Deploy the static site on Cloudflare Pages

1. Put `index.html`, `m.html`, `README.md`, and `firestore.rules` in the root of a GitHub repository (only the two HTML files are needed to serve the site; rules are published separately in Firebase Console).
2. In Cloudflare Dashboard, open **Workers & Pages → Create application → Pages → Import an existing Git repository**.
3. Select the repository. Use build command `exit 0` and build output directory `.`. Deploy.
4. Keep `index.html` and `m.html` in the same directory because the app switches between them based on screen size.

## 3. Verify live sharing

1. Open the deployed `*.pages.dev` URL.
2. In Plyt, open **Share plan → Set up cloud sync**; enter the Firebase Project ID and Web API key. Test the connection.
3. Choose **Start live sharing**, then copy/send the invite link to another device/browser.
4. Add or edit a stop on one device and confirm that the other device receives the change. Also test adding an expense in Budget.

If the connection test or live sync fails, check that Firestore exists, the rules above are published, and the Project ID/API key are correct.
