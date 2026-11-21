# Báo cáo cá nhân — Ngày 11
Họ tên: Đặng Thu Hương               Mã học viên: 2A202603047  
Phiên bản CVAT (Help → About): Server 2.74.1, UI 2.74.1  
Guideline: Road v1.0    Lần nộp (`revision` trong manifest): 1  
Thời điểm export (ngày giờ trên CVAT): 21/11/2026, front 11:35, fisheye 11:19    Số lần nộp lại sau Rework: 0  

## 1. Kiểm project
| Mục | Đúng như guideline Road v1.0? | Ghi chú |
| --- | --- | --- |
| 6 label, đúng kiểu | Có | lane, road_curb (polyline); crosswalk, drivable (polygon); traffic_light, traffic_sign (rectangle). `road_check lint` không báo label lạ (QC1-12). |
| Thuộc tính và giá trị | Có | So với `configs/road_v1_labels.json` (`python -m road_lab.taxonomy`); `weather` có đủ `overcast`. |
| 4 tag ảnh | Có | camera, timeofday, weather, source; mỗi tag một thuộc tính select `value`. Manifest: 16/16 ảnh front và 12/12 ảnh fisheye đủ bốn tag. |

Cách kết thúc polyline trên bản CVAT này: click lần lượt các điểm từ gần xe ra xa, bấm phím **N** để kết thúc.  
Cách đặt tag ảnh trên bản CVAT này: công cụ **Tag** ở thanh bên trái → chọn label → thêm tag; mở **Details** của tag trong bảng Objects để chọn `value`. Đặt `camera` trước.

## 2. Số lượng đã gán
| Task | Ảnh | lane | road_curb | crosswalk | drivable | traffic_light | traffic_sign |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| front | 12 | 47 | 19 | 9 | 23 | 35 | 33 |
| front-extra (đã làm 4 / 4) | 4 | 15 | 5 | 6 | 5 | 11 | 13 |
| fisheye | 8 | 19 | 12 | 11 | 8 | 5 | 8 |
| fisheye-extra (đã làm 4 / 4) | 4 | 3 | 5 | 5 | 3 | 0 | 1 |

## 3. Bảng ca khó
| Ảnh | Nhãn | Phân vân giữa | Đã chọn | Lý do |
| --- | --- | --- | --- | --- |
| A03, A06, A11 | lane L1 | Vẽ vạch đôi bằng một polyline ở giữa hai nét, hay mỗi nét một polyline | Mỗi nét một polyline, cả hai mang `kind = double` | Theo hướng dẫn của coach. Guideline chỉ nói "polyline đặt trên tâm vệt sơn" và không nói riêng vạch đôi; lint cảnh báo QC1-05 "2 polyline cùng position" ở các ảnh này. |
| A03 | (không gán) | Gán `lane` cho các gạch vàng ngắn nằm ngang trong đảo sơn, hay bỏ | Không gán | Không phải vạch kẻ làn chạy dọc; BDD100K cũng bỏ vạch ngang. |
| A03 | lane L2 / road_curb | Mép dải phân cách: vạch sơn vàng hay mép bê tông | `lane L2`, `solid / yellow / single`; bỏ `road_curb` ở đoạn này | `road_curb` chỉ dùng nơi không có sơn (guideline mục 1); không gán cả hai cho cùng một đoạn. |
| A03 | drivable, lane | Gán phần đường chiều ngược lại sau dải phân cách không | Không gán | `direct` / `alternative` chỉ gồm làn cùng chiều; L1/R1/L2/R2 không gồm vạch chiều ngược lại. |
| A03 | traffic_sign | Biển chỉ thấy mặt sau: nhóm nào | `group = other`, `facing = back` | Không đọc được nội dung biển nên không xếp được theo chức năng. |
| A05 | lane L1, R1 | Vẽ L1/R1 theo mép xe đỗ, hay để trống | Để trống | Đường không có vạch sơn. Lint báo QC1-02; coach đã xác nhận miễn, dặn không suy diễn vạch không nhìn thấy. |
| A05 | road_curb | Vỉa hè bên trái bị cả dãy xe đỗ che: nối qua và bật `occluded`, hay chỉ vẽ đoạn thấy | Chỉ vẽ đoạn thấy | Guideline chỉ cho nối qua chỗ che **ngắn**; ở đây bị che kín cả đoạn dài, không đoán được vị trí vỉa hè. |
| A05 | drivable | Mép trái `direct` ở vỉa hè hay ở mép dãy xe đỗ | Mép ngoài dãy xe đỗ | Dải đỗ xe sát lề không thuộc `direct`. |
| A06, A11 | lane | Vạch trắng gần nằm ngang (A06: phía trên vạch qua đường): lane `other` hay không gán | Gán `lane`, `position = other`, `solid / white / single` | Là vạch sơn trắng nhìn thấy rõ nhưng không phải biên làn đang đi (không thuộc L1/R1/L2/R2), nên dùng `other`. Vạch chạy gần ngang nên lint cảnh báo chiều vẽ (QC1-04); GT có thể bỏ loại vạch này vì script chuyển BDD100K loại vạch "vertical / stop line". |
| A09, A14 | lane R1 | Vẽ R1 theo vị trí đoán được, hay để trống | Để trống | Không thấy vạch sơn bên phải. Lint báo QC1-02; coach đã xác nhận miễn, dặn không suy diễn vạch không nhìn thấy. |
| B03, B04 | lane L1, R1 | Camera sau: L1/R1 theo hướng xe chạy (L1 ở phía phải ảnh) hay theo trái/phải của khung ảnh | Theo hướng xe chạy: L1 ở phía phải ảnh, R1 ở phía trái ảnh | Guideline chỉ nói "tính theo làn đang đi", không nói riêng camera sau. Câu hỏi cho lead. |
| B04 | drivable | Cạnh gần xe của polygon | Dừng đúng ở mép thân xe | Thân xe và vùng tối viền ảnh không thuộc vùng lái (guideline mục 4). |

## 4. Phiếu QC 1 nhận được
Bạn kiểm lô của tôi: [điền]  Quyết định nhận được: [điền]  
| Mã lỗi | Số lỗi | Đồng ý? | Đã sửa? |
| --- | ---: | --- | --- |
| [điền] |  |  |  |

## 5. Kiểm file export
| File | Mở lại được? | Số shape ảnh 1 (job / file) | Số shape ảnh 2 (job / file) | Tag camera có trong XML? |
| --- | --- | --- | --- | --- |
| dangthuhuong-front.zip | Có (`road_check lint`, 16 ảnh) | A01: 11 / 11 | A02: 8 / 8 | Có, 16/16 ảnh `camera = front` |
| dangthuhuong-fisheye.zip | Có (`road_check lint`, 12 ảnh) | B01: 8 / 8 | B02: 8 / 8 | Có, 12/12 ảnh, khớp hậu tố FV/RV/MVL/MVR |

Số trong job lấy từ dòng "Items" của bảng Objects trên CVAT trừ 4 tag (A01: Items 15; A02, B01, B02: Items 12). Số trong file đếm bằng `road_lab.cvat_xml.load`. Hai bên khớp: export không mất nhãn.

Cả hai file `contains_images: false` (manifest `revision 1`). Lint lúc nộp: 4 nặng, 25 chính, 1 nhẹ. Cả 4 dòng nặng là QC1-02 (A05 thiếu L1 và R1; A09, A14 thiếu R1) ở ảnh không thấy vạch sơn; coach đã xác nhận miễn theo CHECKPOINTS.

## 6. Ảnh chụp màn hình
1. Tab Labels của project `d11-road-dangthuhuong`: đủ 6 label vẽ và 4 label tag.

   ![Tab Labels của project](report-img/01-labels.png)

2. Một ảnh camera trước đã gán đủ: A08, có lane, drivable direct/alternative, crosswalk, road_curb, đèn và biển, 4 tag (Items: 42).

   ![A08 đã gán đủ](report-img/02-front-A08.png)

3. Một ảnh fisheye đã gán, phóng to đoạn polyline: B04 (camera sau), lane R2 với các điểm đặt dọc theo vạch.

   ![B04 phóng to polyline](report-img/03-fisheye-B04.png)

4. Một đoạn XML có <polyline label="lane">, trích từ `dangthuhuong-front.zip`, ảnh A01:

```xml
<polyline label="lane" source="manual" occluded="0" points="0.52,601.38;176.02,549.22;266.59,523.92;328.47,507.07" z_order="0">
  <attribute name="position">L2</attribute>
  <attribute name="style">solid</attribute>
  <attribute name="color">white</attribute>
  <attribute name="kind">single</attribute>
```

## 7. Tự đánh giá
Lỗi bạn mắc nhiều nhất hôm nay (theo mã QC1-xx), và cách tránh ở Ngày 20: **QC1-05**, nhiều polyline cùng một `position` trên một ảnh (vạch đứt bị tách, hoặc vạch ngoài bị gán nhầm L1/R1 thay vì L2/R2). Lỗi kế tiếp là QC1-04: một số polyline vẽ từ xa về gần. Ở Ngày 20: trước khi vẽ, xác định vạch nào là L1/R1 rồi đặt `position` ngay sau khi vẽ xong mỗi đường; mỗi `position` chỉ một polyline; luôn đặt điểm đầu ở đáy ảnh; chạy `road_check lint` sau mỗi 3–4 ảnh thay vì cuối buổi.  
Nhãn của bạn đi vào model ở đâu? Chọn một lỗi đã sửa và nói nó làm model học sai thế nào: Ở bản export đầu, lane trên camera hai bên (B05–B08) mang `position = L1/R1` thay vì `other` (QC1-03, nặng); đã sửa. `position` là nhãn cho đầu ra "làn của xe mình" mà mô-đun giữ làn (LKA) dùng. Camera hai bên không nhìn thấy làn xe mình, nên nếu vạch ở đó mang nhãn L1/R1, model học rằng một vạch nằm ngang hông xe là biên làn đang đi. Khi chạy thật, model có thể báo xe đang lấn vạch hoặc kéo lái theo một vạch không phải biên làn của mình.

## 8. (Tùy chọn, buffer) Từ polyline đến đích row-anchor
Ảnh A01, lane `R1` (`python -m road_lab.rowanchor dangthuhuong-front.zip --image A01`):

| y | 720 | 680 | 640 | 600 | 560 |
| --- | ---: | ---: | ---: | ---: | ---: |
| x của chúng tôi | 702.5 | 648.8 | 595.2 | 540.5 | 485.4 |
| x của bạn kia | [điền] | [điền] | [điền] | [điền] | [điền] |

Trả lời 3 câu:

1. Polyline R1 của tôi đi từ y = 720 lên y ≈ 505. Dừng sớm 40 px thì điểm cuối ở y ≈ 545, vẫn phủ hàng 560, nên **không hàng nào** đổi thành "không có làn". Nếu polyline vốn dừng ở y ≈ 540–560 thì hàng 560 sẽ mất: model học rằng vạch biến mất sớm hơn thực tế.
2. [điền sau khi có lô front của bạn được kiểm]
3. Không đổi: `x_at_y` trong `geometry.py` lấy min/max của y từng đoạn nên nội suy ra cùng một x dù vẽ chiều nào. Script gặp khó là `road_check`: `lint` và `check` dựa vào điểm đầu để kiểm chiều vẽ và bắt lỗi QC1-04; `rowanchor` cũng in cảnh báo khi điểm đầu cao hơn điểm cuối. Pipeline nào lấy điểm đầu làm điểm gần xe nhất sẽ hiểu ngược làn.
