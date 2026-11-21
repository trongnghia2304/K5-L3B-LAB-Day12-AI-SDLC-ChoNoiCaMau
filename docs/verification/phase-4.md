# Kiểm chứng Phase 4 — Ticket-04 "Phát hành v1.0.0" (Test Contract v1.3)

> Integrator (s0ra1n0) lập ma trận này thay chủ AC-P4-08 vì sát hạn nộp; mỗi dòng dẫn bằng chứng có thật trong repo. Kết quả chạy tự động: [`docs/proof/phase-4/summary.md`](../proof/phase-4/summary.md) (`python .agents/skills/verify-phase/verify.py --phase 4`).

| AC | Nội dung | Chủ | Bằng chứng | Kết quả |
|---|---|---|---|---|
| AC-P4-01 | `contract_version 1.3`, `bench_cmd`, `soak_cmd` ở gốc; gameplay/kho/JSONL giữ v1.2 | s0ra1n0 | `game.contract.json`, `chonoi/contract.py`, `tests/test_contract_v13.py` (4 test) | Đạt |
| AC-P4-02 | Bench cảnh sáng chuẩn 1280×720, nóng 3 s, đo 30 s, `bench.json` 5 trường | nguyenhung1901 | [`bench.json`](../proof/phase-4/bench.json): avg 238.73, 1% low 146.79, 7162 khung / 30.00 s, load 684 ms (đo lại sau cảnh nền); `tests/test_bench_p4.py`; [`lat4-bench.md`](../proof/phase-4/lat4-bench.md) | Đạt |
| AC-P4-03 | Soak 10 Ngày, `days_completed = days`, `crashes = 0`, chạy lại giống hệt | mhoo40 | [`soak.json`](../proof/phase-4/soak.json): 10/10, 0 crash; chạy 2 lần cùng seed 7 giống hệt khi bỏ `duration_s`; `tests/test_soak_p4.py` | Đạt |
| AC-P4-04 | 3 cảnh báo chữ + biểu tượng | tiepnnk01-10 | [`canh-bao.md`](../proof/phase-4/canh-bao.md), ảnh `canh-bao-sang-*.png`, `canh-bao-quan-*.png`; `tests/test_warnings.py` (9 test, cần Node) | Đạt |
| AC-P4-05 | Tương phản cao bật/tắt, 3 cảnh báo ở chế độ này | tiepnnk01-10 | ảnh `canh-bao-*-tuong-phan-cao.png`, `live-day-12-tuong-phan-cao.jpg`, `scene-3-song-tuong-phan-cao.jpg` (phím H) | Đạt |
| AC-P4-06 | Clone sạch → giao diện ≤ 3 lệnh; README đủ mục | mhoo40 | [`clone-sach.txt`](../proof/phase-4/clone-sach.txt): clone mới từ GitHub, 272 test OK, headless day + soak chạy, `live --mode day` trả trang; README mục cách chạy | Đạt (đo trên Windows; chưa thử macOS/Linux) |
| AC-P4-07 | Release `v1.0.0` + gói + release notes; CHANGELOG theo phase; CREDITS, LICENSE | s0ra1n0 | [`release-notes-v1.0.0.md`](../release-notes-v1.0.0.md), [`CHANGELOG.md`](../../CHANGELOG.md), `LICENSE`, `CREDITS.md`; trang Release tạo cùng commit với tag `phase-4` | Đạt khi tạo Release |
| AC-P4-08 | Regression P1–P3 đạt; ma trận + bằng chứng khớp commit tag | trongnghia2304 | 272 test OK (cả `QA_STRICT=1`); `verify.py --phase 4` → [`summary.md`](../proof/phase-4/summary.md) | Đạt |
| AC-P4-09 | Retro (rà hành động P2, một lỗi AI + xác minh); phản tư đủ 5 người | trongnghia2304 | [`retro.md`](../retro.md) mục Phase 4; `docs/members/`: s0ra1n0, trongnghia2304, nguyenhung1901, tiepnnk01-10, mhoo40 (`nguyenlongvu-mhoo40.md`) | Đạt (file mhoo40 đặt tên khác mẫu `<handle>.md`) |
| AC-P4-10 | Phụ đề SFX đúng lúc, chơi khi tắt âm | tiepnnk01-10 | [`phu-de-sfx-tat-tieng.png`](../proof/phase-4/phu-de-sfx-tat-tieng.png); `tests/test_assets.py` (tên SFX khớp sự kiện sim) | Đạt |
| AC-P4-11 | Chơi một Ngày trên giao diện sáng → tối → Shop | trongnghia2304 | [`live-day.txt`](../proof/phase-4/live-day.txt), ảnh `live-day-*.jpg`; `tests/test_live_day.py` | Đạt |

## Ghi chú
- Bench phụ thuộc vsync/tần số màn: cùng máy, lần đo trước cảnh nền được 59.98 avg, lần này 238.73; chưa kiểm nguyên nhân.
- 7 module test cũ vẫn chạy trên manifest v1.1 tạm (`tests/legacy_v11.py`); xem giới hạn trong release notes.
