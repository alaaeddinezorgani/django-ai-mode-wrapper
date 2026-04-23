Got it — here’s the **clean minimal README (corrected for a repo where ONNX is already included)**:

---

# 🧠 U-Net Segmentation API (Django + ONNX)

Simple REST API that runs a trained U-Net model and returns a segmentation mask.

---

## 🚀 Setup

### 1. Install dependencies

```bash id="8c0j3v"
pip install django djangorestframework onnxruntime numpy pillow opencv-python
```

---

### 2. Run migrations

```bash id="x7gk2m"
python manage.py migrate
```

---

### 3. Start server

```bash id="p1q8zn"
python manage.py runserver
```

API runs at:

```id="m8v2kd"
http://127.0.0.1:8000/
```

---

## 🧪 Run inference

### Linux / macOS / WSL

```bash id="r4t9aa"
curl -X POST \
  -F "image=@/path/to/image.jpg" \
  http://127.0.0.1:8000/api/segment/ \
  -o result.json
```

### Windows (PowerShell)

```powershell id="k2n7qp"
curl -X POST -F "image=@C:\path\to\image.jpg" http://127.0.0.1:8000/api/segment/ -o result.json
```

---

## 🖼️ Convert result to image

```bash id="v6m3ld"
python decoder.py
```

This generates:

```id="t9c1xw"
result.png
```

---

## 📤 Output

- API returns `result.json`
- `decoder.py` converts it → `result.png` (segmentation mask)

---

That’s it.
