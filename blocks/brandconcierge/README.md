# Brand Concierge block

## Overview

This block provides a DOM mount point (`#brand-concierge-mount`) for Adobe Brand Concierge. The conversational UI is loaded by the Adobe Experience Platform Web SDK (`alloy.js`) and the Brand Concierge bundle (`brandconciergemain.js`), configured in `head.html` with `window.styleConfiguration` from `scripts/styleconfigurations.js`.

## Configuration

The block model has no authorable fields in Universal Editor (`fields: []`). Runtime behavior is controlled by:

- **Datastream and org**: `datastreamId` and `orgId` in the `alloy("configure", …)` call in `head.html`.
- **Styling and copy**: `scripts/styleconfigurations.js` (`window.styleConfiguration`).

## Integration

- **Mount**: `decorate()` sets `id="brand-concierge-mount"` on the block root so `bootstrapConversationalExperience` can target it.
- **Scripts**: Loaded globally in `head.html` (Alloy base code, `alloy.js` with `nonce="aem"`, then configure / `sendEvent` / `bootstrapConversationalExperience`).

## Behavior

1. Page loads style configuration, Alloy, and configuration.
2. `sendEvent` with `fetchConversationalExperience: true` runs, then Brand Concierge bootstraps into the mount node.

## Error handling

If `datastreamId` / `orgId` are wrong or Edge calls fail, the experience may not render; check the browser console and Adobe Datastream configuration. Network or CSP issues loading `/scripts/alloy.js` or `/scripts/brandconciergemain.js` will prevent the UI from appearing.
