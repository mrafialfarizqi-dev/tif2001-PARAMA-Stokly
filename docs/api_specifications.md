
Token didapatkan dari response endpoint Login, dan wajib disertakan pada setiap request ke endpoint yang dilindungi.

---

## 1. Register

Mendaftarkan pengguna baru.

- **Method**: `POST`
- **Endpoint**: `/auth/register`
- **Headers**: `Content-Type: application/json`
- **Auth**: Tidak diperlukan

### Request Body

```json
{
  "name": "Juuuh",
  "email": "juuuh@example.com",
  "password": "password123"
}
```

### Response — Success (201 Created)

```json
{
  "success": true,
  "message": "User registered successfully",
  "data": {
    "id": 1,
    "name": "Juuuh",
    "email": "juuuh@example.com",
    "role": "staff",
    "created_at": "2026-09-25T10:00:00Z"
  }
}
```

### Response — Error (400 Bad Request)

```json
{
  "success": false,
  "message": "Email already registered"
}
```

### Response — Error (422 Unprocessable Entity)

```json
{
  "success": false,
  "message": "Validation error",
  "errors": {
    "email": "Email is not valid",
    "password": "Password must be at least 8 characters"
  }
}
```

---

## 2. Login

Mengautentikasi pengguna dan mengembalikan token JWT.

- **Method**: `POST`
- **Endpoint**: `/auth/login`
- **Headers**: `Content-Type: application/json`
- **Auth**: Tidak diperlukan

### Request Body

```json
{
  "email": "juuuh@example.com",
  "password": "password123"
}
```

### Response — Success (200 OK)

```json
{
  "success": true,
  "message": "Login successful",
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": {
      "id": 1,
      "name": "Juuuh",
      "email": "juuuh@example.com",
      "role": "staff"
    }
  }
}
```

### Response — Error (401 Unauthorized)

```json
{
  "success": false,
  "message": "Invalid email or password"
}
```

---

## 3. Get Products (contoh endpoint terlindungi, referensi Sprint berikutnya)

- **Method**: `GET`
- **Endpoint**: `/products`
- **Auth**: Wajib (Bearer Token)
- **Query Parameters**:
  - `page` (optional, default: 1) — halaman pagination
  - `limit` (optional, default: 10) — jumlah data per halaman
  - `search` (optional) — filter berdasarkan nama produk

### Response — Success (200 OK)

```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "name": "Kabel USB Type-C",
      "sku": "SKU-001",
      "category": "Elektronik",
      "quantity": 50,
      "unit": "pcs",
      "price": 25000
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 1
  }
}
```

### Response — Error (401 Unauthorized)

```json
{
  "success": false,
  "message": "Token is missing or invalid"
}
```

---

## HTTP Status Code Reference

| Code | Meaning |
|------|---------|
| 200 | OK — request berhasil |
| 201 | Created — data baru berhasil dibuat |
| 400 | Bad Request — permintaan tidak valid |
| 401 | Unauthorized — token tidak ada / tidak valid |
| 403 | Forbidden — tidak punya akses (role tidak sesuai) |
| 404 | Not Found — data tidak ditemukan |
| 422 | Unprocessable Entity — validasi input gagal |
| 500 | Internal Server Error — kesalahan di sisi server |