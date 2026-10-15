# LeafScout — Legal

Privacy Policy and Terms of Use for **LeafScout** (`com.fazogir.plantai`), published by Fazogirapps.

Served via GitHub Pages:

| Document | URL |
|---|---|
| Landing page | https://elyorberdiev.github.io/planty-legal/ |
| Privacy Policy | https://elyorberdiev.github.io/planty-legal/privacy.html |
| Terms of Use | https://elyorberdiev.github.io/planty-legal/terms.html |

These URLs are submitted to App Store Connect and the Google Play Console, and are linked from the app's Settings screen. **Do not rename these files** — changing a path breaks the links already registered with the stores.

## Updating

Edit the HTML, bump the "Last Updated" date in the file header, then commit and push. GitHub Pages redeploys automatically within a minute or two.

For material changes, also update the effective date and give users notice in-app.

## Claims that depend on the app's actual behaviour

These documents describe what the app does today. If a feature changes, the corresponding text must change too — an inaccurate privacy policy is worse than none.

| Text says | True only while |
|---|---|
| "no email, name, or password" | Auth stays anonymous-only. Adding sign-in requires rewriting §2 of the Privacy Policy. |
| "photos are sent to Google's Gemini API" | The AI stays Gemini. Switching provider requires updating §5 and the third-party table. |
| "Firebase Crashlytics and Analytics" | These are actually shipped (planned in F4-01). Remove the row if they are not in the released build. |
| "server function runs in europe-west1" | The Cloud Function region is unchanged. |
| "limit resets at midnight UTC" | The server keeps counting by UTC day (`dayKeyUTC` in `functions/index.js`). |
| "Settings → Delete Account" erases cloud data | The real deletion flow is implemented (F2-02). **Do not publish before that ships** — Apple rejects apps whose deletion is cosmetic. |
| "AI Doctor requests count towards the same allowance" | `checkAndConsumeScan` is called regardless of mode. |
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
