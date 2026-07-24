# Generate and link QR code

Originally used so that petition signors could double-check their municipality, becuase street address does not always match municipality.

## Python code to generate a QR code

Save [generate_qr.txt](generate_qr.txt). This is the script that makes the QR code.

Before running, make sure you have the correct dependencies
```bash
python3 -m pip install "qrcode[pil]"
```

In terminal, run:
```bash
python3 generate_qr.txt <link to site>
```

## For a dynamic QR code (requires githib repo)

Adjust [config.txt](config.txt) so it contains your link.

Run generate_qr.py, and link the code to [index.html](index.html), (e.g., https://graciezeller.github.io/canvassing_qr/index.html). Your repo must be a public page in order for this to work.

Now, you can adjust the link in [config.txt](config.txt) to change the linked destination without breaking the QR code.

> **Note:** The current fallback specified in [index.html](index.html) is [https://myvote.wi.gov/en-us/My-Municipal-Clerk](https://myvote.wi.gov/en-us/My-Municipal-Clerk).

