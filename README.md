# Nexoura legal pages

Public, bilingual English/Arabic privacy and account-deletion pages for the
Nexoura Android app. Plain HTML and CSS; no dependencies, JavaScript, forms,
analytics, remote fonts, API calls or Cloudflare Functions.

## Deploy as a Cloudflare Worker (static assets, Git integration)

`wrangler.jsonc` defines an assets-only Worker named `nexoura-legal` that
serves `public/`. There is no Worker script.

1. Open **Workers & Pages** in Cloudflare, choose **Create** and import this
   repository (**amroA12/nexoura-legal**) as a **Worker**. If it is not listed,
   grant the Cloudflare GitHub app access to this repository.
2. Use these settings:

   | Setting | Value |
   | --- | --- |
   | Project name | `nexoura-legal` (must match `name` in `wrangler.jsonc`) |
   | Build command | Leave empty |
   | Deploy command | `npx wrangler deploy` |
   | Non-production branch deploy command | Leave the default |
   | Preview builds | Off |
   | Protect with Cloudflare Access | Off (the pages must stay public) |
   | Root directory | Leave empty (repository root) |
   | Production branch | `main` |

3. Deploy and verify `/privacy` and `/delete-account` on the `*.workers.dev`
   URL Cloudflare assigns. `/privacy.html` redirects to `/privacy`.
4. In the Worker's **Settings → Domains & Routes**, add the custom domain
   **legal.getnexoura.com** and wait until it and HTTPS are active. Do not
   replace the existing `api.getnexoura.com` DNS record: the backend uses it.

Cloudflare publishes only `public/`. The `_headers` file sets security headers;
`404.html` is returned with HTTP `404` for unknown paths. `/` still displays
the privacy policy, preserving the old root entry point.

Manual alternative: `npx wrangler deploy` from the repository root after
`npx wrangler login`.

## إعدادات النشر بالعربية

اربط هذا الريبو بـ **Cloudflare Worker** من **Workers & Pages**. اسم المشروع
`nexoura-legal`، اترك أمر البناء فارغًا، وأمر النشر `npx wrangler deploy`،
وأطفئ Preview builds وCloudflare Access. بعد نجاح النشر، أضف
`legal.getnexoura.com` من **Settings → Domains & Routes** داخل الـ Worker.
لا تغيّر سجل DNS الخاص بخادم `api`.

الروابط المقترحة بعد تفعيل النطاق:

- `https://legal.getnexoura.com/privacy`
- `https://legal.getnexoura.com/delete-account`

## Release integration and acceptance checks

- Open both pages without signing in, including from a private browser window.
  Expect HTTPS, HTTP `200` and `Content-Type: text/html`. Verify Arabic, mobile
  layout, keyboard navigation and the email address.
- Test an unknown path; expect HTTP `404`, not the privacy policy with `200`.
- The email buttons must open a draft to **wesam.amro12@gmail.com**. Clicking
  does not submit a request; the user must send the email. Support must monitor
  this mailbox and fulfill verified requests. No deletion backend is deployed
  by this repository.
- Enter the final public URLs in Google Play Console. The app currently links
  to `https://api.getnexoura.com/privacy`; changing Console alone does not update
  the app. Update app links in a subsequent release, or configure two exact-path
  redirects on the API host (`/privacy` and `/delete-account`) to the new domain.
  Preserve every other API route. This repository cannot set redirects on a
  different hostname by itself.
- Keep these pages publicly accessible. Do not add a Cloudflare Access login,
  country restriction or challenge that prevents users/reviewers reading them.
- Match the policy and Data safety answers to the actual published app and
  server configuration. Hosting the pages does not deploy the backend,
  perform a deletion, verify Play purchases or guarantee Google Play approval.

## Local preview and future edits

```sh
python -m http.server 8080 --directory public
```

Open `http://localhost:8080/privacy.html` and
`http://localhost:8080/delete-account.html`. Python's simple server does not
implement extensionless routes or `_headers`; use `npx wrangler dev` to test
those locally before using the links in Play Console.

The authoritative privacy document is `public/privacy.html`; keep
`public/index.html` identical when editing it. Edit both language sections,
keep the date synchronized across documents, and keep deletion/retention text
consistent between the two pages. The confirmed support mailbox is
`wesam.amro12@gmail.com`. Recovery backup retention is the most recent seven
backups plus up to four older weekly backups, pruned when new backups are
created; do not replace that with an unverified fixed-day promise.

## Official references

- [Cloudflare Workers: static assets](https://developers.cloudflare.com/workers/static-assets/)
- [Cloudflare Workers: static asset headers](https://developers.cloudflare.com/workers/static-assets/headers/)
- [Cloudflare Workers: custom domains](https://developers.cloudflare.com/workers/configuration/routing/custom-domains/)
- [Cloudflare Workers: Git builds](https://developers.cloudflare.com/workers/ci-cd/builds/)
- [Google Play: account deletion requirements](https://support.google.com/googleplay/android-developer/answer/13327111)
