# Studio pilot status - 2026-10-16

This is the current resume record. Read it before the older ADMIN-BUILD.md. Phase 1 is in progress, not complete.

## Live milestone
- Public content-connected pilot: https://avinoam-studio-pilot.pages.dev/
- Owner manager login: https://avinoam-studio-pilot.pages.dev/studio-admin.html
- Code release: e473d384a6cc36d93f5d20935fd7bfaaf8a08e02
- The original GitHub Pages public site is unchanged. Do not promise that admin changes reach that older URL.
- Cloudflare Free Pages: build `node studio-build.mjs`, output `dist`.

## Editable source and rebuild
`studio-build.mjs` is an integrity-checked, dependency-free build/source pack. This format was used because GitHub multi-file upload failed with HTTP 400, while its normal file editor worked. The committed build file was fetched and compared byte-for-byte with the locally tested file.

Run `node studio-build.mjs --extract` for editable files in `studio-source/`: admin source/bundle/CSS/PWA, public adapter/source/styles, all SQL migrations, content seed and initial build notes. Run `node studio-build.mjs` for deployment output in `dist/`. The original portfolio stays in the repo and is copied to the output. Do not edit a minified output and forget the source pack. For the next development pass, prefer normal multi-file source commits through a working Git route and a standard bundle build; preserve this pack as a recovery artifact.

## Backend actually applied
Isolated Supabase Free project `xbrgftxnnpmfnfekzupb`, no POS data/keys/access. Signup and anonymous sign-in disabled. Initial seed has all 100 photos across 9 categories, 4 package types and 3 explicitly sample reviews. The three text fields are connected. Existing prices and testimonials remain draft/sample content.

Migrations 001, 002 and 003 in the source pack were applied. 002 makes both storage buckets private and grants anonymous read/signing only for assets referenced in the currently published document. Images copied during Publish are not anonymously readable before the content transaction. Existing signed URLs can remain usable for up to one hour, like a cache. Anonymous readers never see drafts/history/owner records. Owner draft/media access requires an allowlist row and authenticator assurance level `aal2`.

## Tested
- Live anonymous public read: 200.
- Live anonymous draft/history/owner reads and save/publish/owner-check RPCs: 401.
- Live anonymous signing of an unpublished/missing asset: no signed URL returned.
- Live manager login page, public hero, 9-category gallery, PWA manifest, active service worker: inspected. No page JS errors.
- Local editor fixture at 320, 390, 768 and 1440px: six pages, no horizontal overflow/JS errors. Change tracking and category reorder pass.
- Client image test: 4000x3000 JPEG reencoded to 1920x1440 WebP; no EXIF/XMP in output.
- Local editor screenshots are fixture-based UI evidence, not authenticated live owner tests.

## Pending, important
No owner account has been created yet. Ben has been sent a secure request to choose a NEW unique app password; do not put any password in chat or this repo. Only Ben initially. Add Avinoam only after Ben says so. Do not send an unreviewed invitation email.

Once the password is saved: create the sole Ben user through Supabase's admin user UI, mark email confirmed without sending an invite, add only that user's UUID to `studio_owners`, sign into the live manager, enroll and verify authenticator safely. Never change his Google/dashboard password. Test live owner read/save/upload, private draft media, preview handshake, explicit publish, anonymous post-publish image signing, concurrent-save conflict, history restore and ZIP backup/restore. Test actual phone add-to-home-screen if available; manifest/worker checks alone do not prove device installation.

Audit follow-ups before calling Phase 1 complete: recheck RLS/grants/function definitions; change the publish expected-version comparison to `IS DISTINCT FROM` so a null expected version cannot skip the conflict guard; verify a nonowner session is rejected; confirm no gallery photo removal leaves that same photo in the fixed status-story set; test slow/offline/error flows and HEIC support/fallback; verify storage/backup size handling; keep published sample-content warnings.

No payments, booking calendar, paid upgrade, card entry or recurring monitor is authorized. Parent owns the next password-setup follow-up. Backend/public pilot can remain safely at the seeded version while that is pending.
