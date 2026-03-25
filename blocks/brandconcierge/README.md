# Brand Concierge block

## Overview

This block provides a DOM mount point (`#brand-concierge-mount`) for Adobe Brand Concierge. The conversational UI is loaded by the Adobe Experience Platform Web SDK (`alloy.js`) and the Brand Concierge bundle (`brandconciergemain.js`), configured in `head.html` with `window.styleConfiguration` from `scripts/styleconfigurations.js`.

## Configuration

The block model has no authorable fields in Universal Editor (`fields: []`). Runtime behavior is controlled by:

- **Datastream and org**: `datastreamId` and `orgId` in the `alloy("configure", …)` call in `head.html`.
  - Datastream ID (techinsiders36 / alexanderd CitiSignal Brand Concierge.): `b60c331c-9ae7-48f5-80b5-374c81ff93f1`
  - IMS Org ID: `907075E95BF479EC0A495C73@AdobeOrg` (in `head.html`; not the same as the Concierge Instance ID).
- **Brand Concierge instance** (for your records; routing in production is via the datastream + AEP setup): `69c3df682a760672d0784c5d`
- **Styling and copy**: `scripts/styleconfigurations.js` (`window.styleConfiguration`); metadata there also lists sandbox name `techinsiders36` and datastream name for documentation.

## Integration

- **Mount**: `decorate()` sets `id="brand-concierge-mount"` on the block root so `bootstrapConversationalExperience` can target it.
- **Scripts**: Loaded globally in `head.html` (Alloy base code, `alloy.js` with `nonce="aem"`, then configure / `sendEvent` / `bootstrapConversationalExperience`).

## Behavior

1. Page loads style configuration, Alloy, and configuration.
2. `sendEvent` with `fetchConversationalExperience: true` runs, then Brand Concierge bootstraps into the mount node.

## Error handling

If `datastreamId` / `orgId` are wrong or Edge calls fail, the experience may not render; check the browser console and Adobe Datastream configuration. Network or CSP issues loading `/scripts/alloy.js` or `/scripts/brandconciergemain.js` will prevent the UI from appearing.

### Troubleshooting: empty mount, nothing loads

1. **Content Security Policy** — `head.html` uses `script-src` with `'strict-dynamic'`, so **every** script (including inline) must use `nonce="aem"`. Without it, the browser never runs `alloy("configure", …)` or the bootstrap block.
2. **DOM timing** — `sendEvent` / `bootstrapConversationalExperience` run after `DOMContentLoaded` so `#brand-concierge-mount` exists (the block is in the document body, not the head).
3. Open DevTools → **Console** for `[Brand Concierge] sendEvent or bootstrap failed:` or network errors to Edge (`edge.adobedc.net`).
