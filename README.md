# 3D Media Player

A single-file, offline 3D video player for the browser. Open `index.html`, pick a video, and watch it in 3D with red/cyan glasses, red/blue glasses, or on a passive polarized 3D display. Everything runs locally in your browser with WebGL. Nothing is uploaded and there are no dependencies or build steps.

![3D Media Player](screenshots/player.png)

## Features

- **3D input modes**
  - Side-by-Side (SBS / Half-SBS)
  - Top-Bottom (TAB / Over-Under)
  - Normal 2D → 3D (auto), a fake depth effect for regular movies
  - Original (3D off)
- **Output styles**
  - Red / Cyan (sky blue)
  - Red / Blue (balanced)
  - Optimized (less ghosting)
  - Black & White
  - Polarized (row-interleaved) for passive polarized 3D TVs and monitors
- Swap left/right eye and adjustable 3D depth
- Playlist with multiple files, drag and drop, and repeat off / all / one
- Playback speed, volume, mute, seek bar, and keyboard shortcuts
- Fullscreen and a landscape mode for phones
- Works on desktop and mobile browsers

| Menu | Mobile |
|------|--------|
| ![Video menu](screenshots/menu.png) | <img src="screenshots/mobile.png" width="220" alt="Mobile view"> |

## Quick start

1. Download or clone this repository.
2. Open `index.html` in Chrome, Edge, Firefox, or Safari.
3. Choose **Media → Open File(s)** or drag a video onto the player.
4. Pick your 3D mode and style from the **Video** menu.
5. Put on your glasses and press `F` for fullscreen.

You can try it right away with the included test clip: `samples/sample_sbs_3d.webm` is a side-by-side 3D scene. Open it and choose **Video → Side-by-Side 3D → Red/Cyan**.

### GitHub Pages (optional)

Enable **Settings → Pages → Deploy from a branch → main / (root)** and the player will be available at `https://<your-username>.github.io/<repo-name>/`. Videos are still played locally from your own device.

## Choosing the right mode

| Your video | Mode to pick |
|------------|--------------|
| 3D movie, Side-by-Side (SBS, Half-SBS) | Side-by-Side 3D |
| 3D movie, Top-Bottom (TAB, OU) | Top-Bottom 3D |
| Normal 2D movie | Normal 2D → 3D (auto). This only adds a mild depth effect. |

If the depth looks inverted, use **Video → Swap Left/Right Eye**.

## Glasses and displays

| Setup | Style to use |
|-------|--------------|
| Red/cyan glasses (any screen or projector) | Red / Cyan, Optimized, or Black & White |
| Red/blue glasses | Red / Blue (balanced) |
| Passive polarized 3D TV or monitor (e.g. LG Cinema 3D) | Polarized (row-interleaved) |

The left eye is the red lens. Anaglyph modes work on any normal screen or projector, since they only use colour.

### Polarized mode notes

- It only works on a **passive polarized 3D display**. A normal screen will just show a striped, doubled picture.
- Use **fullscreen at the display's native resolution** and **100% browser zoom**. Any scaling breaks the row alignment and causes ghosting.
- Some displays expect left on the even rows and others on the odd rows. If the depth is inverted, use Swap Left/Right Eye.
- Dual-projector polarized setups need separate left/right outputs and are not supported.

## Using a projector

Connect your laptop or phone to the projector with HDMI (or screen mirroring), open a video, and press fullscreen. Dim the room and adjust the projector's colour and brightness to reduce ghosting with anaglyph glasses.

## Keyboard shortcuts

| Key | Action |
|-----|--------|
| `Space` | Play / Pause |
| `S` | Stop |
| `←` `→` | Seek 5 seconds |
| `↑` `↓` | Volume |
| `N` / `P` | Next / Previous |
| `M` | Mute |
| `F` | Fullscreen |
| `[` `]` | Slower / Faster |
| `=` | Normal speed |
| `Ctrl+O` | Open file |
| `Esc` | Exit fullscreen, close menus |

## Supported formats and limits

- Playback uses your browser's built-in decoder. **MP4 (H.264) and WebM** work everywhere. **MKV and H.265 (HEVC)** often don't play. Convert them to MP4 (H.264) first, for example with HandBrake or FFmpeg.
- Only local files are supported. Streaming services and DRM-protected content (Netflix, Prime Video, etc.) cannot be played.
- 4K video can be heavy on phones because each frame is uploaded to the GPU. 1080p H.264 is the smoothest.
- Anaglyph output cannot reproduce full colour. Red/cyan keeps more colour than red/blue.

Convert a video with FFmpeg:

```bash
ffmpeg -i input.mkv -c:v libx264 -crf 20 -c:a aac output.mp4
```

## Project structure

```
index.html            The entire player (HTML, CSS, JavaScript, WebGL shader)
samples/              Test clip (synthetic side-by-side 3D scene)
screenshots/          Images used in this README
```

## How it works

The video is uploaded as a WebGL texture each frame. A fragment shader samples the left and right eye views (from the SBS or TAB layout, or a horizontal shift for 2D → 3D), then mixes them into the selected output: colour channels for anaglyph, or alternating screen rows for polarized displays.

## Contributing

Issues and pull requests are welcome. Bug reports are most useful with your browser, device, video format, and the 3D mode and style you selected.
