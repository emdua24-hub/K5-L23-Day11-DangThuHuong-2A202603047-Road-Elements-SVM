# Phiếu QC 1 — Road v1.0

Lô được kiểm (học viên): phamhuynh        Người kiểm (học viên): dangthuhuong  
Ngày: 21/11/2026                        Phiên bản guideline: Road v1.0  
File được kiểm (tên + `revision` trong manifest): `1-front.zip` (sha256 `cbbefacf…68f24fa62`, A01–A12) và `3-fisheye.zip` (sha256 `31be7922…80dc73fc`, B01–B12). Bạn gửi không kèm manifest; coi là lần nộp 1.  
Số ảnh kiểm: 6 (+ 0 ảnh thêm, ghi riêng)   Số nhãn đã kiểm (shape + tag): 59 (35 shape + 24 tag)  

Ảnh đã kiểm: A01, A04, A12 (front); B01 (FV), B03 (RV), B04 (RV). `road_check lint`: front 12 ảnh, 9 cảnh báo; fisheye 8 ảnh, 0 cảnh báo.

## Tổng hợp theo mã
| Mã | Loại lỗi | Số mục kiểm | Số lỗi | Mức |
| --- | --- | ---: | ---: | --- |
| QC1-01 tag camera | sai thuộc tính / bỏ sót | 6 | 0 | nặng |
| QC1-02 đủ L1, R1 | bỏ sót | 6 | 0 | nặng |
| QC1-03 position | sai thuộc tính / thừa | 14 | 0 | nặng |
| QC1-04 tâm vạch, chiều, điểm dừng | sai phạm vi / bỏ sót | 14 | 1 | chính |
| QC1-05 vạch đứt, che | sai phạm vi / sai thuộc tính | 14 | 0 | chính |
| QC1-06 style, color, kind | sai thuộc tính | 14 | 0 | chính |
| QC1-07 vùng lái | sai phạm vi / bỏ sót / thừa | 15 | 1 | chính |
| QC1-08 đèn, màu | bỏ sót / sai phạm vi / sai thuộc tính | 2 | 0 | nặng / chính |
| QC1-09 facing, type, group, biển phụ | sai thuộc tính | 6 | 0 | chính |
| QC1-10 hình in, đủ biển | bỏ sót / sai phạm vi | 2 | 0 | chính |
| QC1-11 fisheye | sai phạm vi / sai thuộc tính | 10 | 0 | chính |
| QC1-12 file | lỗi file | 2 | 0 | nhẹ / nặng |

Cách đếm số mục kiểm:
- QC1-01: tag camera của 6 ảnh. Ba tag còn lại (18 tag) cũng đủ và hợp lệ.
- QC1-02: 6 ảnh, gồm 3 ảnh front và 3 ảnh FV/RV.
- QC1-03 đến QC1-06: 14 polyline `lane` (A01: 4, A04: 5, A12: 2, B03: 2, B04: 1).
- QC1-07: 11 `drivable`, 3 `crosswalk` và 1 `road_curb`.
- QC1-08: 2 đèn ở A12.
- QC1-09: 2 đèn và 4 biển.
- QC1-10: A01 và A12, hai ảnh có biển.
- QC1-11: 10 shape trên ảnh fisheye.
- QC1-12: 2 file.

Ghi chú các ca không tính lỗi:
- **A12:** không có R1. Bên phải làn là dãy xe đỗ, không thấy vạch sơn; lint báo QC1-02, đề nghị coach xác nhận miễn.
- **A04:** vạch đôi L1 (vàng) và R2 (trắng) mỗi nét vẽ một polyline. Lint báo QC1-05, nhưng đây là cách vẽ theo hướng dẫn của coach, nên không tính lỗi.
- **A01:** L2 trắng `kind = double`. Phóng to thấy đúng là hai nét song song.

## Danh sách lỗi (mỗi lỗi một dòng)
| # | Mã | Ảnh | Nhãn (label, id) | Mức | Mô tả ngắn |
| --- | --- | --- | --- | --- | --- |
| 1 | QC1-04 | B04 | lane#0@(515,330), L1 | chính | Polyline bắt đầu ở giữa ảnh (515,330), bỏ cả đoạn vạch phân cách làn BUS chạy xuống sát vạch qua đường, khoảng (240,600). Điểm đầu không sát xe; polygon `drivable` vẫn bám theo đoạn vạch bị bỏ. |
| 2 | QC1-07 | B01 | crosswalk#0@(88,572) | chính | Cạnh trái polygon tràn sang dải lát gạch đỏ ngoài vạch trắng ngoài cùng, khoảng 80–100 px. Cạnh dưới đè lên mép cản xe. |

## Ca mơ hồ
| Ảnh | Câu hỏi cho lead | Lời đáp (nếu có) |
| --- | --- | --- |
| B03, B04 | Camera sau (RV): L1/R1 tính theo hướng xe chạy (L1 ở phía phải ảnh) hay theo trái/phải của khung ảnh? Bài gán theo khung ảnh và nhất quán: B03 R1 ở phải ảnh, B04 L1 ở trái ảnh. Nếu lead chọn theo hướng xe chạy thì 2 lane này sai `position` (QC1-03, nặng). |  |
| B04 | `drivable alternative` gán cho làn BUS bên cạnh. Làn dành riêng cho xe buýt có tính là "làn cùng chiều bên cạnh" không? |  |

## Quyết định
Lỗi nặng: 0   Lỗi chính: 2 / 59 nhãn = 3,4 %   Lỗi nhẹ: 0   Ca chờ lead: 2  
Quyết định (Release / Hold / Rework): **Hold**  
Lý do (theo luật của phiếu): Không có lỗi nặng và tỉ lệ lỗi chính 3,4 % dưới 5 %, nên theo số liệu thì đủ Release. Nhưng ca mơ hồ về L1/R1 ở camera sau ảnh hưởng tới kết luận: nếu lead trả lời "theo hướng xe chạy", B03 và B04 có 2 lỗi nặng QC1-03 và lô phải Rework. Vì vậy Hold đến khi lead trả lời. Nếu lead chấp nhận cách gán theo khung ảnh thì chuyển Release; bạn vẫn nên sửa 2 lỗi chính ở trên.

Loại lỗi: bỏ sót = thiếu nhãn phải có; thừa = gán cái không phải đối tượng; sai phạm vi = hình lệch, sai chiều, dừng sai chỗ, tràn vùng. Ba loại này theo cách chia lỗi gán nhãn của ISO/IEC 5259-4 (mục 8.4.2); "sai thuộc tính" và "lỗi file" là khóa học thêm. Cách xếp mức: [thang mức lỗi](docs/qc_severity_road_v1.0.md).
