# Day 12 — AI SDLC: Quán Ăn Chợ Nổi

> **Làm theo nhóm 4–6 người** · Dùng AI để xây game 2D qua **4 phase** trong **3 giờ**: Út Sen lặn bắt cá, hái rau trên mặt nước rồi tối mở quán phục vụ khách.
> **Yêu cầu từng phase mở đúng mốc giờ tại lab board:** **[Chợ Nổi Lab](https://claude.ai/artifact/E35v787wSx2wDgPTNhMVDg)** (cần đăng nhập claude.ai; có trợ giảng AI cho từng phase).

## Nhóm ChoNoiCaMau — chạy game

Thành viên và vai trò: [`TEAM.md`](TEAM.md). Yêu cầu từng phase (bản chép): [`docs/prd/`](docs/prd/). Chỉ cần Python 3 với thư viện chuẩn (nhóm đã chạy với 3.11 và 3.12), không cài thêm gì.

**Chạy từ clone sạch (macOS / Linux, 3 lệnh):**

```bash
git clone https://github.com/trongnghia2304/K5-L3B-LAB-Day12-AI-SDLC-ChoNoiCaMau.git && cd K5-L3B-LAB-Day12-AI-SDLC-ChoNoiCaMau
PYTHONPATH=tests:. python3 -m unittest discover -s tests -v      # test của nhóm (1 lệnh)
python3 -m chonoi.live --mode gather --inventory-out kho.json    # chơi buổi sáng, mở địa chỉ in ra
```
- **Chơi cả Ngày** (sáng → tối → Shop): `python3 -m chonoi.live --mode day --days 5`
- **Buổi tối:** chạy `python3 -m chonoi.live --mode quan --inventory kho.json`; quán nhận kho buổi sáng mang về.
- **Windows (PowerShell):** đặt `$env:PYTHONPATH="tests;."` rồi chạy `python3 -m unittest discover -s tests -v`. Nếu báo không thấy `python3`, xem ghi chú Windows trong [`AGENTS.md`](AGENTS.md).

**Headless (gather / quan):**
`python3 -m chonoi.headless --mode gather|quan --seed 1 --ticks 600 --out out.jsonl [--scenario f.json] [--inputs f.json] [--inventory kho.json | --inventory-out kho.json]`
**Headless (cả Ngày)**: python3 -m chonoi.headless --mode day --days 5 --seed 1 --ticks 111000 --out day.jsonl
**Benchmark FPS (bench_cmd, 1280×720, làm nóng 3 s, đo 30 s)**: python3 -m chonoi.bench --seed 42 --duration 30 --out bench.json
**Soak test nhiều Ngày (soak_cmd, lặp chu kỳ D1–D5, 0 crash)**: python3 -m chonoi.soak --seed 7 --days 10 --out soak.json

**Điều khiển:**
- **WASD / mũi tên:** bơi (sáng) hoặc đi (tối). Ở buổi sáng, phím xuống là lặn sâu hơn.
- **E:** cập bến hoặc hái nút trên mặt nước (sáng); nấu, bưng, phục vụ (tối).
- **Space:** bắt đàn cá khi đang dưới nước.
- **R:** chơi lại.
- **M:** tắt/bật tiếng. **V:** ẩn/hiện phụ đề SFX. **H:** bật/tắt **chế độ tương phản cao** (nhớ lựa chọn cho lần mở sau).
- **Cảnh báo trên HUD** (chữ + biểu tượng, góc phải): "Thủy quái!" (Cá Sấu trong 15 m, kèm viền tím), "Oxy sắp hết!" (dưới 25%) / "Hết hơi!" (oxy cạn, đang dưới nước), "Khách sắp giận!" (kiên nhẫn dưới 25%).

**Cách tính điểm** (mọi snapshot và dòng `end`): `điểm = earned + 100 × floor(uy tín)`.
- `earned` là tổng thu của lượt, gồm tiền món và tiền boa, đơn vị nghìn đồng.
- Uy tín bắt đầu từ 3.0, kẹp trong [0, 5].
  - Khách được boa thì +0.2.
  - Khách hết kiên nhẫn bỏ đi khi ca còn giờ thì −0.5.

**Cả ngày (Test Contract v1.1):** `python3 -m chonoi.headless --mode day --days 5 --seed 1 --ticks 111000 --out day.jsonl` chạy 5 Ngày: sáng lặn → tối mở quán → Shop.

**Tiến độ** (ghi trung thực):
- **Phase 1:** chi tiết trong [`docs/verification/phase-1.md`](docs/verification/phase-1.md).
- **Phase 2:** đạt 11/12 AC, gồm v1.1, cá bơi/sinh, Cá Sấu, 2 bếp/4 bàn, 6 món, `day`, Shop, pause, `burn`.
  - Chưa đạt: UI Phase 2 (menu, pause, màn chuyển Ngày) trên cửa sổ chơi.
  - Chi tiết: [`docs/verification/phase-2.md`](docs/verification/phase-2.md).
  **Phase 3:** đạt Contract v1.2, kho cá thể `{id, kind, fresh}`, hao độ tươi, sự kiện `spoiled`, giữ chỗ theo ID, tính giá theo độ tươi trung bình, HUD độ tươi (28/28 test QA đạt).
  - Chi tiết: [`docs/verification/phase-3.md`](docs/verification/phase-3.md).
- **Phase 4:** đạt Contract v1.3 (`bench_cmd`, `soak_cmd`), 3 cảnh báo HUD, chế độ tương phản cao, phụ đề SFX, `--mode day` trên cửa sổ chơi, đóng gói phát hành `v1.0.0`.
  - Chi tiết: [`docs/verification/phase-4.md`](docs/verification/phase-4.md).

**Mở rộng đã làm** (khai trong `game.contract.json` → `"extensions"`):
- **`burn` — Cháy món.** `quan.burn_s: 20`. Món chín để trên bếp quá 1.200 tick (`ready_ticks > 1200`) thì cháy: bỏ món, nguyên liệu mất, bếp rảnh ngay, và phát sự kiện `dish_burnt`.
  - Nếu kho còn đủ nguyên liệu, đơn được giữ lại thành `reserved`; thiếu thì thành `unfulfillable`.
  - Món để đúng 1.200 tick vẫn bưng được.
  - Test: `tests/test_quan_lat4.py::test_burn_*`, `tests/test_lat0_v11.py::test_burn_chi_hop_le_khi_bat_extension`.
  - Bằng chứng: [`docs/proof/phase-2/final-run.md`](docs/proof/phase-2/final-run.md).

**Sáng tạo của nhóm:**
- **Đồ họa SVG tự tạo (21 hình)** trong `chonoi/web/assets/`: Út Sen (áo bà ba, khăn rằn, kính lặn, giỏ lưới), cá linh / lóc / kèo, Cá Sấu Năm Chèo, bông điên điển / súng, kèo nèo, bến, bếp, bàn, khách đội nón lá, 6 món ăn, 3 biểu tượng cảnh báo. Sinh bằng `python3 tools/gen_assets.py`.
- **SFX tự tạo (24 hiệu ứng)** bằng Web Audio (`chonoi/web/assets/sfx.js`), kèm **phụ đề** cho mọi hiệu ứng, hiện cả khi tắt tiếng. Phím **M** tắt/bật tiếng, **V** ẩn/hiện phụ đề.
- **Cảnh nền theo PRD-0 mục 3** (`chonoi/web/scene.js`, vẽ bằng code): buổi sáng là mặt cắt sông **phù sa** nhiều lớp (trời hồng bình minh, bờ dừa nước, nhà sàn, tia nắng xiên, bụi phù sa, bong bóng, lục bình trôi, rễ dừa nước, đáy bùn có rong lay); **camera zoom bám theo Út Sen** (sông rộng hơn màn hình theo cả chiều ngang lẫn chiều sâu, có bản đồ nhỏ góc phải dưới và thước độ sâu); buổi tối là **nhà bè nhìn isometric 2.5D** trên sông đêm (sàn gạch bông, khu bếp gạch nung + quầy, hai vách tre có cửa sổ nhìn ra sông, dây đèn vàng, ghế đẩu quanh bàn tròn, bong bóng gọi món, chậu bông giấy, khói bếp); phím di chuyển xoay 45° cho khớp góc nhìn (luật không đổi). Phần tĩnh vẽ một lần vào canvas phụ nên bench vẫn ~60 FPS. Nhãn gỡ lỗi (F1/N1/M1, log sự kiện) ẩn mặc định, phím **I** để hiện.
- **Giao diện mode `day` "Sạp đêm chợ nổi"** (`chonoi/web/day.js`): Shop, màn chuyển pha, tạm dừng, tổng kết theo phong cách pixel.
- Không dùng asset ngoài (xem `CREDITS.md`). Client tự về hình khối cũ nếu asset chưa tải (thiếu `scene.js` thì cảnh nền về như cũ).
- **Tắt ở headless:** asset chỉ nằm ở client; `runner`, `headless`, `sim/` không đọc chúng.
- **Bằng chứng lõi vẫn đúng:** `tests/test_assets.py` (12 test: SVG hợp lệ và khớp script sinh, đủ asset cho mọi món/loài trong dữ liệu, tên sự kiện SFX khớp sim, route `/assets/` chặn truy cập ra ngoài, headless không nhắc tới asset). Toàn bộ suite 272 test OK; headless cùng seed vẫn giống từng byte.

---

## Bắt buộc (chỉ có thế)

Cách làm việc kỹ thuật là **tự do** (push thẳng, nhánh + PR, pair, có hay không CI…). Cái bắt buộc:

1. **Một repo chung:** trưởng nhóm **fork** repo này, đặt tên `K5-L3B-LAB-Day12-AI-SDLC-<ten-nhom>`, thêm mọi thành viên làm collaborator (*Settings → Collaborators*, quyền Write) và để repo **public** (giảng viên xem được, không cần mời).
2. **Cả nhóm cùng commit** bằng tài khoản GitHub của mình, mỗi người **≥ 3 commit có nội dung** (làm cặp thì ghi `Co-authored-by:`).
3. **Điền [`TEAM.md`](TEAM.md):** danh sách thành viên + thỏa thuận làm việc nhóm; sửa `team_size` trong [`game.contract.json`](game.contract.json). Commit của người không có trong `TEAM.md` không được tính.
4. **Gắn tag** `phase-1` … `phase-4` đúng hạn; chạy được tại từng tag.
5. **Ghi chép tối thiểu:** `docs/verification/phase-n.md`, `docs/clarify.md`, `docs/ai-usage-log.md`, `docs/retro.md`, `docs/members/<handle>.md`.

## Bắt đầu nhanh

```bash
git clone <fork-của-nhóm> && cd <fork-của-nhóm>
PYTHONPATH=tests:. python3 -m unittest discover -s tests -v        # smoke test
python3 -m chonoi.live --mode gather                                # chơi thử trên trình duyệt (tùy chọn)
```

Game cần chế độ **headless** theo Test Contract (`python3 -m chonoi.headless --mode gather|quan …`) để được chạy và chấm tự động. **Công nghệ không ràng buộc**: khung Python trong `chonoi/` chỉ là gợi ý, chi tiết ở [`docs/STARTER.md`](docs/STARTER.md).

## Tự do sáng tạo

Ngoài phần đề yêu cầu, nhóm được **thêm** nhân vật, loài cá/cây, sinh vật, món, bản đồ, cơ chế, câu chuyện, đồ họa, âm nhạc… với ba điều kiện: **không phá phần cố định** (tắt ở headless), ghi mục **"Sáng tạo của nhóm"** trong README của nhóm, và **làm lõi đúng trước**.

## Lịch 3 giờ

| Mốc | Phase | Hạn tag |
|---|---|---|
| 0:00 | Phase 1 (PRD-0) | `phase-1` trước 1:00 |
| 1:00 | Phase 2 | `phase-2` trước 1:55 |
| 1:55 | Phase 3 | `phase-3` trước 2:30 |
| 2:30 | Phase 4 | `phase-4` + Release `v1.0.0` trước 3:00 |

Hết giờ mỗi phase thì gắn tag rồi đi tiếp; chỗ chưa xong ghi trung thực trong bằng chứng.

## Đánh giá (100 điểm)

| Hạng mục | Điểm |
|---|---|
| Test tự động ẩn (headless, gồm kiểm tra lại phase trước) | 25 |
| Làm việc nhóm (một repo chung, chia việc, tích hợp, đổi yêu cầu) | 25 |
| Kiểm chứng & bằng chứng | 15 |
| Harness AI (AGENTS.md, skill, nhật ký dùng AI) | 10 |
| Đánh giá & retro | 10 |
| Chất lượng sản phẩm (đúng yêu cầu, mở rộng/sáng tạo) | 10 |
| Demo | 5 |

## Quy định

- Dùng AI thoải mái; **không sao chép repo của nhóm khác**; bằng chứng phải thật.
- Nhân vật, bối cảnh là bản gốc của lab; asset ngoài chỉ dùng CC0/CC-BY/MIT hoặc tự tạo, ghi nguồn trong `CREDITS.md`. Không commit API key, token, `.env`.
