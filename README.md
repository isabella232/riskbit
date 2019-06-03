# riskbit

Debugging and optimizing Clearbit's Risk.js SDK, [see diff](https://github.com/AaronO/riskbit/compare/502ad85b277569dbf1990475ba7d752ae1f8cdfc...master#diff-e28b2440ab6163cd2335eb168e07d61c)

## Problem

Clearbit's `risk.js` web SDK adds a significant CPU overhead to page loads that is proportionnal to the page's / DOM's complexity (so it will be significantly slower on large pages).

The overhead originates from the browser fingerprinting logic's inefficient DOM I/O when detecting features (fonts, system colors, ...)

## Datapoints

1. On https://clearbit.com/risk
    * **Before:** `150-300ms`
    * **After:** `15-30ms`
2. On https://app.gitbook.com/ (when authenticated)
    * **Before:** `500ms-1000ms`
    * **After:** `30-60ms`

## Steps to reproduce

Here's how I'm profiling load times and clearbit's `risk.js` impact on load:

1. Go to to https://clearbit.com/risk (or any other site that uses `risk.js`)
2. Open DevTools
3. Go to `Performance` tab
4. Hit the reload (and profile) icon in the DevTools
5. Search for `a.calculate` or `getCSSFonts` in the profiler's event log

## Steps to test fix

1. Open DevTools
2. Except go to `Sources > Overrides` and select the root folder of this repo (make sure to comfirm the `Allow` prompt)
3. Redo steps `1-5` from above, buth with the override enabled

## Diff

You can see the code I added by looking at [this diff](https://github.com/AaronO/riskbit/compare/502ad85b277569dbf1990475ba7d752ae1f8cdfc...master#diff-e28b2440ab6163cd2335eb168e07d61c)

Since it isn't open-source and I'm working off reversing the production bundle, the code isn't as clean as it should be, but it should be enough for you to reimplement or copy the improvements into the original source.

## Screenshots

Some screenshots of the DevTools and flamecharts:

### Before

![Before](/screenshots/clearbit_before.png)

### After

![After](/screenshots/clearbit_after.png)
