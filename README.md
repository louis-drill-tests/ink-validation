# ink-validation

A test page for the worksheet-ink storage change (exam-drill-kit pull request #81). It is for trying the change on real phones and tablets. It holds **synthetic data only** and has nothing to do with any real study progress.

The page is `index.html`. Once GitHub Pages is on, it is at `https://louis-drill-tests.github.io/ink-validation/`.

## What the page is

- The core of the exam-drill-kit pull request #81 branch (`claude/elegant-ptolemy-pqxb8y-idb-ink`), at commit `f15fe3b`.
- The Medicinal-chemistry course content at commit `97ecf8b` (public repository `lglouis3-create/Medicinal-chemistry`).
- Built by `review/ink-idb/make_device_page.js` through `review/ink-idb/device-page/build.sh` in that kit branch. The page uses the storage namespace `ink-validation` and a validation panel at the top.
- Size: 7,460,665 bytes.

The page keeps its ink in IndexedDB, which is the change under test. "Clean up" in the panel removes everything the page stored. Run the storage-full test only on this site, never on a real drill site.

## Checking that the published page is this file

Every build differs from another only in its `const BUILD = ` line (the time of the build). Compare the SHA-256 of the file with that line removed:

```
grep -v '^const BUILD = ' index.html | sha256sum
curl -s https://louis-drill-tests.github.io/ink-validation/ | grep -v '^const BUILD = ' | sha256sum
```

Both give `bb0e6652de7c10b6a98e579887ad4ec033a61d4bc1b232f259d0e4a2f466b47c`.

On 2026-11-18 the page was rebuilt from the branch head `f15fe3b` and gave the same value, so this file is what that head builds.

## On the devices

Use an iPhone or iPad in Safari, and an Android phone in Chrome. The steps are in section 6 of `review/ink-idb/README.md` in the kit branch. In short, for each of the three samples: prepare it, run 3 rounds of 5 saves (tap the blue button about 8 times after each save), reload, and press Check. Then run the storage-full test, press Copy results, and press Clean up.

The panel's top line shows the browser, Event Timing support and the database state. Send that line with the results.
