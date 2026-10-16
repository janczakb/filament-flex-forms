# Changelog

All notable changes to `janczakb/filament-flex-forms` are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.4] - 2026-10-16

### Updated

- Maintenance and runtime updates for Flex Form fill, embed layouts, and asset planner.

## [1.0.3] - 2026-09-07

### Fixed

- **Studio license bootstrap** — portal plan `lifetime` is a first-class Studio plan (aligned with PHP `LicensePlan::Lifetime`); capability lookup no longer crashes with `… is not iterable`, and lifetime is not remapped to `pro`.
- **Studio asset cache** — `StudioBuild::ensurePublished()` mirrors entry + chunks into `public/js/…/b/{id}/` after `filament:assets` (Docker/Coolify), so HTML stops loading the flat `immutable` URL that CDNs stick for a year.

## [1.0.2] - 2026-08-06 — enterprise hardening

### Fixed

- **Zapier REST API** — `feed` and `requirements` endpoints also require a valid runtime license (same as sample/transfer).
- **Custom CSS (Studio client)** — client validation rejects `url()` / style-tag breakouts to match `DesignCssSanitizer::validate`.
- **Install checklist** — warns multi-admin hosts about Studio permission defaults; reminds queue worker + scheduler.
- **Insights monthly series** — monthly buckets no longer sum daily `unique_respondents` (overcount). Series leaves `0` for month points; the KPI card still uses distinct `respondent_key` across the range.
- **Insights filtered scans** — `insights.filtered_scan_max_rows` (default `100_000`) stops raw-event cursors; meta exposes `filtered_scan_capped` for row budget and `filtered_range_clamped` when Global Insights clamps long filtered ranges to the last 31 days.
- **Insights Redis ping** — `InsightsScalingProfile::redisEnabled()` memoizes the connectivity ping per PHP process (process-local). Today-buffer Cache sticky after Redis soft-fail remains process-local for that calendar day — not a cross-worker Redis sticky lock.
- **Webhooks SSRF** — re-resolve public IPs immediately before the outbound request and pin via `CURLOPT_RESOLVE` when curl is available; residual TOCTOU documented.
- **Google Sheets** — `isConfigured` requires OAuth refresh credentials (`client_id` + `client_secret` + `refresh_token`), not a bare access token.
- **Google Analytics** — split honesty: client gtag needs measurement ID; Measurement Protocol dispatch requires `api_secret` (`isMeasurementProtocolReady`).
- **Integrations** — ghost `require_payment` skip branches removed (no payment product); feeds proceed regardless of that legacy flag.
- **All Entries** — stub hub route, page class, Blade view, and Studio mount removed; `FormResourceUrl::allEntries()` returns `#`.
- **Demo seeder** — `flex-forms:seed-demo-film` refuses production without `--force`.
- **Docs** — prefills documented as embed/popup-only; embed modes Standard / Popup / Full page; access gates use DB `lockForUpdate` (not Redis exact-N); Studio permissions multi-admin warning; support contact via Help Center URL.
- **Studio permissions UI** — missing ability keys in React bootstrap default to **denied** (fail-closed).
- **Forms index** — soft cap via `studio.forms_index_max` (default 500) with `formsTruncated` bootstrap flag.
- **PCI** — credit-card presenters / preview modal never reveal full PAN (last4 / masked only).
- **Packaging** — personal author emails removed from `composer.json`; contact via Help Center support URL.

### Added

- Migration indexes: `respondent_key` + city composites on `flex_form_analytics_events` (idempotent).

## [1.0.1] - 2026-08-06

### Changed

- **Plans** — Standard / Pro / Enterprise differentiation is now: activation seats (**2 / 6 / 100**), Pro **hide branding**, and Enterprise priority support (email or private Slack) plus prioritized feature requests and roadmap input. Advanced **Design** panel and custom CSS are documented as available on all plans (not Pro-only).
- **Pricing** — documented as: Standard **$99 / year**, Pro **$199 / year**, Enterprise **$799 / year**, plus Lifetime **$899** one-time (Pro features · 10 activations · updates forever). Docs clarify that an expired / cancelled yearly license pauses Studio, public fill, and Composer downloads (stored data is kept). README and Help Center note Lifetime under the plans graphic (`art/plans-flex-forms.webp`), which shows yearly tiers only.
- **Support hub** — Support page cards link to Help Center, license portal, GitHub (issues / feature templates / repo). Community and review cards appear only when URLs are configured.
- **Insights / Global Stats** — Top forms today merge now **adds** live buffer counts (was `max()`, which dropped today on multi-day ranges). Form + global **starts** also add today’s buffer. `DateRange::dateKeys()` guard raised so preset `all` no longer cliffs at ~4000 days; collector still accepts in-range daily rows outside the scaffold. Added 500+ invariant / KPI math Pest cases.
- **Insights KPIs** — multi-day `unique_respondents` uses distinct completed `respondent_key` values (not a sum of daily uniques). Filtered submission counts prefer completed DB rows for matching respondents. Answers with no filtered respondent matches stay empty (`no_matches`) instead of soft-unfiltering. Forms index `submissions_count` excludes partials. Today buffer sticks to Cache for a day after Redis soft-fail so Redis/Cache counters do not diverge mid-process (**process-local** sticky date map — not a shared Redis lock).

- **Design panel (Studio)** — refreshed labels and help copy so the Design rail is clearly Flex Forms (not a 1:1 Customizer clone): section title **Design**, **Form fields**, **Content width**, **Text size**, and related control labels/help text in EN/PL.
- **Design panel — font picker** — large Google Fonts lists use a virtualized HeroUI Autocomplete with live search instead of a laggy ComboBox.
- **Design panel — controls** — logo upload uses the Design media control again (icon + clickable square; box-shadow no longer clipped). Button/input width and alignment toggles use the intended Hugeicons set.
- **Form settings — primary color** — compact color trigger height matches the adjacent select control.
- **Docs / README** — Help Center welcome page embeds the product walkthrough video; public README links the same YouTube overview. Design panel docs wording updated to match the UI.

### Fixed

- **GetResponse — Name field picker** — form feed **Name field** lists only Name blocks on the form (not every field). Mapped Name answers resolve via block id / name parts correctly.
- **Integration name mapping** — GetResponse, Campaign Monitor, ConvertKit, MailerLite, ActiveCampaign, Brevo, HubSpot, EmailOctopus, Capsule CRM, and Help Scout name / first / last pickers use `name_fields` (Name blocks only). Dispatch resolves Name answers into first/last/full correctly instead of comma-joining part objects.
- **Form / global Integrations UI** — feed and connection forms use HeroUI Autocomplete (search + virtualized list), Switch, and Checkbox/CheckboxGroup with correct horizontal label alignment. Toggle fields and conditional logic no longer stack control above label; `visible_when` still hides/shows dependent options per integration.
- Insights answer samples continue to render richer previews for signature, file, phone, and related field kinds in the Answers widgets.

## [1.0.0] - 2026-08-01

### Added

- **Studio** — drag-and-drop multi-page form builder with field catalog, Logic board, calculated fields, thank-you page, Customize (including custom CSS), preview, and publish.
- **Public fill** — hosted fill experience with progress / wizard stepper, partial submissions, save-for-later, access gates (password, schedule, limits, duplicates), redirect on completion, and languages / RTL.
- **Share & Embed** — fill links, OG social preview, Standard / Popup / Full page embeds, query-param prefill.
- **Submissions** — inbox, completed/partial filters, delete, CSV export.
- **Contacts** — unified respondent directory with search, sort, delete, CSV export.
- **Insights** — Answers / Visits / Drop-offs, filters, public share links, optional Redis scaling (HLL, today hashes).
- **Integrations** — 25 drivers (email/CRM, Zapier, webhooks, Google Sheets, Slack, GA, Mapbox, reCAPTCHA, Turnstile, and more) with global credentials and per-form conditional feeds.
- **Licensing** — paid plans (Standard / Pro / Enterprise), domain activation via license portal, runtime `LicenseGate`, Pro hide-branding.
- **Help Center** — Mintlify documentation for buyers and operators.

### Security

- Hard-deny (HTTP 403) on Studio mutations, hub CRUD, integration writes, design uploads, and public submit/partials when the runtime license is invalid. Global settings remain available to recover by saving a valid public key.
