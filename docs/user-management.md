---
sidebar_position: 2
---

# Quản lý user

:::note

- Tất cả lệnh bên dưới phải được thực hiện dưới quyền sudo, ta gõ lệnh sau:

```bash
sudo su
```

:::

## Tạo mới user và cấu hình SSH Private key cho nó

### Tạo user mới

- Giả sử ta tạo user mới là **ndmq0310**:

```bash
useradd -m -s /bin/bash -N ndmq0310
```

:::info[Giải thích]

- **-m** : Tạo một thư mục chính (home directory) cho người dùng mới. Thư mục này thường được đặt tại `/home/<tên_người_dùng>` (trong trường hợp này là `/home/ndmq0310`). Nếu không có tùy chọn này, thư mục chính sẽ không được tạo tự động.
- **-s /bin/bash** : Chỉ định shell mặc định cho người dùng. Ở đây, `/bin/bash` là đường dẫn đến shell Bash, nghĩa là khi người dùng **ndmq0310** đăng nhập, họ sẽ sử dụng Bash làm shell giao tiếp. Nếu không chỉ định, hệ thống sẽ sử dụng shell mặc định được cấu hình trong `/etc/default/useradd` (thường là `/bin/sh` hoặc một shell khác).
- **-N**: Không tạo nhóm người dùng riêng cho **ndmq0310**, thay vào đó sử dụng nhóm mặc định (thường là nhóm **users** hoặc nhóm được cấu hình trong hệ thống).

:::

### Thiết lập mật khẩu cho user mới

- Ta gõ lệnh bên dưới và sau đó nhập mật khẩu cho user vừa mới được tạo:

```bash
passwd ndmq0310
```

### Cấu hình SSH Private key

:::info[Thông tin]

- Mục đích của việc này là user **ndmq0310** có thể SSH vào VPS thông qua private key

:::

**Bước 1: Tạo cặp public/private key ở máy tính cá nhân của ta trước**

```bash
ssh-keygen -t rsa -b 2048 -f "/c/Users/MINH CHI/Downloads/ndmq0310_VPS_SSH_Key"
```

:::note[Chú ý]

- Thay đường dẫn lưu file private key lại theo nhu cầu của bạn.
- Có thể nhập **passphrase** hoặc không (passphrase hiểu đơn giản là mật khẩu của file private key. Khi bạn SSH vào VPS bằng Private key, nó sẽ yêu cầu ta nhập mật khẩu của file private key này, **nhớ là nó khác với mật khẩu của user ndmq0310 trên, không liên quan gì hết**)

:::

**Bước 2: Thêm public key vào VPS ứng với user ndmq0310**

- Sau khi tạo xong cặp public/private key, ta sẽ thấy có file đuôi `.pub`, thực hiện copy nội dung file đó vào VPS như sau:

```bash
echo "[NỘI DUNG PUBLIC KEY]" >> /home/ndmq0310/.ssh/authorized_keys
```

**Bước 3: Gán quyền đọc, ghi, thực thi trên folder .ssh cho user (nếu cần)**

```bash
chmod 600 /home/ndmq0310/.ssh/authorized_keys
chmod 700 /home/ndmq0310/.ssh
chown -R ndmq0310:root /home/ndmq0310/.ssh
```

**Bước 4: Restart lại SSH**

```bash
systemctl restart ssh
systemctl restart sshd
```

## Xem tất cả các user trong VPS (2 lệnh)

```bash
cat /etc/passwd
getent passwd
```

## Xem thông tin của user qua UID/Username

```bash
getent passwd [USER_ID/USERNAME]
```

## Xem thông tin uid, gid của một user

```bash
id [USERNAME]

//Xem thông tin uid, gid của user hiện tại
id
```

## Xem tất cả người dùng đang active trong VPS

```bash
who
```

## Hiển thị username của user hiện tại ta đang SSH vào VPS

```bash
whoami
```

## Đổi mật khẩu cho user

```bash
passwd [USERNAME]
```

:::tip

- Để đổi mật khẩu cho user hiện tại ta đang SSH, chỉ cần dùng lệnh:

```bash
passwd
```

:::

## Xóa user

```bash
userdel -rf [USERNAME]
```

## Chuyển đổi qua lại giữa các user

- Ví dụ đang ở user `minhchi1509`, giờ muốn chuyển sang user `ndmq0310` thì dùng lệnh:

```bash
su - ndmq0310 (Ta cần biết mật khẩu của user ndmq0310 (nếu không đang là root).)
sudo -u ndmq0310 -i (Nếu tài khoản minhchi1509 có quyền sudo. Hệ thống không yêu cầu ta nhập mật khẩu của ndmq0310, mà chỉ có thể yêu cầu mật khẩu sudo nếu chưa xác thực gần đây.)
```
