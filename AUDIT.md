# Field Ops Admin — Full Audit — 2026-10-06

## Baseline
- Production baseline: `main`
- Improvement branch: `audit-improved-v1`
- Backend/API contract intentionally preserved.

## Findings

### P0 — PWA cache/update reliability
The previous service worker used a manually versioned cache and intercepted broad GET traffic. The improved worker uses network-first navigation with `cache: no-store`, cleans previous FOS caches, pre-caches the complete declared icon set, and excludes Google Apps Script endpoints.

### P1 — Monolithic frontend
The application is concentrated in `index.html`, including generated utility CSS, custom CSS, compiled React/JSX, screens, API logic and gesture handling.

**Recommendation:** keep the current runtime for this safe pass. A later Vite/module split should be done only after functional validation.

### P1 — CSS/build maintenance
Manual utility patches exist because the generated utility set does not contain every class used by the application. This makes future UI changes harder to verify.

### P1 — Gesture complexity
Pull-to-refresh and horizontal tab swipes coexist with vertical scrolling and nested controls. The current axis-locking approach is sensible; device testing should precede changing thresholds.

### P1 — API refresh architecture
The app already uses one `getAdminDashboardBundle` request rather than five simultaneous Apps Script calls and blocks overlapping refreshes. This is a strong performance improvement and should be retained.

### P2 — Error-state UX
The app has toast errors and connection diagnostics. The next pass should distinguish stale data, timeout, backend failure and offline state.

### P2 — Accessibility
The first hardening pass now adds visible keyboard focus, dark PWA chrome consistency and reduced-motion support. Modal focus management and icon-only labels should still be tested screen-by-screen.

## Changes in audit-improved-v1
- PWA service worker v8 reliability pass
- manifest start URL normalization
- complete icon pre-cache
- dark PWA theme consistency
- visible keyboard focus states
- reduced-motion support
- no backend/API contract changes

## Validation required before merge
- fresh install/update
- offline shell
- Hours / Visits / Summary / Attendance / Beat Plan
- attendance save and report
- Settings/Diagnose
- pull-to-refresh
- horizontal swipe
- mobile Chrome/Safari
- desktop/touch device
