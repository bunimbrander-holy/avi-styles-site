# Studio pilot status - 2026-10-17

This is the current resume record. Read it before the older ADMIN-BUILD.md. Phase 1 is in progress, not complete.

## Live milestone
- Public content-connected pilot: https://avinoam-studio-pilot.pages.dev/
- Owner manager login: https://avinoam-studio-pilot.pages.dev/studio-admin.html
- Code release: e473d384a6cc36d93f5d20935fd7bfaaf8a08e02 (plus 2026-10-17 pack update below)
- The original GitHub Pages public site is unchanged. Do not promise that admin changes reach that older URL.
- Cloudflare Free Pages: build `node studio-build.mjs`, output `dist`.

## Editable source and rebuild
`studio-build.mjs` is an integrity-checked, dependency-free build/source pack. Run `node studio-build.mjs --extract` for editable files in `studio-source/`. Run `node studio-build.mjs` for deployment output in `dist/`. Do not edit a minified output and forget the source pack. For the next development pass, prefer normal multi-file source commits through a working Git route; preserve this pack as a recovery artifact. Pack regenerated 2026-10-17 twice: first with migration 004 and the corrected seed (photo rows normalized to [source, caption]); then with the real approved package content and the public bundle without the illustrative-draft marker.

## Backend actually applied
Isolated Supabase Free project `xbrgftxnnpmfnfekzupb`, no POS data/keys/access. Signup and anonymous sign-in disabled. Seed has 100 photos across 9 categories, 4 package types and 3 explicitly sample reviews. Existing prices and testimonials remain draft/sample content. Migrations 001-004 applied. 004 (2026-10-17): `studio_publish` now uses `IS DISTINCT FROM` for the expected-version check so a null expected version cannot skip the conflict guard, and draft+published photo rows were normalized to 2 elements ([source, caption]) because the seed's 3/4-element rows failed the 003 validator and blocked every save.

## Owner account (done 2026-10-17, recreated 2026-10-18)
- Sole owner user: bunimbrander@gmail.com in `studio_owners`. 2026-10-18: the original user (UUID 73e599f0) was deleted and recreated (new UUID d161f2c8-571d-4355-aea4-dc689256c1f2) because Ben could not retrieve the password/authenticator from the vault; the password is the one Ben chose and saved via a vault request link, and the old authenticator factor died with the old user. Ben re-enrolls MFA on his own phone at first login; the admin setup screen now also shows a copyable setup key next to the QR for phone enrollment. The login form minimum password length was lowered from 12 to 8 at Ben's request (the 12 rule lived only in the app; the Supabase auth floor was already 6). MFA stays mandatory.
- Password lives only in Ben's vault entry "Avinoam Studio admin (Ben)". Never put it in chat, issues, commits or URLs.
- TOTP authenticator enrolled and verified at aal2 through the app's own setup screen; the setup key is stored in the same vault entry. A fresh sign-in (new browser tab) using vault-filled password + authenticator code was tested end to end.
- Avinoam has no login. Add him only after Ben says so. A temporary non-owner test user was created for rejection tests and deleted afterwards.

## Real package content loaded (2026-10-17)
- Ben approved the real package content verbatim. Loaded through the tested app flow: restore into draft, save, preview, publish - no number changed.
- Live packages now: Family/kids (outdoor, 2h) 700 / 1500 starred (album shown as 800 add-on); Bar mitzvah/events (hall, up to 3h) 1200 / 3200 starred / 4500, with the note that rabbi/social coverage is priced per event via Avinoam; Couples album 1700 incl. digital album. 2026-10-18: the Newborn package was removed at Ben/Avinoam's request (no newborn packages exist); the Newborn photo category in the gallery stays.
- The per-package "illustrative draft" marker and the site-wide "prices are illustrative" footer line were removed from the public bundle source; the footer now reads "prices subject to coordination with Avinoam; commercial photography per quote" (outdoor dropped because outdoor family shoots now have listed prices). Deployed and verified live 2026-10-17.
- Live ZIP backup download + restore round trip PASSED: downloaded the backup, replaced only the packages section, restored into the draft, saved and published.
- Sample reviews remain sample content. The old "sample prices" warning now applies only to reviews/testimonials.

## Tested live, authenticated (2026-10-17)
- Owner sign-in + MFA challenge + draft load (read) at aal2.
- Save draft (caption edit; also with an `asset://` upload present) - server validation passes.
- Upload from file picker: JPEG/WebP resize to 1920px WebP and upload to private `studio-drafts`. (Cloud-browser file staging broke async reads with "source image cannot be decoded"; the same file through the app's real handler with stable bytes passes. Retest once on Ben's phone.)
- Preview handshake: save-then-preview posts STUDIO_PREVIEW_DATA to the iframe; Publish stays disabled until the preview acks the current version.
- Publish: confirm dialog, images copied to `studio-public`, published row replaced; success message shown.
- Anonymous checks: published content 200; draft/history/owners/save/publish 401; published image signs and downloads; unpublished/missing image returns no signed URL; direct public-object URL on the private bucket returns 400.
- Concurrent sessions: two signed-in tabs; first tab's save after the second tab saved fails with "Draft changed in another session. Reload before saving." and keeps its edits unsaved.
- Non-owner rejection: a fresh non-allowlisted user gets owner_check=false, empty draft read via RLS, "Not authorized" from save RPC, and no draft-media signing.
- Draft and published content re-verified clean after tests (100 photos, original captions).

## Audit findings resolved 2026-10-17
- Publish null-version guard fixed (004).
- Seed vs validator mismatch fixed (data normalized; seed-content.json corrected in the pack).
- Fixed-photo audit: the only hardcoded photo outside managed content is photo-076, Avi's brand avatar used in the header ring/about/footer; it is not a gallery photo, so gallery removals cannot orphan it. The gallery, photo-sheet menu and story strip all render from published content. Note: the bundled fallback content (used only if the backend is unreachable) was refreshed 2026-10-17 so its packages match the approved real content; its photos remain the original set until the next full site rebuild.

## Still open before Phase 1 can be called complete
- Live history-restore round trip (load an old version into the draft) - not yet run authenticated.
- HEIC upload on a real iPhone/browser that cannot decode HEIC (fallback message) and a real large-phone-photo upload.
- Add-to-home-screen install on Ben's actual phone.
- Slow/offline/error flows (service worker, backend down -> bundled fallback).
- Orphan cleanup: one test image (30d88a06-a591-4926-8633-28014d6f8504.webp) remains in `studio-public` from the publish test; it is unreferenced so anonymous signing is denied, but the bytes can be deleted from storage.
- Sample reviews/testimonials stay until Ben supplies real ones. Package prices are now real and approved.
- No payments, booking calendar, paid upgrade, card entry or recurring monitor is authorized.
