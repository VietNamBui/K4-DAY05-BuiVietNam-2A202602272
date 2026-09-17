# Báo cáo Day 5 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên VLearn. Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202602272
- Ngày / CVAT local: 17/09/2026 / http://localhost:8080
- Công cụ đã dùng: Brush, Polygon, Intelligent Scissors (OpenCV Tools)

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh VLearn đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | cp2_slice.zip | 1 / 1 | 3 |
| cp5_occlusion | chưa có | 0 / 1 | 3 |
| cp3_thin | chưa có | 0 / 1 | 3 |
| cp4_curb | cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | chưa có | 0 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Ảnh `000000181542.jpg`, người phụ nữ mặc áo dài trắng ở chính giữa ảnh đang đi bộ qua vạch sang đường.
- Class và quy tắc tôi dùng để chọn biên: Class `person`. Quy tắc biên: Tôi chỉ vẽ theo phần cơ thể và tà áo dài thật sự nhìn thấy; tà áo dài bay sát mép nào thì tôi bám sát mép đó. Các phần bị thân xe máy và bánh xe phía sau che khuất thì tôi dừng đường biên ngay tại mép vật che, tuyệt đối không tự đoán mò đường viền bị che khuất phía sau.
- Nếu dùng gợi ý sau đó: vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: Sau đó tôi có thử dùng Intelligent Scissors (cây kéo OpenCV Tools). Công cụ này bắt đường viền tương phản khá nhạy ở thân xe và người, nhưng đôi khi bị "hít nhầm" vào bóng râm/bóng đổ đen dưới mặt đường. Tôi đã dùng chuột chỉnh lại các điểm neo (anchor points) để cắt bỏ phần bóng râm trước khi hoàn thành mask.
- Nếu không dùng gợi ý: (Đã ghi nhận ở trên, tôi có kết hợp Polygon và Intelligent Scissors).

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: Task `hard_panoptic`, ảnh 1 (`000000350023.jpg`) và ảnh 2 (`000000460147.jpg`), vùng `sky` và `road`.
- Lỗi thuộc loại: sai lớp / thiếu-thừa vật / gộp-tách / biên / phủ vùng / khác: khác (lỗi dùng nhầm công cụ Rectangle / bounding box thay vì Polygon/Brush để tạo mask).
- Bằng chứng tôi nhìn thấy: Khi xuất COCO 1.0 lần đầu và chạy lệnh kiểm tra `python scripts/inspect_submissions.py --dir submissions`, hệ thống báo lỗi: `annotation 1, 2, 24, 25 thiếu polygon/RLE hợp lệ`. Khi kiểm tra file JSON bên trong ZIP thì thấy 4 đối tượng này có trường `"segmentation": []` rỗng vì đã dùng công cụ hình chữ nhật.
- Quy tắc và hành động sửa: Quy tắc của segmentation là mask phải chứa pixel phân đoạn thực sự, bounding box không thể thay thế mask. Tôi đã mở lại task `hard_panoptic` trên CVAT, xóa 4 bounding box chữ nhật đó đi và dùng công cụ Polygon khoanh lại chuẩn xác vùng `sky` và `road` trên cả 2 ảnh.
- Sau sửa đã Save và export lại chưa? Đã Save và export lại file `COCO 1.0` thành `hard_panoptic.zip`. Chạy lại `inspect_submissions.py` báo `[OK]` với 45 annotations hợp lệ.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): Sau khi sửa lỗi trên, script `inspect_submissions.py` đã báo `[OK]` hoàn toàn cho cả 3 tier chính và 3 checkpoint (`easy_semantic`, `medium_instance`, `hard_panoptic`, `cp1_holes`, `cp2_slice`, `cp4_curb`). Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. Ảnh 1 `easy_semantic` (`7ee6d192-89e2408b.jpg`), dòng xe và các đốm đèn xe li ti ở xa trên dốc cao tốc. | (1) Phải khoét chừa các đốm đèn xe ra; hay (2) Phủ nhãn `road` qua luôn. | Danh sách nhãn task Easy chỉ có 5 class (`road`, `sidewalk`, `building`, `vegetation`, `sky`), không có class xe/đèn xe. Các vật này ở quá xa (chỉ vài pixel ở chân trời). | Quyết định: Phủ nhãn `road` qua vùng này mà không khoét từng đốm đèn, tuân thủ quy tắc không tự bịa thêm class ngoài `classes.json`. |
| 2. Ảnh 2 `easy_semantic` (`817bca71-00000000.jpg`), dãy nhà liền kề có cây cối và mái che xen kẽ. | (1) Nối tất cả các nhà thành 1 đa giác khổng lồ duy nhất; hay (2) Chia nhỏ vẽ từng khối nhà riêng. | Bài semantic chỉ quan tâm loại pixel (`building`) chứ không đếm số lượng nhà. Tuy nhiên nếu nối 1 đường duy nhất sẽ rất dễ nuốt nhầm các tán cây xanh (`vegetation`) ở giữa. | Quyết định: Vẽ tách thành các khối nhà nhỏ riêng biệt (đều gán nhãn `building`). Khi export `Segmentation mask 1.1`, hệ thống sẽ tự động gộp thành 1 lớp mà không làm lẹm vào cây. |
| 3. Ảnh 1 `hard_panoptic` (`000000350023.jpg`), cụm nhiều xe ô tô đang chạy trên đường cao tốc. | (1) Dùng cọ tô chung tất cả các xe vào 1 mask; hay (2) Tô riêng từng chiếc xe (mỗi xe 1 mask, 1 ID riêng). | Trong Panoptic, `car` là "thing" (vật thể đếm được), còn `road` là "stuff" (nền). Hệ thống chấm Panoptic Quality (PQ) so khớp từng instance cá thể với IoU > 0.5. | Quyết định: Bắt buộc vẽ từng chiếc xe là 1 mask riêng biệt (kết thúc mỗi xe rồi mới vẽ xe tiếp theo). Nếu gộp chung sẽ chỉ tạo ra 1 mask xe dị dạng và bị 0 điểm class `car`. |
