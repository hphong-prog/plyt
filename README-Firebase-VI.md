# Plyt — Personal access code update

## What changed
- Added a personal space in Firestore so a user's plan list and non-shared plans can be opened on desktop and iPhone using the same display name + generated access code.
- Shared plans are stored by reference in the personal-space document and continue using the existing `/shares/{shareCode}` documents and invite links.
- Fresh browsers show a personal-space entry screen. Existing spaces require both the saved name and the 20-character access code. New spaces generate a cryptographically random code and show it to the user to save.
- Firebase project ID and web API key are prefilled for the Plyt project shared by the owner.

## Important deployment steps
1. Upload `index.html` and `m.html` to the root of the existing GitHub repository; Vercel should redeploy automatically.
2. In Firebase Console → Firestore Database → Rules, replace the current rules with the included `firestore.rules` and Publish. This is required before personal spaces can be created/accessed.
3. Open the deployed app on the device that already contains plans. Choose **Create new personal space**, enter the person's name, and save the generated access code. Existing local plans in that browser will be linked to the new space.
4. On another device, open the same app and enter exactly the same name and access code. The app loads the personal plan list from Firestore.
5. Test edits on both devices, including one shared plan, before relying on this as the only copy of important data.

## Security model
This does NOT use Firebase Authentication or Gmail login. The random access code is a bearer secret. Anyone who obtains it can access the corresponding personal space; the name is a consistency check, not the security boundary. Save the code in a password manager. If it is lost and no device remains logged in, there is no email-based recovery flow. Invite links have a separate share code and can be shared independently.

The personal space is stored in one Firestore document with a 900,000-character application limit; shared plan content is referenced separately and remains in its existing share document. The app intentionally keeps Firestore collection listing disabled. Rules should not be opened with `allow read, write: if true`.
