# Quán Ăn Chợ Nổi v1.0.0 — release notes

> Integrator (s0ra1n0) viết; số liệu đo trên `main` sau khi mọi PR Phase 4 đã merge. Chỉ ghi điều đã chạy thật.

Game 2 buổi của Út Sen trên chợ nổi Cà Mau: sáng **Lặn & Hái** (oxy, Máu, cá bơi, Cá Sấu Năm Chèo), tối **Mở Quán** (khách, bếp, giữ chỗ, uy tín), giữa các Ngày là **Shop** nâng cấp. Test Contract **v1.3** (gameplay, tuning, kho và JSONL theo v1.2).

## Điểm mới theo phase
| Phase | Contract | Điểm chính |
|---|---|---|
| 1 | v1.0 | Lặn, oxy/ngạt/Máu/ngất, hái và bắt cá, cập bến ghi kho; quán 1 bếp, khách, phục vụ, tiền, uy tín; headless tất định, JSONL |
| 2 | v1.1 | Cá bơi/sinh theo lịch, Cá Sấu (tuần tra, đuổi, cắn, giật lùi), 2 bếp/4 bàn, 6 món, `--mode day` 5 Ngày, Shop nâng cấp oxy/Máu, pause; mở rộng **burn** (món cháy) |
| 3 | v1.2 | Kho **cá thể** `{id, kind, fresh}`, độ tươi hao theo giây, hàng ươn, giữ chỗ theo id, giá theo độ tươi; HUD độ tươi |
| 4 | v1.3 | `bench_cmd` (đo FPS có render), `soak_cmd` (10 Ngày headless); 3 cảnh báo chữ + biểu tượng (thủy quái gần, oxy sắp hết, khách sắp giận) và chế độ tương phản cao (phím H); 21 hình SVG + 24 SFX tự tạo bằng code, có phụ đề (M tắt tiếng, V phụ đề); chơi trọn một Ngày trên giao diện (`live --mode day`); cảnh nền sông phù sa với camera bám Út Sen và nhà bè isometric; LICENSE MIT |

Chi tiết từng thay đổi: [`CHANGELOG.md`](../CHANGELOG.md).

## Cách chạy
Yêu cầu: Python **3.10+**, chỉ thư viện chuẩn, một trình duyệt. Không cần cài gói.
```sh
git clone https://github.com/trongnghia2304/K5-L3B-LAB-Day12-AI-SDLC-ChoNoiCaMau.git
cd K5-L3B-LAB-Day12-AI-SDLC-ChoNoiCaMau
python3 -m chonoi.live --mode day         # mở địa chỉ in ra; chơi sáng → tối → Shop
```
Điều khiển: WASD/mũi tên đi · E hái/cập bến/bếp/bàn · Space bắt cá · P hoặc Esc tạm dừng · Shop: 1 / 2 mua nâng cấp, Enter đóng Shop · H tương phản cao · M tắt tiếng · V phụ đề · I nhãn gỡ lỗi · R chơi lại.
Chơi từng buổi: `python3 -m chonoi.live --mode gather --inventory-out kho.json` rồi `python3 -m chonoi.live --mode quan --inventory kho.json`.
Từ bản zip đính kèm: giải nén, `cd` vào thư mục, chạy lệnh cuối như trên.

| Việc | Lệnh |
|---|---|
| Test | `PYTHONPATH=tests:. python3 -m unittest discover -s tests -v` |
| Headless | `python3 -m chonoi.headless --mode gather\|quan\|day --seed 1 --ticks 600 --out out.jsonl` |
| Benchmark | `python3 -m chonoi.bench --seed 42 --duration 30 --out bench.json` |
| Soak | `python3 -m chonoi.soak --seed 7 --days 10 --out soak.json` |

Windows: dùng `PYTHONPATH="tests;."`; xem ghi chú `python3` trong [`AGENTS.md`](../AGENTS.md).

## Kết quả trên commit phát hành
| Kiểm tra | Kết quả |
|---|---|
| Toàn bộ test (`unittest discover`) | **272 test OK** (cả khi `QA_STRICT=1`) |
| Benchmark `--seed 42 --duration 30` | `avg_fps` 238.73 · `p1_low_fps` 146.79 · 7162 khung / 30.00 s · `load_ms` 684 ([`bench.json`](proof/phase-4/bench.json), máy Windows của nhóm, đo lại sau khi thêm cảnh nền). Mốc báo cáo: avg ≥ 60 đạt, 1% low ≥ 30 đạt, load ≤ 10 s đạt. Lần đo trước cảnh nền được 59.98 avg, khung hình đều quanh 60 — nhiều khả năng do vsync / tần số màn lúc đó, chưa kiểm |
| Soak `--seed 7 --days 10` | `days_completed` 10 / 10, `crashes` 0; chạy lại cùng seed giống hệt (bỏ `duration_s`) ([`soak.json`](proof/phase-4/soak.json)) |
| Ma trận AC-P4 và `verify.py --phase 4` | [`docs/verification/phase-4.md`](verification/phase-4.md) |

## Giới hạn đã biết
- **Benchmark cần trình duyệt có màn hình** để đo khung hình; máy không có màn hình thì `bench_cmd` thoát mã 1, không ghi số giả.
- **7 module test cũ vẫn chạy trên manifest v1.1 tạm** (`tests/legacy_v11.py`): `test_gather_p1`, `test_lat2`, `test_p2_lat2_fish`, `test_quan_p1`, `test_quan_lat3`, `test_quan_lat4`, `test_quan_p2`. Luật chúng bảo vệ không đổi ở v1.2; phần kho cá thể do `test_fresh_p3`, `test_lat0_v12`, `test_quan_p3`, `test_contract_v13` kiểm trên manifest thật.
- Không làm mở rộng Phase 3 (Khách đặt trước, Dòng nước lũ) và bảng xếp hạng cục bộ.
- Bot của `soak_cmd` chỉ cập bến ngay và để quán mở tới hết giờ, không nấu, nên `earned` mỗi Ngày là 0; soak kiểm độ bền và tính tất định qua 10 Ngày, không đo cách chơi giỏi.
- `tests/test_warnings.py` chạy HUD thật bằng Node; máy không có Node thì các test này bị bỏ qua (skip).
- Chỉ đo benchmark trên một máy Windows; số FPS phụ thuộc vsync và tần số màn (hai lần đo cùng máy ra ~60 và ~239), chưa đo trên macOS/Linux.

## Giấy phép và nguồn
Mã của nhóm theo [MIT](../LICENSE). Khung `chonoi/` ban đầu (headless, inputs, scenario, runner, live, web) do giảng viên cấp cho lab. Đồ họa và âm thanh do nhóm tự tạo bằng code, không dùng asset ngoài; xem [`CREDITS.md`](../CREDITS.md).
