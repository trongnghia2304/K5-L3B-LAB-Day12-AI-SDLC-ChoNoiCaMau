# Changelog

Ghi theo từng phase, phase mới nhất ở trên. Mỗi phase ứng với một tag (`phase-1` … `phase-4`) và một phiên bản Test Contract.
Bản phát hành `v1.0.0` = tag `phase-4`. Release notes: [`docs/release-notes-v1.0.0.md`](docs/release-notes-v1.0.0.md).

## Phase 4 — Ticket-04 "Phát hành v1.0.0" · Test Contract v1.3 · tag `phase-4`, Release `v1.0.0`

### Contract v1.3 (s0ra1n0)
- `game.contract.json`: `contract_version: "1.3"`, thêm ở gốc `bench_cmd: "python3 -m chonoi.bench"` và `soak_cmd: "python3 -m chonoi.soak"`.
- Gameplay, tuning, kho cá thể và JSONL **giữ nguyên v1.2**; `contract.py` coi 1.3 như 1.2 (cần `tuning.freshness`), kiểm hai lệnh là chuỗi khác rỗng, vẫn nhận 1.0–1.2. Test: `tests/test_contract_v13.py`.

### Benchmark có render — `bench_cmd` (nguyenhung1901, AC-P4-02)
- `chonoi/bench.py`: dựng `LiveServer` cảnh sáng chuẩn (seed 42, cá bơi, Cá Sấu, độ tươi, Út Sen đứng ở bến), mở trình duyệt `/?bench=1`, canvas 1280×720, làm nóng 3 s rồi đo `requestAnimationFrame`; ghi `bench.json` 5 trường (`avg_fps`, `p1_low_fps`, `frames`, `measured_s`, `load_ms`).
- Không mở được trình duyệt hoặc quá giờ → mã 1, không ghi số giả. Kết quả chạy thật: `docs/proof/phase-4/bench.json`. Test: `tests/test_bench_p4.py`.

### Soak headless — `soak_cmd` (mhoo40, AC-P4-03)
- `chonoi/soak.py`: một `DaySim` chạy N Ngày bằng bot tất định (cập bến ngay → quán mở tới hết giờ → đóng Shop ngay); kế hoạch D1–D5 lặp ở Ngày 6–10; tiền, uy tín, cấp, id cá thể xuyên suốt.
- `soak.json` = `{days, days_completed, crashes, results[{day, earned, reputation, score}], duration_s}`; không ghi xếp hạng. Kết quả: `docs/proof/phase-4/soak.json`. Test: `tests/test_soak_p4.py`.

### Asset tự tạo — đồ họa SVG + SFX có phụ đề (tiepnnk01-10, AC-P4-10)
- 21 SVG trong `chonoi/web/assets/` sinh bởi `tools/gen_assets.py`; HUD dùng sprite cho Út Sen, cá, Cá Sấu (trước đây HUD chưa vẽ thủy quái), cây nổi, bến, bếp, bàn, khách và món; tự về hình khối cũ nếu asset chưa tải.
- 24 SFX tổng hợp bằng Web Audio (`assets/sfx.js`) kèm phụ đề; phím M tắt/bật tiếng, V ẩn/hiện phụ đề.
- `chonoi/live.py`: thêm route tĩnh `GET /assets/<tên>` (chỉ `.svg`/`.js`, tên phẳng, chặn `..` và `/`); các route `/`, `/state`, `/input`, `/restart` không đổi. Đây là file khung giảng viên nên ghi lại lý do: cần để client nạp được asset.
- `tests/test_assets.py` (12 test). Không đổi luật, tick, sự kiện, JSONL hay `game.contract.json`.
- HUD quán chừa thêm chỗ phía trên cho 3 dòng kho (trước đó bị mép phòng che).

### Cảnh báo và chế độ tương phản cao (tiepnnk01-10, nguyenhung1901, AC-P4-04, AC-P4-05)
- 3 cảnh báo có **chữ + biểu tượng** (`warn_*`): "Thủy quái!" (Cá Sấu trong 1,5 × bán kính phát hiện, hoặc đang đuổi; thêm viền tím), "Oxy sắp hết!" / "Hết hơi!" (oxy < 25% / cạn, chỉ khi ở dưới nước), "Khách sắp giận!" (kiên nhẫn < 25%, kèm biểu tượng ngay cạnh khách).
- **Chế độ tương phản cao** (phím **H**, nhớ lựa chọn): chữ đậm có viền (chữ tối viền trắng, chữ sáng viền đen), viền khung dày hơn, cảnh báo nền đen, tăng tương phản và bão hòa cảnh.
- `tests/test_warnings.py` (9 test, chạy HUD thật trong Node với canvas giả; skip nếu không có Node) và bằng chứng ảnh ở `docs/proof/phase-4/`. Không đổi luật, tick, sự kiện hay JSONL.

### Chơi trọn một Ngày trên giao diện (trongnghia2304, AC-P4-11)
- `python3 -m chonoi.live --mode day [--days 1..5]`: sáng → tối → Shop trong cửa sổ chơi; màn chuyển pha, Shop mua nâng cấp, tạm dừng, tổng kết. Bằng chứng: `docs/proof/phase-4/live-day.txt` và ảnh `live-day-*.jpg`.

### Cảnh nền hai buổi và camera (trongnghia2304)
- `chonoi/web/scene.js` (canvas, vẽ bằng code): sáng là mặt cắt sông phù sa nhiều lớp, **camera zoom bám Út Sen** (thế giới rộng hơn khung nhìn, bản đồ nhỏ, thước độ sâu); tối là **nhà bè nhìn isometric 2.5D**, phím di chuyển xoay 45° cho khớp góc nhìn (luật không đổi).
- `index.html` chỉ thêm hook vẽ nền; thiếu `scene.js` thì vẽ như cũ. Nhãn gỡ lỗi ẩn mặc định, phím **I** để hiện; HUD quán xuống dòng theo từng món kho. Bằng chứng: `docs/proof/phase-4/scene.txt`, ảnh `scene-*.jpg`.

### Phát hành, tài liệu, test
- `LICENSE` (MIT cho mã của nhóm); `CREDITS.md` ghi asset tự tạo bằng code, không dùng asset ngoài.
- README: chạy từ clone sạch 3 lệnh, yêu cầu môi trường, điều khiển, test, headless, bench, soak.
- Retro Phase 4 (`docs/retro.md`) và phản tư cá nhân `docs/members/<handle>.md`.
- Nợ AC-P3-19 trả một phần sau tag `phase-3`: 5 module test (`test_smoke_p1`, `test_gather_lat1`, `test_gather_lat1_p2`, `test_lat0_v11`, `test_python_starter_units`) bỏ `legacy_v11`, chạy thẳng kho cá thể.
- Sửa `tests/test_assets.py` bị commit kèm dấu xung đột (`<<<<<<< Updated upstream`) làm `main` đỏ; giữ bản so theo byte của chủ file.

## Phase 3 — Ticket-03 "Mùa Nước Nổi" · Test Contract v1.2 · tag `phase-3`

### Hợp đồng v1.2
**BREAKING — kho đếm theo loại → kho cá thể.** `contract_version` đổi `"1.1"` → `"1.2"`, thêm `tuning.freshness`
`{initial: 100, gather_decay_per_s: 0.5, quan_decay_per_s: 0.25, spoiled_below: 30}`.

| Chỗ | Trước (v1.1) | Sau (v1.2) |
|---|---|---|
| File kho `--inventory` / `--inventory-out` | `{"items": {"ca_loc": 1}}` | `{"items": [{"id": "I1", "kind": "ca_loc", "fresh": 100}]}`; kho rỗng `{"items": []}`; file kho đếm cũ → mã 2 |
| `gather.hold`, `quan.stock` (scenario) | mảng kind / map kind → số | mảng cá thể `{id, kind, fresh}` |
| `customers[].reserved` | map kind → số | mảng id cá thể |
| Giỏ, kho quán trong snapshot | mảng kind / map | mảng cá thể |
| `cook_started.data.items`, `lost` (monster_hit, faint), `gather_end.data.inventory` | mảng kind / map | mảng cá thể |
| Giá món | giá niêm yết | `floor(giá niêm yết × TB fresh các cá thể dùng / 100)`, boa tính trên giá đã chốt |
| Giữ chỗ | theo lượng | theo id: cá thể tươi nhất, hòa id nhỏ; vừa ươn thì nhả hết và thử giữ lại một lần |
| Sự kiện mới | — | `spoiled {item_id, kind, location}` |

**Cách cập nhật fixture:** đổi `"hold": ["ca_loc"]` thành `"hold": [{"id": "I1", "kind": "ca_loc", "fresh": 100}]`; `"stock": {"ca_loc": 1}`
thành mảng cá thể; `"reserved": {"ca_loc": 1}` thành `"reserved": ["I1"]`; file kho cũ phải sinh lại bằng gather v1.2.
Test cũ bị thay bởi luật mới sẽ liệt kê (kèm lý do) ở `docs/verification/phase-3.md` mục AC-P3-19.

**Bật v1.2 (trongnghia2304):**
- `game.contract.json`: `contract_version: "1.2"`, thêm `tuning.freshness`. Bộ chấm và CI đọc thẳng file này, không cần `CHONOI_CONTRACT`.
- **Test cũ dùng fixture kho đếm** chạy trên manifest v1.1 tạm qua `tests/legacy_v11.py` (Ticket-03 mục 3: "hỗ trợ hai phiên bản nội bộ trong lúc chuyển"); code vẫn nhận v1.1. Lúc gắn tag `phase-3` ghim 12 module; **sau tag** s0ra1n0 đã chuyển 5 module của mình sang kho cá thể
  (`test_smoke_p1`, `test_gather_lat1`, `test_gather_lat1_p2`, `test_lat0_v11`, `test_python_starter_units`: fixture `mk_hold`,
  kỳ vọng `lost` / `gather_end.inventory` / file kho là mảng cá thể đã hao đúng số bước, file kho đếm cũ -> mã 2).
  Còn ghim (7): `test_gather_p1`, `test_lat2`, `test_p2_lat2_fish`, `test_quan_p1`, `test_quan_lat3`, `test_quan_lat4`, `test_quan_p2`.
  Lý do: luật các test này bảo vệ (bơi, oxy, Cá Sấu, cá bơi, khách, bếp) không đổi ở v1.2; chỉ dạng kho đổi.
  Chủ mỗi file chuyển fixture sang kho cá thể (có thể dùng `mk_hold` / `mk_stock` như `test_day_p2`) rồi bỏ dòng `from legacy_v11 import ...`.
- Test đổi vì luật mới: `test_lat0_v11.test_so_chuan_v11` kiểm `contract_version` `"1.1"` → `"1.2"` (số chuẩn giữ nguyên).
- File kho làm tròn `fresh` 3 chữ số như JSONL (`items.to_file`) để file **khớp** `gather_end.inventory` (PRD-0 mục G); vẫn đọc lại đúng đơn vị 1/240.
  `test_lat0_v12.test_smoke_ban_giao_file...`: đưa `assertEqual(end["inventory"], saved)` về **sau** khi gán `end`/`saved` (trước đó đứng trước, gây `UnboundLocalError`).
- Kiểm v1.2 trên manifest thật: `test_lat0_v12`, `test_quan_p3`, `test_day_p2`, `test_fresh_p3` (QA, không cần `CHONOI_CONTRACT`).
- `test_fresh_p3.PauseFresh` (AC-P3-10): input cũ THẢ nút pause để tiếp tục, trong khi Ticket-02 4.2 cần cạnh lên kế tiếp — đổi input thành hai lần nhấn (`press`), assert giữ nguyên. `test_lat0_v12` ngất: hàng mất đã hao 1 bước.
- Kết quả: 233 test OK, QA Phase 3 28/28 trên manifest v1.2 thật (`docs/proof/phase-3/enable-v12.txt`).

### v1.2 đã bật trên `main`
- Kho cá thể dùng chung `chonoi/items.py`, file kho / scenario / giỏ cá thể, giữ chỗ theo id, hao kho quán, giá theo tươi (lát 0, 3, 4). Code vẫn nhận manifest v1.1 (không có `tuning.freshness`).
- Test hộp đen Phase 3 `tests/test_fresh_p3.py` (28 test) và ma trận `docs/verification/phase-3.md`.
- HUD (`chonoi/web/index.html`) đọc được cả kho đếm (v1.1) lẫn kho cá thể (v1.2): thanh độ tươi, gạch chéo và cảnh báo hàng ươn, kho khả dụng bỏ phần giữ chỗ và hàng ươn.

### `day` bàn giao kho cá thể (lát 5)
- `day`: chuyển **nguyên mảng cá thể** (id, kind, fresh) từ gather sang quán trong cùng tick, không hao thêm.
- Ngày kế: id mẫu `gather.hold` được cấp **mới** theo thứ tự mảng (kind/fresh ban đầu); id tăng tiếp xuyên pha và Ngày (clarify #19).
- Shop: giữ kho cá thể theo id, hàng thô không hao tươi; kho dư bị bỏ khi Shop đóng.
- `tests/test_day_p2.py` sửa để đúng ở cả v1.1 và v1.2 (fixture qua `mk_hold` / `mk_stock` / `stock_counts` trong `tests/qa_helpers.py`).

### gather: hao giỏ và `spoiled` (lát 2)
- `GatherSim._tick_hold_freshness`: giỏ giảm 0.5/s ở đầu bước (sau hồi nút), hàng mới hao từ bước sau; `spoiled {item_id, kind, location: "hold"}` đúng một lần, theo id. Ví dụ ngưỡng `fresh: 30` của ticket phát `spoiled` ở tick 1.
- `test_lat0_v12`: hàng rơi khi bị cắn/ngất đã hao từ đầu bước (đổi vì luật mới).

## Phase 2 — Ticket-02 · Test Contract v1.1 · tag `phase-2`
- **Nền v1.1 (lát 0):** đàn cá đầu ca 8 con ở điểm sinh khác nhau, 3 loài cá (`ca_linh`, `ca_loc`, `ca_keo`) và cây `keo_neo`; `gather.monsters` (Cá Sấu Năm Chèo M1, thêm M2 từ Ngày 3); 2 bếp, 4 bàn, 6 món; `day_plan` D1–D5; Shop; nút `pause`, `buy`, `shop_done`, `attack`; scenario `day`, `fish[].age_ticks`, `monsters`.
- **Cá bơi và sinh theo lịch (lát 2):** cá bơi theo hướng, sinh lại theo stream `fish_spawn`; sửa tốc độ chéo nhanh √2 lần (phát hiện bằng đo thật).
- **Cá Sấu (lát 1):** tuần tra → đuổi khi trong bán kính phát hiện → cắn (mất 1 Máu, rơi nửa giỏ theo LIFO, bất tử tạm) → giật lùi; ngất do quái `cause: "monster"`. Test: `tests/test_gather_lat1_p2.py`.
- **Quán 2 bếp / 4 bàn, 6 món (lát 3, 4)**; khách chọn món theo kho khả dụng.
- **`--mode day` (lát 5):** 5 Ngày sáng → tối → Shop trong một tiến trình, kho chuyển trong bộ nhớ; Shop nâng cấp oxy / Máu; pause.
- **Mở rộng `burn` (I-1):** manifest `extensions: ["burn"]`, `burn_s: 20`; món để quá giờ thì cháy; `contract.py` chỉ nhận `burn_s` hữu hạn khi bật extension.
- Kết quả lúc gắn tag: 179 test OK, `verify-phase 2` đạt (`docs/proof/phase-2/`).

## Phase 1 — PRD-0 · Test Contract v1.0 · tag `phase-1`
- **Lặn & Hái (gather):** bơi có tăng tốc, chặn mép; lặn/nổi (`dive`, `surface`), oxy trừ mỗi tick dưới nước và hồi trên mặt nước; ngạt mất Máu mỗi 60 tick; hết Máu thì ngất, rơi hết giỏ (LIFO) và kết thúc buổi; hái cây nổi, bắt cá; cập bến ghi kho; hết giờ giữ giỏ.
- **Mở Quán (quan):** khách đến theo lịch, giữ chỗ nguyên liệu, kiên nhẫn; bếp nấu, phục vụ, thanh toán, boa, uy tín.
- **Bàn giao hai tiến trình:** `--inventory-out` (gather) → `--inventory` (quan), file kho `{"items": ...}`.
- **Headless theo Test Contract:** 60 tick/s, JSONL tất định từng byte, mã thoát 0/1/2, mọi số ngẫu nhiên qua `Streams`; chơi thử trên trình duyệt `python3 -m chonoi.live`.
- Kết quả lúc gắn tag: 58 test OK, `verify-phase 1` đạt (`docs/proof/phase-1/`).
