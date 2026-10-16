# Avinoam Studio pilot - durable build record

## Scope
Phase 1 only: phone-first installable content manager. No bookings/calendar or payments. No public signup. Ben is initial owner; Avinoam may be added only after Ben approves. No paid services, upgrades or card entry authorized.

## Isolation
Supabase project `avinoam-studio-pilot`, ref `xbrgftxnnpmfnfekzupb`, West EU/Ireland, Free plan. This is separate from the POS project. Never use POS tables, credentials, roles or permissions. Cloudflare Pages project `avinoam-studio-pilot` is connected only to this repo. Existing GitHub Pages site remains online.

Dashboard: https://supabase.com/dashboard/project/xbrgftxnnpmfnfekzupb
Public API host: https://xbrgftxnnpmfnfekzupb.supabase.co
Cloudflare dashboard: https://dash.cloudflare.com/8557a273842b7f58cfb7c5348cede296/pages/view/avinoam-studio-pilot

## Content/security contract
- `studio_draft` contains one private JSON content document with optimistic version UUID.
- `studio_published` is the only anonymous-readable content table.
- `studio_owners` is a backend allowlist, no client write privileges.
- `studio_history` records published source snapshots and actor, private to owner.
- `studio_save` and `studio_publish` are checked database functions, never raw client table writes.
- Owner access additionally requires JWT `aal2` (authenticator MFA). `studio_owner_check` exposes only own membership boolean before enrollment.
- `studio-drafts` bucket is private; drafts reference `asset://<uuid>.webp`.
- On explicit Publish, optimized image copies go into `studio-public`; publish refuses missing images. Published content resolves only those copies. Public portfolio files are intentionally public; do not upload private client galleries. Old published URLs can remain accessible/cached.
- Browser receives a publishable key only, no service-role/secret key.
- Authentication signup and anonymous sign-in disabled. No invitation email sent. Database password stored securely, never in repo.

Schema: `001-studio-schema.sql` plus MFA owner-check adjustment (kept in migration source before completion). Initial content comes from the approved live site, including all 100 gallery photos, existing sample prices/reviews and visible draft disclaimers.

## Pilot limitations
Free Supabase: 1GB storage, quotas and inactivity pausing. No automatic paid upgrade. Published current site falls back to its bundled last-known content if backend unavailable. Free pilot needs explicit manual document + media backup/restore testing; database backup does not cover photo bytes. No sensitive client/order/payment records belong in Phase 1.

## Build status
Backend tables/RLS/storage/version functions created and seeded. Anonymous read of published data passes; anonymous draft read returns 401. New public signup disabled. App UI and final integration are in progress. Nothing in this document claims the manager is complete or that Ben has finished password/MFA setup.

## Resume checklist
1. Read this file and current commits, not chat recollections.
2. Confirm project ref before any database change.
3. Finish owner login/MFA through secure password setup; never put secrets in issues, commits, URLs or chat.
4. Test anon draft/storage rejection, non-owner rejection, authenticated save conflict, publishing and image privacy.
5. Test 320/390/768/1440, mobile file input, caption/order, Hebrew RTL, reduced motion, preview and installability.
6. Commit each working change, verify deployment and screenshots. Do not introduce calendar/payments.
