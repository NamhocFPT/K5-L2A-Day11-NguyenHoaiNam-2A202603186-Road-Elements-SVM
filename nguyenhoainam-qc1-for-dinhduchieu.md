# Phiếu QC 1 — Road v1.0

Lô được kiểm (học viên): Đinh Đức Hiếu (`dinhduchieu`)        Người kiểm (học viên): Nguyễn Hoài Nam (`nguyenhoainam`)  
Ngày: 21/11/2026                          Phiên bản guideline: Road v1.0  
File được kiểm (tên + `revision` trong manifest): `dinhduchieu-front.zip` (sha256 4d7ae159…ef83), `dinhduchieu-fisheye.zip` (sha256 8dedf4aa…d1df), `revision` 1 — sha256 đã đối chiếu, khớp manifest  
Số ảnh kiểm: 6 (+ 0 ảnh thêm, ghi riêng)   Số nhãn đã kiểm (shape + tag): 69 (45 shape + 24 tag)  

Ảnh kiểm: A01, A04, A07 (front) · B04 (fisheye_rear), B06 (fisheye_left), B08 (fisheye_right). Chọn để phủ đủ 12 mã: tag camera, vàng đôi, đèn, fisheye trước/sau và hai bên.

## Tổng hợp theo mã
| Mã | Loại lỗi | Số mục kiểm | Số lỗi | Mức |
| --- | --- | ---: | ---: | --- |
| QC1-01 tag camera | sai thuộc tính / bỏ sót | 6 | 0 | nặng |
| QC1-02 đủ L1, R1 | bỏ sót | 4 | 1 | nặng |
| QC1-03 position | sai thuộc tính / thừa | 12 | 0 | nặng |
| QC1-04 tâm vạch, chiều, điểm dừng | sai phạm vi / bỏ sót | 12 | 0 | chính |
| QC1-05 vạch đứt, che | sai phạm vi / sai thuộc tính | 6 | 0 | chính |
| QC1-06 style, color, kind | sai thuộc tính | 12 | 0 | chính |
| QC1-07 vùng lái | sai phạm vi / bỏ sót / thừa | 13 | 2 | chính |
| QC1-08 đèn, màu | bỏ sót / sai phạm vi / sai thuộc tính | 5 | 0 | nặng / chính |
| QC1-09 facing, type, group, biển phụ | sai thuộc tính | 17 | 1 | chính |
| QC1-10 hình in, đủ biển | bỏ sót / sai phạm vi | 12 | 0 | chính |
| QC1-11 fisheye | sai phạm vi / sai thuộc tính | 14 | 0 | chính |
| QC1-12 file | lỗi file | 2 | 0 | nhẹ / nặng |

Cách đếm số mục kiểm: QC1-01 = số ảnh; QC1-02 = số ảnh front/fisheye_front/fisheye_rear; QC1-03, 04, 06 = số polyline lane; QC1-05 = số lane đứt; QC1-07 = drivable + crosswalk (gồm cả polygon phải có); QC1-08 = số đèn; QC1-09 = đèn + biển; QC1-10 = số biển; QC1-11 = mọi shape trên ảnh fisheye; QC1-12 = số file zip.

## Danh sách lỗi (mỗi lỗi một dòng)
| # | Mã | Ảnh | Nhãn (label, id) | Mức | Mô tả ngắn |
| --- | --- | --- | --- | --- | --- |
| 1 | QC1-02 | A07 | lane — thiếu L1 | nặng | Có vạch trắng liền chạy sát dải bê tông bên trái làn đang đi, thấy rõ từ khoảng (450,430) tới (600,390), nhưng không có polyline L1 (bỏ sót). Lint cũng báo "chưa có L1". |
| 2 | QC1-07 | A07 | drivable#2 (`direct`), điểm đầu (60,555) | chính | Polygon dừng ở đuôi xe phía trước (y ≈ 405) thay vì kéo tới điểm xa nhất của làn; guideline: gán theo "đi được nếu đường trống", không cắt quanh xe khác (sai phạm vi). |
| 3 | QC1-07 | B06 | drivable — không có | chính | Ảnh chỉ có crosswalk và lane, thiếu polygon `drivable` cho mặt đường (bỏ sót); B06 có trọng tâm "drivable + lane position=other". |
| 4 | QC1-09 | B04 | traffic_light#1, (1057,235) | chính | Đèn hình người đỏ dưới biển cấm vào là đèn đi bộ nhưng gán `type = vehicle`; đúng là `pedestrian` (sai thuộc tính). |

Ảnh ghi "không lỗi": **A01** (đủ 4 tag, L1/R1/L2/R2 đúng tâm vạch và chiều, 3 polygon drivable đúng direct/alternative, 2 biển sát mép).

## Ca mơ hồ
| Ảnh | Câu hỏi cho lead | Lời đáp (nếu có) |
| --- | --- | --- |
| B04 | `drivable#1` gán `alternative` cho làn BUS bên trái. Làn chỉ dành cho xe buýt có tính là "làn cùng chiều bên cạnh" không? (không đổi kết luận vì lô đã Rework) | |
| A05, A09 (ngoài 6 ảnh kiểm) | Lint báo QC1-02 "chưa có L1/R1"; cần coach xác nhận ảnh không thấy vạch để miễn. | |

## Quyết định
Lỗi nặng: 1   Lỗi chính: 3 / 69 nhãn = 4,3 %   Lỗi nhẹ: 0   Ca chờ lead: 2 (không ảnh hưởng kết luận)  
Quyết định (Release / Hold / Rework): **Rework**  
Lý do (theo luật của phiếu): còn 1 lỗi nặng (QC1-02, A07 thiếu L1 khi vạch thấy được) → Rework. Tỉ lệ lỗi chính 4,3 % ≤ 5 % nên riêng lỗi chính không buộc Rework. Cần sửa: thêm L1 ở A07, kéo drivable direct ở A07 tới cuối làn, thêm drivable ở B06, đổi type đèn B04 thành pedestrian; sau đó export lại giữ tên zip và tạo manifest `--revision 2`.

Loại lỗi: bỏ sót = thiếu nhãn phải có; thừa = gán cái không phải đối tượng; sai phạm vi = hình lệch, sai chiều, dừng sai chỗ, tràn vùng. Ba loại này theo cách chia lỗi gán nhãn của ISO/IEC 5259-4 (mục 8.4.2); "sai thuộc tính" và "lỗi file" là khóa học thêm. Cách xếp mức: [thang mức lỗi](docs/qc_severity_road_v1.0.md).
