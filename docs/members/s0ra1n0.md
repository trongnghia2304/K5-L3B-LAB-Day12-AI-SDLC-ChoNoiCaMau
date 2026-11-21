# s0ra1n0 — phản tư cá nhân (≤ 200 từ)

**Phần tôi làm:** lát 1 gather và Integrator cả buổi.
- P1: oxy/ngạt/Máu/ngất + 13 test ([`03f8514`](https://github.com/trongnghia2304/K5-L3B-LAB-Day12-AI-SDLC-ChoNoiCaMau/commit/03f8514)); chốt contracts, tag `phase-1` ([`9d19210`](https://github.com/trongnghia2304/K5-L3B-LAB-Day12-AI-SDLC-ChoNoiCaMau/commit/9d19210)).
- P2: nền contract v1.1 ([`181eb57`](https://github.com/trongnghia2304/K5-L3B-LAB-Day12-AI-SDLC-ChoNoiCaMau/commit/181eb57)); Cá Sấu + 14 test ([`91fd19e`](https://github.com/trongnghia2304/K5-L3B-LAB-Day12-AI-SDLC-ChoNoiCaMau/commit/91fd19e)); mở burn ([`3caea79`](https://github.com/trongnghia2304/K5-L3B-LAB-Day12-AI-SDLC-ChoNoiCaMau/commit/3caea79)).
- P3: `items.py`, kho cá thể ([`4f33304`](https://github.com/trongnghia2304/K5-L3B-LAB-Day12-AI-SDLC-ChoNoiCaMau/commit/4f33304)); kiểm trước tag ([`80a7f7e`](https://github.com/trongnghia2304/K5-L3B-LAB-Day12-AI-SDLC-ChoNoiCaMau/commit/80a7f7e)).
- P4: contract 1.3, LICENSE ([`ba98561`](https://github.com/trongnghia2304/K5-L3B-LAB-Day12-AI-SDLC-ChoNoiCaMau/commit/ba98561)); sửa `main` đỏ, CHANGELOG, release notes ([`0b34693`](https://github.com/trongnghia2304/K5-L3B-LAB-Day12-AI-SDLC-ChoNoiCaMau/commit/0b34693)); ma trận P4, clone sạch, tag + Release.

**Đã giúp ai:** giải 5 file xung đột lát 0 ↔ lát 5 (Tiệp); sửa `_available()` cho Nghĩa và Hùng làm quán v1.2; `CHONOI_CONTRACT` để cả nhóm thử v1.2 trước khi bật.

**AI sai ở đâu:**
- Báo lab board "không có yêu cầu"; tôi mở trình duyệt đọc lại.
- Đoán sai số món rơi ở cú cắn thứ hai; tôi tính tay lại theo Ticket rồi mới sửa test.
- `verify.py` báo kho "chưa đạt" dù 233 test xanh: công cụ còn dùng giỏ đếm cũ.

**Bài học:** test xanh chưa đủ. Phải chạy thật đúng lệnh bên chấm dùng và tính tay một mốc số trước khi tin kết quả.
