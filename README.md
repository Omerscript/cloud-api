# 📁 Secure Cloud Storage API – Developer Guide

Welcome to the official documentation for the **Secure Cloud Storage API**. This guide will walk you through everything you need to know to start using the API effectively.

> **Developer:** [@LIEI_T](https://t.me/LIEI_T) on Telegram


## 🌐 API Base URL

https://brand-fiber-commonwealth-delight.trycloudflare.com

All endpoints are relative to this base URL.

---

## 🔐 Authentication & Security

The API uses a simple username/password system with additional security layers:

- **Password Encryption:** All passwords are hashed using SHA256 with salt
- **Rate Limiting:** Maximum 10 requests per minute per user
- **Brute Force Protection:** 5 failed attempts → 30-minute ban
- **API Keys:** Each user receives a unique API key upon registration
- **File Validation:** File size limit (50MB) and allowed extensions check

---

## 📋 Available Endpoints

### 1. Register User

Create a new account.

**Endpoint:** `POST /register`

**Request Body:**
```json
{
  "username": "your_username",
  "password": "your_password"
}
```

**Success Response:**
```json
{
  "success": true,
  "api_key": "ed69ce38ee452a480b380bcfa075af86",
  "message": "✅ تم التسجيل"
}
```

**Error Response:**
```json
{
  "success": false,
  "error": "❌ هذا الاسم محجوز"
}
```

---

### 2. Upload File

Upload a file to your storage.

**Endpoint:** `POST /upload`

**Request Body:**
```json
{
  "username": "your_username",
  "password": "your_password",
  "file_name": "example.txt",
  "file_data": "SGVsbG8gV29ybGQ="  // Base64 encoded
}
```

**Success Response:**
```json
{
  "success": true,
  "file_id": "1734567890123abcd",
  "message": "✅ تم الرفع بنجاح"
}
```

**Error Response:**
```json
{
  "success": false,
  "error": "❌ نوع الملف غير مسموح (.exe)"
}
```

---

### 3. List Files

Get a list of all files for a user.

**Endpoint:** `POST /list`

**Request Body:**
```json
{
  "username": "your_username",
  "password": "your_password"
}
```

**Success Response:**
```json
{
  "success": true,
  "files": [
    {
      "id": "1734567890123abcd",
      "name": "example.txt",
      "size": 2048,
      "upload_date": "2026-06-26T15:30:00",
      "download_count": 3
    }
  ],
  "count": 1
}
```

---

### 4. Download File

Download a specific file.

**Endpoint:** `POST /download`

**Request Body:**
```json
{
  "username": "your_username",
  "password": "your_password",
  "file_id": "1734567890123abcd"
}
```

**Success Response:**
```json
{
  "success": true,
  "file_name": "example.txt",
  "file_data": "SGVsbG8gV29ybGQ="  // Base64 encoded
}
```

**Error Response:**
```json
{
  "success": false,
  "error": "❌ الملف غير موجود"
}
```

---

### 5. Delete File

Delete a file permanently.

**Endpoint:** `POST /delete`

**Request Body:**
```json
{
  "username": "your_username",
  "password": "your_password",
  "file_id": "1734567890123abcd"
}
```

**Success Response:**
```json
{
  "success": true,
  "message": "✅ تم الحذف بنجاح"
}
```

---

## 🛡️ Security Features

### Rate Limiting
- **Limit:** 10 requests per minute
- **Action:** Returns error message when exceeded
- **Reset:** Resets after 60 seconds

### Brute Force Protection
- **Threshold:** 5 failed attempts
- **Penalty:** 30-minute ban
- **Tracking:** Per username

### File Validation
- **Max Size:** 50 MB
- **Allowed Extensions:** txt, json, pdf, doc, docx, xls, xlsx, jpg, jpeg, png, gif, bmp, svg, webp, mp3, mp4, wav, ogg, flac, zip, rar, 7z, gz, tar, py, js, html, css, xml, csv, ppt, pptx, odt, ods, odp
- **Blocked Names:** admin, root, test, system, etc.

### Password Requirements
- Minimum 8 characters
- At least one uppercase letter
- At least one lowercase letter
- At least one digit

### Username Requirements
- Length: 3-20 characters
- Allowed: alphanumeric and underscore
- Reserved names: admin, root, test, user, system, owner, mod, bot, telegram

---

## 🧪 Testing Examples

### PowerShell (Windows)

```powershell
# Register
Invoke-RestMethod -Uri "https://brand-fiber-commonwealth-delight.trycloudflare.com/register" `
    -Method POST `
    -ContentType "application/json" `
    -Body '{"username":"testuser","password":"Test1234"}'

# Upload
Invoke-RestMethod -Uri "https://brand-fiber-commonwealth-delight.trycloudflare.com/upload" `
    -Method POST `
    -ContentType "application/json" `
    -Body '{"username":"testuser","password":"Test1234","file_name":"hello.txt","file_data":"SGVsbG8gV29ybGQ="}'

# List
Invoke-RestMethod -Uri "https://brand-fiber-commonwealth-delight.trycloudflare.com/list" `
    -Method POST `
    -ContentType "application/json" `
    -Body '{"username":"testuser","password":"Test1234"}'
```

### cURL (Linux/Mac)

```bash
# Register
curl -X POST https://brand-fiber-commonwealth-delight.trycloudflare.com/register \
  -H "Content-Type: application/json" \
  -d '{"username":"testuser","password":"Test1234"}'

# Upload
curl -X POST https://brand-fiber-commonwealth-delight.trycloudflare.com/upload \
  -H "Content-Type: application/json" \
  -d '{"username":"testuser","password":"Test1234","file_name":"hello.txt","file_data":"SGVsbG8gV29ybGQ="}'

# List
curl -X POST https://brand-fiber-commonwealth-delight.trycloudflare.com/list \
  -H "Content-Type: application/json" \
  -d '{"username":"testuser","password":"Test1234"}'
```

### Python

```python
import requests
import base64

BASE_URL = "https://brand-fiber-commonwealth-delight.trycloudflare.com"

# Register
response = requests.post(f"{BASE_URL}/register", json={
    "username": "testuser",
    "password": "Test1234"
})
print(response.json())

# Upload
file_data = base64.b64encode(b"Hello World").decode('utf-8')
response = requests.post(f"{BASE_URL}/upload", json={
    "username": "testuser",
    "password": "Test1234",
    "file_name": "hello.txt",
    "file_data": file_data
})
print(response.json())

# List
response = requests.post(f"{BASE_URL}/list", json={
    "username": "testuser",
    "password": "Test1234"
})
print(response.json())
```

---

## ❓ Common Errors & Solutions

| Error Message | Cause | Solution |
|---------------|-------|----------|
| ❌ هذا الاسم محجوز | Username is reserved | Choose a different username |
| ❌ بيانات ناقصة | Missing required fields | Check all required parameters |
| ❌ نوع الملف غير مسموح | File extension not allowed | Use supported file types |
| ⚠️ الملف مكرر | File already exists | Rename the file or delete the old one |
| ⛔ تم حظرك 30 دقيقة | 5 failed attempts | Wait 30 minutes and try again |
| ⏳ وصلت للحد الأقصى | Rate limit exceeded | Wait 60 seconds before retrying |

---

## 📊 Error Codes

| HTTP Code | Description |
|-----------|-------------|
| 200 | Success |
| 201 | Created (User registered) |
| 400 | Bad Request |
| 403 | Forbidden |
| 404 | Not Found |
| 429 | Too Many Requests |
| 500 | Internal Server Error |

---

## 📌 Quick Reference

| Action | Endpoint | Method |
|--------|----------|--------|
| Register | `/register` | POST |
| Upload | `/upload` | POST |
| Download | `/download` | POST |
| Delete | `/delete` | POST |
| List Files | `/list` | POST |

---

## 💡 Best Practices

1. **Keep your credentials secure** - Never share your username, password, or API key
2. **Use descriptive filenames** - Makes organization easier
3. **Check file size before uploading** - Max is 50MB
4. **Handle errors gracefully** - Always check the response status
5. **Respect rate limits** - 10 requests per minute

---

## 🔗 Useful Links

- **Base URL:** https://brand-fiber-commonwealth-delight.trycloudflare.com
- **Developer:** [@LIEI_T](https://t.me/LIEI_T) on Telegram

---

## 🎯 Summary

This API provides a simple, secure, and efficient way to manage your files online. With the endpoints listed above and proper authentication, you can easily integrate cloud storage into your applications.

**Key Features:**
- ✅ Unlimited storage (within Telegram's limits)
- ✅ Password-protected files
- ✅ Multiple file types supported
- ✅ Built-in security features
- ✅ Rate limiting & brute force protection
- ✅ Simple REST API

---

*Built with ❤️ by [@LIEI_T](https://t.me/LIEI_T)*
```
