# ink-validation

A test page for the worksheet-ink storage change (exam-drill-kit pull request #81). It is for trying the change on real phones and tablets. It holds **synthetic data only** and has nothing to do with any real study progress.

The page is `index.html`, at `https://louis-drill-tests.github.io/ink-validation/`.

## What the page is

- The core of the exam-drill-kit pull request #81 branch (`claude/elegant-ptolemy-pqxb8y-idb-ink`), built from core commit `a3a71ed` (the branch head is `7dc982a`, which only adds this page and its README), merged with `main` at 64302e2.
- The Medicinal-chemistry course content at commit `97ecf8b` (public repository `lglouis3-create/Medicinal-chemistry`).
- Built by `review/ink-idb/make_device_page.js` through `review/ink-idb/device-page/build.sh` in that kit branch. The page uses the storage namespace `ink-validation` and a validation panel.
- Size: 7,476,975 bytes.

The page keeps its ink in IndexedDB, which is the change under test. It writes generated strokes straight into the page store. It does not go through a worksheet's draw or erase, so it checks storage and timing only. A check with a real worksheet draw and erase needs worksheets-ink (kit pull request #17, not merged) and stays open.

"Clean up" removes everything the page stored. Run the storage-full test only on this site, never on a real drill site.

## Checking that the published page is this file

Every build differs from another only in its `const BUILD = ` line (the time of the build). Compare the SHA-256 of the file with that line removed:

```
grep -v '^const BUILD = ' index.html | sha256sum
curl -s https://louis-drill-tests.github.io/ink-validation/ | grep -v '^const BUILD = ' | sha256sum
```

Both give `ef3230cb6ff7039bedf75e873ed09de92955df3310da9158a059f3fc05a94203`.

On 2026-11-19 the kit's `build.sh` was run again from the branch and gave the same value, so this file is what the branch builds. (An earlier version of this page, with fingerprint `bb0e6652…b47c`, carried pages from one sample into the next; it was replaced.)

## On the devices

Use an iPhone or iPad in Safari, and an Android phone in Chrome. One sample at a time:

1. Open the page and note the line at the top of the panel (browser, Event Timing yes or no, database).
2. Press **1. Answers at both caps**. Each "prepare" first erases the test profile, so a sample never carries the pages of an earlier one, then records a manifest (every page id, a hash of each page's content, the answer count and a hash of the answer list). Wait for "prepared".
3. Press **Run 3 rounds of 5 saves**. After each "tap the blue button now", tap the blue button about 8 times in 2 seconds. This takes about 45 seconds.
4. Reload the page and press **Check after reload**. The panel shows a PASS or FAIL line, and the numbers must be: sample 1, 0 pages and 0 strokes; sample 2, 69 pages and 2,085 strokes; sample 3, 171 pages and 5,145 strokes.
5. Repeat steps 2 to 4 with **2. + ink, 69 pages**, then **3. + ink, 171 pages** (no Clean up between them).
6. Press **Copy results** and paste the text to the kit session.
7. Only on this separate site: press **Storage-full test** (it asks first). It says PASS only when a write ended in the state `refused` (the browser's QuotaExceededError) and every page stored before the test still matches. It says FAIL when no write was refused, when a write ended in another state, or when a stored page differs. Then reload, press **Check after reload**, then **Copy results** again.
8. Press **Clean up**.

Provisional pass targets: for each sample and phone, the median save is 200 ms or less in each of the 3 rounds, the slowest tap is 200 ms or less, every check after a reload reads PASS, and the storage-full test reads PASS. A phone that misses a target is reported with its numbers.
