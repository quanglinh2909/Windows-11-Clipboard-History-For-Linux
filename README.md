# Windows 11 Clipboard History For Linux

Clipboard manager cho Linux với giao diện kiểu Windows 11 (Tauri + Rust + React).

## Cài đặt nhanh (package)

```bash
curl -fsSL https://raw.githubusercontent.com/gustavosett/Windows-11-Clipboard-History-For-Linux/master/scripts/install.sh | bash
```

> Cách này phù hợp khi bạn dùng bản phát hành chính thức.

---

## Cài từ source (khuyến nghị khi vừa sửa code)

Nếu bạn vừa sửa source (đặc biệt các fix hotkey/Win+V), hãy build và cài local để đảm bảo app chạy đúng theo code mới nhất trên máy bạn.

### 1) Cài dependencies

```bash
make deps
make rust
make node
source ~/.cargo/env
```

### 2) Build release local

```bash
cd src-tauri
cargo build --release -q
```

### 3) Cài binary local vào hệ thống

```bash
sudo install -Dm755 \
	src-tauri/target/release/win11-clipboard-history-bin \
	/usr/lib/win11-clipboard-history/win11-clipboard-history-bin
```

### 4) Khởi động lại app để áp dụng bản mới

```bash
pkill -f win11-clipboard-history-bin 2>/dev/null || true
nohup /usr/bin/win11-clipboard-history >/tmp/win11.log 2>&1 < /dev/null &
```

---

## Quy trình cập nhật sau mỗi lần sửa source

Mỗi khi sửa Rust backend (ví dụ `src-tauri/src/main.rs`):

```bash
cd src-tauri
cargo build --release -q
sudo install -Dm755 target/release/win11-clipboard-history-bin /usr/lib/win11-clipboard-history/win11-clipboard-history-bin
pkill -f win11-clipboard-history-bin 2>/dev/null || true
nohup /usr/bin/win11-clipboard-history >/tmp/win11.log 2>&1 < /dev/null &
```

---

## Lưu ý cho Ubuntu/Unity (X11)

- Nếu `Win+V` hoạt động không ổn định sau khi sửa code, hãy đảm bảo bạn đã cài lại **binary local** theo các bước trên.
- Kiểm tra nhanh app đang chạy từ binary local:

```bash
pgrep -af win11-clipboard-history-bin
```

Bạn nên thấy đường dẫn:

`/usr/lib/win11-clipboard-history/win11-clipboard-history-bin`

---

## Chạy dev mode

```bash
make dev
```

> `make dev` dùng binary build tại máy hiện tại, nên thường tránh được lỗi tương thích GLIBC của binary prebuilt.

