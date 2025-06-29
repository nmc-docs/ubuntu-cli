---
sidebar_position: 5
---

# Tìm kiếm

- **Tìm kiếm thư mục bên trong thư mục hiện tại:**

```bash
find . -type d -name <FOLDER_NAME_TO_FIND>
```

- **Tìm kiếm file bên trong thư mục hiện tại:**

```bash
find . -type f -name <FILE_NAME_TO_FIND>
```

- **Tìm kiếm các file/folder trong thư mục hiện tại mà user làm owner hoặc group được chỉ định:**

```bash
find ./* -user [USERNAME] -exec ls -l {} \;
```

```bash
find ./* -group [GROUP_NAME] -exec ls -l {} \;
```

- **Tìm tất cả các tệp/thư mục trong thư mục hiện tại và các thư mục con thuộc sở hữu của người dùng `ndmq0310`, sau đó chuyển quyền sở hữu của chúng sang người dùng `root` và nhóm `root`.**

```bash
find . -user ndmq0310 -exec sudo chown root:root {} \;
```

- **Thực hiện đổi permissions về mặc định cho tất cả file/folder bên trong thư mục hiện tại:**

```bash
find . -type f -exec chmod u=rw,g=r,o=r {} \;
find . -type d -exec chmod u=rwx,g=rx,o=rx {} \;
```
