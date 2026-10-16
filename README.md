# Image Viewer for Rigee

A [Rigee](https://github.com/Qyuan-Tech/rigee) plugin: PNG, JPEG, GIF, WebP, AVIF, BMP and ICO
images open in a viewer instead of the text editor — fitted to the pane, with zoom (buttons, `+`
`−` `0` `1`, Ctrl/Cmd and the wheel, a pinch) that keeps the pixel under the pointer where it was,
panning by dragging, and pixels drawn as squares from 300% up.

## Installing

In Rigee, open the **Plugins** view, search for `image` and install **Qyuan-Tech/rigee-plugin-image**.
Rigee downloads the latest release, checks it against the checksum GitHub reports for it, and asks
before anything runs. It needs Rigee 1.1.0 or later.

What it declares: **read files on this computer** — the image being opened, read in Rigee's main
process and handed to the window. As with every Rigee plugin, that is a statement of what it is
for, not a limit Rigee enforces.

## What it opens, and what it does not

**SVG is deliberately not claimed.** It is source people edit, and opening it as a picture would
take the text away — VS Code makes the same call. **TIFF and HEIC** are not claimed because
Chromium cannot draw them, and a viewer that opened only to say so would be worse than the text
editor's honest refusal. Files over 100 MB are refused: past any photograph or screenshot, the bytes
are cloned to the window and decoded there all at once.

## How it is built

Two halves, because Electron leaves no choice: the bytes can only be read in the main process, and
only the window can draw them. Chromium decodes every format claimed, so the window's half is a few
kilobytes of toolbar around an `<img>`.

```
src/protocol.ts           the one command the halves share, typed for both
src/main/index.ts         main half: one bounded read
src/renderer/index.tsx    renderer half: binds the editor
```

```bash
pnpm install
pnpm run typecheck
pnpm run build          # dist/main.js and dist/renderer.js
npm pack                # rigee-plugin-image-<version>.tgz, the release asset
```

To try a build in Rigee without publishing it, unpack the tarball's `package/` into
`<userData>/plugins/image/` and approve it in the Plugins view.

## Releasing

Set the same version in `version` and `rigee.version`, commit, and push a tag `v<version>`. The
release workflow checks the three agree, builds, packs, and publishes a release carrying
`rigee-plugin-image-<version>.tgz` — the one asset Rigee installs from.

## Licence

MIT
