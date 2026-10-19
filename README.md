# LeafScout — Legal

Privacy Policy and Terms of Use for **LeafScout** (`com.fazogir.plantai`), published by Fazogirapps.

Served via GitHub Pages:

| Document | URL |
|---|---|
| Landing page | https://elyorberdiev.github.io/planty-legal/ |
| Privacy Policy | https://elyorberdiev.github.io/planty-legal/privacy.html |
| Terms of Use | https://elyorberdiev.github.io/planty-legal/terms.html |
| Delete Your Account | https://elyorberdiev.github.io/planty-legal/delete-account.html (Play Data safety — «Delete account URL») |

These URLs are submitted to App Store Connect and the Google Play Console, and are linked from the app's Settings screen. **Do not rename these files** — changing a path breaks the links already registered with the stores.

## Updating

Edit the HTML, bump the "Last Updated" date in the file header, then commit and push. GitHub Pages redeploys automatically within a minute or two.

For material changes, also update the effective date and give users notice in-app.

## Claims that depend on the app's actual behaviour

These documents describe what the app does today. If a feature changes, the corresponding text must change too — an inaccurate privacy policy is worse than none.

| Text says | True only while |
|---|---|
| "anonymous by default; optional sign-in with Google or Apple" (§2) | `AccountService` (2026-10-19): only Google and Sign in with Apple, only `linkWith*` to the anonymous uid, email scope only for Apple. A new provider or data field (name, phone) requires updating §2, §8 and Data safety / App Privacy. |
| "Feedback you send" (§3.3): reason, optional comment ≤ 500, AI answer, language/version/platform, no photo; deleted with the account | `FeedbackRepository.payload` + `isValidFeedback` in `firestore.rules`; stored under `users/{uid}/feedback` so `deleteAccount`'s `recursiveDelete` erases it. A new field (e.g. the photo) requires updating §3.3 and Data safety / App Privacy. |
| "Delete account revokes Apple access" | `AccountService.revokeAppleIfLinked()` runs before server deletion. |
| "photos are sent to Google's Gemini API" | The AI stays Gemini. Switching provider requires updating §5 and the third-party table. |
| "Firebase Crashlytics and Analytics" | These are actually shipped (planned in F4-01). Remove the row if they are not in the released build. |
| "server function runs in europe-west1" | The Cloud Function region is unchanged. |
| "limit resets at midnight UTC" | The server keeps counting by UTC day (`dayKeyUTC` in `functions/index.js`). |
| "Settings → Delete Account" erases cloud data | The real deletion flow is implemented (F2-02). **Do not publish before that ships** — Apple rejects apps whose deletion is cosmetic. |
| "one AI Doctor check per calendar day … causes, treatment and prevention are part of Pro"; Pro checks "count towards the same allowance" | `counterFor` in `functions/scan_limit.js` (`FREE_DOCTOR_PER_DAY = 1`, Pro → shared `usage/scan`) and `doctorView` in `functions/doctor.js` (free answers carry no causes/treatment/prevention). |
| "Pro raises your allowance to 100 AI scans per calendar day" | `PRO_SCAN_LIMIT_PER_DAY` in `functions/scan_limit.js` stays 100. Changing it requires updating the summary and §8.2 of the Terms; the app copy follows `ScanUsageRepository.proLimit` (kept in sync by `test/scan_limit_sync_test.dart`). |

## Publishing checklist

1. Push this folder to a **public** repo (`planty-legal`). GitHub Pages needs public, or a paid plan.
2. Settings → Pages → Source: `main` branch, `/` root.
3. Wait for the first build, then open all three URLs and check them on a phone.
4. Put the URLs into:
   - `lib/features/profile/presentation/settings_page.dart` (Privacy Policy, Terms of Use)
   - App Store Connect → App Privacy → Privacy Policy URL
   - App Store Connect → App Information → EULA (or link the Terms)
   - Google Play Console → Store listing → Privacy Policy
