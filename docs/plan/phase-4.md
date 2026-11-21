# Kế hoạch Phase 4 — Ticket-04 "Phát hành v1.0.0" (2:30–3:00, 30 phút)

Yêu cầu: [`docs/prd/phase-4.md`](../prd/phase-4.md) · Test Contract **v1.3** (gameplay giữ v1.2) · **hạn 3:00: tag `phase-4` + GitHub Release `v1.0.0` cùng trỏ một commit trên `main`**. Chia theo vai trong [`TEAM.md`](../../TEAM.md) và 4 vai Ticket-04 gợi ý (Đóng gói, Bench/QA, Release/Integrator, Điều phối retro).

## 0. Hiện trạng `main` (`0d89857`)

| Có                                                                                                                                                                              | Chưa có                                                                                                               |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| Contract v1.2 đã bật, 233 test OK, QA Phase 3 28/28 (PR #21)                                                                                                                    | **tag `phase-3`**                                                                                                     |
| `extensions: ["burn"]`, `--mode day`, Shop, pause                                                                                                                               | `bench_cmd`, `soak_cmd`, `contract_version: "1.3"`                                                                    |
| README có lệnh chạy gather/quan, test, headless                                                                                                                                 | **LICENSE**; README chưa có bench/soak, yêu cầu môi trường, chế độ tương phản                                         |
| HUD độ tươi (Tiệp, P3)                                                                                                                                                          | 3 cảnh báo + chế độ tương phản cao; **`live.py` chưa có `--mode day`** (demo phải chơi trọn một Ngày)                 |
| CHANGELOG mục Phase 3                                                                                                                                                           | CHANGELOG theo từng phase P1–P4, release notes, `docs/verification/phase-4.md`, retro, `docs/members/*.md` đủ 5 người |
| **Đang làm:** tiepnnk01-10 tạo asset bằng code — hình SVG (Út Sen, cá, cây, Cá Sấu, bến, nhà bè, bếp, bàn, món, icon HUD) và SFX Web Audio + **phụ đề SFX** (mở rộng Ticket-04) |                                                                                                                       |

## 1. Tiêu chí chấp nhận (AC-P4-xx) — nguồn cho `docs/verification/phase-4.md`

| Mã       | Tiêu chí (Ticket-04)                                                                                                                                                                                                                                                                       | Người                                                     | Kiểm bằng                              |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------- | -------------------------------------- |
| AC-P4-01 | `contract_version: "1.3"`, `bench_cmd`, `soak_cmd` ở gốc manifest; gameplay/tuning/kho/JSONL giữ v1.2                                                                                                                                                                                      | s0ra1n0                                                   | test contract + regression             |
| AC-P4-02 | `bench_cmd --seed 42 --duration 30 --out bench.json` chạy cảnh sáng chuẩn (độ tươi, cá bơi, Cá Sấu, Út Sen đứng ở bến), 1280×720, làm nóng 3 s, đo 30 s; `bench.json` đúng 5 trường; artifact trong `docs/proof/phase-4/`                                                                  | nguyenhung1901                                            | chạy lệnh + kiểm khóa/công thức        |
| AC-P4-03 | `soak_cmd --seed 7 --days 10 --out soak.json`: 10 Ngày một tiến trình, D1–D5 lặp ở Ngày 6–10, tiền/earned/uy tín/cấp/id xuyên suốt; `days_completed = days`, `crashes = 0`; `results[]` = `{day, earned, reputation, score}` lúc Shop đóng; chạy lại cùng seed giống hệt (bỏ `duration_s`) | mhoo40                                                    | chạy lệnh 2 lần, so                    |
| AC-P4-04 | 3 cảnh báo có chữ hoặc biểu tượng: **thủy quái gần**, **oxy sắp hết**, **khách sắp giận**                                                                                                                                                                                                  | tiepnnk01-10                                              | chơi thử + ảnh chụp                    |
| AC-P4-05 | **Chế độ tương phản cao** bật/tắt được; chơi thử cả 3 cảnh báo ở chế độ này                                                                                                                                                                                                                | tiepnnk01-10                                              | ảnh chụp 3 cảnh báo ở tương phản cao   |
| AC-P4-06 | Clone sạch → bản có giao diện chạy bằng ≤ 3 lệnh (macOS/Linux); README ghi yêu cầu môi trường, điều khiển, lệnh test, headless, bench                                                                                                                                                      | mhoo40                                                    | thử clone sạch, ghi lại lệnh + kết quả |
| AC-P4-07 | GitHub Release `v1.0.0` kèm bản đóng gói chạy được + release notes (điểm mới, cách chạy, giới hạn đã biết); `CHANGELOG.md` theo từng phase; rà `CREDITS.md`, `LICENSE`                                                                                                                     | s0ra1n0                                                   | trang Release + file                   |
| AC-P4-08 | Regression P1–P3 còn hiệu lực đạt; `docs/verification/phase-4.md` + bằng chứng khớp commit gắn tag                                                                                                                                                                                         | trongnghia2304                                            | `verify.py --phase 4`                  |
| AC-P4-09 | Retro **mở đầu bằng rà hành động P2**, điều tốt / cần sửa / hành động tiếp, **một lỗi AI và cách xác minh** (`docs/retro.md`); `docs/members/<handle>.md` đủ 5 người (≤ 200 từ, dẫn commit)                                                                                                | trongnghia2304 điều phối, **mỗi người tự viết phần mình** | file trong repo                        |
| AC-P4-10 | Mở rộng **phụ đề SFX**: phụ đề hiện đúng lúc cho mọi SFX đã có, chơi thử khi tắt âm                                                                                                                                                                                                        | tiepnnk01-10                                              | ảnh chụp/ghi hình                      |
| AC-P4-11 | Demo: chơi **một Ngày** trên giao diện từ Lặn & Hái → Mở Quán → đóng Shop (`live.py --mode day`)                                                                                                                                                                                           | trongnghia2304 (server + vẽ Shop/màn chuyển pha)          | chơi thử trước 3:00                    |

## 2. Việc từng người (đánh dấu `[x]`, ghi commit/PR)

### s0ra1n0 — Integrator → **Release/Integrator**

- [x] **Ngay:** gắn tag `phase-3` (trên `dcefdfe` = `0d89857` + docs, code giống hệt) (đã trễ 2:30, ghi trung thực trong `docs/verification/phase-3.md`)
- [x] (PR #25) `game.contract.json`: `contract_version: "1.3", `bench_cmd: "python3 -m chonoi.bench"`, `soak_cmd: "python3 -m chonoi.soak"`; `contract.py` nhận `"1.3"` (cùng luật v1.2, cần `freshness`)
- [x] (PR #25) Thêm `LICENSE` (MIT cho code nhóm); rà `CREDITS.md` (asset tự tạo bằng code — ghi rõ, không dùng tài sản ngoài)
- [x] `CHANGELOG.md` sắp lại theo **từng phase** (P1 → P4); viết release notes `docs/release-notes-v1.0.0.md`: điểm mới, cách chạy, **giới hạn đã biết** (7 module test còn ghim v1.1, bench cần trình duyệt…)
- [ ] Bản đóng gói: `git archive --format=zip -o choNoi-v1.0.0.zip HEAD`; tạo Release `v1.0.0` + tag `phase-4` **trên cùng commit** trước 3:00
- AC: P4-01, P4-07

### mhoo40 — lát 2 → **soak + Đóng gói**

- [x] `chonoi/soak.py` (`python3 -m chonoi.soak --seed 7 --days 10 --out soak.json`): một `DaySim` chạy N Ngày bằng **bot tất định** (ví dụ: cập bến ngay → quán không thao tác → `shop_done`); kế hoạch Ngày 6–10 dùng lại D1–D5 (`((day-1) % 5) + 1`, báo Tiệp vì đụng `day.py`); bắt lỗi mỗi Ngày → `crashes`; ghi `{days, days_completed, crashes, results, duration_s}`; **không ghi xếp hạng**
- [x] Test: chạy 2 lần cùng seed, `results` giống hệt; `days_completed == 10`, `crashes == 0`
- [x] **Đóng gói:** thử clone sạch vào thư mục mới, chạy đúng các lệnh trong README (≤ 3 lệnh), ghi lại lệnh + kết quả vào `docs/proof/phase-4/clone-sach.txt`; sửa README mục "chạy game" (yêu cầu: Python 3.10+, chỉ thư viện chuẩn; điều khiển; test; headless; bench; soak)
- AC: P4-03, P4-06

### trongnghia2304 — lát 3, PO → **Bench/QA (ma trận) + Điều phối retro + `live --mode day`**

- [ ] **PO:** chép Ticket-04 vào `docs/prd/phase-4.md` (đã làm); ghi chỗ mơ hồ vào `docs/clarify.md` (bench chạy ở đâu nếu máy chấm không có màn hình; soak có bắt buộc bot trong repo)
- [x] `live.py --mode day`: một `DaySim` cho cửa sổ chơi (sáng → tối → Shop), nhận nút `pause`, `shop_done`, `buy`; snapshot có `day`, `phase` để giao diện đổi cảnh
- [x] **Vẽ Shop + màn chuyển pha** (mức đủ demo, không cần đồ họa đẹp), viết trong **file riêng `chonoi/web/day.js`** nạp từ `index.html` bằng một dòng `<script>` (báo Tiệp, không sửa phần khác của `index.html`):
  - Shop: tiền hiện có, cấp đã mua / đang áp dụng, 2 nút mua `oxygen` / `hp` (giá cấp kế, mờ khi không đủ tiền hoặc tối đa), nút "Đóng Shop" (`shop_done`), đồng hồ Shop, thông báo `bought` / `buy_failed`
  - Màn chuyển pha theo event: `day_start` + `day_plan` → "Ngày N" + món Má Tư định nấu và nguyên liệu cần; `phase_start(quan)` → kho mang về; `quan_end` → khách đã phục vụ / bỏ đi, tiền, boa, uy tín; hết `--days` → tổng kết
  - Chơi thử trọn một Ngày (cập bến → mở quán → đóng Shop) trước phút 25, chụp ảnh vào `docs/proof/phase-4/`
- [ ] `docs/verification/phase-4.md` theo AC-P4-01…11 — làm ở **phút 25–28** khi các PR đã merge; chạy `verify.py --phase 4` trên commit sẽ gắn tag
- [ ] **Retro** (`docs/retro.md`): mở đầu rà hành động đã chốt ở retro P2; điều tốt / cần sửa / hành động tiếp; **một lỗi AI + cách nhóm xác minh** (ví dụ: gộp tự động `gather.py` sinh hao giỏ hai lần, phát hiện bằng grep sau gộp; test xanh nhưng cá bơi chéo nhanh √2, phát hiện bằng đo thật)
- [ ] Nhắc cả nhóm viết `docs/members/<handle>.md` (≤ 200 từ) trước phút 25; viết phần của mình
- AC: P4-08, P4-09, P4-11

### nguyenhung1901 — lát 4 → **bench_cmd**

- [ ] `chonoi/bench.py` (`python3 -m chonoi.bench --seed 42 --duration 30 --out bench.json`):
  - dựng `LiveServer` cảnh sáng chuẩn (seed, cá bơi, Cá Sấu mặc định, Út Sen đứng ở bến, độ tươi), mở trình duyệt (`webbrowser`) tới `/?bench=1&duration=30`, cửa sổ 1280×720;
  - trang (khối `// --- bench (Hùng)` riêng trong `chonoi/web/index.html`, báo Tiệp) làm nóng 3 s rồi ghi thời gian mỗi khung bằng `requestAnimationFrame` trong 30 s, `POST /bench`;
  - server tính `frames`, `measured_s`, `avg_fps = frames / measured_s`, `p1_low_fps` = nghịch đảo thời gian trung bình của `max(1, ceil(0.01 × frames))` khung chậm nhất (giây), `load_ms` = từ lúc khởi chạy lệnh tới khung đầu của cảnh đo; ghi `bench.json` rồi thoát mã 0
  - không mở được trình duyệt / quá thời gian → thoát mã 1, **không bịa số**
- [ ] Lưu `bench.json` chạy thật vào `docs/proof/phase-4/`; ghi mốc avg ≥ 60, 1% low ≥ 30, load ≤ 10 s (chỉ báo cáo)
- [ ] Hỗ trợ Tiệp cảnh báo **khách sắp giận** ở HUD quán (kiên nhẫn < 25%)
- AC: P4-02

### tiepnnk01-10 — QA lead, UI → **asset + cảnh báo + tương phản + phụ đề**

- [ ] asset SVG + SFX Web Audio (tắt ở headless, không đổi tick/event/snapshot) + **phụ đề SFX**; ghi mục "Sáng tạo của nhóm" trong README + `CREDITS.md` (tự tạo bằng code)
- [ ] 3 cảnh báo chữ + biểu tượng: "Thủy quái!" (Cá Sấu gần), "Hết hơi!" / oxy < 25%, khách kiên nhẫn < 25%
- [ ] Nút / phím bật **tương phản cao**; chụp 3 cảnh báo ở chế độ này → `docs/proof/phase-4/`
- [ ] Giữ `chonoi/web/index.html`: Hùng chỉ thêm một khối bench có chú thích; trongnghia2304 chỉ thêm một dòng `<script src="day.js">`; người khác không sửa file này
- [ ] Nếu kịp: áp asset/tương phản cao lên màn Shop của `day.js` (đọc chung biến CSS)
- AC: P4-04, P4-05, P4-10

## 3. Mốc thời gian (30 phút)

| Phút  | Việc                                                                                                                                                                                                                    |
| ----- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0–2   | s0ra1n0 gắn `phase-3`. Mọi người đọc mục 2                                                                                                                                                                              |
| 2–15  | Song song: contract 1.3 + LICENSE (s0ra1n0), soak (mhoo40), bench (Hùng), asset/cảnh báo/tương phản (Tiệp), `live --mode day` + vẽ Shop/màn chuyển pha trong `day.js` (trongnghia2304). Merge từng PR nhỏ khi test xanh |
| 15    | Clone sạch thử (mhoo40) trên bản tích hợp                                                                                                                                                                               |
| 15–25 | Retro + `docs/members/*.md` (mỗi người), standup phút 20; bench và soak chạy thật, lưu artifact                                                                                                                         |
| 25–28 | `verify.py --phase 4`, ma trận, CHANGELOG, release notes                                                                                                                                                                |
| 28–30 | s0ra1n0: tag `phase-4` + Release `v1.0.0` (kèm zip) trên **cùng commit** `main`                                                                                                                                         |

## 4. Ưu tiên khi thiếu giờ

1. Tag `phase-3`; contract 1.3 + `bench_cmd` / `soak_cmd` **chạy được và ra đúng JSON** (điểm tính theo chạy được, không theo FPS).
2. README chạy từ clone sạch ≤ 3 lệnh, LICENSE, CHANGELOG theo phase, Release `v1.0.0` + tag `phase-4` cùng commit.
3. `docs/members/*.md` đủ 5 người, retro rà hành động P2, ma trận Phase 4.
4. 3 cảnh báo + tương phản cao.
5. `live --mode day` cho demo; phụ đề SFX.

## 5. Rủi ro

- **Bench cần trình duyệt có màn hình** — máy chấm có thể không có; ghi vào clarify + giới hạn đã biết, không giả số liệu.
- **`index.html` bị nhiều người sửa** (asset, cảnh báo, bench, Shop) → Tiệp giữ file; Hùng chỉ thêm khối bench có chú thích; Shop/màn chuyển pha nằm ở `chonoi/web/day.js` riêng, chỉ thêm một dòng nạp; merge sớm, nhỏ.
- **trongnghia2304 ôm nhiều việc** (server day, vẽ Shop, ma trận, retro) → ma trận dời về phút 25–28; vẽ Shop ở mức đủ demo; phút 22 chưa xong vẽ Shop thì demo bằng Shop dạng chữ + nút tối thiểu.
- **Tag và Release lệch commit** → s0ra1n0 tạo Release từ đúng tag `phase-4` (chọn tag khi tạo Release).
- **Merge sát giờ làm `main` đỏ** → sau phút 25 chỉ merge tài liệu; mọi PR code phải chạy toàn bộ test trước.
