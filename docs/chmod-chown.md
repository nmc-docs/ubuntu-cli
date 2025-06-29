---
sidebar_position: 4
---

# `chmod`, `chown`

:::info[Thông tin]

- Lệnh `chmod` và `chown` là hai lệnh quan trọng trong các hệ điều hành dựa trên Unix/Linux, được sử dụng để quản lý quyền truy cập và quyền sở hữu của tệp và thư mục.

:::

## Lệnh `chmod`

:::info[Thông tin]

- Mục đích: Lệnh `chmod` được sử dụng để thay đổi quyền truy cập (permissions) của tệp hoặc thư mục. Quyền truy cập xác định ai có thể đọc (read), ghi (write) hoặc thực thi (execute) tệp/thư mục.

:::

- Cú pháp:

```bash
chmod -R [ACCESS_PERMISSION_CONFIG] [FILE/FOLDER]
```

- Trong đó:
  - `-R`: Thay đổi quyền truy cập cho cả thư mục và tất cả các tệp/thư mục con bên trong (đệ quy).

---

**Quyền truy cập**:

- Quyền được chia thành 3 nhóm: **owner** (người sở hữu), **group** (nhóm), **others** (người khác).
- Mỗi nhóm có 3 loại quyền:
  - **r** (read - đọc): Quyền xem nội dung tệp hoặc liệt kê thư mục.
  - **w** (write - ghi): Quyền chỉnh sửa hoặc xóa tệp/thư mục.
  - **x** (execute - thực thi): Quyền chạy tệp (nếu là chương trình) hoặc truy cập vào thư mục.

---

**Cách biểu diễn quyền:**

1. **Số (Octal)**:

- Quyền được biểu diễn bằng số từ 0 đến 7:
  - **4** : read (r)
  - **2** : write (w)
  - **1** : execute (x)
- Tổng hợp quyền bằng cách cộng các số: ví dụ, 7 = rwx (4+2+1), 6 = rw- (4+2), 5 = r-x (4+1).
- Quyền cho cả 3 nhóm được viết dưới dạng 3 chữ số, ví dụ: **755 (owner: rwx, group: r-x, others: r-x)**.

2. **Ký hiệu (Symbolic)**:

- Sử dụng các ký hiệu: `u` (user/owner), `g` (group), `o` (others), `a` (all).
- Thêm (`+`), bớt (`-`), hoặc gán chính xác (`=`) quyền: `r`, `w`, `x`.

---

**Ví dụ**:

```bash
chmod -R u=rw,g=rw,o=r /usr/src/task_management_api_docker/
```

:::info

- Trong ví dụ trên, ta gán quyền cho thư mục và các file, thư mục con bên trong:
  - **owner** (người sở hữu): `read`, `write`
  - **group** (nhóm): `read`, `write`
  - **others** (người khác): `read`

:::

:::caution[Lưu ý]

- Quyền `x` trên thư mục cho phép truy cập vào thư mục (vào được nhưng chưa chắc xem được nội dung nếu thiếu `r`).
- Sử dụng lệnh `ls -l` để kiểm tra quyền hiện tại của tệp/thư mục.

:::

## Lệnh `chown`

:::info[Thông tin]

- Lệnh `chown` được sử dụng để thay đổi người sở hữu (**owner**) hoặc nhóm sở hữu (**group**) của tệp hoặc thư mục.

:::

- Cú pháp:

```bash
chown [tùy chọn] user[:group] file
```

- **user**: Tên hoặc ID người dùng mới.
- **group**: Tên hoặc ID nhóm mới (tùy chọn).
- **file**: Tên tệp hoặc thư mục cần thay đổi quyền sở hữu.
- **Tùy chọn**:
  - `-R`: Thay đổi quyền sở hữu cho cả thư mục và các tệp/thư mục con bên trong (đệ quy).

---

**Ví dụ**:

- Chuyển quyền sở hữu tệp cho người dùng **john**:

```bash
chown john file.txt
```

- Chuyển quyền sở hữu tệp cho người dùng **john** và nhóm **developers**:

```bash
chown john:developers file.txt
```

- Thay đổi nhóm sở hữu mà không thay đổi owner:

```bash
chown :developers file.txt
```

- Thay đổi quyền sở hữu đệ quy cho thư mục:

```bash
chown -R john:developers my_folder/
```

:::note[Lưu ý]

- Chỉ người dùng có quyền (thường là `root` hoặc người dùng với `sudo`) mới có thể thay đổi quyền sở hữu.

---

- Sử dụng lệnh `ls -l` để kiểm tra thông tin người sở hữu và nhóm của tệp/thư mục.

```bash
ls -l file.txt
```

Kết quả ví dụ: `-rwxr-xr-x 1 john developers 4096 Jun 29 2025 file.txt`

- `-rwxr-xr-x`: Quyền (`owner: rwx`, `group: r-x`, `others: r-x`).
- `john`: Người sở hữu.
- `developers`: Nhóm sở hữu.

:::
