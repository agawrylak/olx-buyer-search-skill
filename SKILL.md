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

1. Clarify the buyer intent when needed.

For broad or general requests, use model knowledge and quick product research to infer sensible buyer criteria before searching. Examples include beginner-friendliness, reliability, common defects, ease of setup, parts availability, compatibility, safety, support/community size, and typical value for money in the requested category.

Ask the user 1-3 concise clarifying questions before searching only when missing information would materially change the search results or ranking. Do not ask questions just to be thorough if reasonable assumptions are enough.

Clarify important missing details such as:

- budget or price ceiling
- location, delivery, pickup, or travel radius
- intended use or skill level
- required size, capacity, compatibility, language, standard, or other specification
- risk tolerance for repairs, modifications, incomplete items, or older models
- hard exclusions that are not obvious from the prompt

If you proceed without asking, state the assumptions or buyer criteria used in the final answer.

2. Parse the buyer intent into hard requirements and preferences.

Hard requirements are constraints that make an offer useless if missing. Examples:

- required listing or transaction type, such as sale, rental, service, accommodation, product, or bundle
- required condition, such as new, used, working, complete, not damaged, or not for parts
- required model, version, generation, size, compatibility, capacity, power, language, standard, or other capability
- required price bound
- required location, delivery, pickup, date, availability, or seller constraint
- explicit reject criteria

Preferences improve ranking but do not decide basic usefulness.

Hard requirements and preferences can come from the user's prompt, user clarifications, model knowledge, and product research. Keep inferred assumptions separate from explicit user requirements; do not treat model assumptions as hard requirements unless they are necessary for the buyer's stated goal.

3. Generate 3-8 focused Polish OLX queries.

Use realistic OLX wording. Prefer concise query terms.

For standardized physical products, include both narrow requirement-heavy queries and broader product/category queries. Some sellers omit capability words such as size, capacity, compatibility, runtime, supported standard, language, generation, or condition, even when the product satisfies them. Use broader searches to discover named models, then verify hard requirements from listing details and external product facts.

For location-bound searches, services, rentals, or accommodation, include the place and listing type in most queries. Then add required features, dates, capacity, or constraints as separate query variants.

Prefer category-scoped OLX URLs when the requested listing type has a clear OLX category. This avoids keyword-only false positives from unrelated categories. Examples:

- board games: `https://www.olx.pl/sport-hobby/gry-planszowe/q-{slug}/`
- children's games: `https://www.olx.pl/dla-dzieci/zabawki/gry-dla-dzieci/q-{slug}/`
- books: `https://www.olx.pl/muzyka-edukacja/ksiazki/q-{slug}/`
- electronics: use the relevant `/elektronika/.../q-{slug}/` subcategory when obvious

Use the all-offers URL only when the category is unclear or when category-scoped results are too sparse.

Product query patterns:

- `{product type} {required feature}`
- `{product type} {brand or model}`
- `{product type} {condition}`
- `{product type} {compatible device or standard}`
- `{category synonym} {budget or location}`

Location-bound query patterns:

- `{place} {listing type} {required feature}`
- `{place} {service type} {required capability}`
- `{place} {rental type} {date or capacity}`
- `{place} {product type} {pickup or delivery wording}`
- `{nearby place or district} {listing type} {required feature}`

Avoid generic amenity-only or feature-only queries if the listing type matters, such as:

- `{required feature}` by itself
- `{amenity}` by itself
- `{brand}` by itself when it commonly returns accessories or parts
- `{capacity}` by itself
- `{condition}` by itself

For standardized products, do not rely only on requirement phrases. Sellers often omit facts such as capacity, size, compatibility, power, runtime, language, supported standards, age range, or player count, even when the model satisfies them. Requirement-heavy queries can also create false positives from unrelated categories where the same words mean something else. Use category-scoped searches and mix query types:

- direct requirement query: `{product type} {hard requirement}`
- broad category query: `{product type} {general subtype}`
- condition/value query: `{product type} używane`, `{product type} stan bardzo dobry`
- brand/model queries from likely matches: `{brand}`, `{model}`, `{product line}`
- synonym queries for common seller wording: local names, abbreviations, misspellings, older model names, or category jargon

Examples:

- For a product with required capability, search both `{product type} {capability}` and `{brand/model}` queries, then verify the capability from listing facts or external specs.
- For a compatibility-sensitive item, search both `{product type} {compatible device}` and `{model}` queries, then reject incompatible variants.
- For a location-bound request, search `{place} {listing type}` plus variants for required features, capacity, dates, or seller constraints.
- For a service request, search `{place} {service type}` plus variants for required equipment, availability, certification, or delivery area.

When a listing names a recognizable model but omits a required product fact, inspect it anyway and verify the fact externally. Reject it only if the named model/version fails the hard requirements or if the OLX listing has listing-specific problems such as missing components, wrong edition/generation, accessory-only listing, poor condition, damage, wrong language/region when relevant, or incompatible variant.

4. Search OLX using browser-equivalent URLs.

Open or fetch these URLs using whatever browser, web-fetch, or search capability is available in the host system.

Use:

`https://www.olx.pl/oferty/q-{slug}/`

Or, when a clear category exists:

`https://www.olx.pl/{category-path}/q-{slug}/`

Examples:

- `https://www.olx.pl/oferty/q-{place}-{listing-type}-{feature}/`
- `https://www.olx.pl/{category-path}/q-{product-type}-{feature}/`
- `https://www.olx.pl/{category-path}/q-{brand-or-model}/`
- `https://www.olx.pl/{region}/q-{listing-type}-{required-feature}/`

Use default OLX relevance ordering. Do not add `search[order]=created_at:desc` unless the user explicitly asks for newest.

Do not add `used` or `new` filters unless the user explicitly asks for item condition.

5. Search deeply and track coverage.

Do not stop at the first result page unless the query has very few results or the first page clearly proves the query is irrelevant.

Always perform deep coverage by default: scan roughly 100-300 result cards across query variants when feasible. Inspect multiple result pages per promising query, rather than treating page 1 as representative.

Prefer breadth first, then depth:

- scan result cards across multiple queries and pages
- deduplicate repeated listings before opening them
- shortlist promising listings by title, category, price, location, condition, and snippet evidence
- open the strongest shortlisted listings for detailed verification

Open at least 20-40 promising listings when available. If tool, time, access, or result-quality limits prevent this, say so in the final answer.

Deduplicate by listing URL, listing ID, title plus location plus price, or clearly identical photos/snippets. Do not count promoted duplicate cards twice.

If early searches are sparse or noisy, expand queries using category synonyms, model aliases, common misspellings, broader category terms, and specific models or brands discovered in early result pages.

Track coverage for the final answer:

- queries used
- pages checked
- approximate result cards scanned
- listings opened
- useful listings accepted

6. Inspect promising listings.

Open listings that appear to satisfy hard requirements or have strong title/location/category evidence.

Use listing title, location, price, description, attributes, and evidence snippets.

For standardized or researchable items, supplement the listing with general product knowledge and web research when it improves matching. This includes, but is not limited to, games, electronics, tools, appliances, vehicles, parts, books, media, instruments, sports gear, furniture, baby products, collectibles, and branded products.

For general buyer questions, use model knowledge and research before and during listing inspection to define practical evaluation criteria, not only to verify individual listing facts.

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

7. Reject aggressively.

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

8. Rank only accepted listings.

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

Answer in the same language as the buyer's prompt. If the prompt mixes languages, use the dominant language unless the user explicitly asks for another language.

Start with a short decision summary:

- Say whether there are useful matches.
- Name the top listing if one is clearly worth contacting.
- If none are useful, say why.

Then return these sections.

## Assumptions And Criteria

If the user did not provide detailed criteria, briefly state the assumptions and product-selection criteria used. Make clear which criteria came from the user and which were inferred from model knowledge or research.

## Search Coverage

Briefly state:

- queries used
- pages checked
- approximate result cards scanned
- listings opened
- useful listings accepted
- any coverage limits, such as blocked pages, sparse results, duplicates, or time/tool limits

## Best Matches

For each useful listing:

- title
- URL
- price
- location
- match level
- why it was chosen: 2-4 concise sentences explaining the key tradeoffs, listing-specific evidence, and why it ranks above alternatives
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

- wrong listing or transaction type
- accessory, part, expansion, or service instead of the requested main item
- damaged, incomplete, for-parts, or repair-needed listings
- wrong model, version, generation, size, language, standard, or compatibility
- over budget or missing required delivery/pickup/location/date
- missing required capability, capacity, condition, amenity, or permission
