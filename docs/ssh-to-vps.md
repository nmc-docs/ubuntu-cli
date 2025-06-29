---
sidebar_position: 1
slug: /
---

# SSH vào VPS

- Câu lệnh để SSH vào VPS bằng Private Key (khuyến nghị):

```bash
ssh -i [PATH_TO_PRIVATE_KEY] [USERNAME]@[VPS_IP]
```

- Ví dụ:

```bash
ssh -i ~/.ssh/id_rsa newuser@10.25.68.23
```

## Sự khác biệt giữa PPK và PEM

| Tính năng                  | Save Private Key (PPK)     | Export OpenSSH Key (PEM)                |
| -------------------------- | -------------------------- | --------------------------------------- |
| **Định dạng**              | `.ppk` (PuTTY Format)      | `.pem` hoặc không có đuôi (OpenSSH)     |
| **Dùng cho hệ điều hành**  | Windows (PuTTY, Pageant)   | Linux/macOS (OpenSSH, SSH client)       |
| **Dùng với `ssh -i`**      | ❌ Không (Cần chuyển đổi)  | ✅ Có thể dùng trực tiếp                |
| **Mã hóa passphrase**      | Hỗ trợ                     | Hỗ trợ                                  |
| **Chuyển đổi được không?** | ✅ Có thể đổi sang OpenSSH | ✅ Có thể đổi sang `.ppk` bằng PuTTYgen |

**💡 Khi nào nên dùng cái nào?**

- **Dùng Windows, kết nối qua PuTTY?** → **Dùng "Save private key" (`.ppk`).**
- **Dùng Linux/macOS, SSH bằng terminal?** → **Dùng "Export OpenSSH Key" (`.pem`).**
- **Dùng trên Windows nhưng muốn kết nối với OpenSSH?** → **Dùng "Export OpenSSH Key" và dùng `ssh -i` trong PowerShell hoặc Git Bash.**

🔁 3. **Chuyển đổi giữa các định dạng**

- **Từ PPK ➜ PEM**:
  - Mở PuTTYgen ➜ Load file `.ppk` ➜ "Conversions" ➜ "Export OpenSSH key".
- **Từ PEM ➜ PPK**:
  - Mở PuTTYgen ➜ Load file `.pem` ➜ "Save private key".

## Cách ngăn chặn SSH vào VPS bằng password, chỉ được SSH vào bằng private key

- Vào file `/etc/ssh/sshd_config` và các **file inclucde** (nếu có):

```bash
nano /etc/ssh/sshd_config
```

và chỉnh lại (hoặc thêm) các trường sau:

```text
PermitRootLogin no
PubkeyAuthentication yes
PermitEmptyPasswords no
ChallengeResponseAuthentication no
UsePAM yes
PasswordAuthentication no
```

- Sau đó restart lại SSH:

```bash
systemctl restart ssh
systemctl restart sshd
```
