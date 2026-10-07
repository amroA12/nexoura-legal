# Nexoura legal pages

Public, bilingual English/Arabic privacy and account-deletion pages for the
Nexoura Android app. Plain HTML and CSS; no dependencies, JavaScript, forms,
analytics, remote fonts, API calls or Cloudflare Functions.

## Deploy with Cloudflare Pages (Git integration)

1. Open **Workers & Pages** in Cloudflare and create a **Pages** project using
   **Import an existing Git repository** (the exact menu wording may vary).
2. Connect GitHub and authorize **amroA12/nexoura-legal**. If the account is not
   listed, grant the Cloudflare GitHub app access to this repository.
3. Use these settings:

   | Setting | Value |
   | --- | --- |
   | Production branch | `main` |
   | Framework preset | `None` |
   | Build command | `exit 0` |
   | Build output directory | `public` |
   | Root directory | Leave empty (repository root) |
   | Environment variables | None |

4. Deploy and verify `/privacy` and `/delete-account` on the actual `*.pages.dev`
   URL Cloudflare assigns. Both HTML files have extensionless URLs in Pages.
5. In the Pages project's **Custom domains**, add **legal.getnexoura.com** (the
   proposed domain; not activated by this repository). Use Cloudflare's setup
   flow to attach DNS and wait until the domain and HTTPS are active. Do not
   replace the existing `api.getnexoura.com` DNS record: the backend uses it.

Cloudflare publishes only `public/`. The `_headers` file sets security headers;
`404.html` prevents unknown paths from becoming successful SPA responses. `/`
still displays the privacy policy, preserving the old root entry point.

Alternative: upload the contents of `public/` using Pages Direct Upload.
Choose Git integration initially if you want automatic publishing from `main`;
Direct Upload projects cannot later switch to Git integration without creating
a new project.

## إعدادات النشر بالعربية

أنشئ مشروع **Cloudflare Pages** واربطه بهذا الريبو. اختر فرع `main`، وإطار
العمل `None`، وأمر البناء `exit 0`، ومجلد النشر `public`. اترك مجلد الجذر
ومتغيرات البيئة فارغة. بعد نجاح النشر، أضف `legal.getnexoura.com` من
**Custom domains** داخل المشروع. لا تغيّر سجل DNS الخاص بخادم `api`.

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
implement Pages' extensionless routes or `_headers`; test those on a Pages
deployment before using its links in Play Console.

The authoritative privacy document is `public/privacy.html`; keep
`public/index.html` identical when editing it. Edit both language sections,
keep the date synchronized across documents, and keep deletion/retention text
consistent between the two pages. The confirmed support mailbox is
`wesam.amro12@gmail.com`. Recovery backup retention is the most recent seven
backups plus up to four older weekly backups, pruned when new backups are
created; do not replace that with an unverified fixed-day promise.

## Official references

- [Cloudflare Pages: static HTML](https://developers.cloudflare.com/pages/framework-guides/deploy-anything/)
- [Cloudflare Pages: route matching and 404 behavior](https://developers.cloudflare.com/pages/configuration/serving-pages/)
- [Cloudflare Pages: custom domains](https://developers.cloudflare.com/pages/configuration/custom-domains/)
- [Cloudflare Pages: response headers](https://developers.cloudflare.com/pages/configuration/headers/)
- [Cloudflare Pages: direct upload](https://developers.cloudflare.com/pages/get-started/direct-upload/)
- [Google Play: account deletion requirements](https://support.google.com/googleplay/android-developer/answer/13327111)
