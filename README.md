# EPUB Baseline JPEG Converter

A browser-based tool for optimizing EPUB files for e-ink readers. Converts images to baseline JPEG, fixes common EPUB issues, and supports Rotate & Split mode for wide image handling.

**No installation required · Works 100% offline after loading**

<img width="865" height="957" alt="image" src="https://github.com/user-attachments/assets/b25594e0-d3bd-42ad-ae65-d240cb8692cc" />

## Features

### Simple & Advanced Modes
- **Simple Mode** (default): Drop files, convert, download. No configuration needed.
- **Advanced Mode**: Full control with quality settings, image selection grid, detailed logs, and validation info.

### Image Conversion
- **Format conversion**: PNG, GIF, WebP, BMP → baseline JPEG
- **Quality control**: 1-95% JPEG quality with presets (Low 60%, Medium 75%, High 85%, Max 95%)
- **Smart scaling**: Automatically scales images to fit e-reader screen (default 480×800) — dramatically reduces file size and improves e-reader responsiveness
- **Grayscale mode**: Converts all images to grayscale for e-ink displays

### Rotate & Split Mode
Optimized for manga and light novels with horizontal spread images:

**Three processing modes** (per-image selection):
| Mode | Color | Description |
|------|-------|-------------|
| ↻ CW | 🔵 Blue | Rotate 90° clockwise + split right-to-left (Japanese manga) |
| ↺ CCW | 🟠 Orange | Rotate 90° counter-clockwise + split left-to-right (Western comics) |
| ┃┃ Vertical | 🟢 Green | Split vertically without rotation (panoramic images) |

**Selection features**:
- Thumbnail grid with per-image mode selection
- Images displayed in OPF spine reading order
- Quick selection buttons: "Select Landscape", "Select All", "Clear"
- Small images (≤480×800) auto-locked — no processing needed

**Overlap settings**:
- Separate overlap for rotation modes (CW/CCW): 5%, 10%, or 15%
- Separate overlap for vertical split: 5%, 10%, or 15%

### EPUB Repairs
- **SVG cover fix**: Unwraps images from problematic SVG containers
- **SVG-wrapped images**: Detects and fixes `<svg><image xlink:href="..."/></svg>` patterns
- **Cover metadata**: Ensures `<meta name="cover" content="..."/>` exists in OPF
- **Manifest sync**: Updates image references and media-types after conversion
- **NCX identifier sync**: Synchronizes `dtb:uid` with OPF `dc:identifier`
- **EPUB OCF compliance**: Writes `mimetype` as first uncompressed entry

### Batch Processing
- Drop multiple EPUB files at once
- Individual progress tracking
- Combined statistics (total saved, conversion count)
- Download all as single ZIP file

### User Interface
- **Dark/Light theme**: Toggle with 🌙/☀️ button, preference saved
- **User Guide**: Built-in documentation (📖 button or footer link)
- **Export logs**: Save detailed conversion report as text file

## Usage

### Simple Mode (Default)
1. Drag & drop an EPUB file onto the drop zone
2. Click **Convert to Baseline JPEG**
3. Download the converted EPUB

Default settings: 85% quality, grayscale ON, no rotation/split.

### Advanced Mode
1. Toggle **⚙️ Advanced Mode** below the drop zone
2. Drop an EPUB file
3. Adjust JPEG quality if needed
4. Select images for Rotate & Split in the thumbnail grid
5. Choose mode for each image: ↻CW, ↺CCW, or ┃┃Vertical
6. Adjust overlap settings if needed
7. Toggle Grayscale on/off
8. Click **Convert to Baseline JPEG**
9. Review detailed logs and stats
10. Download the converted EPUB

### Batch Mode
1. Drop multiple EPUB files at once
2. Configure quality settings (applies to all)
3. Click **Convert All**
4. Download individual files or **Download All as ZIP**

## Configuration Options

| Option | Default | Description |
|--------|---------|-------------|
| JPEG Quality | 85% | Compression level (1-95%). Lower = smaller file, higher = better quality |
| Rotate & Split | OFF | Manually select images and choose CW/CCW/Vertical mode |
| Rotate Overlap | 15% | Overlap between split pages for CW/CCW modes |
| Vertical Overlap | 15% | Overlap between split pages for Vertical mode |
| Grayscale | ON | Convert all images to grayscale |
| Screen Width | 480px | Target e-reader screen width |
| Screen Height | 800px | Target e-reader screen height |

## Technical Details

### Image Processing Pipeline

```
1. Load image from EPUB
2. Convert to RGB (handle RGBA/LA/P modes)
3. Check if image selected for Rotate & Split
4. If selected:
   a. CW/CCW mode:
      - Scale width to screen height (800px)
      - Rotate 90° clockwise or counter-clockwise
      - Split into pages with configured overlap
      - CW: right-to-left order, CCW: left-to-right order
   b. Vertical mode:
      - Scale to fit height (800px)
      - Split vertically left-to-right with configured overlap
5. If not selected: scale to fit 480×800
6. Apply grayscale (if enabled)
7. Encode as baseline JPEG (non-progressive)
8. Update all references (XHTML, OPF, CSS, NCX)
```

### Split Algorithm

For a 2400×1600 image rotated CW with 15% overlap on 480×800 screen:
- Scale width to 800px → 2400×800
- Rotate 90° CW → 800×2400
- Overlap: 480 × 0.15 = 72px
- Step: 480 - 72 = 408px
- Number of parts: ceil((2400 - 72) / 408) = 6 parts
- Extraction order: right → left (CW) or left → right (CCW)

### EPUB Modifications

| File Type | Modifications |
|-----------|---------------|
| Images | Convert format, scale, rotate, split, grayscale |
| XHTML | Update `<img>` src, duplicate elements for splits, fix IDs |
| OPF | Update manifest hrefs, media-types, add split image entries, ensure cover meta |
| NCX | Sync `dtb:uid` with OPF identifier |
| CSS | Update image references |

### Validation

Pre-conversion checks:
- ✓ Valid ZIP structure
- ✓ `mimetype` file present
- ✓ OPF file exists
- ✓ Images found for conversion

Post-conversion checks:
- ✓ All images successfully converted
- ✓ Manifest references valid
- ✓ `mimetype` is first entry (OCF compliance)

## Browser Compatibility

| Browser | Status |
|---------|--------|
| Chrome 90+ | ✅ Full support |
| Firefox 88+ | ✅ Full support |
| Safari 14+ | ✅ Full support |
| Edge 90+ | ✅ Full support |

**Requirements**: Modern browser with ES2020 support, Canvas API, File API

## File Size Estimates

Typical compression results (85% quality, grayscale enabled):

| Source | Result |
|--------|--------|
| 3000×4000 PNG (5MB) | 480×800 JPEG (~50KB) — **99% reduction** |
| PNG (lossless) | 70-90% smaller |
| WebP | 30-50% smaller |
| High-quality JPEG | 20-40% smaller |

Smart scaling to 480×800 is the biggest factor — large source images become dramatically smaller while looking identical on e-ink screens.

## Privacy

- **No server uploads**: All processing happens in your browser
- **No data collection**: Files never leave your device
- **Works offline**: After initial page load, no internet required

## Dependencies

- [JSZip 3.10.1](https://stuk.github.io/jszip/) - ZIP file handling
- [Atkinson Hyperlegible Next](https://fonts.google.com/specimen/Atkinson+Hyperlegible) - UI font

## Changelog

### v2.9.0
- **User Guide**: Built-in documentation page (`guide.html`)
- Help button (📖) in header and footer link

### v2.8.0
- **Dark/Light theme toggle**: Remembers preference in localStorage
- Professional neutral color scheme (black/gray for dark, white/gray for light)

### v2.7.0
- **Three processing modes**: CW (blue), CCW (orange), Vertical (green)
- **Per-image mode selection**: Choose different modes for different images
- **Separate overlap settings**: Configure rotation and vertical overlap independently
- **Smart selection blocking**: Images ≤480×800 are locked (no processing needed)
- Vertical split disabled for images with width ≤480px

### v2.6.0
- **Advanced Mode toggle**: Simple mode by default (just drop & convert), Advanced mode for full control
- **Export log**: Download detailed conversion report as text file
- Split image logging shows detailed part info (dimensions, sizes)

### v2.5.1
- Fixed collision bug when EPUBs contain same-named images in different folders
- Split images now keyed by full path instead of basename

### v2.5.0
- **Renamed "Light Novel Mode" to "Rotate & Split"** for clarity
- **Manual image selection**: Thumbnail grid to select which images to rotate & split
- **Reading order**: Images displayed in OPF spine order
- **Quick selection buttons**: Select Landscape, Select All, Clear Selection

### v2.4.0
- Image picker grid for manual Rotate & Split selection
- Removed automatic landscape detection

### v2.3.0
- Enhanced logging system with timestamps and colored tags
- Per-image logging with dimensions, format, and compression stats
- Visual summary table at end of conversion
- Batch processing summary with total stats

### v2.2.2
- NCX `dtb:uid` syncs with OPF `dc:identifier`
- Fixed duplicate ID issue when splitting images (part suffix appended)
- EPUB OCF compliance: `mimetype` written first as uncompressed entry
- OPF manifest uses correct relative paths for subdirectories

## Contributing

Issues and pull requests welcome! Please test with various EPUB sources before submitting.

## Credits

Made by **Megabit & pablohc**

Optimized for [CrossPoint Reader](https://github.com/nicemicro/crosspoint-reader) and XTEink X4 e-readers.
