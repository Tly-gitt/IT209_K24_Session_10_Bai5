# BÁO CÁO BÀI TẬP

## 1. Cấu trúc tổ chức tệp tin

- `docker-compose.yml`: Tệp điều phối chính xác định 2 service `backend` và `database`.
- `.env.example`: Mẫu khai báo các biến môi trường nhạy cảm phục vụ triển khai.
- `.gitignore`: Chặn tệp `.env` chứa mật khẩu thực tế bị commit lên GitHub.
- `README.md`: Hướng dẫn vận hành và minh chứng kết quả.

---

## 2. Hướng dẫn thiết lập & Vận hành

### Bước 1: Chuẩn bị biến môi trường
Từ tệp mẫu `.env.example`, sao chép tạo tệp cấu hình thực tế

### Bước 2: Khởi chạy toàn bộ hạ tầng
```powershell
docker compose up -d
```

### Bước 3: Kiểm tra trạng thái
```powershell
docker compose ps
```

### Bước 4: Xem log dịch vụ
```powershell
docker compose logs -f
```

### Bước 5: Hạ dịch vụ
```powershell
docker compose down
```

---

## 3. Kết quả

### a. Kiểm tra khởi động hệ thống (`docker compose up -d`)
- **Lệnh thực thi:**
  ```powershell
  docker compose up -d
  ```
- **Kết quả Terminal:**
  ```text
  [+] Running 4/4
   ✔ Network ex5_backend-net   Created                                       0.1s
   ✔ Volume "ex5_pgdata"       Created                                       0.0s
   ✔ Container ex5-database-1  Started                                       0.8s
   ✔ Container ex5-backend-1   Started                                       1.2s
  ```

### b. Kiểm tra trạng thái các service (`docker compose ps`)
- **Lệnh thực thi:**
  ```powershell
  docker compose ps
  ```
- **Kết quả Terminal:**
  ```text
  NAME             IMAGE                COMMAND                  SERVICE    CREATED         STATUS         PORTS
  ex5-backend-1    ex5-backend          "./app"                  backend    2 minutes ago   Up 2 minutes   0.0.0.0:8080->8080/tcp
  ex5-database-1   postgres:15-alpine   "docker-entrypoint.s…"   database   2 minutes ago   Up 2 minutes   5432/tcp
  ```

### c. Kiểm tra kết nối mạng nội bộ qua log
- Log từ container `backend` xác nhận đã kết nối thành công tới database qua hostname `database` trong cùng mạng `backend-net`:
  ```text
  ex5-backend-1   | Connected to PostgreSQL database at database:5432 successfully.
  ```

---

## 4. Kết luận
- Hạ tầng đa dịch vụ đã được chuẩn hóa qua `docker-compose.yml`.
- Dữ liệu cơ sở dữ liệu được bảo toàn qua named volume `pgdata`.
- Toàn bộ bí mật cấu hình được bóc tách hoàn toàn vào `.env`, tuân thủ nguyên tắc 12-Factor App và bảo mật Git.