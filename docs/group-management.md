---
sidebar_position: 3
---

# Quản lý group

## Group trong hệ điều hành Linux là gì

- Trong Linux (hệ điều hành phổ biến trên VPS), một "group" là tập hợp các tài khoản người dùng (users) được gộp lại để dễ dàng quản lý quyền truy cập vào tài nguyên (file, thư mục, hoặc dịch vụ). Mỗi người dùng trên hệ thống có thể thuộc một hoặc nhiều nhóm.
- Vai trò của group:

  - **Phân quyền**: Quyền truy cập (đọc, ghi, thực thi) được gán cho một nhóm thay vì từng người dùng riêng lẻ, giúp quản lý dễ dàng hơn.
  - **Ví dụ**: Một nhóm có tên `webadmin` có thể được tạo để quản lý quyền truy cập vào thư mục chứa website (`/var/www/html`). Tất cả người dùng trong nhóm `webadmin` sẽ có quyền giống nhau đối với thư mục đó.

- Cách hoạt động:

  - Mỗi file/thư mục trên Linux có quyền truy cập được định nghĩa cho **owner**, **group**, và **others** (chủ sở hữu, nhóm, và những người khác).
  - Ví dụ: Nếu file `index.html` thuộc nhóm `webadmin` và có quyền `rw-r-----`, thì chỉ chủ sở hữu và các thành viên trong nhóm `webadmin` có thể đọc/ghi file này.

## Xem tất cả các group trong hệ thống (2 lệnh)

```bash
cat /etc/group
getent group
```

## Xem thông tin của một group

```bash
getent group [GROUP_ID/GROUP_NAME]
```

## Tạo group

```bash
groupadd [GROUP_NAME]
```

## Xóa group

```bash
groupdel [GROUP_NAME]
```

## Đổi tên group

```bash
groupmod -n [NEW_GROUP_NAME] [OLD_GROUP_NAME]
```

## Xem tất cả groups mà 1 user đã tham gia

```bash
groups [USERNAME]
```

## Thêm user vào group

```bash
usermod -aG [GROUP_NAME_OR_ID] [USERNAME]
```

:::info[Chú thích]

- `-a` (append): Thêm user vào nhóm mà không xóa các nhóm mà user đã là thành viên. Nếu không sử dụng `-a`, lệnh sẽ thay thế các nhóm mà user thuộc về với nhóm mới.
- `-G` (groups): Chỉ định nhóm (hoặc nhóm) mà user sẽ được thêm vào. Bạn có thể liệt kê một hoặc nhiều nhóm, ngăn cách bằng dấu phẩy (không có khoảng trắng).

:::

## Xóa user khỏi group

```bash
gpasswd -d [USERNAME] [GROUP_NAME_OR_ID]
```
