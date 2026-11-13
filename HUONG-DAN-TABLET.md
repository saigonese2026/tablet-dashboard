# Tablet Dashboard: biến Galaxy Tab A thành màn hình Lịch + Việc cần làm

Bản chuyển từ "Kindle Dashboard" sang máy tính bảng Android (Galaxy Tab A). Màn hình hiện:
- **lịch Google** hôm nay và ngày mai,
- **việc cần làm** (Google Tasks),
- **thời tiết + chất lượng không khí**,
- (tuỳ chọn) **mức dùng Claude**.

**Khác bản Kindle ở đâu:**
- **Không cần jailbreak**, không cần cáp USB. Tablet chỉ mở một trang web bằng Chrome.
- Ảnh vẽ sẵn cỡ **1200×1920** (màn 16:10 dựng đứng), trang tự làm mới 5 phút/lần, giữ màn hình luôn sáng.
- **Chế độ đêm**: từ 22h đến 6h trang tự đảo màu (nền đen, chữ xám tối) cho đỡ chói.
- Màn LCD tốn điện hơn e-ink, nên **để tablet cắm sạc thường xuyên** (xem Phần 3, bước 4 để bảo vệ pin).

```
Google Calendar + Tasks ──► Apps Script (của bạn) ──► GitHub Actions vẽ ảnh ──► GitHub Pages ──► Chrome trên tablet hiển thị
```

**Chi phí: 0 đồng.**

---

## Cần chuẩn bị

- **Galaxy Tab A** có Chrome (Android nào cũng có sẵn hoặc tải ở Play Store) và Wi-Fi.
- **Tài khoản Google** có lịch và việc cần làm muốn hiện lên.
- **Tài khoản GitHub** (miễn phí).
- **Máy tính** (Mac hoặc Windows) để chạy script cài đặt.
- Một **giá đỡ dựng đứng** cho tablet (bố cục là khổ dọc).
- Khoảng **20–30 phút**.

| File / thư mục | Dùng để |
|---|---|
| `apps_script.gs` | Code lấy lịch + việc cần làm (Phần 1) |
| `setup_github.sh` | Script tự tạo repo GitHub và cài mọi thứ (Phần 2) |
| `render.py`, `lunar.py`, `.github/` | Code vẽ ảnh, chạy trên GitHub |
| `tablet/` | Trang web hiển thị trên tablet (tự đưa lên GitHub Pages) |
| `mac/` | Chỉ dùng cho phần mức dùng Claude (Phần 5) |

---

## Phần 1: Lấy dữ liệu Google bằng Apps Script (~10 phút)

Giống hệt bản Kindle:

1. Mở **https://script.google.com** (đúng tài khoản có lịch) → **New project**, đặt tên `Tablet Dashboard`.
2. Mở **`apps_script.gs`** bằng TextEdit/Notepad, copy toàn bộ, xoá code mẫu trong Apps Script rồi dán vào.
3. Đổi dòng `const TOKEN = 'DOI-THANH-CHUOI-BI-MAT';` thành **chuỗi bí mật của bạn** (chữ và số, 10–20 ký tự). **Save**.
4. Cột trái **Services** → **+** → **Google Tasks API** → **Add**.
5. Chọn hàm **`testCollect`** → **Run** → cho phép quyền. Gặp *"Google hasn't verified this app"* thì bấm **Advanced → Go to … (unsafe) → Allow** (script của chính bạn).
6. **Deploy → New deployment** → bánh răng → **Web app**: *Execute as* **Me**, *Who has access* **Anyone** → **Deploy** → copy **Web app URL**.
7. **DATA_URL** = `<Web app URL>?token=<chuỗi bí mật>`. Dán thử vào trình duyệt, thấy JSON có tên sự kiện/việc là đúng. **Không gửi link này cho ai.**

---

## Phần 2: Tạo repo GitHub (~5 phút)

**Bước 1: cài công cụ** (1 lần)
- **macOS**: `brew install gh git` (chưa có Homebrew thì cài ở https://brew.sh).
- **Windows**: cài **Git for Windows** và **GitHub CLI** (https://cli.github.com), rồi dùng **Git Bash** để chạy lệnh.

**Bước 2: chạy script**
```bash
cd đường-dẫn-tới/tablet-dashboard
bash setup_github.sh
```
Script sẽ: mở trình duyệt đăng nhập GitHub → hỏi tên repo (Enter = `tablet-dashboard`) → hỏi toạ độ thành phố (TP.HCM: `10.7769`, `106.7009`; Hà Nội: Enter) → hỏi DATA_URL (dán vào sẽ không hiện chữ, cứ Enter) → tự làm phần còn lại.

Cuối cùng nó in ra **Link tablet**, dạng:
```
https://<tên-github>.github.io/tablet-dashboard/<chuỗi-ngẫu-nhiên>/
```
và lưu vào file `LINK-TABLET.txt`. Mở thử trên máy tính, thấy dashboard là thành công.

> **Làm tay trên web** (nếu không chạy được script): làm như Cách B của bản Kindle (tạo repo Public, upload toàn bộ thư mục kể cả `.github` và `tablet`, tạo 2 secret `DATA_URL` + `SECRET_PATH`, Settings → Pages → Source: GitHub Actions, rồi Actions → **Cập nhật dashboard tablet** → Run workflow). Link tablet là `https://<tên-github>.github.io/<tên-repo>/<SECRET_PATH>/`.

---

## Phần 3: Cài lên Galaxy Tab A (~5 phút)

1. **Gửi link sang tablet** một cách riêng tư: ví dụ đăng nhập cùng tài khoản Google trên Chrome máy tính và tablet rồi dùng "Gửi tới thiết bị của bạn", hoặc tự gõ tay. Đừng dán vào nhóm chat.
2. Trên tablet mở link bằng **Chrome** → menu **⋮** → **Thêm vào màn hình chính** (*Add to Home screen*) → **Cài đặt** (*Install*).
   Trên màn hình chính sẽ có biểu tượng **Lịch & Todo**. Mở từ đó thì trang chạy toàn màn hình, không có thanh địa chỉ.
   (Nếu mở trong Chrome thường: **chạm 2 lần** vào màn hình để bật/tắt toàn màn hình.)
3. **Giữ màn hình luôn sáng**: trang đã tự xin giữ sáng khi đang mở. Để chắc chắn hơn:
   - **Cài đặt → Màn hình → Thời gian chờ màn hình** → chọn mức dài nhất.
   - Tắt **Chế độ giờ ngủ / Bedtime mode** và **Màn hình luôn bật (AOD)** nếu không cần.
   - **Cài đặt → Chăm sóc thiết bị → Pin** → thêm Chrome vào **Ứng dụng không bao giờ ngủ** để Chrome không bị tắt ngầm.
4. **Bảo vệ pin khi cắm sạc suốt ngày**: **Cài đặt → Chăm sóc thiết bị → Pin → Cài đặt pin khác → Bảo vệ pin** → bật (giới hạn sạc 80–85%). Rất nên bật, pin sẽ không bị phồng.
5. **Khoá màn hình vào ứng dụng** (tuỳ chọn, chống ai đó vô tình thoát): **Cài đặt → Bảo mật → Cài đặt bảo mật khác → Ghim cửa sổ** → bật. Mở Lịch & Todo → nút đa nhiệm → bấm vào biểu tượng ứng dụng → **Ghim ứng dụng này**. Bỏ ghim: giữ nút Quay lại + Đa nhiệm.

Xong. Trang tự tải ảnh mới 5 phút/lần (GitHub vẽ ảnh mới khoảng 15 phút/lần từ 8h–24h, 4 tiếng/lần ban đêm). Mất mạng thì góc dưới hiện thông báo đỏ, ảnh cũ vẫn giữ nguyên.

---

## Phần 3b: Máy Android cũ (Android 5–7, ví dụ Galaxy Tab A 7.0 SM-T285)

Android dưới 7.1 **không nhận chứng chỉ của GitHub Pages** (Let's Encrypt), Chrome báo *"Kết nối của bạn không phải là kết nối riêng tư"*. Firefox cũng không cài được trên Android 5. Cách xử lý: để Apps Script tải ảnh từ GitHub rồi gửi sang tablet qua tên miền Google (máy cũ vẫn nhận).

1. Trong Apps Script, sửa dòng `const BASE = '';` thành link thư mục ảnh của bạn, có `/` ở cuối:
   `const BASE = 'https://<tên-github>.github.io/<tên-repo>/<SECRET_PATH>/';`
2. Save → chạy thử một hàm bất kỳ → cấp thêm quyền *"kết nối với dịch vụ bên ngoài"*.
3. **Deploy → Manage deployments → ✏️ → Version: New version → Deploy** (link giữ nguyên).
4. Trên tablet mở bằng Chrome: `<DATA_URL>&view=1` (tức `.../exec?token=<TOKEN>&view=1`) → menu ⋮ → **Thêm vào Màn hình chính**.
5. Bấm ✕ để tắt dòng *"Ứng dụng này do một người dùng Google Apps Script tạo"* (Google luôn hiện dòng này, không bỏ hẳn được).
6. Chrome đời cũ không tự giữ màn hình sáng được: **Cài đặt → Cài đặt cho người phát triển → bật "Bật màn hình"** (màn hình không tắt khi đang sạc). Mục này chỉ có sau khi bật chế độ nhà phát triển (chạm 7 lần vào *Số hiệu bản tạo*).

Link này chứa TOKEN, giữ kín như DATA_URL.

### Khung Claude trực tiếp trên tablet (máy Android cũ, Phần 3b)

Trang `...&view=1` có khung **CLAUDE** riêng ở dưới màn hình: phiên 5 giờ + tuần này, số %, thanh tiến độ, ▲ thời gian đã trôi, giờ đặt lại và đếm ngược. Khung này lấy số liệu thẳng từ Apps Script mỗi 5 phút (không chờ GitHub vẽ lại ảnh).

- Mac gửi số liệu 5 phút/lần (`mac/install.sh` đặt `StartInterval` = 300). Cài: chép thư mục `mac/` vào `~/tablet-dashboard-mac`, lưu DATA_URL vào `~/.kindle-dashboard-url`, chạy `bash ~/tablet-dashboard-mac/install.sh`.
- Apps Script cần hàm `dashClaude` (có sẵn trong `apps_script.gs`), nhớ Deploy bản mới.
- Workflow đặt `DASH_CLAUDE_IMG: "0"` để ảnh không vẽ Claude trùng, và `DASH_H: "890"` để chừa chỗ cho khung.
- Mac ngủ / tắt quá 20 phút thì khung hiện **SỐ LIỆU CŨ**.

---

## Phần 4: Tuỳ chỉnh

**Trên tablet** (thêm vào cuối link, rồi cài lại biểu tượng ở Phần 3 bước 2):

| Muốn | Thêm vào link |
|---|---|
| Làm mới 10 phút/lần | `?phut=10` |
| Đổi giờ chế độ đêm (ví dụ 23h–7h) | `?dem=23-7` |
| Tắt chế độ đêm | `?dem=0` |
| Kết hợp | `?phut=10&dem=23-7` |

**Trên GitHub**, file `.github/workflows/update.yml` (mở → ✏️ → sửa → Commit):

| Muốn | Sửa gì |
|---|---|
| Cập nhật ngay | Tab **Actions** → Cập nhật dashboard tablet → **Run workflow** |
| Đổi thành phố thời tiết | `DASH_LAT`, `DASH_LON` |
| Chữ to/nhỏ hơn | `DASH_SCALE` (mặc định `"1.3"`; `"1.15"` thì chữ nhỏ hơn, hiện được nhiều việc hơn) |
| Ảnh khớp đúng màn hình | `DASH_W`, `DASH_H`. Mặc định 1200×1920 hợp với Tab A 16:10. Thấy viền trắng 2 bên/trên dưới thì xem độ phân giải máy ở trang thông số và đặt theo khổ dọc (ví dụ máy 1340×800 → `DASH_W: "800"`, `DASH_H: "1340"`, kèm `DASH_SCALE: "0.9"`). |
| Ẩn bớt lịch | Settings → Secrets and variables → Actions → **Variables** → `DASH_SKIP_CALENDARS` = tên các lịch, cách nhau dấu phẩy |

---

## Phần 5 (tuỳ chọn): Hiện mức dùng Claude

Làm y như Phần 5 của bản Kindle (cần gói Claude Pro/Max và một chiếc Mac luôn bật). Chỗ khác duy nhất: link tải file `mac/` dùng tên repo của bạn (mặc định `tablet-dashboard`). Đặt biến `DASH_LAYOUT` = `claude` để chuyển sang bố cục Claude.

---

## Xử lý sự cố

| Hiện tượng | Cách sửa |
|---|---|
| Mở link báo 404 | Workflow chưa chạy xong, chưa bật **Settings → Pages → Source: GitHub Actions**, hoặc link thiếu dấu `/` cuối / sai chuỗi bí mật. |
| Màn hình tự tắt sau một lúc | Làm lại Phần 3 bước 3. Nếu vẫn tắt: mở trang, chạm vào màn hình 1 lần (trình duyệt cần một lần chạm để giữ sáng). |
| Ảnh không đổi | GitHub chạy trễ 10–30 phút là bình thường. Run workflow tay rồi chờ tối đa 5 phút. |
| Góc dưới báo "Không tải được ảnh mới" | Tablet mất Wi-Fi. Ảnh cũ vẫn hiện, có mạng lại thì tự hết. |
| Actions báo lỗi "kiểm tra token" | Secret `DATA_URL` sai token. Dán DATA_URL vào trình duyệt: ra `{"error":"forbidden"}` là sai. |
| Actions ❌ ở bước "Tải font Inter" hoặc deploy | Lỗi mạng/GitHub thoáng qua, bấm **Re-run jobs**. |

---

## Bảo mật: 2 điều nhớ

1. **Không gửi DATA_URL, TOKEN hay Link tablet cho ai.** Ai có Link tablet là xem được lịch của bạn. Lỡ lộ link: tạo lại secret `SECRET_PATH` trên GitHub (chuỗi mới), Run workflow, rồi cài lại biểu tượng trên tablet bằng link mới. Lỡ lộ DATA_URL: đổi TOKEN trong Apps Script (Deploy → New version) rồi sửa secret `DATA_URL`.
2. Repo là **công khai**, nên **không dán token thật vào `apps_script.gs` trên GitHub**.

---

*Dựa trên bộ "Kindle Dashboard" (phần tắt giao diện Kindle dựa theo usetrmnl/trmnl-kindle, MIT). Thời tiết: Open-Meteo. Font Inter (SIL OFL), tự tải khi chạy.*
