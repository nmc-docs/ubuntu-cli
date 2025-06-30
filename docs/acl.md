---
sidebar_position: 6
---

# Phân quyền với ACL

:::info[Định nghĩa]

- Thư viện **ACL (Access Control List)** trong Linux được sử dụng để quản lý quyền truy cập chi tiết hơn so với mô hình phân quyền truyền thống (owner, group, others). ACL cho phép gán quyền truy cập cho nhiều người dùng hoặc nhóm cụ thể trên một tệp hoặc thư mục.
- Ta dùng ACL khi ta cần **phân quyền chi tiết cho nhiều user hoặc group**, mà không muốn thay đổi owner hoặc group của file.

:::

## Cài đặt

```bash
apt update
apt install acl
```

## Các lệnh ACL cơ bản

### Xem ACL của tệp/thư mục

- Sử dụng lệnh `getfacl` để xem thông tin ACL của một tệp hoặc thư mục.
- Kết quả sẽ hiển thị quyền truyền thống (owner, group, others) và các mục ACL bổ sung (nếu có).
- Ví dụ:

```bash
getfacl testfile
```

Kết quả mẫu:

```bash
# file: testfile
# owner: user1
# group: group1
user::rw-
user:alice:r-x
group::r--
other::r--
```

- Ta thêm `grep` nếu muốn lọc theo user, group:

```bash
getfacl -R /usr/src/task_management_api_docker | grep "group:devteam_write:"
```

```bash
getfacl testfile | grep "user:ndmq0310:"
```

:::info[Thông tin thêm]

- Option `-R` liệt kê ACL cho thư mục và các tệp con (đệ quy)

:::

---

### Thiết lập ACL

- Sử dụng lệnh `setfacl` để thêm, sửa hoặc xóa quyền ACL.
- **Thêm quyền ACL cho người dùng cụ thể**: Gán quyền đọc, ghi, thực thi (`rwx`) cho người dùng `alice` trên tệp `testfile`:

```bash
setfacl -m u:alice:rwx testfile
```

- **Thêm quyền ACL cho nhóm cụ thể**: Gán quyền đọc và thực thi (`r-x`) cho nhóm `developers`:

```bash
setfacl -m g:developers:r-x testfile
```

- **Áp dụng ACL cho thư mục và các tệp con (đệ quy)**: Thêm quyền ACL cho thư mục và tất cả nội dung bên trong:

```bash
setfacl -R -m g:developers:rx /đường/dẫn/thư/mục
```

```bash
setfacl -R -m u:alice:rwx /đường/dẫn/thư/mục
```

- **Phân quyền cho nhiều người dùng**: Giả sử bạn có một tệp `project.txt` và muốn:
  - Người dùng `alice` có quyền đọc/ghi (`rw`).
  - Người dùng `bob` có quyền chỉ đọc (`r`).

```bash
setfacl -m u:alice:rw,u:bob:r project.txt
```

### Xóa ACL

- **Xóa tất cả ACL của group hoặc user đối với file / thư mục**:

```bash
setfacl -x g:[GROUP_NAME] project.txt
setfacl -x u:[USERNAME] project.txt
```

```bash
setfacl -R -x g:[GROUP_NAME] /đường/dẫn/thư/mục
setfacl -R -x u:[USERNAME] /đường/dẫn/thư/mục
```

- **Xóa toàn bộ ACL của file / thư mục**:

```bash
setfacl -b testfile.txt
```

```bash
setfacl -R -b /đường/dẫn/thư/mục
```
