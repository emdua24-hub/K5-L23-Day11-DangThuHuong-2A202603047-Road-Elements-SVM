# Phiếu QC 1 — Road v1.0

Lô được kiểm (học viên): phamhuynh        Người kiểm (học viên): dangthuhuong  
Ngày: 21/11/2026                        Phiên bản guideline: Road v1.0  
File được kiểm (tên + `revision` trong manifest): `phamhuynh-fisheye.zip` (không kèm manifest; sha256 `31be7922…80dc73fc`). Chưa nhận `phamhuynh-front.zip`, nên phiếu này chỉ kiểm Set B.  
Số ảnh kiểm: 6 (+ 0 ảnh thêm, ghi riêng)   Số nhãn đã kiểm (shape + tag): 44 (20 shape + 24 tag)  

Ảnh đã kiểm: B01 (FV), B02 (FV), B03 (RV), B04 (RV), B06 (MVL), B08 (MVR). `road_check lint` trên file: 8 ảnh, 0 cảnh báo.

## Tổng hợp theo mã
| Mã | Loại lỗi | Số mục kiểm | Số lỗi | Mức |
| --- | --- | ---: | ---: | --- |
| QC1-01 tag camera | sai thuộc tính / bỏ sót | 6 | 0 | nặng |
| QC1-02 đủ L1, R1 | bỏ sót | 4 | 0 | nặng |
| QC1-03 position | sai thuộc tính / thừa | 6 | 0 | nặng |
| QC1-04 tâm vạch, chiều, điểm dừng | sai phạm vi / bỏ sót | 6 | 1 | chính |
| QC1-05 vạch đứt, che | sai phạm vi / sai thuộc tính | 6 | 0 | chính |
| QC1-06 style, color, kind | sai thuộc tính | 6 | 0 | chính |
| QC1-07 vùng lái | sai phạm vi / bỏ sót / thừa | 9 | 1 | chính |
| QC1-08 đèn, màu | bỏ sót / sai phạm vi / sai thuộc tính | 5 | 0 | nặng / chính |
| QC1-09 facing, type, group, biển phụ | sai thuộc tính | 5 | 0 | chính |
| QC1-10 hình in, đủ biển | bỏ sót / sai phạm vi | 1 | 0 | chính |
| QC1-11 fisheye | sai phạm vi / sai thuộc tính | 20 | 0 | chính |
| QC1-12 file | lỗi file | 1 | 0 | nhẹ / nặng |

Cách đếm số mục kiểm:
- QC1-01: tag camera của 6 ảnh. Ba tag còn lại (18 tag) cũng đủ và hợp lệ.
- QC1-02: 4 ảnh FV/RV. B01, B02 không thấy vạch làn; bên trái làn ở B03 là vỉa hè, không có sơn.
- QC1-03 đến QC1-06: 6 polyline `lane`.
- QC1-07: 6 `drivable`, 2 `crosswalk` và 1 `road_curb`.
- QC1-08, QC1-09: 5 `traffic_light` ở B02.
- QC1-10: ảnh B02, ảnh duy nhất trong 6 ảnh có trọng tâm đèn/biển.
- QC1-11: cả 20 shape đều trên ảnh fisheye.
- QC1-12: 1 file đã nhận.

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
Lỗi nặng: 0   Lỗi chính: 2 / 44 nhãn = 4,5 %   Lỗi nhẹ: 0   Ca chờ lead: 2  
Quyết định (Release / Hold / Rework): **Hold**  
Lý do (theo luật của phiếu): Không có lỗi nặng và tỉ lệ lỗi chính 4,5 % chưa vượt 5 %, nên theo số liệu thì đủ Release. Nhưng ca mơ hồ về L1/R1 ở camera sau ảnh hưởng tới kết luận: nếu lead trả lời "theo hướng xe chạy", B03 và B04 có 2 lỗi nặng QC1-03 và lô phải Rework. Vì vậy Hold đến khi lead trả lời. Nếu lead chấp nhận cách gán theo khung ảnh thì chuyển Release; bạn vẫn nên sửa 2 lỗi chính ở trên.

Loại lỗi: bỏ sót = thiếu nhãn phải có; thừa = gán cái không phải đối tượng; sai phạm vi = hình lệch, sai chiều, dừng sai chỗ, tràn vùng. Ba loại này theo cách chia lỗi gán nhãn của ISO/IEC 5259-4 (mục 8.4.2); "sai thuộc tính" và "lỗi file" là khóa học thêm. Cách xếp mức: [thang mức lỗi](docs/qc_severity_road_v1.0.md).
