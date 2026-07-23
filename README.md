# Generate and link QR code

Originally used so that petition signors could double-check their municipality, becuase street address does not always match municipality.

## Python code to generate a QR code
Save code chunk below as `generate_qr.py`

<details>
<summary>Click to expand code</summary>

```python
#!/usr/bin/env python3
"""
generate_qr.py
Generate a QR code image for any URL.
Setup (one time):
    pip install qrcode[pil] --break-system-packages
Usage:
    python3 generate_qr.py <url> [output_filename] [--box-size N] [--border N] [--svg]
Examples:
    # Basic usage -> saves qrcode.png in the current folder
    python3 generate_qr.py https://graciezeller.github.io/canvassing_qr/index.html
    # Custom output filename
    python3 generate_qr.py https://graciezeller.github.io/canvassing_qr/index.html my_qr.png
    # Bigger / smaller code (box-size controls pixel size per module)
    python3 generate_qr.py https://example.com my_qr.png --box-size 12 --border 2
    # Save as scalable SVG instead of PNG (better for print)
    python3 generate_qr.py https://example.com my_qr.svg --svg
"""
import argparse
import sys

def generate_png(url: str, output_path: str, box_size: int, border: int) -> None:
    import qrcode
    img = qrcode.make(url, box_size=box_size, border=border)
    img.save(output_path)

def generate_svg(url: str, output_path: str, box_size: int, border: int) -> None:
    import qrcode
    import qrcode.image.svg
    factory = qrcode.image.svg.SvgPathImage
    qr = qrcode.QRCode(
        image_factory=factory,
        box_size=box_size,
        border=border,
    )
    qr.add_data(url)
    qr.make(fit=True)
    img = qr.make_image(fill_color="black", back_color="white")
    img.save(output_path)

def main() -> None:
    parser = argparse.ArgumentParser(description="Generate a QR code for a URL.")
    parser.add_argument("url", help="The URL to encode in the QR code")
    parser.add_argument(
        "output",
        nargs="?",
        default=None,
        help="Output filename (default: qrcode.png or qrcode.svg)",
    )
    parser.add_argument(
        "--box-size",
        type=int,
        default=10,
        help="Size of each QR module in pixels (default: 10). Increase for larger/higher-res codes.",
    )
    parser.add_argument(
        "--border",
        type=int,
        default=4,
        help="Width of the white quiet-zone border in modules (default: 4). Don't go below 4 for reliable scanning.",
    )
    parser.add_argument(
        "--svg",
        action="store_true",
        help="Save as scalable SVG instead of PNG (recommended for print/posters)",
    )
    args = parser.parse_args()
    output = args.output or ("qrcode.svg" if args.svg else "qrcode.png")
    try:
        if args.svg:
            generate_svg(args.url, output, args.box_size, args.border)
        else:
            generate_png(args.url, output, args.box_size, args.border)
    except ImportError:
        print(
            "Missing dependency. Install it first with:\n"
            "  pip install qrcode[pil] --break-system-packages",
            file=sys.stderr,
        )
        sys.exit(1)
    print(f"QR code for '{args.url}' saved to: {output}")

if __name__ == "__main__":
    main()
```

</details>

In terminal, run:
```bash
python3 generate_qr.py <link to site>
```

## For a dynamic QR code

Adjust [config.txt](config.txt) so it contains your link.

Run generate_qr.py, and link the code to [index.html](index.html), (e.g., https://graciezeller.github.io/canvassing_qr/index.html). Your repo must be a public page in order for this to work.

Now, you can adjust the link in [config.txt](config.txt) to change the linked destination without breaking the QR code.

> **Note:** The current fallback specified in [index.html](index.html) is [https://myvote.wi.gov/en-us/My-Municipal-Clerk](https://myvote.wi.gov/en-us/My-Municipal-Clerk).

