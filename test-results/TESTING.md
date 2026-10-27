# Testing: allow zoom on mobile without accidental song changes

Branch `fix/mobile-zoom`. All automated checks ran on 2026-10-27 against two local production builds served with the same hash routing as the live site:

- **Old**: current `master`, byte-identical to the live site (`main.f9f45417.chunk.js`)
- **New**: this branch

Devices (Playwright 1.59 device profiles):

- **iPhone 15**: WebKit, mobile emulation, iOS Safari user agent
- **Pixel 7**: Chromium, mobile emulation, Android Chrome user agent
- **Desktop**: Chromium, 1366×768

**Result: the new build passed 65 of 65 checks.** On the old build, 32 of 57 applicable checks passed. Every old-build failure is one of the zoom or double-tap problems this PR fixes. Desktop and all no-zoom behavior were already passing and still pass.

The harness is in `docs/pr-assets/harness/`. All 148 screenshots, before and after, are published with the preview at
<https://brotatotes.github.io/songs-zoom-preview/test-results/>. The paths are `old|new/<device>/<case>.png`, and the "view" links below point there.

## Other checks

- `npm install -force && npm run build` passes on Node 16.20.2 ("Compiled successfully", no warnings), as in CI.
- Prettier 1.19.1 with the repo config, applied to every changed file. ESLint with the repo config reports no errors on the changed files.
- Unit tests in `src/test/utils` pass: 5 suites, 10 tests.
- The repo's own Puppeteer end-to-end suite, run against the new build with the system Chromium, passed 119 of 122 tests:
  - Both "Infinite scroll … loads the full song list" tests fail identically on the old build, so this PR did not cause them.
  - One feedback-form test timed out once while two browser test runs were sharing the machine. It passed 3 of 3 times on its own.
  - All swipe and double-click tests pass, including "swipe right at song 1 does not navigate below 1".

## Results

A = unchanged no-zoom behavior · B = zoomed / multi-touch · C = sheet-music double tap · D = input focus zoom · E = desktop

| Case | Device | Book | Old (master/live) | New (this PR) | New result detail | Screenshot |
|---|---|---|---|---|---|---|
| A1 swipe left/right on lyrics | iPhone 15 (WebKit) | SFOG | pass | PASS | 5 -> 6 -> 5 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/iphone/a1_swipe_left_lyrics_sfog.png) |
| A1 swipe left/right on lyrics | iPhone 15 (WebKit) | SHL | pass | PASS | 5 -> 6 -> 5 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/iphone/a1_swipe_left_lyrics_shl.png) |
| A1 swipe left/right on lyrics | Pixel 7 (Chromium) | SFOG | pass | PASS | 5 -> 6 -> 5 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/a1_swipe_left_lyrics_sfog.png) |
| A1 swipe left/right on lyrics | Pixel 7 (Chromium) | SHL | pass | PASS | 5 -> 6 -> 5 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/a1_swipe_left_lyrics_shl.png) |
| A2 swipe left/right on sheet music | iPhone 15 (WebKit) | SFOG | pass | PASS | 5 -> 6 -> 5 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/iphone/a2_swipe_music_sfog.png) |
| A2 swipe left/right on sheet music | iPhone 15 (WebKit) | SHL | pass | PASS | 5 -> 6 -> 5 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/iphone/a2_swipe_music_shl.png) |
| A2 swipe left/right on sheet music | Pixel 7 (Chromium) | SFOG | pass | PASS | 5 -> 6 -> 5 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/a2_swipe_music_sfog.png) |
| A2 swipe left/right on sheet music | Pixel 7 (Chromium) | SHL | pass | PASS | 5 -> 6 -> 5 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/a2_swipe_music_shl.png) |
| A3 first/last song edges | iPhone 15 (WebKit) | SFOG | pass | PASS | 1 -> 1, 280 -> 280 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/iphone/a3_last_song_edge_sfog.png) |
| A3 first/last song edges | iPhone 15 (WebKit) | SHL | pass | PASS | 1 -> 1, 533 -> 533 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/iphone/a3_last_song_edge_shl.png) |
| A3 first/last song edges | Pixel 7 (Chromium) | SFOG | pass | PASS | 1 -> 1, 280 -> 280 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/a3_last_song_edge_sfog.png) |
| A3 first/last song edges | Pixel 7 (Chromium) | SHL | pass | PASS | 1 -> 1, 533 -> 533 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/a3_last_song_edge_shl.png) |
| A4 header: toggle music/lyrics, download, back | iPhone 15 (WebKit) | SFOG | pass | PASS | music=True download=['sfog_12', 'blob:'] lyrics=True back->/sfog | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/iphone/a4_header_music_sfog.png) |
| A4 header: toggle music/lyrics, download, back | iPhone 15 (WebKit) | SHL | pass | PASS | music=True download=['shl_12', 'blob:'] lyrics=True back->/shl | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/iphone/a4_header_music_shl.png) |
| A4 header: toggle music/lyrics, download, back | Pixel 7 (Chromium) | SFOG | pass | PASS | music=True download=['sfog_12', 'blob:'] lyrics=True back->/sfog | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/a4_header_music_sfog.png) |
| A4 header: toggle music/lyrics, download, back | Pixel 7 (Chromium) | SHL | pass | PASS | music=True download=['shl_12', 'blob:'] lyrics=True back->/shl | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/a4_header_music_shl.png) |
| A5 song-number search (type 123, open) | iPhone 15 (WebKit) | SHL | pass | PASS | first result '123. O Worship The King', url song 123, scale on focus 1 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/iphone/a5_search_shl.png) |
| A5 song-number search (type 123, open) | Pixel 7 (Chromium) | SHL | pass | PASS | first result '123. O Worship The King', url song 123, scale on focus 1 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/a5_search_shl.png) |
| A6 vertical scroll of a long song | iPhone 15 (WebKit) | SHL | pass | PASS | scrollTop 0 -> 350, song 260, overflow-y/touch-action/scrollH/clientH ['auto', 'pan-y pinch-zoom', 2751, 615] _WebKit: checked overflow-y/touch-action/scrollHeight, then scrolled by script (no native touch scroll in the simulator)_ | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/iphone/a6_vertical_scroll_shl.png) |
| A6 vertical scroll of a long song | Pixel 7 (Chromium) | SHL | pass | PASS | scrollTop 0 -> 534, song 260 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/a6_vertical_scroll_shl.png) |
| A7 vertical scroll with slight sideways drift | iPhone 15 (WebKit) | SFOG | pass | PASS | 226, 226 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/iphone/a7_vertical_drift_sfog.png) |
| A7 vertical scroll with slight sideways drift | iPhone 15 (WebKit) | SHL | pass | PASS | 260, 260 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/iphone/a7_vertical_drift_shl.png) |
| A7 vertical scroll with slight sideways drift | Pixel 7 (Chromium) | SFOG | pass | PASS | 226, 226 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/a7_vertical_drift_sfog.png) |
| A7 vertical scroll with slight sideways drift | Pixel 7 (Chromium) | SHL | pass | PASS | 260, 260 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/a7_vertical_drift_shl.png) |
| B1 swipe while zoomed (scale stubbed to 2) on lyrics, then zoomed out | iPhone 15 (WebKit) | SFOG | **fail**: zoomed: 6,5; zoomed out: 6 | PASS | zoomed: 5,5; zoomed out: 6 _visualViewport.scale stubbed; real pinch tested separately in Chromium_ | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/iphone/b1_zoomed_stub_swipe_lyrics_sfog.png) |
| B1 swipe while zoomed (scale stubbed to 2) on lyrics, then zoomed out | iPhone 15 (WebKit) | SHL | **fail**: zoomed: 6,5; zoomed out: 6 | PASS | zoomed: 5,5; zoomed out: 6 _visualViewport.scale stubbed; real pinch tested separately in Chromium_ | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/iphone/b1_zoomed_stub_swipe_lyrics_shl.png) |
| B1 swipe while zoomed (scale stubbed to 2) on lyrics, then zoomed out | Pixel 7 (Chromium) | SFOG | **fail**: zoomed: 6,5; zoomed out: 6 | PASS | zoomed: 5,5; zoomed out: 6 _visualViewport.scale stubbed; real pinch tested separately in Chromium_ | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/b1_zoomed_stub_swipe_lyrics_sfog.png) |
| B1 swipe while zoomed (scale stubbed to 2) on lyrics, then zoomed out | Pixel 7 (Chromium) | SHL | **fail**: zoomed: 6,5; zoomed out: 6 | PASS | zoomed: 5,5; zoomed out: 6 _visualViewport.scale stubbed; real pinch tested separately in Chromium_ | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/b1_zoomed_stub_swipe_lyrics_shl.png) |
| B1 swipe while zoomed (scale stubbed to 2) on music, then zoomed out | iPhone 15 (WebKit) | SHL | **fail**: zoomed: 6,5; zoomed out: 6 | PASS | zoomed: 5,5; zoomed out: 6 _visualViewport.scale stubbed; real pinch tested separately in Chromium_ | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/iphone/b1_zoomed_stub_swipe_music_shl.png) |
| B1 swipe while zoomed (scale stubbed to 2) on music, then zoomed out | Pixel 7 (Chromium) | SHL | **fail**: zoomed: 6,5; zoomed out: 6 | PASS | zoomed: 5,5; zoomed out: 6 _visualViewport.scale stubbed; real pinch tested separately in Chromium_ | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/b1_zoomed_stub_swipe_music_shl.png) |
| B2 real pinch zoom (Chromium) on lyrics, pan, zoom out, swipe | Pixel 7 (Chromium) | SFOG | **fail**: scale 1.00; pans -> 6,5,6; scale after zoom-out 1.00; swipe -> 7 | PASS | scale 2.00; pans -> 5,5,5; scale after zoom-out 1.00; swipe -> 6 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/b2_real_pinch_lyrics_sfog.png) |
| B2 real pinch zoom (Chromium) on lyrics, pan, zoom out, swipe | Pixel 7 (Chromium) | SHL | **fail**: scale 1.00; pans -> 6,5,6; scale after zoom-out 1.00; swipe -> 7 | PASS | scale 2.00; pans -> 5,5,5; scale after zoom-out 1.00; swipe -> 6 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/b2_real_pinch_lyrics_shl.png) |
| B2 real pinch zoom (Chromium) on music, pan, zoom out, swipe | Pixel 7 (Chromium) | SHL | **fail**: scale 1.00; pans -> 6,5,6; scale after zoom-out 1.00; swipe -> 7 | PASS | scale 2.00; pans -> 5,5,5; scale after zoom-out 1.00; swipe -> 6 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/b2_real_pinch_music_shl.png) |
| B3 two-finger sequences on lyrics | iPhone 15 (WebKit) | SFOG | **fail**: [6, 7, 6]; then 1-finger -> 7 | PASS | [5, 5, 5]; then 1-finger -> 6 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/iphone/b3_two_finger_lyrics_sfog.png) |
| B3 two-finger sequences on lyrics | iPhone 15 (WebKit) | SHL | **fail**: [6, 7, 6]; then 1-finger -> 7 | PASS | [5, 5, 5]; then 1-finger -> 6 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/iphone/b3_two_finger_lyrics_shl.png) |
| B3 two-finger sequences on lyrics | Pixel 7 (Chromium) | SFOG | **fail**: [6, 7, 6]; then 1-finger -> 7 | PASS | [5, 5, 5]; then 1-finger -> 6 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/b3_two_finger_lyrics_sfog.png) |
| B3 two-finger sequences on lyrics | Pixel 7 (Chromium) | SHL | **fail**: [6, 7, 6]; then 1-finger -> 7 | PASS | [5, 5, 5]; then 1-finger -> 6 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/b3_two_finger_lyrics_shl.png) |
| B3 two-finger sequences on music | iPhone 15 (WebKit) | SHL | **fail**: [6, 7, 6]; then 1-finger -> 7 | PASS | [5, 5, 5]; then 1-finger -> 6 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/iphone/b3_two_finger_music_shl.png) |
| B3 two-finger sequences on music | Pixel 7 (Chromium) | SHL | **fail**: [6, 7, 6]; then 1-finger -> 7 | PASS | [5, 5, 5]; then 1-finger -> 6 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/b3_two_finger_music_shl.png) |
| B4 one-finger pan moves around the pinch-zoomed page | Pixel 7 (Chromium) | SFOG | **fail**: scale 1.00; offsetLeft 0 -> 0 -> 0; song 5 | PASS | scale 2.00; offsetLeft 103 -> 206 -> 0; song 5 _real synthesized pinch_ | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/b4_zoomed_pan_sfog.png) |
| B4 one-finger pan moves around the pinch-zoomed page | Pixel 7 (Chromium) | SHL | **fail**: scale 1.00; offsetLeft 0 -> 0 -> 0; song 5 | PASS | scale 2.00; offsetLeft 103 -> 206 -> 0; song 5 _real synthesized pinch_ | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/b4_zoomed_pan_shl.png) |
| C1 double tap enlarges sheet music | iPhone 15 (WebKit) | SFOG | **fail**: rendered 393 -> 393px (style 393px, max-width 100%), scrollLeft 0 | PASS | rendered 393 -> 786px (style 786px, max-width none), scrollLeft 275 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/iphone/c1_music_double_tap_enlarged_sfog.png) |
| C1 double tap enlarges sheet music | iPhone 15 (WebKit) | SHL | **fail**: rendered 393 -> 393px (style 393px, max-width 100%), scrollLeft 0 | PASS | rendered 393 -> 786px (style 786px, max-width none), scrollLeft 275 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/iphone/c1_music_double_tap_enlarged_shl.png) |
| C1 double tap enlarges sheet music | Pixel 7 (Chromium) | SFOG | **fail**: rendered 412 -> 412px (style 824px, max-width 100%), scrollLeft 0 | PASS | rendered 412 -> 824px (style 824px, max-width none), scrollLeft 288 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/c1_music_double_tap_enlarged_sfog.png) |
| C1 double tap enlarges sheet music | Pixel 7 (Chromium) | SHL | **fail**: rendered 412 -> 412px (style 824px, max-width 100%), scrollLeft 0 | PASS | rendered 412 -> 824px (style 824px, max-width none), scrollLeft 288 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/c1_music_double_tap_enlarged_shl.png) |
| C2 sideways pans/flicks while enlarged | iPhone 15 (WebKit) | SFOG | — | PASS | songs 5,5,5; scrollLeft 275->275; scrollWidth 786 > clientWidth 393, overflow-x auto _WebKit synthetic touches don't scroll natively; scrollability checked via overflow_ | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/iphone/c2_music_enlarged_after_pans_sfog.png) |
| C2 sideways pans/flicks while enlarged | iPhone 15 (WebKit) | SHL | — | PASS | songs 5,5,5; scrollLeft 275->275; scrollWidth 786 > clientWidth 393, overflow-x auto _WebKit synthetic touches don't scroll natively; scrollability checked via overflow_ | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/iphone/c2_music_enlarged_after_pans_shl.png) |
| C2 sideways pans/flicks while enlarged | Pixel 7 (Chromium) | SFOG | — | PASS | songs 5,5,5; scrollLeft 288->412; scrollWidth 824 > clientWidth 412, overflow-x auto | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/c2_music_enlarged_after_pans_sfog.png) |
| C2 sideways pans/flicks while enlarged | Pixel 7 (Chromium) | SHL | — | PASS | songs 5,5,5; scrollLeft 288->412; scrollWidth 824 > clientWidth 412, overflow-x auto | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/c2_music_enlarged_after_pans_shl.png) |
| C3 second double tap restores; swipe works again | iPhone 15 (WebKit) | SFOG | — | PASS | width 393, class 'song-page-center', scrollLeft 0; swipe -> 6 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/iphone/c3_music_restored_sfog.png) |
| C3 second double tap restores; swipe works again | iPhone 15 (WebKit) | SHL | — | PASS | width 393, class 'song-page-center', scrollLeft 0; swipe -> 6 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/iphone/c3_music_restored_shl.png) |
| C3 second double tap restores; swipe works again | Pixel 7 (Chromium) | SFOG | — | PASS | width 412, class 'song-page-center', scrollLeft 0; swipe -> 6 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/c3_music_restored_sfog.png) |
| C3 second double tap restores; swipe works again | Pixel 7 (Chromium) | SHL | — | PASS | width 412, class 'song-page-center', scrollLeft 0; swipe -> 6 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/c3_music_restored_shl.png) |
| C4 switch to lyrics while enlarged, swipe, switch back | iPhone 15 (WebKit) | SHL | **fail**: enlarged 393; lyrics scrollWidth/clientWidth/scrollLeft/overflow [393, 393, 0, 'hidden'];  | PASS | enlarged 786; lyrics scrollWidth/clientWidth/scrollLeft/overflow [393, 393, 0, 'hidden']; swipe -> 6; music back 393 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/iphone/c4_toggle_to_lyrics_while_enlarged_shl.png) |
| C4 switch to lyrics while enlarged, swipe, switch back | Pixel 7 (Chromium) | SHL | **fail**: enlarged 412; lyrics scrollWidth/clientWidth/scrollLeft/overflow [412, 412, 0, 'hidden'];  | PASS | enlarged 824; lyrics scrollWidth/clientWidth/scrollLeft/overflow [412, 412, 0, 'hidden']; swipe -> 6; music back 412 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/c4_toggle_to_lyrics_while_enlarged_shl.png) |
| C5 rotate to landscape and back (normal and enlarged) | iPhone 15 (WebKit) | SHL | **fail**: portrait 393; landscape 393; landscape enlarged 393; portrait enlarged 393; restored 393;  | PASS | portrait 393; landscape 659; landscape enlarged 1318; portrait enlarged 786; restored 393; swipe -> 6 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/iphone/c5_landscape_enlarged_shl.png) |
| C5 rotate to landscape and back (normal and enlarged) | Pixel 7 (Chromium) | SHL | **fail**: portrait 412; landscape 412; landscape enlarged 824; portrait enlarged 412; restored 412;  | PASS | portrait 412; landscape 839; landscape enlarged 1678; portrait enlarged 824; restored 412; swipe -> 6 | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/c5_landscape_enlarged_shl.png) |
| C6 two-finger taps on music do not enlarge | iPhone 15 (WebKit) | SHL | pass | PASS | 393 -> 393 |  |
| C6 two-finger taps on music do not enlarge | Pixel 7 (Chromium) | SHL | pass | PASS | 412 -> 412 |  |
| C7 two slow taps (700 ms apart) do not enlarge | iPhone 15 (WebKit) | SHL | pass | PASS | 393 -> 393 |  |
| C7 two slow taps (700 ms apart) do not enlarge | Pixel 7 (Chromium) | SHL | pass | PASS | 412 -> 412 |  |
| D1 search + feedback inputs: font-size >= 16px, no zoom on focus | iPhone 15 (WebKit) | SHL | pass | PASS | sizes ['17px', '17px', '17px', '17px']; scale after focus [1, 1, 1] | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/iphone/d1_feedback_focused_shl.png) |
| D1 search + feedback inputs: font-size >= 16px, no zoom on focus | Pixel 7 (Chromium) | SHL | pass | PASS | sizes ['16px', '16px', '16px', '16px']; scale after focus [1, 1, 1] | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/pixel/d1_feedback_focused_shl.png) |
| E1 desktop: prev/next buttons, double-click enlarge/restore, mouse drag doesn't navigate | Desktop (Chromium) | SFOG | pass | PASS | fabs=True; widths 683px(683) -> 1366px(1366) -> 683px(683); class 'song-page-center'; drag stays=True | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/desktop/e1_music_dblclick_sfog.png) |
| E1 desktop: prev/next buttons, double-click enlarge/restore, mouse drag doesn't navigate | Desktop (Chromium) | SHL | pass | PASS | fabs=True; widths 683px(683) -> 1366px(1366) -> 683px(683); class 'song-page-center'; drag stays=True | [view](https://brotatotes.github.io/songs-zoom-preview/test-results/new/desktop/e1_music_dblclick_shl.png) |

## How each kind of check was simulated

- **Swipes on Pixel 7** use real, trusted touch events through Chrome DevTools Protocol `Input.dispatchTouchEvent`. These also drive native scrolling.
- **Swipes on iPhone (WebKit)** can't be produced as trusted touches in Playwright WebKit. The harness dispatches `TouchEvent`s built with `document.createTouch`/`createTouchList`. Ionic's gesture code and the app's own handlers receive them the same way as real touches. The browser doesn't scroll from them, so case A6 on iPhone checks that the content is vertically scrollable (`overflow-y: auto`, `touch-action` includes `pan-y`, 2751 px of content in a 615 px area) and then scrolls by script.
- **Pinch zoom on Pixel 7 (B2, B4)** uses a real synthesized pinch (`Input.synthesizePinchGesture`). On the old build the pinch is refused (scale stays 1.00). On the new build the page zooms to 2.00, and one-finger drags then move around the zoomed page without changing the song.
- **Zoomed state on iPhone (B1)**: WebKit in Playwright can't pinch, so `visualViewport.scale` is stubbed to 2 to exercise the "don't swipe while zoomed" gate, then set back to 1 to confirm that swiping returns.
- **Double tap (C)** uses Playwright `touchscreen.tap` twice, 90 ms apart, on both phones.
- **Rotation (C5)** resizes the viewport to landscape and back with `set_viewport_size`.

## What the simulators can't prove (please test on real phones)

1. **A real two-finger pinch on an iPhone.** WebKit here can't pinch, and removing `user-scalable=no` is the change that matters on iOS. Recent iOS Safari versions often ignore `user-scalable=no` for accessibility, but behavior varies by version and in home-screen mode. The old song-page CSS (`touch-action: pan-y`) also blocked pinch, which was confirmed in Chromium. Only a real iPhone can confirm that pinch now works and feels right.
2. **Home-screen ("Add to Home Screen") mode.** The app declares `apple-mobile-web-app-capable`. Standalone mode can't be emulated. Please check pinch, swipe and double tap from the home-screen icon on an iPhone.
3. **Real finger timing and accuracy.** The double-tap window is 300 ms and 30 px. Swipe thresholds are unchanged. Real fingers are less precise than scripted ones.
4. **Edge-swipe "back" gestures.** iOS Safari and Android gesture navigation have their own edge swipes. The no-zoom swipe area keeps the same `touch-action` behavior as before (browser handles vertical only), so this should be unchanged, but it can only be confirmed by hand.
5. **Rotation on a real phone.** This fires real orientation events. Here it was simulated by resizing the viewport.
6. **The native Capacitor iOS/Android apps** (`ios/`, `android/`) were not built or tested. This PR targets the website.
7. **Other browsers** such as Samsung Internet and Firefox for Android were not tested.
