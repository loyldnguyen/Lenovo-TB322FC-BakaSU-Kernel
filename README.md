<div align="center">

# 🐱 Lenovo TB322FC · BakaSU Kernel

**Build kernel GKI `android15-6.6` cho Lenovo TB322FC, tích hợp sẵn BakaSU + SUSFS, hoàn toàn tự động bằng GitHub Actions.**

[![Build](https://img.shields.io/github/actions/workflow/status/loyldnguyen/Lenovo-TB322FC-BakaSU-Kernel/main.yml?style=for-the-badge&logo=githubactions&logoColor=white&label=Build)](https://github.com/loyldnguyen/Lenovo-TB322FC-BakaSU-Kernel/actions/workflows/main.yml)
![Kernel](https://img.shields.io/badge/Kernel-6.6-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![Android](https://img.shields.io/badge/Android-15-3DDC84?style=for-the-badge&logo=android&logoColor=white)
![Root](https://img.shields.io/badge/Root-BakaSU-ff69b4?style=for-the-badge)
![Clang](https://img.shields.io/badge/Clang-18%20r510928-262D3A?style=for-the-badge&logo=llvm&logoColor=white)

[✨ Tính năng](#-tính-năng) •
[📱 Phiên bản hỗ trợ](#-phiên-bản-zuxos-được-hỗ-trợ) •
[🚀 Cách build](#-cách-build) •
[⚙️ Tuỳ chọn](#️-các-tuỳ-chọn-khi-build) •
[🔑 Manager tuỳ chỉnh](#-dùng-manager-với-package-name-tuỳ-chỉnh) •
[📲 Cài đặt](#-cài-đặt) •
[🙏 Credits](#-credits)

</div>

---

> [!CAUTION]
> Flash kernel tự build luôn có rủi ro **bootloop**. Hãy **sao lưu `boot.img` gốc** trước khi flash và chỉ làm khi bạn biết cách khôi phục bằng `fastboot`. Mọi rủi ro bạn tự chịu trách nhiệm.

## ✨ Tính năng

| | Tính năng | Mô tả |
|:-:|---|---|
| 🦊 | **BakaSU** | Root ở mức kernel, tự lấy bản mới nhất (hoặc branch bạn chọn) và đính kèm manager APK tương ứng |
| 🫥 | **SUSFS** | Ẩn root: sus path, sus mount, sus kstat, sus map, spoof uname / cmdline, open redirect, priority UID filter |
| 🔑 | **Manager tuỳ chỉnh** | Hỗ trợ manager đổi package name & tự ký: tự đọc chữ ký từ APK, có thể khoá đúng 1 package |
| 🧩 | **KPatch-Next (KPN)** | Tuỳ chọn vá kernel module implementation bằng KPN |
| 🌐 | **Netfilter / IPSet** | Bổ sung IP set, NAT IPv6, addrtype… cho các module tường lửa / proxy |
| 🚄 | **BBR + ECN** | Thuật toán TCP BBR, FQ qdisc và ECN |
| 🔤 | **Fix Unicode bypass** | Vá lỗi bypass bằng ký tự Unicode vô hình |
| 🖱️ | **USB polling 1 ms** | Chuột / bàn phím USB (dây hoặc 2.4 GHz) chạy 1000 Hz |
| 🗜️ | **zstd 1.5.7** | Nâng cấp zstd cho zram, kèm bản vá **CVE-2026-43499** |
| 🎭 | **Giống kernel gốc** | Giữ nguyên tên kernel, thời gian build, user/host và thông tin compiler như bản stock |
| ⚡ | **Build nhanh** | ccache (kèm public cache), cache toolchain, tải song song |

## 📱 Phiên bản ZUXOS được hỗ trợ

| ZUXOS | Kernel | Tên kernel gốc |
|---|:-:|---|
| `1.1.10.043` → `1.1.11.120` | `6.6.56` | `-android15-8-g38447e018c92-ab12829524-4k` |
| `1.1.11.188` → `1.5.10.117` | `6.6.89` | `-android15-8-g14220ae4ce65-ab13680582-4k` |
| `1.5.10.096` | `6.6.102` | `-android15-8-g06bc416365ef-ab14559830-4k` |
| `1.5.10.183` → `1.5.10.294` | `6.6.118` | `-android15-8-g350f378eb496-ab14943902-4k` |

> [!TIP]
> Kiểm tra phiên bản hiện tại trong **Cài đặt → Thông tin máy**, hoặc chạy `uname -r` để xem tên kernel đang dùng.

## 🚀 Cách build

1. **Fork** repo này về tài khoản của bạn.
2. Vào tab **Actions** → bật workflow nếu GitHub hỏi.
3. Chọn **Build Lenovo TB322FC BakaSU Kernel** → **Run workflow**.
4. Chọn phiên bản ZUXOS và các tuỳ chọn, rồi bấm **Run workflow**.
5. Đợi build xong (lần đầu khoảng 20–40 phút, các lần sau nhanh hơn nhờ ccache), tải artifact ở cuối trang run.

Artifact **`BakaSU_kernel-android15-6.6`** gồm:

```text
📦 BakaSU_kernel-android15-6.6.zip
 ├── kernel          # Image kernel đã build
 └── *.apk           # Manager BakaSU (hoặc manager tuỳ chỉnh của bạn)
```

## ⚙️ Các tuỳ chọn khi build

| Input | Mặc định | Mô tả |
|---|:-:|---|
| `releases` | `1.5.10.183 to 1.5.10.294` | Phiên bản ZUXOS cần build |
| `ksu_ref` | *(trống = `main`)* | Branch / tag / commit của BakaSU |
| `custom_kernel_suffix` | *(trống = stock)* | Hậu tố tên kernel tuỳ chỉnh |
| `custom_kernel_time` | *(trống = stock)* | Thời gian build tuỳ chỉnh, ví dụ `Thu Feb 26 02:40:50 UTC 2026` |
| `kernel_module_impl` | `N/A` | `KPN` để vá bằng KPatch-Next |
| `enable_netfilter` | ✅ | Bật Netfilter / IPSet |
| `enable_bbr_ecn` | ✅ | Bật BBR + ECN |
| `enable_unicode_bypass` | ✅ | Vá Unicode bypass |
| `enable_susfs` | ✅ | Bật SUSFS |
| `usb_poll_1ms` | ✅ | USB HID polling 1 ms |
| `manager_package_name` | *(trống)* | Chỉ chấp nhận manager có package này |
| `manager_apk_url` | *(trống)* | Link APK manager tự ký: tự đọc chữ ký và đóng gói kèm |
| `manager_sign_size` | *(trống)* | Kích thước chứng chỉ ký (vd `0x377`), dùng khi không có APK URL |
| `manager_sign_hash` | *(trống)* | SHA-256 chứng chỉ ký, dùng khi không có APK URL |

## 🔑 Dùng manager với package name tuỳ chỉnh

BakaSU nhận diện manager **bằng chữ ký APK**, không phải bằng package name. Vì vậy khi bạn build manager với package riêng (ví dụ `-PKSU_PACKAGE_NAME=com.my.manager`) và **ký bằng key của mình**, kernel cần được báo chữ ký đó.

### Cách 1: Đưa link APK (khuyên dùng)

Upload APK manager lên đâu đó tải trực tiếp được (ví dụ GitHub Releases), rồi điền:

```yaml
manager_apk_url:       https://github.com/<you>/<repo>/releases/download/v1/manager.apk
manager_package_name:  com.my.manager   # không bắt buộc
```

Workflow sẽ:

- ✅ Kiểm tra APK chỉ ký **v2** (BakaSU từ chối APK có chữ ký v1 / v3 / v4)
- ✅ Đọc kích thước và SHA-256 của chứng chỉ, đưa vào kernel qua `KSU_EXPECTED_SIZE` / `KSU_EXPECTED_HASH`
- ✅ Đối chiếu `manager_package_name` với package thật trong APK
- ✅ Kiểm tra lại chữ ký và package đã nằm trong `Image` sau khi build
- ✅ Đóng gói APK của bạn vào artifact thay cho manager chính thức

### Cách 2: Nhập chữ ký thủ công

Điền `manager_sign_size` và `manager_sign_hash`. Hash là SHA-256 của chứng chỉ ký (`apksigner verify --print-certs manager.apk` → dòng `certificate SHA-256 digest`); size là độ dài chứng chỉ ở dạng DER, tính bằng byte.

> [!IMPORTANT]
> - Khi build manager, hãy **tắt ký v1, v3 và v4** (chỉ để v2), nếu không kernel sẽ không nhận manager.
> - Nếu điền `manager_package_name`, kernel **chỉ** nhận đúng package đó, kể cả manager BakaSU chính thức cũng sẽ không được nhận.
> - Để trống cả 4 input thì workflow chạy như bình thường với chữ ký BakaSU có sẵn.

## 📲 Cài đặt

File `kernel` là Image thô, cần được đưa vào `boot.img` gốc đúng phiên bản máy bạn đang dùng:

```bash
# 1. Giải nén boot.img gốc
magiskboot unpack boot.img

# 2. Thay kernel bằng bản vừa build
cp /đường/dẫn/tới/kernel ./kernel

# 3. Đóng gói lại
magiskboot repack boot.img new-boot.img

# 4. Flash
fastboot flash boot new-boot.img
fastboot reboot
```

Sau khi khởi động, cài **manager APK** trong artifact và mở lên để kiểm tra trạng thái.

> [!NOTE]
> Nếu bị bootloop, vào fastboot và flash lại `boot.img` gốc: `fastboot flash boot boot.img`.

## 🙏 Credits

- [Baka-SU/BakaSU](https://github.com/Baka-SU/BakaSU): giải pháp root
- [tiann/KernelSU](https://github.com/tiann/KernelSU): dự án gốc
- [simonpunk/susfs4ksu](https://gitlab.com/simonpunk/susfs4ksu): SUSFS
- [KernelSU-Next/KPatch-Next](https://github.com/KernelSU-Next/KPatch-Next): KPN
- [cctv18/oppo_oplus_realme_sm8750](https://github.com/cctv18/oppo_oplus_realme_sm8750): bản vá zstd, CVE, netfilter, toolchain & ccache
- [Numbersf/Action-Build](https://github.com/Numbersf/Action-Build): bản vá Unicode bypass
- [showdo/Public_Ccache](https://github.com/showdo/Public_Ccache): public ccache
- [Android Common Kernel](https://android.googlesource.com/kernel/common): mã nguồn kernel

<div align="center">

---

Nếu thấy hữu ích, hãy tặng repo một ⭐ nhé!

</div>
