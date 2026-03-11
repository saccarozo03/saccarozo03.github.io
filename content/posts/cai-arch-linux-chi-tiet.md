---
title: "[Linux programing Course] Bài -1: Huong dan cai dat Arch Linux"
date: 2026-03-11
draft: false
series: ["Linux programing Course"]
weight: 0
tags: ["learning-log", "linux"]
---
# Hướng dẫn cài Arch Linux từ A đến Z
> **Dành cho:** Người muốn học Linux internals, Embedded Linux, Dev Environment  
> **Môi trường:** QEMU/KVM trên Ubuntu/Linux Mint (cũng áp dụng được cho máy thật)  
> **Thời gian:** ~1-2 giờ

---

## 📌 Trước khi bắt đầu — Hiểu kiến trúc tổng quan

Khi cài Arch Linux, bạn đang tự tay xây dựng hệ thống từ số 0. Khác với Ubuntu có installer tự động, Arch bắt bạn hiểu từng bước:

```
┌─────────────────────────────────────────────┐
│              QUÁ TRÌNH BOOT LINUX            │
│                                              │
│  UEFI Firmware                               │
│      ↓  (tìm EFI partition)                  │
│  GRUB Bootloader                             │
│      ↓  (load kernel)                        │
│  Linux Kernel                                │
│      ↓  (mount filesystem)                   │
│  systemd (PID 1)                             │
│      ↓  (start services)                     │
│  Login prompt                                │
└─────────────────────────────────────────────┘
```

Bạn sẽ tự tay cấu hình **từng tầng** trong sơ đồ trên.

---

## PHẦN 1 — Cài QEMU + Virt-Manager trên Ubuntu (máy host)

> **Tại sao dùng QEMU thay VirtualBox?**  
> QEMU dùng KVM — một module thẳng trong Linux kernel, cho phép máy ảo chạy gần tốc độ native. Ngoài ra QEMU có thể **emulate CPU ARM** — rất quan trọng khi làm Embedded Linux với Raspberry Pi / BeagleBone sau này.

---

### Bước 1 — Kiểm tra CPU có hỗ trợ KVM không

```bash
egrep -c '(vmx|svm)' /proc/cpuinfo
```

**Giải thích lệnh:**
- `egrep` — tìm kiếm text với regex (extended grep)
- `-c` — chỉ đếm số dòng khớp, không in ra nội dung
- `'(vmx|svm)'` — tìm chữ `vmx` (Intel) hoặc `svm` (AMD)
- `/proc/cpuinfo` — file ảo do kernel tạo ra, chứa thông tin CPU

**Kết quả:**
- Số **> 0** → CPU hỗ trợ ✅
- Số **= 0** → Vào BIOS/UEFI bật "Virtualization Technology" hoặc "SVM Mode"

---

### Bước 2 — Kiểm tra KVM sẵn sàng chưa

```bash
sudo apt install cpu-checker -y
kvm-ok
```

**Giải thích:**
- `cpu-checker` — package cung cấp lệnh `kvm-ok`
- `kvm-ok` — kiểm tra cả CPU lẫn kernel module `kvm` đã load chưa

**Kết quả mong muốn:**
```
INFO: /dev/kvm exists
KVM acceleration can be used
```

> `/dev/kvm` là file đặc biệt (character device) do kernel tạo ra khi module KVM được load. QEMU giao tiếp với KVM qua file này.

---

### Bước 3 — Cài QEMU và Virt-Manager

```bash
sudo apt install -y qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virt-manager
```

**Giải thích từng package:**

| Package | Vai trò | Tại sao cần |
|---|---|---|
| `qemu-kvm` | Phần mềm tạo và chạy máy ảo | Đây là "động cơ" chính |
| `libvirt-daemon-system` | Daemon `libvirtd` chạy ngầm | Quản lý tất cả máy ảo, network ảo |
| `libvirt-clients` | Bộ công cụ CLI như `virsh` | Điều khiển máy ảo bằng terminal |
| `bridge-utils` | Tạo network bridge | Cho máy ảo kết nối internet |
| `virt-manager` | Giao diện GUI | Dễ tạo/quản lý máy ảo bằng click |

---

### Bước 4 — Thêm user vào group libvirt và kvm

```bash
sudo usermod -aG libvirt $USER
sudo usermod -aG kvm $USER
```

**Giải thích lệnh:**
- `usermod` — lệnh chỉnh sửa thông tin user
- `-a` — append (thêm vào, không xóa group cũ)
- `-G libvirt` — thêm vào group `libvirt`
- `$USER` — biến môi trường tự động lấy tên user hiện tại

**Tại sao cần làm bước này?**  
Linux kiểm soát quyền truy cập qua "group". Mặc định chỉ `root` mới được dùng QEMU. Thêm user vào group `libvirt` và `kvm` giúp bạn dùng mà không cần `sudo` mỗi lần.

Áp dụng thay đổi ngay (không cần logout):
```bash
newgrp libvirt
```

---

### Bước 5 — Bật và kiểm tra libvirtd

```bash
sudo systemctl enable --now libvirtd
```

**Giải thích:**
- `systemctl` — công cụ quản lý service của systemd
- `enable` — đặt service tự chạy khi boot
- `--now` — chạy ngay lập tức (không cần reboot)
- `libvirtd` — daemon quản lý máy ảo

Kiểm tra:
```bash
sudo systemctl status libvirtd
```

**Kết quả mong muốn:** thấy `● libvirtd.service` với chữ `active (running)` màu xanh lá.

---

### Bước 6 — Tải ISO Arch Linux

```bash
cd ~/Downloads
wget https://geo.mirror.pkgbuild.com/iso/latest/archlinux-x86_64.iso
```

**Giải thích:**
- `geo.mirror.pkgbuild.com` — mirror chính thức của Arch, tự động redirect đến server gần bạn nhất (geo = geolocation)
- File ISO ~1.1GB, mất 5-15 phút tùy mạng

Xác minh file không bị lỗi trong quá trình tải:
```bash
sha256sum archlinux-x86_64.iso
```

So sánh output với hash SHA256 tại https://archlinux.org/download/  
Nếu hash **khớp hoàn toàn** → file nguyên vẹn ✅  
Nếu **không khớp** → file bị corrupt, tải lại

> **Tại sao phải xác minh hash?** Nếu file bị lỗi (mất mạng giữa chừng, server lỗi), cài từ ISO corrupt sẽ gây lỗi cực khó debug. Kiểm tra hash là thói quen tốt với mọi ISO.

---

## PHẦN 2 — Tạo máy ảo trong Virt-Manager

### Bước 7 — Mở Virt-Manager

```bash
virt-manager
```

### Bước 8 — Tạo máy ảo mới

1. Click **"Create a new virtual machine"** (icon dấu `+`)
2. Chọn **"Local install media (ISO or CDROM)"** → **Forward**
3. Click **Browse** → **Browse Local** → chọn file `archlinux-x86_64.iso`
4. Virt-Manager tự nhận diện **"Arch Linux"** → **Forward**

**Cấu hình tài nguyên:**
- RAM: **4096 MB** (4GB) — đủ để cài thoải mái
- CPU: **2 cores** — tăng tốc độ build/compile

5. Disk: **20 GB** — Arch base nhỏ ~2GB, còn lại để cài tools
6. Đặt tên: `arch-linux-learn`
7. ✅ Tick **"Customize configuration before install"** → **Finish**

### Bước 9 — Cấu hình UEFI (quan trọng!)

Trong cửa sổ config vừa hiện:
1. Click **"Overview"** ở panel trái
2. Mục **"Firmware"** → đổi sang:
   ```
   UEFI x86_64: /usr/share/OVMF/OVMF_CODE_4M.fd
   ```
3. Click **Apply** → **Begin Installation**

**Tại sao phải dùng UEFI?**  
Máy tính từ 2012 trở đi đều dùng UEFI thay BIOS cũ. Học cài với UEFI = đúng thực tế, áp dụng được cho máy thật sau này.

Nếu không thấy option UEFI:
```bash
sudo apt install ovmf -y
sudo systemctl restart libvirtd
```
Đóng Virt-Manager và mở lại.

---

## PHẦN 3 — Cài Arch Linux (bên trong máy ảo)

Sau khi boot vào ISO, bạn thấy dấu nhắc:
```
root@archiso ~ #
```

> Đây là môi trường **live** — chạy thẳng từ RAM, chưa có gì trên ổ cứng. Mọi thay đổi sẽ mất khi reboot, trừ những gì bạn ghi vào ổ cứng `/dev/vda`.

**Lưu ý quan trọng:** Trong môi trường này **không có chuột**, chỉ dùng bàn phím.

| Phím | Tác dụng |
|---|---|
| `Ctrl+C` | Dừng lệnh đang chạy |
| `Tab` | Tự hoàn thành lệnh/đường dẫn |
| `↑ ↓` | Xem lại lệnh đã gõ |
| `Ctrl+L` | Xóa màn hình |

---

### 🔵 GIAI ĐOẠN 1 — Chuẩn bị môi trường

#### Bước 10 — Kiểm tra kết nối internet

```bash
ping -c 3 8.8.8.8
```

**Giải thích:**
- `ping` — gửi gói tin ICMP đến địa chỉ đích để kiểm tra kết nối
- `-c 3` — chỉ gửi 3 gói rồi dừng (không có `-c` sẽ ping mãi)
- `8.8.8.8` — DNS server của Google, luôn online, dùng để test

**Kết quả mong muốn:**
```
3 packets transmitted, 3 received, 0% packet loss
```

> **Tại sao ping IP thay vì domain?**  
> Trong live ISO, DNS có thể chưa cấu hình đúng → ping domain bị treo dù có internet. Ping IP bypass DNS, kiểm tra kết nối thuần túy.

Nếu ping bị treo → kiểm tra card mạng:
```bash
ip addr show
```
Tìm interface `enp1s0` hoặc `eth0` có dòng `inet 192.168.x.x` → đã có IP.  
Nếu chưa có IP:
```bash
dhclient enp1s0
```

#### Bước 11 — Đồng bộ đồng hồ hệ thống

```bash
timedatectl set-ntp true
timedatectl status
```

**Giải thích:**
- `timedatectl` — công cụ quản lý thời gian của systemd
- `set-ntp true` — bật đồng bộ thời gian qua NTP (Network Time Protocol)

**Kết quả mong muốn:**
```
System clock synchronized: yes
NTP service: active
```

> **Tại sao phải đồng bộ đồng hồ?**  
> `pacman` kiểm tra **chữ ký số (GPG signature)** của mọi package. Chữ ký có thời hạn hiệu lực. Nếu đồng hồ sai lệch > vài phút → chữ ký bị coi là "hết hạn" hoặc "chưa có hiệu lực" → cài package bị lỗi.

---

### 🔵 GIAI ĐOẠN 2 — Chia và format ổ cứng

> **Đây là giai đoạn quan trọng nhất.** Hiểu được phần này là hiểu được một trong những thứ cốt lõi nhất của Linux và mọi hệ điều hành.

#### Khái niệm cần biết trước

```
Ổ cứng vật lý /dev/vda (20GB)
├── /dev/vda1  512MB  → EFI partition  (UEFI boot)
├── /dev/vda2  2GB    → Swap partition (RAM dự phòng)
└── /dev/vda3  17.5GB → Root partition (toàn bộ hệ thống)
```

- **EFI partition:** UEFI tìm vào đây đầu tiên khi máy khởi động, đọc GRUB bootloader
- **Swap partition:** Khi RAM đầy, kernel di chuyển dữ liệu ít dùng sang đây, tránh crash
- **Root partition (/):** Chứa toàn bộ hệ thống Linux — kernel, apps, config, home...

#### Bước 12 — Xem ổ cứng hiện có

```bash
lsblk
```

**Giải thích:**
- `lsblk` — list block devices, hiển thị tất cả ổ cứng và partition dạng cây

```bash
# Hoặc xem chi tiết hơn:
fdisk -l
```

**Kết quả mong muốn:**
```
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
vda    254:0    0   20G  0 disk
```

> **⚠️ Lưu ý:** Trong QEMU ổ cứng ảo tên là `vda` (virtio disk). Trên máy thật SSD/HDD thường là `sda`, NVMe là `nvme0n1`. **Luôn xác nhận đúng tên trước khi chia partition!**

#### Bước 13 — Chia partition bằng fdisk

```bash
fdisk /dev/vda
```

**Giải thích:** `fdisk` là công cụ chia partition tương tác. Mọi thay đổi chỉ được ghi vào ổ cứng khi bạn gõ `w` — trước đó có thể thoát bằng `q` an toàn.

Bạn sẽ thấy dấu nhắc:
```
Command (m for help):
```

Gõ lần lượt theo thứ tự sau:

**1. Tạo bảng partition GPT:**
```
g
```
```
Created a new GPT disklabel (GUID: ...)
```
> GPT (GUID Partition Table) là chuẩn mới, bắt buộc với UEFI. MBR cũ chỉ hỗ trợ tối đa 4 partition và ổ cứng < 2TB.

**2. Tạo partition EFI (partition 1 — 512MB):**
```
n
```
```
(Enter — giữ mặc định số 1)
```
```
(Enter — giữ mặc định first sector 2048)
```
```
+512M
```
```
Created a new partition 1 of type 'Linux filesystem' and of size 512 MiB.
```

**3. Tạo partition Swap (partition 2 — 2GB):**
```
n
```
```
(Enter — giữ mặc định số 2)
```
```
(Enter — giữ mặc định first sector)
```
```
+2G
```
```
Created a new partition 2 of type 'Linux filesystem' and of size 2 GiB.
```

**4. Tạo partition Root (partition 3 — toàn bộ còn lại):**
```
n
```
```
(Enter — giữ mặc định số 3)
```
```
(Enter — giữ mặc định first sector)
```
```
(Enter — dùng hết ~17.5GB còn lại)
```
```
Created a new partition 3 of type 'Linux filesystem' and of size 17.5 GiB.
```

**5. Đổi type partition 1 thành EFI System:**
```
t
```
```
1
```
```
1
```
```
Changed type of partition 'Linux filesystem' to 'EFI System'.
```
> Bước này đánh dấu partition 1 là "EFI System" trong bảng GPT để UEFI firmware nhận ra và tìm bootloader đúng chỗ.

**6. Lưu và thoát:**
```
w
```
```
The partition table has been altered.
Syncing disks.
```

Kiểm tra kết quả:
```bash
lsblk
```

**Kết quả mong muốn:**
```
NAME   SIZE TYPE
vda     20G disk
├─vda1 512M part
├─vda2   2G part
└─vda3 17.5G part
```

#### Bước 14 — Format các partition

Mỗi partition cần được format với filesystem phù hợp trước khi dùng.

**Format partition EFI — bắt buộc phải là FAT32:**
```bash
mkfs.fat -F32 /dev/vda1
```
> UEFI firmware chỉ đọc được FAT32. Đây là chuẩn bắt buộc theo spec UEFI, không thể dùng ext4 hay btrfs cho EFI partition.

**Format partition Swap:**
```bash
mkswap /dev/vda2
```
> Swap có cấu trúc đặc biệt, không phải filesystem thông thường. `mkswap` tạo header và cấu trúc cần thiết để kernel quản lý swap.

**Format partition Root — dùng ext4:**
```bash
mkfs.ext4 /dev/vda3
```
> ext4 (Fourth Extended Filesystem) là filesystem Linux phổ biến và ổn định nhất. Hỗ trợ journaling (ghi log trước khi thay đổi) — giúp phục hồi dữ liệu khi mất điện đột ngột.

#### Bước 15 — Mount partition vào hệ thống

Mount = "gắn" partition vào một thư mục để có thể đọc/ghi.

**Mount Root trước tiên:**
```bash
mount /dev/vda3 /mnt
```
> `/mnt` là thư mục tạm trong live ISO, dùng làm điểm mount cho hệ thống mới.

**Tạo thư mục và mount EFI:**
```bash
mkdir -p /mnt/boot/efi
mount /dev/vda1 /mnt/boot/efi
```
> `-p` tạo cả thư mục cha nếu chưa tồn tại. EFI partition phải mount vào `/boot/efi` để GRUB tìm được.

**Bật Swap:**
```bash
swapon /dev/vda2
```

Kiểm tra lại toàn bộ:
```bash
lsblk
```

**Kết quả mong muốn:**
```
NAME   SIZE TYPE MOUNTPOINTS
vda     20G disk
├─vda1 512M part /mnt/boot/efi
├─vda2   2G part [SWAP]
└─vda3 17.5G part /mnt
```

---

### 🔵 GIAI ĐOẠN 3 — Cài hệ thống

#### Bước 16 — Cài các package cơ bản bằng pacstrap

```bash
pacstrap /mnt base linux linux-firmware networkmanager vim
```

**Giải thích từng thành phần:**

| Package | Vai trò | Chi tiết |
|---|---|---|
| `base` | Hệ thống tối thiểu | bash, glibc, coreutils, systemd... |
| `linux` | Linux kernel | File `/boot/vmlinuz-linux` |
| `linux-firmware` | Firmware cho hardware | WiFi, GPU, Bluetooth... |
| `networkmanager` | Quản lý mạng | Tự động kết nối mạng sau boot |
| `vim` | Text editor | Để chỉnh file config |

> **`pacstrap` khác `pacman` chỗ nào?**  
> `pacman` cài package vào hệ thống **đang chạy**.  
> `pacstrap /mnt` cài package vào thư mục `/mnt` — tức là hệ thống Arch **trên ổ cứng**, không phải live ISO.

Download ~200-300MB, mất 3-10 phút. Chờ đến khi về lại dấu nhắc `root@archiso`.

#### Bước 17 — Tạo file fstab

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

**Giải thích:**
- `genfstab` — generate fstab, tự động tạo nội dung file fstab từ các partition đang mount
- `-U` — dùng UUID thay tên `/dev/vda1` (UUID là mã định danh duy nhất, không thay đổi dù thêm ổ cứng mới)
- `>>` — append vào file (không ghi đè)
- `/mnt/etc/fstab` — file đích trên hệ thống mới

> **fstab là gì?**  
> `/etc/fstab` (filesystem table) là "bản đồ mount" của Linux. Mỗi lần boot, kernel đọc file này để biết mount partition nào vào thư mục nào. Thiếu file này → boot thất bại.

Kiểm tra nội dung:
```bash
cat /mnt/etc/fstab
```

**Kết quả mong muốn** — thấy 3 entry:
```
# /dev/vda3
UUID=xxxx-xxxx  /          ext4   rw,relatime  0 1

# /dev/vda1
UUID=xxxx-xxxx  /boot/efi  vfat   umask=0022   0 2

# /dev/vda2
UUID=xxxx-xxxx  none       swap   defaults     0 0
```

#### Bước 18 — Chroot vào hệ thống mới

```bash
arch-chroot /mnt
```

**Giải thích:**
- `chroot` (change root) — thay đổi thư mục root từ `/` của live ISO sang `/mnt` (hệ thống mới)
- `arch-chroot` — phiên bản của Arch, tự động bind mount `/proc`, `/sys`, `/dev` để mọi lệnh hoạt động đúng

Dấu nhắc đổi thành:
```
[root@archiso /]#
```

> Từ đây bạn đang **"đứng bên trong"** hệ thống Arch mới. Mọi lệnh gõ sẽ ảnh hưởng đến ổ cứng thật, không phải RAM của live ISO nữa.

---

### 🔵 GIAI ĐOẠN 4 — Cấu hình hệ thống

#### Bước 19 — Cài đặt Timezone

```bash
ln -sf /usr/share/zoneinfo/Asia/Ho_Chi_Minh /etc/localtime
```

**Giải thích:**
- `ln -sf` — tạo symbolic link (shortcut), `-s` = symbolic, `-f` = force (ghi đè nếu đã tồn tại)
- `/usr/share/zoneinfo/Asia/Ho_Chi_Minh` — file timezone data của Việt Nam
- `/etc/localtime` — file timezone mà hệ thống đọc

> Để xem danh sách timezone khác:  
> `ls /usr/share/zoneinfo/Asia/`

Đồng bộ hardware clock:
```bash
hwclock --systohc
```
> Ghi giờ từ system clock (software) vào hardware clock (RTC chip trên mainboard). Đảm bảo giờ đúng sau mỗi lần tắt máy.

#### Bước 20 — Cấu hình Locale

Locale xác định ngôn ngữ, định dạng số, tiền tệ, ngày tháng của hệ thống.

Mở file cấu hình:
```bash
vim /etc/locale.gen
```

**Cách dùng vim:**
```
/en_US.UTF-8     ← gõ để tìm kiếm, Enter để nhảy đến kết quả
x                ← xóa ký tự # ở đầu dòng
:wq              ← lưu và thoát
```

Dòng cần bỏ comment:
```
# Trước:
#en_US.UTF-8 UTF-8

# Sau:
en_US.UTF-8 UTF-8
```

Generate locale:
```bash
locale-gen
```

Tạo file config locale:
```bash
echo "LANG=en_US.UTF-8" > /etc/locale.conf
```

> **Tại sao dùng en_US thay vì tiếng Việt?**  
> Terminal Linux hiển thị tiếng Việt dễ bị lỗi font trong môi trường không có desktop. Dùng `en_US` cho hệ thống, tiếng Việt cài sau khi có desktop environment.

#### Bước 21 — Đặt Hostname

Hostname là "tên" của máy tính trong mạng nội bộ.

```bash
echo "archlinux" > /etc/hostname
```

> Có thể đặt tên tùy thích: `arch-dev`, `my-arch`, `duc-laptop`...

Cấu hình file hosts — "danh bạ DNS" cục bộ:
```bash
vim /etc/hosts
```

Gõ `i` vào insert mode, thêm nội dung:
```
127.0.0.1   localhost
::1         localhost
127.0.1.1   archlinux.localdomain   archlinux
```

Gõ `Esc` rồi `:wq` để lưu.

> **`/etc/hosts` hoạt động thế nào?**  
> Khi ứng dụng cần resolve hostname, hệ thống tra `/etc/hosts` **trước** khi hỏi DNS server. `127.0.0.1` và `::1` (IPv6) đều trỏ về localhost — máy chính nó.  
> Dòng `127.0.1.1` giúp resolve hostname của máy thành địa chỉ loopback, một số ứng dụng cần điều này.

#### Bước 22 — Tạo password root và user

**Đặt password cho root:**
```bash
passwd
```
> Nhập password 2 lần. Khi gõ sẽ **không hiện ký tự gì** — đây là tính năng bảo mật của Linux, không phải lỗi.

**Tạo user thường:**
```bash
useradd -m -G wheel -s /bin/bash tenuser
```

**Giải thích từng option:**
| Option | Ý nghĩa |
|---|---|
| `-m` | Tạo thư mục home `/home/tenuser` |
| `-G wheel` | Thêm vào group `wheel` để dùng sudo |
| `-s /bin/bash` | Đặt bash làm shell mặc định |
| `tenuser` | Thay bằng tên user bạn muốn |

**Đặt password cho user:**
```bash
passwd tenuser
```

**Cài sudo:**
```bash
pacman -S sudo
```

**Cấp quyền sudo cho group wheel:**
```bash
EDITOR=vim visudo
```

Tìm dòng này và bỏ dấu `#`:
```
# Trước:
# %wheel ALL=(ALL:ALL) ALL

# Sau:
%wheel ALL=(ALL:ALL) ALL
```

> **`%wheel ALL=(ALL:ALL) ALL` nghĩa là gì?**  
> `%wheel` — tất cả user trong group wheel  
> `ALL=` — trên mọi máy (dùng khi có nhiều host)  
> `(ALL:ALL)` — được chạy với quyền của bất kỳ user:group nào  
> `ALL` — được chạy bất kỳ lệnh nào  
> → Tóm lại: user trong group wheel có thể chạy mọi lệnh với `sudo`

#### Bước 23 — Cài bootloader GRUB

> **GRUB là gì?**  
> GRUB (Grand Unified Bootloader) là chương trình **đầu tiên chạy** sau UEFI firmware. Nhiệm vụ của GRUB là load Linux kernel vào RAM và khởi động hệ thống. Không có GRUB → máy không boot được.

**Cài package:**
```bash
pacman -S grub efibootmgr
```

| Package | Vai trò |
|---|---|
| `grub` | Bootloader chính |
| `efibootmgr` | Quản lý boot entries UEFI, GRUB cần để đăng ký vào NVRAM |

**Cài GRUB vào EFI partition:**
```bash
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB
```

**Giải thích từng option:**
| Option | Ý nghĩa |
|---|---|
| `--target=x86_64-efi` | Cài cho hệ thống UEFI 64-bit |
| `--efi-directory=/boot/efi` | Chỉ đường đến EFI partition đã mount |
| `--bootloader-id=GRUB` | Tên hiển thị trong menu boot UEFI |

**Kết quả mong muốn:**
```
Installation finished. No error reported.
```

**Tạo file config GRUB:**
```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

> `grub-mkconfig` tự động quét hệ thống, tìm kernel, tạo menu boot. `-o` chỉ đường dẫn output.

**Kết quả mong muốn:**
```
Found linux image: /boot/vmlinuz-linux
Found initrd image: /boot/initramfs-linux.img
```

---

### 🔵 GIAI ĐOẠN 5 — Hoàn thiện và reboot

#### Bước 24 — Bật NetworkManager tự động

```bash
systemctl enable NetworkManager
```

> **Tại sao phải enable?**  
> `pacman -S networkmanager` chỉ cài package, không tự bật service.  
> `systemctl enable` tạo symlink trong `/etc/systemd/system/` để service tự chạy khi boot.  
> Nếu quên bước này → sau reboot sẽ không có internet.

#### Bước 25 — Thoát chroot và reboot

**Thoát khỏi chroot:**
```bash
exit
```

**Unmount tất cả partition:**
```bash
umount -R /mnt
```
> `-R` = recursive, unmount `/mnt` và tất cả partition con (`/mnt/boot/efi`).  
> **Tại sao phải unmount trước khi reboot?**  
> Unmount đảm bảo kernel flush hết dữ liệu từ RAM cache xuống ổ cứng. Reboot mà không unmount có thể gây filesystem corruption.

**Reboot:**
```bash
reboot
```

> Máy ảo sẽ khởi động lại. Nếu boot lại vào ISO thay vì Arch:  
> Virt-Manager → chọn máy ảo → **View** → **Details** → **Boot Options** → kéo **VirtIO Disk** lên trên **CDROM** → Apply

---

## PHẦN 4 — Sau khi cài xong

### Bước 26 — Đăng nhập lần đầu

Màn hình login hiện ra:
```
archlinux login:
```

Đăng nhập bằng user vừa tạo (không dùng root hàng ngày):
```
login: tenuser
Password: (gõ password)
```

Dấu nhắc thành công:
```
[tenuser@archlinux ~]$
```

### Bước 27 — Kiểm tra hệ thống

```bash
# Kiểm tra kernel đang chạy
uname -r

# Kiểm tra kết nối internet
ping -c 3 8.8.8.8

# Kiểm tra ổ cứng và partition
lsblk

# Kiểm tra RAM và Swap
free -h

# Kiểm tra CPU
lscpu | head -20
```

---

## PHẦN 5 — Áp dụng trên máy thật (khác máy ảo)

Khi cài lên máy thật, lưu ý các điểm khác biệt:

| Điểm khác biệt | Máy ảo QEMU | Máy thật |
|---|---|---|
| Tên ổ cứng | `/dev/vda` | SSD/HDD: `/dev/sda`, NVMe: `/dev/nvme0n1` |
| Kết nối WiFi | Có sẵn (NAT) | Cần cài `iwd` hoặc dùng `iwctl` |
| Driver GPU | Không cần | Cần cài `mesa`, `nvidia`, hoặc `amdgpu` |
| Dual boot | Không áp dụng | Cần cẩn thận với Windows EFI partition |

**Kết nối WiFi trong live ISO (máy thật):**
```bash
iwctl
station wlan0 scan
station wlan0 get-networks
station wlan0 connect "TenWifi"
exit
```

**Lưu ý quan trọng khi dual boot với Windows:**
- Windows đã có EFI partition rồi → **không tạo EFI partition mới**, dùng lại cái cũ
- Chỉ tạo Swap và Root partition mới
- Mount EFI partition của Windows vào `/mnt/boot/efi`

---

## Tổng kết — Những gì bạn đã học được

```
Cài Arch Linux thành công = Bạn đã hiểu:

✅ Kiến trúc boot: UEFI → GRUB → Kernel → systemd
✅ Partition: GPT, EFI System, Swap, Root
✅ Filesystem: FAT32 (EFI), ext4 (Linux), swap
✅ Mount system: /etc/fstab, mount/umount
✅ chroot: Kỹ thuật "bước vào" hệ thống Linux khác
✅ Package manager: pacman, pacstrap
✅ systemd: systemctl enable/start/status
✅ User management: useradd, passwd, sudo, wheel group
✅ Bootloader: GRUB install và config
```

**So với người dùng Ubuntu/Windows thông thường:**  
Họ không biết bất kỳ điều nào trong danh sách trên. Bạn thì biết tất cả — và biết từ thực hành! 💪

---

## Tài liệu tham khảo

- [Arch Linux Installation Guide (chính thức)](https://wiki.archlinux.org/title/Installation_guide)
- [Arch Wiki — GRUB](https://wiki.archlinux.org/title/GRUB)
- [Arch Wiki — fstab](https://wiki.archlinux.org/title/Fstab)
- [Arch Wiki — Users and groups](https://wiki.archlinux.org/title/Users_and_groups)
- [QEMU/KVM — Arch Wiki](https://wiki.archlinux.org/title/QEMU)
