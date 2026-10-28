# Songs sheet music viewer: test preview

This is a temporary preview build of the `fix/mobile-zoom` branch of [Church-Life-Apps/Songs](https://github.com/Church-Life-Apps/Songs) (PR #310). It is only for testing on real phones. The live site, https://hymnsandspiritualsongs.com/, is unchanged.

Open: https://brotatotes.github.io/songs-zoom-preview/#/shl/6

To try it, switch a song to sheet music and tap the music. It opens full screen. Pinch or double tap to zoom, drag to move around, and close with the X or the back button. Page zoom is off, as on the live site.

## Viewer screenshots

Screenshots are under `test-results/viewer/`, named `<device>_<book>_<step>.png`:

- Devices: `iphone` (WebKit, iPhone 15), `pixel` (Chromium, Pixel 7) and `desktop` (Chromium, 1366×768)
- Steps: `3_viewer_open`, `4_viewer_zoomed`, `5_closed`

Examples:

- https://brotatotes.github.io/songs-zoom-preview/test-results/viewer/iphone_shl_3_viewer_open.png
- https://brotatotes.github.io/songs-zoom-preview/test-results/viewer/iphone_shl_4_viewer_zoomed.png
- https://brotatotes.github.io/songs-zoom-preview/test-results/viewer/pixel_sfog_3_viewer_open.png
- https://brotatotes.github.io/songs-zoom-preview/test-results/viewer/desktop_shl_4_viewer_zoomed.png

`test-results/TESTING.md` and `test-results/old|new/` are from the earlier page-zoom design. That design has been replaced, and those files are kept for reference only.
