# NetHarvester privacy

NetHarvester is designed as a local forensic instrument, not a data service.

## What NetHarvester stores

While capture is enabled, the extension records the following into its own local IndexedDB database inside your browser profile:

- HTTP request/response metadata and bodies, bounded by configurable per-response limits
- WebSocket and SSE session frames, with timestamps and sequence numbers
- Runtime state snapshots: page storage, reachable global stores, cookies readable by page scripts, and IndexedDB records
- Cookies across accessible Firefox cookie stores and containers
- Inferred OpenAPI documents and mock route examples
- Complete static page archives created with **SAVE PAGE**, including original site code and assets
- Structural scrape rules and extracted values
- Settings and diagnostics counters

This data is available to anyone with access to your browser profile. Cookies and state archives are often equivalent to live account credentials.

## What NetHarvester transmits

**Nothing automatically.** The extension makes no telemetry, analytics, update-check, AI, cloud sync, CDN, or third-party API requests. The manifest declares `browser_specific_settings.gecko.data_collection_permissions.required = ["none"]`.

Two user-initiated features talk to servers:

1. **Replay/fuzzing** sends requests you explicitly compose or generate through the inspected page or extension background fetch. Those requests go to the URLs you target.
2. **Mock server** intercepts matching page requests locally; no matching request leaves the browser. Unmatched requests behave normally.
3. **SAVE PAGE** fetches missing page assets from the target site's own origins, using the configured host permissions. The generated archive is written to your Downloads folder and is never uploaded by NetHarvester.

NetHarvester can generate a dependency-free Node.js loopback mock server. That file only runs if you choose to execute it outside the extension; it binds `127.0.0.1` by default and transmits no data to the maintainer.

Captures and archives are never uploaded.

## What third parties can see

NetHarvester does not hide its presence from the pages being inspected. Its MAIN-world hooks are observable JavaScript; sophisticated sites can detect wrapped APIs. The extension adds no remote calls that would reveal it to a network observer.

## User controls

- Pause capture from Overview settings.
- Toggle body, header, dedupe, stream, and auto-heal capture separately.
- Clear network captures by scope; clear streams/snapshots from their panels; clear all data via the extension's IndexedDB deletion in `about:debugging` (or use the UI clear actions).
- Configure per-response body caps, retention, and per-tab caps.
- Uninstall the extension to remove its local storage (browser-managed profile cleanup still applies).

## Handling recommendations

- Treat exported ZIPs, cURL commands, and Netscape cookie jars as bearer credentials.
- Do not paste sensitive captures into issue trackers.
- Use a dedicated browser profile for sensitive investigations.
- Clear evidence when an investigation is complete.
- Obtain authorization before fuzzing, probing, or replaying traffic against any system.

## Contact

This is a local project; there is no data controller or cloud backend. For code issues, use the repository's issue tracker without attaching unredacted captures or secrets.
