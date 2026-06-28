---
name: venue-admin-populator
description: Use when Codex needs to create or update a venue record in the admin.salventia.com admin UI from source material, including venue URLs, official websites, same-site pages, uploaded/provided files, local PDFs, spreadsheets, documents, images/screenshots, or pasted source text. Applies to Salventia venue forms, venue-owned descriptions, profile and therapy mappings, suggested mappings, rooms, seasons, rate plans, packages/programs, extra services, and age discount tiers.
---

# Venue Admin Populator

Use the open `admin.salventia.com` admin tab to create or update a venue from source material. Source material may be URLs, same-site links, uploaded/provided files, local files, screenshots/images, pasted text, PDFs, spreadsheets, or documents.

## Workflow

1. Inspect the venue admin form and all venue-owned sections before researching or extracting facts. Use the UI as the source of truth for available fields, `source_url` fields, relation mappings, creatable subresources, and row-level controls.
2. Search for an existing venue by name and likely slug variants. Update it if found; otherwise create a new inactive candidate.
3. Inspect every main field, section, and venue-owned subresource editor, including row-level controls and hidden/conditional inputs inside created or existing mappings.
4. Research and extract facts from the provided sources. Prefer official venue sources and relevant same-site pages. Follow source links that correspond to admin-owned sections, especially program/sanatorium pages, treatment lists, price pages, package detail anchors, room pages, gallery pages, and rating/review sources. Use provided files as first-class sources; extract only the facts supported by those files. Use third-party sources only for structured facts not present in official sources.
5. Populate every supported field and row. Add `source_url` wherever the UI provides it. For file-only sources where no URL exists, use the closest supported citation field if available, such as notes naming the source file; do not invent a URL.
6. Do not create unsupported rows or force data into unrelated fields.
7. Save, reload or reopen the record, and verify persisted values in every section.
8. Report what was filled and list empty fields/sections with concise reasons.

## Rules

- Use only facts supported by provided URLs, relevant same-site pages, or provided files, except local spoken language may be inferred from country or website localization.
- Keep tags empty unless explicitly instructed; do not put structured information into tags.
- Link all source-provided venue images by default. Prefer linking by source URL when the UI supports remote image references; otherwise upload the image. Only skip images that are broken, exact duplicates, obviously irrelevant, or technically unusable, and report skipped images with reasons. If curation is desired, ask first.
- Provide English, Russian, and Arabic localized text for venue-owned text you create or edit; preserve existing locales.
- Ensure each localized field is written in its own locale; do not reuse English copy in Russian, Arabic, or other locale-specific fields.
- Do not edit shared global profile or therapy catalog text.
- Store amenities in the dedicated amenities relation when available, not as `amenity:*` tags. Map only clear existing amenity catalog matches; if a source-listed amenity is missing from the catalog or does not clearly match an existing catalog entry, create a suggested amenity row with source evidence instead of creating a direct amenity catalog record.
- Put medical conditions in profile mappings or suggested profiles. Match source labels to existing profile catalog labels case-insensitively across every available locale before creating a suggestion; normalize casing when comparing source text, translated labels, synonyms, and admin search results. When the source clearly targets an existing catalog profile, add the actual venue profile relation; do not leave the match only as a suggestion. Mark direct condition-group profiles primary when an official program or indication list directly centers that group; leave secondary or inferred subconditions non-primary.
- Put treatments and procedures in therapy mappings, suggested therapies, or programs. Map only clear catalog matches. Check catalog labels in both English and the source language using case-insensitive comparison, but do not multiply one broad source therapy into multiple disease-specific catalog rows. When the source procedure clearly maps to an existing catalog therapy, add the actual venue therapy relation with notes and `source_url`; do not leave the match only as a suggestion. If there is no single clear catalog entry, leave it as a suggested therapy.
- Create suggested profile or therapy rows only for source-listed labels that are useful but not clear catalog matches, using the UI-supported suggestion fields and source evidence. Suggested labels should preserve the source language when the source page is not English; when helpful, append an English translation in parentheses, for example `Ванны Хвойные (Coniferous baths)`, and set the candidate locale to the source language.
- Extra services require explicit prices.
- Create room categories when enough room facts exist.
- Create rate plans when native-currency pricing and enough pricing detail are available.
- Make the best effort to encode pricing using the rate-plan structure exposed by the UI. If an exact rate or pricing rule cannot be encoded, do not invent unsupported rows or values; report precisely what could not be encoded.
- Encode prices using the rate-row fields supported by the UI, such as package, room, season, occupancy or guest count, basis, board, min/max nights, weekdays, extra beds, and `source_url` where available.
- Before entering a large tariff matrix, estimate the number of resulting rate rows and check whether the UI/API exposes a bulk ingest path such as the rate-plan ingest endpoint. If only row-by-row entry is available and the matrix is large, confirm the intended scope or board subset before committing to long manual entry.
- If a source defines local weekend days, encode weekdays as the complement of that source-defined weekend, even when it differs from Saturday/Sunday assumptions. Report the mapping used.
- If the user narrows tariff scope, such as to one board type, remove or skip unsupported board rows and verify the remaining board counts after reload.
- Use package-level board only when the source explicitly states the package itself requires that board arrangement.
- Do not set package `required_board` merely because a rate, accommodation offer, or included-price note includes meals; meal inclusion should normally be encoded on rate rows when supported.
- Create one canonical program/package per source-named offer when duration-based tiers describe the same offer. Do not split that offer into separate packages solely because included counts, price rows, or eligibility vary by stay length; encode those differences with the package, therapy-tier, and rate-row fields the UI supports. Leave price or duration empty if not supported by the UI or source.
- Populate package eligibility fields such as minimum nights when the source states them. Do not stop at package names when a source-backed offer also gives stay-length eligibility, included services, or package-specific source anchors.
- When a package/program includes treatment quantities, link clear included therapy catalog matches in package therapy rows with quantities and source notes. Treat therapy quantities as stay-length tiers by default: author rows per `(therapy, min_nights, max_nights, quantity)` when counts vary by package duration, keep tiers non-overlapping, and use a blank min/max universal row only when the source gives one count that applies to every eligible stay length.
- Do not force diagnostics, consultations, lab tests, or procedures without clear catalog matches into package therapy rows; keep them in customer-facing package text or suggestion fields when the UI supports suggestions.
- For tiered packages or programs, model non-overlapping package eligibility and therapy count tiers when the UI supports min/max nights. If the source says the final listed tier applies to "or more" nights, leave that tier's max nights empty/open-ended instead of capping it at the listed threshold.
- Do not duplicate the same pricing constraint onto package fields unless the package editor specifically requires it or the source states it as a package-level requirement.
- Do not create rate plans as published unless the user explicitly asks for publication and the source supports production-ready pricing.
- Customer-facing description and copy fields, including venue full descriptions, package/program descriptions, room descriptions, extra service text, and localized variants, must be direct narrative only. Use extracted facts directly; do not mention evidence provenance such as "according to the official site/table", "the source says", file names, source URLs, or source-specific framing. Keep traceability in `source_url`, notes, or the final report.
- Full description must be customer-facing and factual. Include the venue's own description and any found non-structured information relevant to customers as narrative copy, without internal curation notes or source references.
- Prefer the official venue name from the venue's own site, contact page, or source title over a third-party tour/operator title. Use a third-party name only when the official source is missing or ambiguous, and keep spelling/capitalization aligned with the most authoritative source.
- Do not treat a tour-operator or package-marketplace score as the venue's overall or treatment rating unless the page clearly defines it as a review rating for the venue. Prefer recognized review/rating sources when available; otherwise leave rating fields empty rather than importing ambiguous scores.
- Relation notes should name the specific official program, package, or indication list that supports the mapping, not just a generic site note. Use the most specific source URL available, including same-page anchors when stable.

## Package Therapy Tiers

- The promoted package structure stores included therapy counts on package therapy rows, not in package text alone, when the therapy has a clear catalog match.
- Prefer the tiered row shape because source packages commonly scale included sessions by stay length. Example: 7-10 nights = 3 sessions, 11-14 nights = 6 sessions, 15+ nights = 8 sessions becomes three rows for the same therapy with `(min_nights, max_nights, quantity)` of `(7, 10, 3)`, `(11, 14, 6)`, and `(15, blank, 8)`.
- Use a blank min/max universal row only when the package source states a single count without length-specific variation, or when the package itself has one eligibility range and the therapy count applies throughout that whole range.
- Do not mix a universal row with bounded rows for the same therapy. If any bounded tier exists for a therapy in a package, every row for that therapy should be a bounded tier that together represents only the source-supported length ranges.
- Keep package-level `min_nights` / `max_nights` for package eligibility. Keep therapy-row `min_nights` / `max_nights` for included count changes within that package. Do not copy package bounds onto every therapy row unless the count evidence is tied to those bounds.
- When only total procedures are listed for a package duration and the individual therapy split is unclear, do not invent per-therapy quantities. Put the aggregate statement in package copy/notes and create suggested therapy rows only when the UI supports them and the source label is useful.

## Source Handling

- For URLs, inspect the provided page and relevant same-site links needed for discovered admin fields.
- For provided files, identify file type and extract facts with an appropriate parser or visual inspection. For spreadsheets, inspect sheets/tables/formulas as needed. For PDFs/documents, extract text and inspect rendered pages when layout matters. For images/screenshots, use visual evidence only for facts visible in the image.
- Preserve traceability. Prefer UI `source_url` fields for web evidence. For file evidence, cite the file name/path in notes or the final report when the UI has no file-source field.
- If a file and a URL disagree, prefer the official source unless the user identifies the file as more authoritative. Report material conflicts.

## Rate Verification

- After rate entry, reload or reopen the record and verify persisted row counts.
- For tariff matrices, verify actual rate rows, not only the presence of a rate plan, rooms, and seasons. Open the rates subpanel or use the API when available, then verify board counts, season coverage, room coverage, occupancy or guest-count coverage, weekday-mask coverage, prices, and any min/max-night or package restrictions that were encoded.
- If source evidence is a local file and the UI only accepts `http(s)` source URLs, do not invent a URL. Cite the file name in the final report and use notes fields only when the UI provides a suitable place.
