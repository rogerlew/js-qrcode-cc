# QR Code Generator

A pure JavaScript QR code generator that runs entirely in the browser. No dependencies, no server required.

**[Live Demo](https://rogerlew.github.io/js-qrcode-cc/)** | **[Run Tests](https://rogerlew.github.io/js-qrcode-cc/tests.html)**

![QR Code Example](https://api.qrserver.com/v1/create-qr-code/?size=150x150&data=https://rogerlew.github.io/js-qrcode-cc/)

## Features

- **ISO/IEC 18004:2024 Compliant** - Follows the official QR code standard
- **Zero Dependencies** - Pure JavaScript, works offline
- **All Error Correction Levels** - L (7%), M (15%), Q (25%), H (30%)
- **Automatic Mode Selection** - Optimizes for Numeric, Alphanumeric, or Byte encoding
- **Full Unicode Support** - Encode any text via UTF-8
- **Versions 1-40** - From 21x21 to 177x177 modules
- **Downloadable PNG** - Export QR codes directly from the browser

## Quick Start

### Use Online

Visit [rogerlew.github.io/js-qrcode-cc](https://rogerlew.github.io/js-qrcode-cc/), enter your URL or text, and download the QR code.

### Use in Your Project

```html
<canvas id="qr"></canvas>
<script src="https://rogerlew.github.io/js-qrcode-cc/index.html"></script>
<script>
  // Generate and render a QR code
  const qrData = QRCode.generate('https://example.com', 'M');
  QRCode.render(document.getElementById('qr'), qrData, 8);
</script>
```

Or copy the `QRCode` module (~1300 lines) from `index.html` into your own project.

## API

### `QRCode.generate(data, eccLevel)`

Generates QR code data.

| Parameter | Type | Description |
|-----------|------|-------------|
| `data` | string | Text or URL to encode |
| `eccLevel` | string | Error correction: `'L'`, `'M'` (default), `'Q'`, or `'H'` |

Returns an object with:
- `matrix` - 2D array of 0s and 1s
- `version` - QR version (1-40)
- `size` - Matrix dimensions
- `maskPattern` - Applied mask (0-7)
- `mode` - Encoding mode used

### `QRCode.render(canvas, qrData, moduleSize)`

Renders QR code to a canvas element.

| Parameter | Type | Description |
|-----------|------|-------------|
| `canvas` | HTMLCanvasElement | Target canvas |
| `qrData` | object | Output from `generate()` |
| `moduleSize` | number | Pixels per module (default: 8) |

## Examples

### Generate a QR Code Programmatically

```javascript
const canvas = document.createElement('canvas');
const qr = QRCode.generate('Hello, World!', 'M');
QRCode.render(canvas, qr, 10);
document.body.appendChild(canvas);
```

### Download as PNG

```javascript
function downloadQR(text, filename) {
  const canvas = document.createElement('canvas');
  const qr = QRCode.generate(text, 'H');
  QRCode.render(canvas, qr, 10);

  const link = document.createElement('a');
  link.download = filename || 'qrcode.png';
  link.href = canvas.toDataURL('image/png');
  link.click();
}

downloadQR('https://github.com', 'github-qr.png');
```

### Get Data URL for Images

```javascript
function getQRDataURL(text) {
  const canvas = document.createElement('canvas');
  const qr = QRCode.generate(text, 'M');
  QRCode.render(canvas, qr, 8);
  return canvas.toDataURL('image/png');
}

// Use in an img tag
document.getElementById('myImage').src = getQRDataURL('https://example.com');
```

## Error Correction Levels

| Level | Recovery | Best For |
|-------|----------|----------|
| L | ~7% | Clean environments, maximum data |
| M | ~15% | General purpose (default) |
| Q | ~25% | Industrial, some dirt expected |
| H | ~30% | Heavy wear, logos overlay |

Higher error correction = larger QR code for same data.

## Development

### Run Locally

```bash
# Clone the repo
git clone https://github.com/rogerlew/js-qrcode-cc.git
cd js-qrcode-cc

# Serve with any static server
npx serve .
# or
python -m http.server 8000
```

### Run Tests

Open `tests.html` in a browser or visit `/tests` on your local server. The test suite includes 49 tests covering:

- Mode detection and encoding
- Reed-Solomon error correction
- Mask pattern evaluation
- Format and version information
- End-to-end integration tests

## How It Works

1. **Analyze** input to select optimal encoding mode
2. **Encode** data with mode indicator and character count
3. **Generate** Reed-Solomon error correction codewords
4. **Build** matrix with finder patterns, timing, and alignment
5. **Place** data modules in upward zigzag pattern
6. **Apply** best mask pattern (lowest penalty score)
7. **Add** format information (ECC level + mask)

For implementation details, see [AGENTS.md](AGENTS.md).

## Browser Support

Works in all modern browsers with HTML5 Canvas support:
- Chrome, Firefox, Safari, Edge
- Mobile browsers (iOS Safari, Chrome for Android)

## License

MIT License - feel free to use in personal and commercial projects.

## Acknowledgments

- [ISO/IEC 18004:2024](https://www.iso.org/standard/62021.html) - QR Code specification
- [Thonky QR Code Tutorial](https://www.thonky.com/qr-code-tutorial/) - Invaluable reference
- Built with [Claude Code](https://claude.ai/claude-code)
