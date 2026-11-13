# Báo cáo cá nhân — PhamTrungThanh-2A202604050

Đổi tên file theo quy tắc `MSSV_HoTen.md`. Ví dụ: `20241234_NguyenVanAn.md`.

Dữ liệu: 12 ảnh người COCO val2017 (Lin et al., 2014) và 8 ảnh chân dung Wikimedia Commons, chỉ dùng trong lớp.

## 0. Thông tin học viên

- Họ tên: Phạm Trung Thành
- MSSV: 2A202604050
- Email: `26ai.thanhpt@vinuni.edu.vn`
- Link repository: `https://github.com/ptt07042005/K5-L2B-DAY04-PhamTrungThanh-2A202604050-KeypointPose`
- Phiên bản CVAT (Help → About): 2.82
- Máy: hệ điều hành Windows 11 · RAM 8 GB (Docker cấp ~6.9 GB) · chạy CVAT chế độ đầy đủ
- Bộ ảnh (`src/exports/assignment.json` do `lab.py setup` tạo): có
- Giờ bắt đầu: 16:55
- Giờ kết thúc: 19:30

## 1. Kiểm tra label (CP0)

| Label | Kiểu | Số điểm | Tên điểm đúng bảng của lab? | Đúng thứ tự? | Số cạnh |
|---|---|---:|---|---|---:|
| `person` | Skeleton | 17 | Có | Có | 19 |
| `face` | Skeleton | 13 | Có | Có | 13 |

Mẫu `person` dựng theo người quay mặt về camera (các điểm `left_*` nằm bên phải khung mẫu)? Có

Cách đổi cờ `occluded` / `outside` cho **một điểm** trên phiên bản CVAT này (một câu):

Rê chuột vào đúng điểm cần đổi trên skeleton cho điểm đó sáng lên, sau đó bấm phím `Q` để bật/tắt `occluded` (`v = 1`), bấm phím `O` để bật/tắt `outside` (`v = 0`), hoặc nhấp chuột phải vào điểm đó để chọn trên menu 4 nút.

## 2. Số lượng đã gán và thời gian (CP1, CP2)

| Nhóm ảnh | Số ảnh | Số skeleton đã gán | Số người dưới 60 px (không gán) | Số poster / gương / màn hình bỏ qua | Thời gian (phút) |
|---|---:|---:|---:|---:|---:|
| `easy` | 5 | 5 | 0 | 1 | 25 |
| `occluded` | 3 | 7 | 0 | 0 | 25 |
| `back` | 2 | 4 | 0 | 0 | 20 |
| `small` | 1 | 2 | 7 | 0 | 12 |
| `crowd` | 1 | 3 | 3 | 0 | 13 |
| `faces` | 8 | 8 | — | 0 | 25 |

- Số ảnh người đã xong ở mốc phút 105: 10 / 10
- Thời gian thực tế (phút): CP1 95 · CP2 25 · CP3 20
- Ảnh tốn thời gian nhất là ảnh nào, vì sao?

Ảnh tốn thời gian nhất là `000000167027.jpg` (nhóm `small`) và `000000157271.jpg` (nhóm `occluded`). Với `000000167027.jpg`, hai người đi bộ ở rìa trái rất nhỏ (~77–80 px) và người ngoài cùng bên trái đứng sát mép khung hình nên phải phóng to tối đa để kiểm tra ngưỡng 60 px của từng khách ngồi dưới dù vàng và xử lý các điểm sát biên trái. Với `000000157271.jpg`, hai vận động viên chồng lấn lên nhau và người áo xanh số 17 quay lưng hoàn toàn nên cần khóa (`L`) từng skeleton và nội suy vị trí khuỷu tay trái của người áo trắng bị che sau lưng người số 17.

## 3. Bảng ca khó (CP1, CP2)

Mỗi điểm bạn phân vân một dòng, nhất là phân vân giữa `v = 1` và `v = 0`, hoặc không chắc hướng người.

| Ảnh | Người / mặt | Điểm | Cờ đã chọn | Phân vân giữa | Lý do chọn |
|---|---|---|---:|---|---|
| `000000137767.jpg` | Người #1 (váy hồng) | `left_ear` | 1 | `v = 1` và `v = 0` | Tóc xõa dày và đầu hơi xoay nghiêng che tai trái, nhưng vẫn ước lượng được vị trí đối xứng qua trục mặt và gọng kính râm nên chọn `v = 1`. |
| `000000137767.jpg` | Người #1 (váy hồng) | `left_elbow` | 1 | `v = 1` và `v = 0` | Khuỷu tay trái khuất sau phần thân áo khoác hoa, nhưng nhìn thấy rõ mỏm vai trái ở trên và bàn tay trái lộ ra sát rìa phải chân váy hồng nên nội suy chắc chắn được vị trí khớp. |
| `000000157271.jpg` | Người #1 (áo trắng) | `left_elbow` | 1 | `v = 1` và `v = 0` | Cánh tay trái vươn qua sau lưng vận động viên áo xanh số 17 làm khuỷu tay bị che hoàn toàn, nhưng thấy rõ vai trái và bàn tay trái chạm đĩa bay trắng nên ước lượng chắc vị trí (`v = 1`). |
| `000000167027.jpg` | Người #1 (bên trái ngoài cùng) | `left_elbow`, `left_wrist` | 0 | `v = 0` và `v = 1` | Người đi bộ sát rìa trái ảnh làm cánh tay trái và cổ tay trái khuất ra ngoài mép trái khung hình, nên bật `outside` (`v = 0`) thay vì kéo điểm về mép ảnh. |
| `000000205650.jpg` | Người #1 (nhân viên tàu) | `left_elbow` | 1 | `v = 1` và `v = 2` | Ống tay áo vest đen bên trái gập lại nằm chìm vào vùng tối của vách cửa toa tàu, nhưng thấy rõ vai trái và bàn tay trái cầm kính trước ngực nên đặt `v = 1`. |
| `000000461389.jpg` | Người #3 (quần đùi xanh) | `left_hip`, `right_hip` | 2 | `v = 2` và `v = 0` | Phần thân trên bị cắt ngang sát mép trên bức ảnh, nhưng phần hông quần đùi xanh đen và đầu đùi vẫn nằm trong khung ảnh (cách mép trên vài pixel) nên giữ `v = 2`. |
| `face_146049.jpg` | Mặt #1 (mũ cói, kính râm) | `left_eye_*`, `right_eye_*` (8 điểm mắt) | 1 | `v = 1` và `v = 0` | Đeo kính râm tối màu che mắt, theo quy tắc mục 4 của Guideline đặt ước lượng sau tròng kính theo gọng và bật `occluded` (`v = 1`) cho cả 8 điểm mắt. |
| `face_333134.jpg` | Mặt #1 (cô gái tóc vàng) | `right_eye_inner` | 1 | `v = 1` và `v = 2` | Lọn tóc mái màu vàng rủ chéo qua sống mũi che phủ góc khóe mắt trong bên phải, nhưng ước lượng chắc chắn vị trí từ đường mí mắt và sống mũi nên bật `occluded` (`v = 1`). |

## 4. Bảng tự review (CP3)

Đi từ ảnh cuối về ảnh đầu. 5 ảnh người (ít nhất 2 ảnh `back` hoặc `crowd`) và 3 ảnh mặt. Ít nhất 8 dòng, kể cả dòng `ok`. Ghi lỗi trước, sửa sau.

Mã lỗi dùng đúng một trong: `lr_swap`, `vis_flag`, `position`, `missing`, `extra`, `ok`.

| Ảnh | Người / mặt | Điểm | Mã lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|---|
| `000000381889.jpg` | Người #1, #2, #3 (nhóm `crowd`) | Toàn bộ 17 điểm | `ok` | Đã kiểm tra 3 người chính cao trên 60 px, tách riêng từng skeleton không để xương vắt sang người khác, các phần khuất dưới bàn bánh kem đã bật `outside`. | Không cần sửa |
| `000000167027.jpg` | Người #1 (nhóm `small`, đi bộ bên trái) | `left_shoulder`, `left_knee` | `position` | Lúc đầu kéo `left_shoulder` tràn ra ngoài mép trái ảnh (`x = -0`, báo lỗi) và `left_knee` sát mép (`x = 1`); đã kéo `left_shoulder` về `x = 1` và `left_knee` vào giữa bắp chân trái. | Có |
| `000000351176.jpg` | Người #1 (trọng tài), #2 (nữ VĐV nhóm `back`) | Toàn bộ 17 điểm | `ok` | Nữ VĐV quay lưng đã đảo `left_*` sang bên trái ảnh và tắt `outside` cho mũi + 2 mắt; trọng tài ngồi ghế cao quay mặt giữ đúng trái/phải. | Không cần sửa |
| `000000210604.jpg` | Người #1, #2 (nhóm `back`, nhảy bắt đĩa) | Toàn bộ 17 điểm | `ok` | Người mặc áo bib xanh lá quay lưng đã đảo `left_*` sang bên trái ảnh, người áo trắng quay mặt giữ đúng trái/phải, hai chân không bắt chéo. | Không cần sửa |
| `000000157271.jpg` | Người #1 (áo trắng, nhóm `occluded`) | `nose`, `right_eye`, `left_ankle`, `right_ankle` | `position` | Lúc đầu đặt `nose` và `right_eye` hơi lệch sang cánh tay người số 17 và 2 điểm `ankle` hơi thấp xuống thân giày; đã kéo lại về đúng chóp mũi, hốc mắt và tâm mắt cá chân. | Có |
| `face_333134.jpg` | Mặt #1 (nhóm tóc che) | `left_eye_upper`, `right_eye_upper` | `position` | Hạ điểm mí trên sát xuống đường chân mi đang cụp nhìn xuống để phản ánh đúng độ mở mắt nhỏ. | Có |
| `face_146049.jpg` | Mặt #1 (nhóm kính râm) | 8 điểm mắt (`left_eye_*`, `right_eye_*`) | `vis_flag` | Lúc xuất lần đầu quên lưu cờ `occluded` (`v = 1`) cho 8 điểm mắt sau kính râm tối; đã bật `Q` (`v = 1`) cho cả 8 điểm mắt và lưu lại. | Có |
| `face_81239.jpg` | Mặt #1 (nhóm kính trong) | Toàn bộ 13 điểm | `ok` | Kính cận trong suốt nhìn rõ mắt, đủ 13 điểm `v = 2`, mí trên và mí dưới đặt đúng mép mí. | Không cần sửa |

Tổng hợp số lỗi theo mã:

| `lr_swap` | `vis_flag` | `position` | `missing` | `extra` | `ok` |
|---:|---:|---:|---:|---:|---:|
| 0 | 1 | 3 | 0 | 0 | 4 |

Điểm còn phân vân giữa `v = 1` và `v = 0` (ghi cả hai lựa chọn và lý do chọn):

- Ở ảnh `000000157271.jpg` (Người #1 áo trắng): Điểm `left_elbow` nằm khuất hoàn toàn sau lưng áo xanh số 17. Hai lựa chọn là `v = 1` (`occluded`) hoặc `v = 0` (`outside`). Em chọn `v = 1` (`occluded`) vì thấy rõ vai trái và cổ tay/bàn tay trái ngay sát chiếc đĩa bay trắng nên nội suy chắc chắn được vị trí khuỷu tay trên đường thẳng nối hai khớp.
- Ở ảnh `000000167027.jpg` (Người #1 đi bộ sát mép trái): Điểm `left_elbow` và `left_wrist` ở cánh tay trái bị khuất sát/ngoài biên trái. Hai lựa chọn là `v = 1` (`occluded`) hoặc `v = 0` (`outside`). Em chọn `v = 0` (`outside`) vì cánh tay trái đã bị cắt khuất ở rìa trái khung hình, không kéo điểm ngoài ảnh về nằm trên mép ảnh.

## 5. Kiểm file xuất và kết quả tự kiểm (CP4)

Tự kiểm tay trong file JSON:

| File | Người / mặt kiểm (ảnh, thứ tự) | Số phần tử `keypoints` | Điểm `outside` ra `v = 0`, `occluded` ra `v = 1`? | `num_keypoints` khớp? |
|---|---|---:|---|---|
| `PhamTrungThanh_persons.zip` | `000000137767.jpg`, người #1 | 51 | Có | Có |
| `PhamTrungThanh_persons.zip` | `000000157271.jpg`, người #2 (áo xanh #17) | 51 | Có | Có |
| `PhamTrungThanh_faces.zip` | `face_146049.jpg`, mặt #1 | 39 | Có | Có |

Sau khi xuất, CVAT ghi `occluded` thành `v = 1` và `outside` thành `v = 0`. Tọa độ của điểm `outside` trong file là `0, 0` hay tọa độ cũ? Tọa độ cũ

Hai job đã chuyển sang completed trên CVAT? Có

Dán phần **TÓM TẮT** in ra từ `src/check_submission.py`:

```text
----- TÓM TẮT (dán vào báo cáo mục 5) -----
- File person: PhamTrungThanh_persons.zip
  Số ảnh: 12 · số skeleton `person`: 21 · mỗi annotation 51 số
  Skeleton theo ảnh: 000000137767=1, 000000157271=2, 000000167027=2, 000000205650=1, 000000210604=2, 000000261732=1, 000000308257=1, 000000351176=2, 000000381889=3, 000000390585=1, 000000461389=3, 000000469061=2
  Cờ v: v=2 281 (79%) · v=1 11 (3%) · v=0 65 (18%)
  num_keypoints trung bình: 13.9
- File face: PhamTrungThanh_faces.zip
  Số ảnh: 8 · số skeleton `face`: 8 · mỗi annotation 39 số
  Skeleton theo ảnh: face_105668=1, face_146049=1, face_147624=1, face_333134=1, face_421231=1, face_449454=1, face_496353=1, face_81239=1
  Cờ v: v=2 96 (92%) · v=1 8 (8%) · v=0 0 (0%)
  num_keypoints trung bình: 13.0
- Ghi chú: 65 điểm v = 0 vẫn mang tọa độ cũ (CVAT 2.x xuất như vậy; chuẩn COCO ghi 0, 0). Không phải lỗi: điểm v = 0 bị bỏ qua khi huấn luyện và khi chấm.
- Số [LỖI]: 0 · số [CẢNH BÁO]: 3
Kết quả: ĐẠT (đọc lại từng cảnh báo ở trên)
```

Các `[CẢNH BÁO]` còn giữ nguyên và lý do:

- Giữ nguyên `[CẢNH BÁO] 000000167027.jpg #1: left_ear v = 2 nằm sát mép ảnh (1, 270)` và `left_shoulder v = 2 nằm sát mép ảnh (1, 280)`: Người đi bộ số 1 trong ảnh `000000167027.jpg` thực tế đang bước đi sát rìa vỉa hè ở ngay biên trái của bức ảnh (`x = 1`), phần vành tai trái và mỏm vai áo đen bên trái vẫn còn nhìn thấy ngay sát mép trái khung hình nên giữ `v = 2`.
- Giữ nguyên `[CẢNH BÁO] 000000308257.jpg #1: cặp ankle nằm ngược phía so với các cặp còn lại`: Trong ảnh `000000308257.jpg`, cô gái đang ngồi khoanh chân (bắt chéo hai cẳng chân/bàn chân thật) trên bậc đá để dùng laptop. Do đó, từ đầu gối phải (`right_knee` ở bên trái ảnh) cẳng chân vắt chéo sang bàn chân phải (`right_ankle` ở bên phải ảnh), và từ đầu gối trái (`left_knee` ở bên phải ảnh) cẳng chân gập luồn sang cổ chân trái (`left_ankle` ở bên trái ảnh). Đây là tư thế bắt chéo chân thật trong ảnh chứ không phải lỗi đổi trái/phải (`lr_swap`).

## 6. Ảnh chụp màn hình

1. Label `person` trong label constructor (đủ 17 điểm có tên):

   

2. Một người quay lưng đã gán, cờ đã đặt:

   

3. Một khuôn mặt đã gán 13 điểm, phóng to vùng mắt:

   

4. Một đoạn JSON xuất của một người:

   

Nguồn ảnh: COCO val2017 / Wikimedia Commons.

## 7. Tự đánh giá

1. Lỗi bạn mắc nhiều nhất hôm nay, và cách bạn sẽ tránh ở dự án thật:

   Lỗi dễ mắc nhất là đặt điểm của người đứng sát biên ảnh chạm ra ngoài mép ảnh (`x = -0` ở `000000167027.jpg`) và quên lưu cờ `occluded` (`v = 1`) cho các điểm mắt nằm sau tròng kính râm tối (`face_146049.jpg`). Ở dự án thật, em sẽ luôn bật hiển thị tên điểm, phóng to tối đa ở vùng biên ảnh để kiểm tra tọa độ không bị tràn ra ngoài khung hình, và thực hiện bước self-QC rà soát cờ `v` theo 4 câu hỏi trước khi bấm `Ctrl+S`.

2. Một quy tắc bạn đề xuất thêm vào guideline (kèm ảnh ví dụ nếu có):

   Đề xuất bổ sung quy tắc cụ thể cho trường hợp **mắt nhắm hoặc nhìn cụp sát xuống** trong bài toán 13 điểm khuôn mặt (ví dụ ảnh `face_333134.jpg`): hướng dẫn rõ việc kéo điểm mí trên (`*_eye_upper`) hạ sát xuống đường chân mi trên (cách điểm mí dưới `*_eye_lower` vài pixel theo đúng khe mắt đang khép lại, tuyệt đối không đặt lên nếp gấp mí mắt hay bầu mắt) để đảm bảo công thức tính tỉ số mở mắt (EAR) cho hệ thống giám sát tài xế phản ánh đúng trạng thái mắt nhắm/cụp.

3. Nhãn của bạn đi vào model ở đâu? Chọn một lỗi bạn đã sửa và nói nó sẽ làm model học sai thế nào (heatmap, trọng số trong loss, hay bước lật ảnh):

   Nhãn keypoint đi vào model ở 3 nơi: (1) tọa độ `(x, y)` của các điểm `v > 0` được dùng để sinh mặt phẳng đích **Gaussian heatmap** (với độ rộng đỉnh phụ thuộc vào $\sigma_k$), (2) cờ visibility `v` quyết định **trọng số trong hàm loss** (`v = 0` có trọng số 0 nên không tính vào loss; `v = 1` và `v = 2` có trọng số 1 nên model bị phạt nếu đoán lệch), và (3) các cặp `flip_pairs` được dùng để **đổi tên trái/phải khi lật ngang ảnh (horizontal flip)** lúc tăng cường dữ liệu. Với lỗi thực tế em đã sửa ở ảnh `000000157271.jpg` (lúc đầu đặt lệch vị trí `nose` và `right_eye` của người áo trắng sang cánh tay người số 17), vì mắt và mũi có $\sigma$ rất nhỏ (`0.025`–`0.026`), nếu không sửa thì đỉnh Gaussian heatmap sinh ra sẽ nằm sai lệch sang vùng cánh tay của người khác, làm model học sai đặc trưng thị giác của vùng mặt và bị sụt giảm mạnh điểm OKS.

## 8. Khó khăn và lỗi kỹ thuật

- Hệ thống Docker và CVAT 2.82 hoạt động ổn định trong suốt buổi thực hành, không xảy ra lỗi mất kết nối hay mất dữ liệu.
- Ban đầu khi thao tác tắt điểm trên CVAT còn dễ nhầm giữa phím chữ `O` (`outside`) và số `0`, hoặc dễ kéo điểm ở rìa ảnh chạm ra ngoài biên âm (`-0`), nhưng đã phát hiện kịp thời nhờ script `src/check_submission.py` và sửa triệt để.

## 9. Tài liệu tham khảo và công cụ AI đã dùng

- Tài liệu của lớp: `docs/GUIDELINE.md`, `docs/CVAT_CHEATSHEET.md`, `docs/COCO_KEYPOINTS.md`, `CHECKPOINTS.md`, `RUBRIC.md`.
- Bộ dữ liệu: Microsoft COCO Keypoints (Lin et al., 2014) và Wikimedia Commons.
- Công cụ AI đã dùng: Sử dụng trợ lý AI (Gemini) để tra cứu quy tắc đặt cờ visibility theo Guideline, hỏi thao tác phím tắt trên CVAT 2.82 và giải thích thông báo lỗi/cảnh báo khi chạy `src/check_submission.py`.