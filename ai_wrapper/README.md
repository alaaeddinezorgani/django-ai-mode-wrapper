# 🧠 U-Net Segmentation API (Django + ONNX)

This project provides a simple REST API to run a trained **U-Net segmentation model** using ONNX Runtime inside Django.

Users only need to clone the repo and run the server.

---

# 🚀 Quick Start

## 1. Clone the repository

```bash id="c2j9q1"
git clone <your-repo-url>
cd <your-project-folder>
```

---

## 2. Create virtual environment (recommended)

### Linux / macOS

```bash id="p9k2mx"
python -m venv venv
source venv/bin/activate
```

### Windows (PowerShell)

```powershell id="w8q1jd"
python -m venv venv
venv\Scripts\activate
```

---

## 3. Install dependencies

```bash id="f7k2ld"
pip install django djangorestframework onnxruntime numpy pillow opencv-python
```

---

## 4. Run migrations

```bash id="m2x9qp"
python manage.py migrate
```

---

## 5. Start the server

```bash id="k9d1sv"
python manage.py runserver
```

Server will run at:

```
http://127.0.0.1:8000/
```

---

# 📦 Model setup

Place your model files here:

```text id="v8k2ab"
ai/model.onnx
ai/model.onnx.data   (if present)
```

⚠️ Both files must stay in the same directory.

---

# 🧠 Inference logic

Inference is handled in:

```text id="d1k8zx"
ai/decoder.py
```

This file:

- loads the ONNX model
- preprocesses images (RGB, 224×224)
- runs inference
- returns a binary segmentation mask

---

# 🧪 Test the API

## Linux / macOS / WSL

```bash id="x8m1pq"
curl -X POST -F "image=@/full/path/to/image.jpg" http://127.0.0.1:8000/api/segment/
```

Example:

```bash id="n4q2lx"
curl -X POST -F "image=@/home/user/image.jpg" http://127.0.0.1:8000/api/segment/
```

---

## Windows (PowerShell)

```powershell id="z2k9mf"
curl -X POST -F "image=@C:\path\to\image.jpg" http://127.0.0.1:8000/api/segment/
```

If curl fails:

```powershell id="t9w1nc"
Invoke-RestMethod -Uri "http://127.0.0.1:8000/api/segment/" `
  -Method Post `
  -Form @{ image = Get-Item "C:\path\to\image.jpg" }
```

---

# 📤 API Response

```json id="q8m1xz"
{
  "mask": "<base64 PNG image>"
}
```

- `mask` = segmentation result encoded as PNG (base64)

---

# 🖼️ View result

Save response:

```bash id="h2p9xw"
curl -X POST -F "image=@image.jpg" http://127.0.0.1:8000/api/segment/ -o response.json
```

Decode:

```python id="k1z9mq"
import json, base64, io
from PIL import Image

data = json.load(open("response.json"))
img = Image.open(io.BytesIO(base64.b64decode(data["mask"])))
img.show()
```

---

# 📁 Project structure

```text id="r7k1ab"
project/
│
├── ai/
│   ├── model.onnx
│   ├── model.onnx.data
│   ├── decoder.py   ← inference logic
│   ├── model.py
│   ├── views.py
│   ├── urls.py
│
├── project/
│   ├── urls.py
│
└── manage.py
```

---

# ⚡ Notes

- Input size: **224×224 RGB**
- Output: **binary segmentation mask**
- First request may be slower (model loading)
- Works on CPU (GPU optional if installed)
