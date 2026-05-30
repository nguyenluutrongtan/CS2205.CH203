# ĐỀ CƯƠNG NGHIÊN CỨU

## Tóm tắt

Đề tài này đề xuất xây dựng một quy trình đánh giá để kiểm tra tính nhất quán hình học 3D của các mô hình ước lượng độ sâu từ video. Vấn đề chính được đặt ra là: một video độ sâu có thể nhìn mượt giữa các khung hình, nhưng chưa chắc đã đúng về mặt hình học khi camera di chuyển.

Trong đề tài này, VGGT không được dùng như một mô hình tạo độ sâu chuẩn để chấm trực tiếp kết quả của mô hình khác. Thay vào đó, VGGT được dùng như một nguồn thông tin hình học hỗ trợ, vì mô hình này có thể cung cấp nhiều tín hiệu cùng lúc như vị trí camera, tham số camera, các điểm được theo dõi qua nhiều khung hình và độ tin cậy của các điểm đó. Các tín hiệu này giúp kiểm tra xem bản đồ độ sâu do một mô hình khác tạo ra có tạo thành cấu trúc 3D ổn định qua thời gian hay không.

Ví dụ, nếu cùng một điểm trên vật thể được nhìn thấy trong nhiều khung hình, khi dùng độ sâu dự đoán để đưa điểm đó vào không gian 3D, vị trí 3D của điểm này cần tương đối ổn định sau khi quy về cùng một hệ tọa độ. Nếu vị trí 3D thay đổi quá nhiều, điều đó cho thấy bản đồ độ sâu có thể chưa nhất quán theo thời gian hoặc chưa đúng về mặt hình học. Để tránh phụ thuộc quá mức vào VGGT, đề tài sẽ kiểm chứng bằng các lỗi được tạo có kiểm soát, dữ liệu có độ sâu thật nếu có, và các hình minh họa trực quan.

## 1. Tên đề tài

**Tên tiếng Việt:** Đánh giá và hiệu chỉnh tính nhất quán hình học 3D cho ước lượng độ sâu video bằng VGGT

**Tên tiếng Anh:** VGGT-Guided Evaluation and Test-Time Adaptation for 3D-Consistent Video Depth Estimation

## 2. Giới thiệu

Ước lượng độ sâu từ video là một bài toán quan trọng trong thị giác máy tính. Bài toán này có thể được ứng dụng trong robot, xe tự hành, thực tế tăng cường, dựng lại không gian 3D và định vị camera. Với mỗi khung hình trong video, mô hình cần dự đoán khoảng cách từ camera đến các điểm trong cảnh.

Các mô hình gần đây như Depth Anything, Depth Anything V2 và Video Depth Anything đã cho kết quả rất tốt về mặt hình ảnh. Bản đồ độ sâu được tạo ra thường rõ ràng hơn, ít nhiễu hơn và ổn định hơn qua thời gian. Tuy nhiên, ổn định về mặt hình ảnh chưa đồng nghĩa với đúng về mặt hình học. Một chuỗi bản đồ độ sâu có thể nhìn mượt nhưng vẫn bị sai tỉ lệ, bị lệch khi camera di chuyển, hoặc làm cùng một vật thể bị biến dạng trong không gian 3D.

![Minh họa Video Depth Anything](assets/proposal/video_depth_anything_teaser.png)

*Hình 1. Minh họa kết quả của Video Depth Anything trên video dài. Mô hình hướng tới việc tạo bản đồ độ sâu ổn định theo thời gian. Nguồn: trang dự án Video Depth Anything.*

Các cách đánh giá phổ biến như sai số độ sâu trung bình hoặc độ mượt theo thời gian thường chưa đủ để phát hiện các lỗi này. Trong nhiều trường hợp, dữ liệu thật về độ sâu và camera cũng không có sẵn đầy đủ, làm cho việc đánh giá càng khó hơn.

VGGT là một mô hình thị giác 3D có khả năng ước lượng nhiều thông tin hình học từ ảnh hoặc video. Ngoài bản đồ độ sâu, VGGT còn có thể cung cấp thông tin về camera và các điểm được theo dõi qua nhiều khung hình. Vì vậy, đề tài đề xuất dùng các thông tin hình học này để xây dựng cách đánh giá nhằm phát hiện các lỗi nhất quán 3D trong kết quả ước lượng độ sâu video.

![Kiến trúc VGGT](assets/proposal/vggt_architecture.png)

*Hình 2. Kiến trúc tổng quát của VGGT. Mô hình nhận nhiều ảnh đầu vào và dự đoán đồng thời thông tin camera, bản đồ độ sâu, bản đồ điểm 3D và điểm theo dõi. Đây là lý do đề tài dùng VGGT như nguồn thông tin hình học hỗ trợ, thay vì dùng VGGT như dữ liệu đúng tuyệt đối. Nguồn: trang dự án VGGT.*

Đề tài được xây dựng ở mức đề cương nghiên cứu. Trọng tâm là thiết kế cách đánh giá trên các bộ dữ liệu có sẵn, cách tạo lỗi có kiểm soát để kiểm chứng các chỉ số đánh giá và cách hiệu chỉnh nhẹ kết quả ở thời điểm kiểm thử. Đề tài không đặt mục tiêu tạo một bộ dữ liệu chuẩn mới.

## 3. Đầu vào và đầu ra của đề tài

### 3.1. Đầu vào

Đầu vào dự kiến gồm:

- Một đoạn video ngắn, khoảng 8 đến 64 khung hình.
- Ảnh màu của từng khung hình.
- Bản đồ độ sâu do mô hình cần đánh giá tạo ra, ví dụ Video Depth Anything hoặc Depth Anything V2.
- Thông tin hình học do VGGT cung cấp, gồm vị trí camera, tham số camera, các điểm được theo dõi qua nhiều khung hình và độ tin cậy nếu có.
- Độ sâu thật hoặc vị trí camera thật nếu bộ dữ liệu có cung cấp, ví dụ KITTI, Sintel, TUM RGB-D hoặc ScanNet.

### 3.2. Đầu ra

Đầu ra dự kiến gồm:

- Một nhóm chỉ số để đánh giá tính nhất quán hình học 3D của video độ sâu.
- Bảng so sánh giữa bản đồ độ sâu ban đầu và các bản đồ độ sâu đã được tạo lỗi có kiểm soát.
- Biểu đồ thể hiện sai số tăng hay giảm theo mức độ lỗi.
- Hình minh họa trực quan, ví dụ vẽ điểm theo dõi thật và điểm được chiếu lại lên ảnh.
- Một chương trình hoặc sổ tay thử nghiệm nhỏ nếu cần minh họa tính khả thi.

## 4. Mục tiêu nghiên cứu

Mục tiêu tổng quát của đề tài là xây dựng một quy trình đánh giá và hiệu chỉnh nhẹ để xem kết quả ước lượng độ sâu từ video có nhất quán trong không gian 3D hay không.

Các mục tiêu cụ thể gồm:

1. Nghiên cứu cách dùng thông tin hình học từ VGGT để hỗ trợ đánh giá độ sâu video.
2. Đề xuất các chỉ số đánh giá dựa trên điểm theo dõi, vị trí camera và sai số chiếu lại lên ảnh.
3. Xây dựng cách tạo lỗi có kiểm soát trên bản đồ độ sâu, ví dụ làm trôi tỉ lệ, tạo nhiễu theo thời gian, làm trễ khung hình hoặc làm méo cục bộ.
4. Kiểm tra xem chỉ số đánh giá đề xuất có phân biệt được độ sâu ban đầu và độ sâu đã bị tạo lỗi hay không.
5. Đánh giá sơ bộ mối liên hệ giữa chỉ số đánh giá đề xuất và các sai số truyền thống trên bộ dữ liệu có thông tin thật.
6. Nghiên cứu khả năng dùng các chỉ số đánh giá hình học làm tín hiệu để hiệu chỉnh kết quả độ sâu ở thời điểm kiểm thử mà không cần huấn luyện lại toàn bộ mô hình.

## 5. Câu hỏi nghiên cứu

Đề tài tập trung trả lời ba câu hỏi:

**Câu hỏi 1:** Các chỉ số đánh giá dựa trên thông tin hình học từ VGGT có phát hiện được lỗi không nhất quán 3D trong video độ sâu hay không?

**Câu hỏi 2:** Khi mức độ lỗi được tạo ra tăng dần, giá trị sai số của chỉ số đánh giá đề xuất có tăng tương ứng hay không?

**Câu hỏi 3:** Cách đánh giá hình học 3D có phát hiện được những lỗi mà cách đo độ mượt theo thời gian chưa phát hiện được hay không?

## 6. Nội dung và phương pháp nghiên cứu

### 6.1. Tổng quan phương pháp đề xuất

![Sơ đồ phương pháp đề xuất](assets/proposal/proposal_pipeline_diagram.png)

*Hình 3. Sơ đồ tổng quát của đề tài. Video đầu vào được đưa qua mô hình ước lượng độ sâu và VGGT. Độ sâu dự đoán được đánh giá bằng các thông tin hình học do VGGT cung cấp, sau đó có thể được hiệu chỉnh nhẹ ở thời điểm kiểm thử.*

Sơ đồ trên thể hiện vai trò khác nhau của hai nhánh xử lý. Mô hình như Video Depth Anything hoặc Depth Anything V2 được dùng để tạo bản đồ độ sâu cần đánh giá. VGGT được dùng để cung cấp thông tin hình học hỗ trợ như camera, điểm theo dõi và độ tin cậy. Đề tài không dùng VGGT để thay thế dữ liệu đúng tuyệt đối, mà dùng các thông tin hình học này để kiểm tra tính nhất quán của kết quả độ sâu.

### 6.2. Khảo sát tài liệu và công cụ liên quan

Các công trình liên quan sẽ được khảo sát theo ba nhóm chính.

Nhóm thứ nhất là các mô hình ước lượng độ sâu, gồm Depth Anything, Depth Anything V2 và Video Depth Anything. Depth Anything và Depth Anything V2 là các mô hình mạnh cho ước lượng độ sâu từ ảnh đơn, có thể được dùng như đường cơ sở khi xử lý từng khung hình độc lập. Video Depth Anything phù hợp hơn với bối cảnh của đề tài vì mô hình này hướng trực tiếp đến độ sâu video ổn định theo thời gian. Các mô hình này sẽ được xem là nguồn tạo bản đồ độ sâu cần đánh giá.

Nhóm thứ hai là các mô hình và phương pháp hình học 3D từ ảnh hoặc video, gồm VGGT và MegaSaM. VGGT đặc biệt quan trọng trong đề tài vì có thể cung cấp nhiều thông tin hình học cùng lúc, như camera, bản đồ điểm 3D, bản đồ độ sâu và điểm theo dõi. MegaSaM là một hướng liên quan về ước lượng cấu trúc 3D và chuyển động camera từ video thực tế, giúp đặt đề tài vào bối cảnh rộng hơn của bài toán dựng hình học 3D từ video. Nhóm công trình này giúp xác định cách dùng thông tin hình học để đánh giá độ sâu video.

Nhóm thứ ba là các bộ dữ liệu đánh giá, gồm KITTI, Sintel, TUM RGB-D và ScanNet. KITTI phù hợp với cảnh ngoài trời và bài toán xe tự hành; Sintel có dữ liệu tổng hợp với chuyển động rõ, phù hợp để kiểm tra lỗi hình học có kiểm soát. TUM RGB-D và ScanNet phù hợp với cảnh trong nhà, có thông tin RGB-D và camera pose hữu ích cho đánh giá. Các bộ dữ liệu này được dùng để chọn các đoạn video ngắn phục vụ thử nghiệm, không nhằm tạo một bộ dữ liệu chuẩn mới.

Kết quả của bước khảo sát là xác định mô hình nào được dùng để tạo bản đồ độ sâu, VGGT sẽ cung cấp những thông tin hình học nào, và bộ dữ liệu nào phù hợp cho thử nghiệm ban đầu.

### 6.3. Chuẩn bị dữ liệu thử nghiệm

Một số đoạn video ngắn sẽ được chọn từ các bộ dữ liệu có sẵn. Mỗi đoạn video cần có chuyển động camera hoặc thay đổi góc nhìn đủ rõ để kiểm tra tính nhất quán hình học. Ở giai đoạn đầu, số lượng video có thể nhỏ, khoảng 5 đến 10 đoạn, nhằm ưu tiên kiểm tra quy trình nghiên cứu.

Dữ liệu trung gian dự kiến được lưu theo một cấu trúc thống nhất:

| Thành phần | Ý nghĩa |
| :--- | :--- |
| `frames` | Ảnh màu theo thời gian |
| `depth` | Bản đồ độ sâu của mô hình cần đánh giá |
| `K` | Tham số camera |
| `pose` | Vị trí và hướng camera theo thời gian |
| `tracks_xy` | Tọa độ các điểm được theo dõi |
| `tracks_valid` | Điểm có tồn tại ở khung hình tương ứng hay không |
| `tracks_conf` | Độ tin cậy của điểm nếu có |

### 6.4. Tạo lỗi có kiểm soát

Từ bản đồ độ sâu ban đầu, đề tài sẽ tạo thêm các phiên bản bị lỗi để kiểm tra độ nhạy của chỉ số đánh giá. Các loại lỗi dự kiến gồm:

- **Trôi tỉ lệ:** nhân độ sâu ở mỗi khung hình với một hệ số thay đổi theo thời gian.
- **Nhiễu theo thời gian:** thêm nhiễu làm độ sâu thay đổi bất thường giữa các khung hình.
- **Trễ khung hình:** dùng độ sâu của khung hình trước cho khung hình hiện tại.
- **Méo cục bộ:** làm sai độ sâu trong một vùng ảnh cụ thể.
- **Làm mượt quá mức:** làm mất chi tiết nhỏ trong bản đồ độ sâu.

Vì mức độ lỗi được kiểm soát, thứ tự mong muốn là:

```text
độ sâu ban đầu < lỗi nhẹ < lỗi nặng
```

Một chỉ số đánh giá tốt cần phản ánh được thứ tự này.

### 6.5. Chỉ số 1: độ ổn định 3D của điểm theo dõi

Với một điểm được theo dõi qua nhiều khung hình, tọa độ ảnh và độ sâu tại mỗi khung hình sẽ được dùng để đưa điểm đó vào không gian 3D. Sau đó, các điểm 3D này được quy về cùng một hệ tọa độ bằng thông tin camera.

Nếu bản đồ độ sâu nhất quán, cùng một điểm vật lý trong cảnh cần có vị trí 3D gần nhau qua nhiều khung hình. Nếu các vị trí này phân tán quá xa, điều đó cho thấy độ sâu có thể chưa ổn định về mặt hình học.

### 6.6. Chỉ số 2: sai số chiếu lại lên ảnh

Một điểm 3D được tạo từ khung hình này sẽ được chiếu sang khung hình khác bằng thông tin camera. Sau đó, vị trí chiếu lại được so sánh với vị trí điểm theo dõi thật trên ảnh.

Nếu sai số giữa hai vị trí nhỏ, kết quả độ sâu có tính nhất quán tốt hơn. Nếu sai số lớn, bản đồ độ sâu hoặc thông tin hình học có thể đang gặp vấn đề. Chỉ số đánh giá này dễ kiểm tra bằng hình minh họa vì có thể vẽ trực tiếp hai điểm lên ảnh.

### 6.7. Chỉ số 3: độ trôi tỉ lệ theo thời gian

Các mô hình ước lượng độ sâu từ một camera thường khó xác định đúng tỉ lệ tuyệt đối. Vì vậy, đề tài cần tách riêng lỗi trôi tỉ lệ khỏi các lỗi hình học khác.

Chỉ số này đánh giá xem tỉ lệ độ sâu có thay đổi bất thường giữa các khung hình hay không. Nhờ đó, có thể phân biệt trường hợp bản đồ độ sâu có hình dạng tương đối đúng nhưng tỉ lệ bị trôi, với trường hợp bản đồ độ sâu bị sai cục bộ.

### 6.8. Kiểm chứng các chỉ số đánh giá

Các chỉ số đánh giá sẽ được kiểm chứng theo ba hướng:

1. **Kiểm tra xếp hạng:** độ sâu ban đầu cần có sai số thấp hơn độ sâu đã bị tạo lỗi.
2. **Kiểm tra tăng dần:** sai số cần tăng khi mức độ lỗi tăng.
3. **Kiểm tra tương quan:** nếu có dữ liệu thật, chỉ số đánh giá đề xuất sẽ được so sánh với các sai số truyền thống như AbsRel, RMSE hoặc sai số chiếu lại.

Ngoài các bảng số liệu, hình minh họa sẽ được dùng để giải thích kết quả và phát hiện các trường hợp thất bại.

### 6.9. Hiệu chỉnh ở thời điểm kiểm thử

Sau khi các chỉ số đánh giá hình học được kiểm chứng, đề tài sẽ xem xét dùng chúng để hiệu chỉnh kết quả độ sâu ở thời điểm kiểm thử. Mục tiêu của bước này không phải là huấn luyện lại mô hình từ đầu, mà là điều chỉnh nhẹ kết quả của từng video để làm cho độ sâu nhất quán hơn về mặt hình học.

Cách làm dự kiến đi từ đơn giản đến phức tạp:

1. **Hiệu chỉnh tỉ lệ và độ lệch theo video:** tìm một hệ số tỉ lệ và một độ lệch phù hợp để bản đồ độ sâu ổn định hơn qua các khung hình.
2. **Hiệu chỉnh nhỏ trên bản đồ độ sâu:** thêm một phần sửa nhỏ vào bản đồ độ sâu, đồng thời giữ cho kết quả không thay đổi quá mạnh so với dự đoán ban đầu.
3. **Hiệu chỉnh một phần nhỏ của mô hình nếu có điều kiện:** chỉ điều chỉnh một số tham số nhỏ, thay vì cập nhật toàn bộ mô hình.

Bước này sẽ được xem như hướng mở rộng sau khi phần đánh giá đã cho tín hiệu đáng tin cậy. Nếu chỉ số đánh giá chưa ổn định hoặc phụ thuộc quá nhiều vào VGGT, phần hiệu chỉnh sẽ không được xem là kết quả chính.

## 7. Kết quả dự kiến

Kết quả dự kiến của đề tài gồm:

1. Một quy trình đánh giá tính nhất quán hình học 3D của video độ sâu trên các bộ dữ liệu có sẵn.
2. Ba nhóm chỉ số đánh giá: độ ổn định 3D của điểm theo dõi, sai số chiếu lại lên ảnh và độ trôi tỉ lệ theo thời gian.
3. Một quy trình tạo lỗi có kiểm soát để đánh giá độ nhạy của chỉ số đánh giá.
4. Một thử nghiệm ban đầu trên một số đoạn video ngắn nhằm chứng minh chỉ số đánh giá có khả năng phân biệt kết quả tốt và kết quả bị lỗi.
5. Một thử nghiệm hiệu chỉnh nhẹ ở thời điểm kiểm thử nếu các chỉ số đánh giá ban đầu đủ ổn định.
6. Bảng kết quả, biểu đồ và hình minh họa thể hiện sai số theo mức độ lỗi.

Nếu thời gian cho phép, một sổ tay thử nghiệm nhỏ sẽ được xây dựng để minh họa quy trình. Tuy nhiên, sổ tay này không phải là yêu cầu bắt buộc nếu đề tài chỉ dừng ở mức đề cương.

## 8. Phạm vi và giới hạn

Đề tài tập trung vào xây dựng quy trình đánh giá và chỉ số đánh giá chẩn đoán trên các bộ dữ liệu có sẵn, không tạo một bộ dữ liệu chuẩn mới. Phần hiệu chỉnh ở thời điểm kiểm thử chỉ được xem là hướng mở rộng nhẹ, chưa tập trung vào việc huấn luyện một mô hình ước lượng độ sâu mới.

VGGT được dùng như nguồn thông tin hình học hỗ trợ, không được xem là chuẩn đúng tuyệt đối. Vì vậy, kết quả cần được hiểu là đánh giá chẩn đoán, không phải kết luận cuối cùng rằng mô hình này đúng và mô hình khác sai.

Thử nghiệm ban đầu được giới hạn ở các đoạn video ngắn và số lượng bộ dữ liệu nhỏ để bảo đảm tính khả thi. Việc hiệu chỉnh kết quả ở thời điểm kiểm thử sẽ chỉ được thực hiện sau khi các chỉ số đánh giá đã được kiểm chứng sơ bộ.

## 9. Rủi ro và cách giảm thiểu

| Rủi ro | Vấn đề | Cách giảm thiểu |
| :--- | :--- | :--- |
| VGGT không phải dữ liệu đúng tuyệt đối | Chỉ số đánh giá có thể chỉ phản ánh mức độ giống với VGGT | Dùng lỗi có kiểm soát, so sánh với dữ liệu thật nếu có và bổ sung hình minh họa |
| Nhầm quy ước camera | Nhầm hướng biến đổi camera có thể làm sai số bị tính sai | Kiểm tra bằng cách vẽ điểm chiếu lại trên vài khung hình trước khi chạy hàng loạt |
| Độ sâu thiếu tỉ lệ tuyệt đối | Độ sâu từ một camera thường không xác định được đúng đơn vị thật | Báo cáo riêng sai số sau khi căn chỉnh và độ trôi tỉ lệ |
| Vật thể chuyển động | Vật thể tự chuyển động làm giả định cảnh tĩnh bị sai | Dùng độ tin cậy, loại bỏ điểm theo dõi không ổn định và dùng cách tính sai số ít nhạy với ngoại lệ |
| Hiệu chỉnh làm kết quả xấu hơn | Nếu chỉ số đánh giá chưa đáng tin, việc hiệu chỉnh theo chỉ số đánh giá có thể gây sai lệch | Chỉ hiệu chỉnh nhẹ, so sánh với kết quả ban đầu và dừng ở mức thử nghiệm nếu tín hiệu chưa rõ |
| Chi phí tính toán | Chạy nhiều mô hình trên video có thể tốn GPU | Bắt đầu bằng video ngắn, độ phân giải thấp và lưu kết quả trung gian |
| Dữ liệu không thống nhất | Mỗi bộ dữ liệu có định dạng camera và độ sâu khác nhau | Chuẩn hóa dữ liệu về một cấu trúc trung gian thống nhất |

## 10. Kết luận

Đề tài đề xuất một hướng nghiên cứu khả thi nhằm đánh giá và hiệu chỉnh nhẹ tính nhất quán hình học 3D của video độ sâu. Thay vì tạo một bộ dữ liệu chuẩn mới hoặc huấn luyện mô hình mới ngay từ đầu, đề tài tập trung vào cách dùng thông tin hình học do VGGT cung cấp để đánh giá kết quả trên các bộ dữ liệu có sẵn. Cách tiếp cận này phù hợp với giai đoạn nghiên cứu ban đầu vì có thể kiểm chứng bằng các lỗi được tạo có kiểm soát, đồng thời có thể mở rộng sang hiệu chỉnh ở thời điểm kiểm thử nếu các chỉ số đánh giá cho kết quả đáng tin cậy.

## 11. Tài liệu tham khảo

Jianyuan Wang, Minghao Chen, Nikita Karaev, Andrea Vedaldi, Christian Rupprecht, David Novotny: VGGT: Visual Geometry Grounded Transformer. CVPR 2025: 5294-5306.

Sili Chen, Hengkai Guo, Shengnan Zhu, Feihu Zhang, Zilong Huang, Jiashi Feng, Bingyi Kang: Video Depth Anything: Consistent Depth Estimation for Super-Long Videos. CVPR 2025: 22831-22840.

Lihe Yang, Bingyi Kang, Zilong Huang, Zhen Zhao, Xiaogang Xu, Jiashi Feng, Hengshuang Zhao: Depth Anything V2. NeurIPS 2024.

Lihe Yang, Bingyi Kang, Zilong Huang, Xiaogang Xu, Jiashi Feng, Hengshuang Zhao: Depth Anything: Unleashing the Power of Large-Scale Unlabeled Data. CVPR 2024: 10371-10381.

Zhengqi Li, Richard Tucker, Forrester Cole, Qianqian Wang, Linyi Jin, Vickie Ye, Angjoo Kanazawa, Aleksander Holynski, Noah Snavely: MegaSaM: Accurate, Fast and Robust Structure and Motion from Casual Dynamic Videos. CVPR 2025: 10486-10496.

Andreas Geiger, Philip Lenz, Raquel Urtasun: Are we ready for Autonomous Driving? The KITTI Vision Benchmark Suite. CVPR 2012: 3354-3361.

Daniel J. Butler, Jonas Wulff, Garrett B. Stanley, Michael J. Black: A Naturalistic Open Source Movie for Optical Flow Evaluation. ECCV 2012: 611-625.

Jürgen Sturm, Nikolas Engelhard, Felix Endres, Wolfram Burgard, Daniel Cremers: A Benchmark for the Evaluation of RGB-D SLAM Systems. IROS 2012: 573-580.

Angela Dai, Angel X. Chang, Manolis Savva, Maciej Halber, Thomas A. Funkhouser, Matthias Nießner: ScanNet: Richly-annotated 3D Reconstructions of Indoor Scenes. CVPR 2017: 5828-5839.
