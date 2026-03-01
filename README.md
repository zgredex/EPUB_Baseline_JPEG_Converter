# EPUB Baseline JPEG Converter

Browser-based tool for optimizing EPUB files for e-ink readers. Converts images to baseline JPEG, scales to target device, and fixes common EPUB issues.

**No installation · No uploads · 100% offline**

## Why?

E-ink readers struggle with large images. This tool:

| Problem | Solution |
|---------|----------|
| 3000×4000px PNGs | Scaled to 480×800 |
| 5MB per image | ~50KB per image |
| Slow page turns | Fast, responsive |
| Broken SVG covers | Auto-fixed |

**Typical reduction: 70-90% smaller file size.**

## Features

### Core
- **Format conversion** — PNG, GIF, WebP, BMP → baseline JPEG
- **Smart scaling** — Fits images to 480×800 (preserves aspect ratio)
- **Grayscale** — Optimized for e-ink displays
- **Quality control** — 1-95% with presets

### Rotate & Split
For manga/comics with horizontal spreads:

| Mode | Color | Use case |
|------|-------|----------|
| ↻ CW | 🔵 Blue | Japanese manga (right-to-left) |
| ↺ CCW | 🟠 Orange | Western comics (left-to-right) |
| ┃┃ Vertical | 🟢 Green | Split only, no rotation |

- Per-image mode selection
- Configurable overlap: 5% / 10% / 15%

### Auto-Repairs
- SVG-wrapped covers → unwrapped
- Missing cover metadata → added
- NCX identifier mismatch → synced
- Manifest media-types → updated

### UX
- **Simple Mode** — Drop, convert, download
- **Advanced Mode** — Full control
- **Batch processing** — Multiple EPUBs → ZIP
- **Dark/Light theme** — Remembers preference
- **Export logs** — Detailed conversion report
- **User Guide** — Built-in documentation

## Quick Start

```
1. Open index.html in browser
2. Drop EPUB file
3. Click Convert
4. Download
```

Default settings (85% quality, grayscale ON) work for most e-readers.

## Advanced Usage

Toggle **⚙️ Advanced Mode** for:

- Quality slider & presets
- Image selection grid
- Rotate & Split controls
- Overlap settings
- Detailed logs

### Rotate & Split Workflow

1. Enable Advanced Mode
2. Load EPUB
3. Click images to select (or "Select Landscape")
4. Choose mode: ↻CW / ↺CCW / ┃┃Vertical
5. Set overlap
6. Convert

## Settings

| Setting | Default | Description |
|---------|---------|-------------|
| Quality | 85% | JPEG compression |
| Grayscale | ON | For e-ink |
| Rotate Overlap | 15% | CW/CCW modes |
| Vertical Overlap | 15% | Vertical mode |
| Max Width | 480px | Target screen |
| Max Height | 800px | Target screen |

## How It Works

```
Image Input
    ↓
Scale to fit 480×800
    ↓
[If selected] Rotate 90° + Split with overlap
    ↓
Apply grayscale
    ↓
Encode baseline JPEG
    ↓
Update EPUB references (XHTML, OPF, NCX)
    ↓
Output optimized EPUB
```

## Files

```
index.html    # Converter
guide.html    # User Guide
README.md     # This file
```

## Dependencies

- [JSZip](https://stuk.github.io/jszip/) — ZIP handling (CDN)
- [Atkinson Hyperlegible](https://fonts.google.com/specimen/Atkinson+Hyperlegible) — Font (Google Fonts)

No build step. Just open `index.html`.

## Browser Support

Chrome 90+ · Firefox 88+ · Safari 14+ · Edge 90+

## Privacy

- No server uploads
- No tracking/analytics
- No cookies (only localStorage for theme)
- Works offline after first load

## Changelog

**v2.9.0** — User Guide, help button

**v2.8.0** — Dark/Light theme, neutral colors

**v2.7.0** — Three modes (CW/CCW/Vertical), per-image selection, overlap settings

**v2.6.0** — Advanced Mode toggle, export logs

**v2.5.0** — Image grid, reading order from OPF spine

## Target Device

Optimized for **XTEink X4** (480×800 @ 220 PPI).

To change target dimensions, modify `MAX_WIDTH` and `MAX_HEIGHT` in the code.

## License

MIT

## Credits

Made by **Megabit & pablohc**
