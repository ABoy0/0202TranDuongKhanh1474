# 📝 DANH SÁCH URL TEST API PRODUCTS

## ✅ GET - LẤY TẤT CẢ SẢN PHẨM

### Test Hợp Lệ
```
GET http://localhost:3000/products
GET http://localhost:3000/products?page=1&limit=5
GET http://localhost:3000/products?page=2&limit=10
GET http://localhost:3000/products?minPrice=20&maxPrice=80
GET http://localhost:3000/products?title=red
GET http://localhost:3000/products?title=classic&page=1&limit=5
GET http://localhost:3000/products?minPrice=0&maxPrice=50&page=1&limit=10
```

### ❌ Test Validation - Lỗi Page/Limit
```
GET http://localhost:3000/products?page=abc&limit=5
→ Kỳ vọng: 400 error "page must be a positive integer"

GET http://localhost:3000/products?page=0&limit=5
→ Kỳ vọng: 400 error "page must be a positive integer"

GET http://localhost:3000/products?page=1&limit=-5
→ Kỳ vọng: 400 error "limit must be a positive integer"

GET http://localhost:3000/products?page=1&limit=0
→ Kỳ vọng: 400 error "limit must be a positive integer"

GET http://localhost:3000/products?page=1.5&limit=5
→ Kỳ vọng: 400 error "page must be a positive integer"
```

### ❌ Test Validation - maxPrice < minPrice
```
GET http://localhost:3000/products?minPrice=100&maxPrice=50
→ Kỳ vọng: 400 error "maxPrice cannot be less than minPrice"

GET http://localhost:3000/products?minPrice=1000&maxPrice=500&page=1
→ Kỳ vọng: 400 error "maxPrice cannot be less than minPrice"
```

---

## ✅ GET - LẤY SẢN PHẨM THEO ID

### Test Hợp Lệ
```
GET http://localhost:3000/products/1
→ Trả về: "Majestic Mountain Graphic T-Shirt"

GET http://localhost:3000/products/2
→ Trả về: "Classic Red Pullover Hoodie"

GET http://localhost:3000/products/10
→ Trả về: "Classic Blue Baseball Cap"

GET http://localhost:3000/products/46
→ Trả về: "Sleek All-Terrain Go-Kart"
```

### ❌ Test - ID Không Tồn Tại
```
GET http://localhost:3000/products/99999
→ Kỳ vọng: 404 error "id not found"

GET http://localhost:3000/products/0
→ Kỳ vọng: 404 error "id not found"
```

---

## ✅ GET - LẤY SẢN PHẨM THEO SLUG (CHỨC NĂNG MỚI)

### Test Hợp Lệ (Slug từ Project)
```
GET http://localhost:3000/products/slug/classic-red-pullover-hoodie
→ Trả về: Sản phẩm "Classic Red Pullover Hoodie"

GET http://localhost:3000/products/slug/majestic-mountain-graphic-t-shirt
→ Trả về: Sản phẩm "Majestic Mountain Graphic T-Shirt"

GET http://localhost:3000/products/slug/classic-blue-baseball-cap
→ Trả về: Sản phẩm "Classic Blue Baseball Cap"

GET http://localhost:3000/products/slug/sleek-all-terrain-go-kart
→ Trả về: Sản phẩm "Sleek All-Terrain Go-Kart"

GET http://localhost:3000/products/slug/elegant-glass-tumbler-set
→ Trả về: Sản phẩm "Elegant Glass Tumbler Set"

GET http://localhost:3000/products/slug/radiant-citrus-eau-de-parfum
→ Trả về: Sản phẩm "Radiant Citrus Eau de Parfum"
```

### ❌ Test - Slug Không Tồn Tại
```
GET http://localhost:3000/products/slug/slug-khong-ton-tai
→ Kỳ vọng: 404 error "slug not found"

GET http://localhost:3000/products/slug/invalid-slug-123
→ Kỳ vọng: 404 error "slug not found"
```

---

## ✅ POST - TẠO SẢN PHẨM MỚI

### Test Hợp Lệ
```
POST http://localhost:3000/products
Content-Type: application/json

{
  "title": "iPhone 15 Pro",
  "price": 999,
  "description": "Điện thoại cao cấp Apple",
  "category": {"id": 1, "name": "Electronics"},
  "images": ["https://example.com/iphone.jpg"]
}
→ Kỳ vọng: 201 Created với slug "iphone-15-pro"
```

### ✅ Test Vietnamese Slug (CHỨC NĂNG MỚI)
```
POST http://localhost:3000/products
Content-Type: application/json

{
  "title": "Điện thoại Samsung Galaxy S24",
  "price": 15000000,
  "description": "Điện thoại cao cấp Samsung",
  "category": {"id": 2, "name": "Electronics"},
  "images": ["https://example.com/samsung.jpg"]
}
→ Kỳ vọng: Slug = "dien-thoai-samsung-galaxy-s24" (tự động convert)
```

### ✅ Test Vietnamese Slug với Ký Tự Đặc Biệt
```
POST http://localhost:3000/products
Content-Type: application/json

{
  "title": "Áo thun Nam - Chất lượng Cao!!!",
  "price": 250000,
  "description": "Áo thun nam 100% cotton",
  "category": {"id": 1, "name": "Clothes"},
  "images": ["https://example.com/shirt.jpg"]
}
→ Kỳ vọng: Slug = "ao-thun-nam-chat-luong-cao" (xóa ký tự đặc biệt)
```

### ❌ Test - Title Trống
```
POST http://localhost:3000/products
Content-Type: application/json

{
  "title": "",
  "price": 100,
  "description": "Mô tả",
  "category": {"id": 1},
  "images": ["https://example.com/img.jpg"]
}
→ Kỳ vọng: 400 error "title is required and cannot be empty"
```

### ❌ Test - Price Không Phải Số
```
POST http://localhost:3000/products
Content-Type: application/json

{
  "title": "Product Test",
  "price": "abc",
  "description": "Mô tả",
  "category": {"id": 1},
  "images": ["https://example.com/img.jpg"]
}
→ Kỳ vọng: 400 error "price must be a valid positive number"
```

### ❌ Test - Price Âm
```
POST http://localhost:3000/products
Content-Type: application/json

{
  "title": "Product Test",
  "price": -100,
  "description": "Mô tả",
  "category": {"id": 1},
  "images": ["https://example.com/img.jpg"]
}
→ Kỳ vọng: 400 error "price must be a valid positive number"
```

### ❌ Test - Description Trống
```
POST http://localhost:3000/products
Content-Type: application/json

{
  "title": "Product Test",
  "price": 100,
  "description": "",
  "category": {"id": 1},
  "images": ["https://example.com/img.jpg"]
}
→ Kỳ vọng: 400 error "description is required and cannot be empty"
```

### ❌ Test - Category Trống
```
POST http://localhost:3000/products
Content-Type: application/json

{
  "title": "Product Test",
  "price": 100,
  "description": "Mô tả",
  "category": null,
  "images": ["https://example.com/img.jpg"]
}
→ Kỳ vọng: 400 error "category is required"
```

### ❌ Test - Images Trống
```
POST http://localhost:3000/products
Content-Type: application/json

{
  "title": "Product Test",
  "price": 100,
  "description": "Mô tả",
  "category": {"id": 1},
  "images": []
}
→ Kỳ vọng: 400 error "images is required and must be a non-empty array"
```

---

## ✅ PUT - CẬP NHẬT SẢN PHẨM

### Test Hợp Lệ - Update Title (Slug tự động cập nhật)
```
PUT http://localhost:3000/products/54
Content-Type: application/json

{
  "title": "Áo thun Adidas New 2024"
}
→ Kỳ vọng: Slug tự động thành "ao-thun-adidas-new-2024"
```

### Test Hợp Lệ - Update Price
```
PUT http://localhost:3000/products/54
Content-Type: application/json

{
  "price": 299.99
}
→ Kỳ vọng: 200 OK, price cập nhật
```

### Test Hợp Lệ - Update Cả Title và Price
```
PUT http://localhost:3000/products/53
Content-Type: application/json

{
  "title": "Laptop Gaming ASUS TUF 2024",
  "price": 25000000
}
→ Kỳ vọng: Title + slug + price cập nhật
```

### ❌ Test - Update với Price Không Hợp Lệ
```
PUT http://localhost:3000/products/1
Content-Type: application/json

{
  "price": "invalid"
}
→ Kỳ vọng: 400 error "price must be a valid positive number"
```

### ❌ Test - Update Title Trống
```
PUT http://localhost:3000/products/2
Content-Type: application/json

{
  "title": ""
}
→ Kỳ vọng: 400 error "title cannot be empty"
```

### ❌ Test - Update ID Không Tồn Tại
```
PUT http://localhost:3000/products/99999
Content-Type: application/json

{
  "title": "New Title"
}
→ Kỳ vọng: 404 error "id not found"
```

---

## ✅ DELETE - XÓA SẢN PHẨM

### Test Hợp Lệ
```
DELETE http://localhost:3000/products/53
→ Kỳ vọng: Soft delete, isDeleted = true, updatedAt cập nhật

DELETE http://localhost:3000/products/54
→ Kỳ vọng: Soft delete, isDeleted = true, updatedAt cập nhật
```

### ❌ Test - Delete ID Không Tồn Tại
```
DELETE http://localhost:3000/products/99999
→ Kỳ vọng: 404 error "id not found"
```

---

## 📊 BẢNG TÓM TẮT

| Chức Năng | URL | Phương Thức | Lỗi Kỳ Vọng |
|-----------|-----|-----------|-----------|
| Lấy tất cả | `/products` | GET | - |
| Phân trang | `/products?page=1&limit=5` | GET | Page/Limit phải số dương |
| Filter giá | `/products?minPrice=0&maxPrice=100` | GET | maxPrice < minPrice → 400 |
| Lấy theo ID | `/products/1` | GET | ID không tồn tại → 404 |
| Lấy theo Slug | `/products/slug/classic-red-pullover-hoodie` | GET | Slug không tồn tại → 404 |
| Tạo sản phẩm | `/products` | POST | Title/Price/Desc trống → 400 |
| Slug Tiếng Việt | Title: "Điện thoại Samsung" | POST | Slug: dien-thoai-samsung |
| Cập nhật | `/products/1` | PUT | Price không hợp lệ → 400 |
| Slug auto-update | PUT với title mới | PUT | Slug tự cập nhật |
| Xóa sản phẩm | `/products/1` | DELETE | Soft delete |

---

## 🧪 CÁCH TEST NHANH (Dùng CURL)

### Windows PowerShell:
```powershell
# GET
Invoke-WebRequest -Uri "http://localhost:3000/products/slug/classic-red-pullover-hoodie"

# POST
$body = @{
    title = "Điện thoại Samsung"
    price = 500
    description = "Test"
    category = @{id=1; name="Electronics"}
    images = @("https://example.com/img.jpg")
} | ConvertTo-Json

Invoke-WebRequest -Uri "http://localhost:3000/products" -Method POST -Body $body -ContentType "application/json"
```

### Mac/Linux (BASH/ZSH):
```bash
# GET
curl http://localhost:3000/products/slug/classic-red-pullover-hoodie

# POST
curl -X POST http://localhost:3000/products \
  -H "Content-Type: application/json" \
  -d '{"title":"Điện thoại Samsung","price":500,"description":"Test","category":{"id":1},"images":["https://example.com/img.jpg"]}'
```

---

## 💡 GỢI Ý THÊM

1. **Dùng Postman hoặc Thunder Client** để lưu các request
2. **Dùng test-api.html** trong folder NNPTUD-S2 để test giao diện
3. **Kiểm tra console server** để xem các query được ghi lại
4. **Kiểm tra isDeleted = false** trong kết quả GET để xác nhận soft delete hoạt động
