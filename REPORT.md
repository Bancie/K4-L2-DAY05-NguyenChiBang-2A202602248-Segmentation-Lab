# Báo cáo Day 5 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên VLearn. Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202602248
- Ngày / CVAT local: 17/09/2026, CVAT local lớp
- Công cụ đã dùng: Brush, Polygon, gợi ý tự động (SAM / AI Tools)

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
| cp5_occlusion | cp5_occlusion.zip | 1 / 1 | 3 |
| cp3_thin | cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | cp6_coverage.zip | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

Không gặp lỗi export. Đã Save trên CVAT rồi xuất: semantic = Segmentation mask 1.1; instance/panoptic = COCO 1.0.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: `000000181542.jpg` — xe máy bên trái, bị tà áo dài trắng của người phụ nữ đi bộ giữa đường cắt làm hai mảng.
- Class và quy tắc tôi dùng để chọn biên: class `motorcycle`. Chỉ tô phần thân/bánh còn nhìn thấy; chỗ áo che thì dừng mask, không đoán phần xe phía sau người. Đầu xe và đuôi xe cùng một instance.
- Nếu dùng gợi ý sau đó: không dùng cho object đầu này.
- Nếu không dùng gợi ý: không dùng (Polygon + Merge). Object khác mới dùng SAM; khi SAM tràn nền hoặc gán nhầm class thì xóa/sửa, không giữ nguyên đề xuất.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: `medium_instance` / `000000181542.jpg` / xe máy bên trái bị người đi bộ cắt làm hai mảng.
- Lỗi thuộc loại: sai lớp / thiếu-thừa vật / gộp-tách / biên / phủ vùng / khác: gộp-tách
- Bằng chứng tôi nhìn thấy: CVAT lúc đầu có hai object `motorcycle` riêng (đầu xe và đuôi xe); dialog Propagation hiện ra — đó là copy sang frame khác, không phải gộp hai mảnh.
- Quy tắc và hành động sửa: một vật bị che vẫn là một instance; chỉ vẽ phần nhìn thấy. Chọn cả hai object, Merge (M), không tô xuyên người.
- Sau sửa đã Save và export lại chưa? Có. ZIP hiện tại là bản sau khi Merge.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): chạy scorer local với `tiers_gt` của coach, **không có điểm trước/sau riêng cho lần Merge**. Bản ZIP hiện tại: `medium_instance` **18.9 / 32** (mean matched IoU 0.801, R@0.5 0.83, submitted 81 vs GT 71, FP 22 / FN 12). `easy_semantic` **20 / 20** (mIoU 0.867; `sidewalk` IoU 0.710 thấp nhất). `hard_panoptic` **6.1 / 30** (PQ 0.292; `sidewalk`/`bus`/`bicycle`/`traffic light` PQ 0). Tổng ba tier **45.0 / 82**. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. `medium_instance` / `000000181542.jpg` — xe máy trái bị tà áo dài cắt đôi | Tách 2 object motorcycle, hoặc 1 instance với 2 mảng rời | Vật bị che vẫn một instance; chỉ vẽ phần nhìn thấy | Giữ 1 `motorcycle`, Merge hai mảng, không tô xuyên người |
| 2. `hard_panoptic` / `000000460147.jpg` — xe trên thùng xe vận chuyển giữa đường | Gộp hết vào mask `truck`, hoặc từng xe trên thùng là `car` riêng | Thing đếm được phải tách instance; chỉ phần nhìn thấy | Xe nâng/chở = `truck`; mỗi xe trên thùng = `car` riêng. Có nên bỏ xe quá nhỏ/mờ phía xa không? |
| 3. `cp1_holes` / `000000144300.jpg` — chống chữ L đỏ và nhà kéo Eriba phía sau Honda | Gán chống xe / caravan thành `motorcycle`, hoặc bỏ vì không có class | Task chỉ có person/bicycle/car/motorcycle/bus/truck; kính/khe nằm trong mask xe, không khoét | Không tạo object cho chống xe; không nhận SAM tô caravan thành `motorcycle`. Van xanh đậm = `car`. Kính chắn gió Honda nằm trong mask xe máy |
