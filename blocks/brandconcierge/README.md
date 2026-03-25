# Brand Concierge Block

## Overview

This block provides a DOM mount point (`#brand-concierge-mount`) for Adobe Brand Concierge. The conversational UI is loaded by the Adobe Experience Platform Web SDK (`alloy.js`) and `brandconciergemain.js`, configured in `head.html`, not inside the block script.

## Configuration

- **Authoring**: Add the **BrandConcierge** block to a section in Universal Editor; the block has no authored fields (`component-models` uses an empty `fields` array).
- **Runtime**: Styling and behavior come from `/scripts/styleconfigurations.js` (`window.styleConfiguration`) and Alloy bootstrap in `head.html`.
- **Required secrets**: Set `datastreamId` and `orgId` in the Alloy `configure` call in `head.html` to your Datastream ID and IMS org ID (see the One Adobe tutorial).

## Integration

- **Mount selector**: `brandconcierge.js` sets `id="brand-concierge-mount"` on the block element; Alloy’s `bootstrapConversationalExperience` uses `#brand-concierge-mount`.
- **Scripts** (loaded globally from `head.html`): `styleconfigurations.js`, `alloy.js`, inline Alloy queue + configure + `sendEvent` + `bootstrapConversationalExperience`, and dynamic load of `brandconciergemain.js`.

## Behavior

1. Page loads; Alloy initializes and fetches the conversational experience.
2. On success, Brand Concierge bootstraps into `#brand-concierge-mount`.
3. **Sticky session**: `stickySession: true` in `head.html` enables a session cookie as described in the tutorial.

## Error handling

- If `datastreamId` / `orgId` are wrong or missing, Alloy calls may fail; check the browser console and Network tab to Edge (`edge.adobedc.net`).
- If the mount element is absent on a page, bootstrap may not attach; ensure the block exists on pages where Brand Concierge should appear.
