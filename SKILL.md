---
name: olx-buyer-search
description: Use when searching OLX.pl listings from natural-language buyer intent, comparing real offers, or rejecting mismatched OLX results.
---

# OLX Buyer Search

Use this skill for OLX.pl buyer searches. It is intended for agents with browser, web-fetch, or search capabilities and does not depend on any local application.

## Core Principle

Return useful matches, not best-available garbage.

If no listing satisfies the hard intent, say that no useful matches were found. Do not present rejected listings as recommendations.

## Workflow

1. Parse the buyer intent into hard requirements and preferences.

Hard requirements are constraints that make an offer useless if missing. Examples:

- required location or named place
- requested listing type
- required capacity
- required amenity
- required price bound
- required date or availability
- explicit reject criteria
- must allow event, party, pets, delivery, pickup, etc. when stated as required

Preferences improve ranking but do not decide basic usefulness.

2. Generate 3-8 focused Polish OLX queries.

Use realistic OLX wording. Prefer concise query terms.

For physical products, include both narrow requirement-heavy queries and broader product/category queries. Some sellers omit capability words such as player count, runtime, capacity, compatibility, or "family", even when the product satisfies them. Use broader searches to discover named models, then verify hard requirements from listing details and external product facts.

Prefer category-scoped OLX URLs when the requested listing type has a clear OLX category. This avoids keyword-only false positives from unrelated categories. Examples:

- board games: `https://www.olx.pl/sport-hobby/gry-planszowe/q-{slug}/`
- children's games: `https://www.olx.pl/dla-dzieci/zabawki/gry-dla-dzieci/q-{slug}/`
- books: `https://www.olx.pl/muzyka-edukacja/ksiazki/q-{slug}/`
- electronics: use the relevant `/elektronika/.../q-{slug}/` subcategory when obvious

Use the all-offers URL only when the category is unclear or when category-scoped results are too sparse.

Product query examples:

- `drukarka 3d bambu lab a1`
- `fotelik 15-36 isofix`
- `laptop thinkpad 32gb ram`
- `pralka slim 45 cm`
- `gra planszowa rodzinna`

Accommodation query examples:

- `{place} nocleg sauna jacuzzi`
- `{place} domek sauna jacuzzi`
- `{place} domki sauna bania`
- `{place} willa sauna jacuzzi`
- `{place} wieczor kawalerski sauna`
- `{place} 10 osob sauna`

Avoid generic amenity-only or feature-only queries if the listing type matters, such as:

- `sauna bania`
- `jacuzzi sauna`
- `mobilne spa`
- `isofix`
- `drukarka`

For standardized products, do not rely only on requirement phrases. Sellers often omit facts such as capacity, size, compatibility, power, runtime, language, supported standards, age range, or player count, even when the model satisfies them. Requirement-heavy queries can also create false positives from unrelated categories where the same words mean something else. Use category-scoped searches and mix query types:

- direct requirement query: `{product type} {hard requirement}`
- broad category query: `{product type} {general subtype}`
- condition/value query: `{product type} używane`, `{product type} stan bardzo dobry`
- brand/model queries from likely matches: `{brand}`, `{model}`, `{product line}`
- synonym queries for common seller wording: local names, abbreviations, misspellings, older model names, or category jargon

Examples:

- board games: search both `gra planszowa 4 graczy` and broader/category terms like `gra planszowa rodzinna`, plus title queries such as `carcassonne` or `wsiąść do pociągu`
- electronics: search both `laptop 32gb ram` and model/series queries, then verify RAM, CPU, screen, and generation from listing facts or specs
- child seats: search both `fotelik 15-36 isofix` and model queries, then verify weight range, standard, and accident-free condition
- appliances: search both `pralka slim 45 cm` and model queries, then verify depth, capacity, and condition

When a listing names a recognizable model but omits a required product fact, inspect it anyway and verify the fact externally. Reject it only if the named model/version fails the hard requirements or if the OLX listing has listing-specific problems such as missing components, wrong edition/generation, accessory-only listing, poor condition, damage, wrong language/region when relevant, or incompatible variant.

3. Search OLX using browser-equivalent URLs.

Open or fetch these URLs using whatever browser, web-fetch, or search capability is available in the host system.

Use:

`https://www.olx.pl/oferty/q-{slug}/`

Or, when a clear category exists:

`https://www.olx.pl/{category-path}/q-{slug}/`

Examples:

- `https://www.olx.pl/oferty/q-okuninka-dom-jacuzzi-sauna/`
- `https://www.olx.pl/oferty/q-okuninka-nocleg-sauna-jacuzzi/`
- `https://www.olx.pl/sport-hobby/gry-planszowe/q-carcassonne/`
- `https://www.olx.pl/sport-hobby/gry-planszowe/q-gra-planszowa-4-graczy/`

Use default OLX relevance ordering. Do not add `search[order]=created_at:desc` unless the user explicitly asks for newest.

Do not add `used` or `new` filters unless the user explicitly asks for item condition.

4. Inspect promising listings.

Open listings that appear to satisfy hard requirements or have strong title/location/category evidence.

Use listing title, location, price, description, attributes, and evidence snippets.

For standardized or researchable items, supplement the listing with general product knowledge and web research when it improves matching. This includes, but is not limited to, games, electronics, tools, appliances, vehicles, parts, books, media, instruments, sports gear, furniture, baby products, collectibles, and branded products.

Keep these fact types separate:

- OLX listing facts: price, location, condition, completeness, photos, damage, seller constraints, delivery/pickup, exact model/version if stated, bundled accessories, warranty, dates, and availability.
- External/product facts: normal specifications, compatibility, dimensions, capacity, player count, running time, supported standards, required accessories, edition differences, safety recalls, common defects, market context, and whether the listed item type normally satisfies the buyer's intended use.

Use external research when:

- the listing names a recognizable model, edition, product line, ISBN, part number, or brand
- the buyer asks for capability, compatibility, fit, dimensions, performance, age range, capacity, play time, language, safety, or similar product facts
- the OLX listing omits a product fact that is likely available from authoritative sources
- there is a risk of confusing a base product, expansion, accessory, replacement part, counterfeit, incompatible version, or different generation

Prefer authoritative or high-signal sources:

- manufacturer, publisher, official manual, datasheet, support page, recall page
- reputable retailer or distributor pages
- recognized domain databases and communities for the category
- multiple independent sources when the fact is important or uncertain

Do not let external research override OLX-specific defects or constraints. A product model may normally satisfy the buyer's requirements, but the specific listing can still fail because it is damaged, incomplete, wrong version, pickup-only, too expensive, missing accessories, or otherwise unsuitable.

When using external research, cite evidence as either `OLX evidence` or `external evidence` in the result. If a hard requirement is satisfied only by external product facts, mark the match as partial or weak unless the listing's model/version is clear enough to make the inference reliable.

5. Reject aggressively.

Reject a listing if:

- required location is absent or clearly wrong
- listing type is wrong
- required amenity is missing
- required capacity is missing or too small
- explicit reject criterion appears
- it is the wrong transaction type, such as service/rental/sale when the buyer asked for another type
- it is the wrong item class, such as accommodation when the buyer wants a product, or an accessory when the buyer wants the main item
- it only has keyword overlap but no evidence for the hard intent
- it is a sale listing when the user wants rental/accommodation, unless user allowed both
- external research shows the named model/version does not satisfy a hard requirement
- the listing is probably an accessory, expansion, part, service, replica, counterfeit, incompatible generation, or wrong edition when the buyer needs the main compatible item

Wrong listing type is always a hard reject.

6. Rank only accepted listings.

Strong match:

- satisfies all hard requirements with evidence
- has multiple preferences
- has low uncertainty
- listing-specific facts and external product facts agree, when external research was needed

Partial match:

- satisfies all hard requirements
- misses one or more preferences
- has some uncertainty that can be verified
- relies on external product facts because the listing omits a relevant specification, but the listed model/version is clear

Weak match:

- likely satisfies hard requirements but has important missing details to confirm
- depends on an uncertain model/version inference or an unverified compatibility/capability claim

Reject:

- fails any hard requirement

Do not include rejects in recommendations.

## Output Format

Start with a short decision summary:

- Say whether there are useful matches.
- Name the top listing if one is clearly worth contacting.
- If none are useful, say why.

Then return these sections.

## Best Matches

For each useful listing:

- title
- URL
- price
- location
- match level
- why it is useful
- evidence
- external research used, if any
- what to ask the seller
- risks or missing facts

## Contact First

List 1-3 listings to contact first and why.

## Questions To Ask

Give concrete seller questions.

Examples:

- Is the item fully functional and ready to use?
- Is it complete, with all required accessories, cables, manuals, keys, or mounting parts?
- Has it ever been repaired, damaged, modified, or serviced?
- Can you provide a photo of the serial number, model label, or exact version?
- Is there proof of purchase, warranty, or transfer documentation?
- Can I test it at pickup, or can you send a short video showing it working?
- For accommodation: is the required amenity included, available on the requested date, and included in the price?

## Rejected Patterns

Briefly mention common rejected patterns only if useful:

- mobile sauna/spa services
- sauna/jacuzzi products
- wrong city
- too-small capacity
- no required amenity
- sale listings instead of rental
