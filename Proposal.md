# ĐỀ CƯƠNG NGHIÊN CỨU

## 1. Tên đề tài tiếng Việt

**Đánh giá và hiệu chỉnh tính nhất quán hình học 3D cho ước lượng độ sâu video bằng VGGT**

## 2. Tên đề tài tiếng Anh

**VGGT-Guided Evaluation and Test-Time Adaptation for 3D-Consistent Video Depth Estimation**

## 3. Tóm tắt

Mặc dù các mô hình ước lượng độ sâu từ video hiện đại ngày càng đạt được sự ổn định cao giữa các khung hình, tính nhất quán về hình học 3D vẫn là một vấn đề còn nhiều hạn chế. Khi camera di chuyển, bản đồ độ sâu có thể bị trôi tỉ lệ, sai lệch vị trí hoặc làm biến dạng vật thể mà các chỉ số đánh giá trên từng khung hình như AbsRel hay RMSE không phản ánh đầy đủ. Trong khi đó, phương pháp đánh giá bằng sai số chiếu lại truyền thống thường phụ thuộc vào thông tin camera biết trước và chỉ xét từng cặp khung hình liền kề. Để giải quyết khoảng trống này, đề tài đề xuất một khung đánh giá mới tận dụng VGGT như một nguồn thông tin hình học hỗ trợ, không phải chuẩn tham chiếu tuyệt đối. Từ đó, ba nhóm chỉ số bổ sung cho nhau được xây dựng, gồm độ ổn định vị trí 3D của cùng một điểm qua nhiều khung hình, sai số chiếu lại trên các khoảng thời gian dài, và mức biến động của tỉ lệ cùng độ lệch theo thời gian. Khung đề xuất còn tích hợp cơ chế kiểm tra độ tin cậy của chính thông tin từ VGGT, cho phép đánh giá cấu trúc 3D trên các quỹ đạo điểm dài hạn thay vì chỉ xem xét từng cặp khung hình. Phương pháp được kiểm chứng trên TUM RGB-D và Sintel với nhiều dạng sai lệch hình học được thiết kế với các mức độ tăng dần; các chỉ số đề xuất được đối chiếu với thông tin hình học chuẩn và so sánh trực tiếp với TAE về khả năng phản ánh mức độ sai lệch. Ngoài ra, nghiên cứu thử nghiệm chiến lược hiệu chỉnh tỉ lệ và độ lệch theo thời gian ở đầu ra mà không cần cập nhật trọng số mô hình. Kết quả hướng đến là một quy trình có thể tái lập để phát hiện, giải thích và giảm thiểu những sai lệch hình học khó nhận biết nếu chỉ xem xét độ ổn định giữa các khung hình.

## 4. Giới thiệu

Ước lượng độ sâu từ video là một bài toán quan trọng trong thị giác máy tính, có thể được ứng dụng trong robot, xe tự hành, thực tế tăng cường, dựng lại không gian 3D và định vị camera. Với mỗi khung hình trong video, mô hình cần dự đoán khoảng cách từ camera đến các điểm trong cảnh.

Depth Anything và Depth Anything V2 đã cải thiện đáng kể chất lượng ước lượng độ sâu từ ảnh [3], [4], trong khi Video Depth Anything mở rộng theo hướng ổn định thời gian cho video [2]. Tuy nhiên, ổn định về mặt hình ảnh chưa đồng nghĩa với đúng về mặt hình học. Một chuỗi bản đồ độ sâu có thể nhìn mượt nhưng vẫn bị sai tỉ lệ, bị lệch khi camera di chuyển, hoặc làm cùng một vật thể bị biến dạng trong không gian 3D.

![Minh họa Video Depth Anything](assets/proposal/video_depth_anything_teaser.png)

*Hình 1. Minh họa kết quả của Video Depth Anything trên video dài. Mô hình hướng tới việc tạo bản đồ độ sâu ổn định theo thời gian. Nguồn: Video Depth Anything [2].*

Các chỉ số như AbsRel và RMSE chủ yếu đo sai số theo từng khung hình. TAE mở rộng đánh giá sang tính nhất quán thời gian bằng cách chiếu độ sâu giữa hai khung hình liên tiếp với camera đã biết [10], và đã được Video Depth Anything sử dụng để so sánh [2]. Các công trình như MegaSaM đã khai thác đồng thời cấu trúc và chuyển động từ video động [5], nhưng trọng tâm là tái tạo cảnh thay vì đánh giá độc lập kết quả của một mô hình độ sâu. Vì vậy, khoảng trống mà đề tài kiểm tra là liệu camera và điểm theo dõi từ một mô hình hình học tổng quát có thể thay thế một phần ground truth để chẩn đoán lỗi 3D hay không, đặc biệt với tương ứng điểm dài hạn, và mức độ tin cậy của phép thay thế này là bao nhiêu.

VGGT có khả năng đồng thời ước lượng camera, độ sâu, điểm 3D và điểm theo dõi từ nhiều ảnh [1]. Do đó, đề tài dùng các đầu ra này như tín hiệu hỗ trợ để xây dựng chỉ số đánh giá, thay vì xem VGGT là chuẩn đúng tuyệt đối. Để tránh kết luận vòng tròn, cùng một nhóm chỉ số sẽ được tính theo hai giao thức: dùng hình học ground truth làm tham chiếu độc lập và dùng hình học VGGT. Mức tương quan giữa hai giao thức là bằng chứng để xác định khi nào tín hiệu VGGT đủ tin cậy.

![Kiến trúc VGGT](assets/proposal/vggt_architecture.png)

*Hình 2. Kiến trúc tổng quát của VGGT. Mô hình nhận nhiều ảnh đầu vào và dự đoán đồng thời thông tin camera, bản đồ độ sâu, bản đồ điểm 3D và điểm theo dõi. Nguồn: VGGT [1].*

Ba câu hỏi nghiên cứu được đặt ra: (1) các chỉ số đề xuất có tăng tương ứng với mức độ sai lệch hình học được tạo chủ động hay không; (2) kết quả đánh giá bằng tín hiệu VGGT có nhất quán với kết quả dựa trên ground truth và các thước đo hiện có hay không; và (3) hiệu chỉnh tỉ lệ-độ lệch theo thời gian bằng tín hiệu VGGT có giảm lỗi hình học mà không làm giảm độ chính xác độ sâu hay không. Đầu vào của đề tài là video ngắn, bản đồ độ sâu dự đoán và các tín hiệu hình học; đầu ra là chỉ số, bảng so sánh, biểu đồ độ nhạy, hình minh họa và kết quả hiệu chỉnh trước-sau.

## 5. Mục tiêu

Mục tiêu tổng quát của đề tài là xây dựng và kiểm chứng một quy trình dùng tín hiệu VGGT để đánh giá, sau đó hiệu chỉnh nhẹ tính nhất quán hình học 3D của độ sâu video.

Các mục tiêu cụ thể gồm:

1. Xây dựng quy trình chuẩn hóa độ sâu, camera, điểm theo dõi và độ tin cậy từ VGGT, trong đó tỉ lệ-độ lệch được căn chỉnh chung theo clip và các điểm không hợp lệ, bị che khuất hoặc thuộc vùng động được xử lý rõ ràng.
2. Đề xuất và kiểm chứng ba nhóm chỉ số về ổn định điểm 3D, chiếu lại dài hạn và trôi tỉ lệ bằng lỗi có kiểm soát; so sánh giao thức dùng VGGT với giao thức ground truth, AbsRel, RMSE và TAE.
3. Xây dựng một thử nghiệm thích nghi tại thời điểm kiểm thử (test-time adaptation) ở mức đầu ra bằng hiệu chỉnh affine theo thời gian; đánh giá định lượng mức thay đổi của chỉ số hình học và độ chính xác độ sâu trước-sau hiệu chỉnh.

## 6. Nội dung và phương pháp

### 6.1. Phạm vi, đầu vào và đầu ra

Đề tài tập trung vào đánh giá tính nhất quán hình học 3D của bản đồ độ sâu video trên các bộ dữ liệu có sẵn, không tạo bộ dữ liệu mới và không huấn luyện lại mô hình ước lượng độ sâu từ đầu. Video Depth Anything được chọn làm mô hình độ sâu chính; Depth Anything V2 có thể được dùng làm đối chứng theo từng khung hình [2], [3]. VGGT chỉ cung cấp tín hiệu hỗ trợ và không được dùng làm nhãn đúng [1].

Đầu vào gồm các clip khoảng 8 đến 64 khung hình, ảnh màu, bản đồ độ sâu cần đánh giá, camera và điểm theo dõi do VGGT cung cấp, cùng dữ liệu ground truth dùng riêng cho kiểm chứng. TUM RGB-D và Sintel là hai bộ dữ liệu kiểm chứng bắt buộc: TUM RGB-D cung cấp RGB-D và quỹ đạo camera trong cảnh thực [8], còn Sintel cung cấp độ sâu, camera và luồng quang học tổng hợp [7]. KITTI hoặc ScanNet chỉ được dùng bổ sung nếu thời gian và tài nguyên cho phép [6], [9].

Đầu ra gồm mã nguồn thực nghiệm, ba nhóm chỉ số, bảng so sánh hai giao thức hình học, biểu đồ độ nhạy theo mức lỗi, hình minh họa điểm 3D và điểm chiếu lại, cùng kết quả trước-sau của bước hiệu chỉnh affine.

### 6.2. Quy trình nghiên cứu

![Sơ đồ phương pháp đề xuất](assets/proposal/proposal_pipeline_diagram.png)

*Hình 3. Sơ đồ tổng quát của đề tài. Hai giao thức đánh giá lần lượt dùng tín hiệu VGGT và dữ liệu ground truth độc lập. Nhánh hiệu chỉnh chỉ sử dụng tín hiệu VGGT; ground truth chỉ dùng để kiểm chứng kết quả.*

Quy trình nghiên cứu gồm bốn bước chính. Thứ nhất, các công trình liên quan được khảo sát để xác định giả thuyết, thước đo so sánh và biến cần kiểm soát. TAE được dùng làm thước đo so sánh cho sai số chiếu lại giữa hai khung hình liên tiếp với camera đã biết [10]; phần đề xuất tập trung vào điểm theo dõi dài hạn, nhiều khoảng cách thời gian và tình huống camera chỉ được ước lượng.

Thứ hai, các clip được chọn theo mức chuyển động camera, độ che khuất và tỉ lệ vùng động. Dữ liệu trung gian được chuẩn hóa về cùng độ phân giải, quy ước camera và hệ tọa độ. Với độ sâu tương đối, một cặp tỉ lệ-độ lệch được ước lượng chung cho toàn clip trong không gian nghịch đảo độ sâu và được giữ cố định khi đánh giá. Không căn chỉnh riêng từng khung hình vì thao tác đó có thể xóa lỗi trôi tỉ lệ cần đo. Trong giao thức ground truth, phép căn chỉnh dùng các pixel ground truth hợp lệ; trong giao thức VGGT, phép căn chỉnh dùng các điểm VGGT có độ tin cậy cao. Ground truth không được dùng trong giao thức VGGT hoặc bước hiệu chỉnh.

Thứ ba, các phiên bản độ sâu bị lỗi được tạo bằng các phép biến đổi có tham số, gồm trôi tỉ lệ-độ lệch theo thời gian, nhiễu thời gian, trễ khung hình, méo cục bộ và làm mượt quá mức. Mỗi loại có ít nhất ba mức độ để tạo thứ tự ground truth từ lỗi nhẹ đến lỗi nặng. Thử nghiệm được tách theo cảnh tĩnh và vùng động; điểm theo dõi có độ tin cậy thấp, không quan sát được, ra ngoài ảnh hoặc không vượt chiều dài tối thiểu bị loại. Vùng động và che khuất được xác định từ nhãn hoặc luồng quang học khi có; khi không có nhãn, nghiên cứu dùng kiểm tra thuận-nghịch và thống kê robust trên phần dư.

Thứ tư, cùng một nhóm chỉ số được tính theo hai giao thức. Giao thức tham chiếu dùng camera ground truth và tương ứng điểm từ luồng quang học hoặc phép chiếu độ sâu ground truth; giao thức hỗ trợ dùng camera, điểm theo dõi và độ tin cậy từ VGGT. Kết quả được phân tích bằng bảng, biểu đồ và hình minh họa để xác định độ nhạy của chỉ số, mức đồng thuận giữa hai giao thức và các điều kiện thất bại.

### 6.3. Chỉ số đánh giá và kiểm chứng

Nhóm thứ nhất là độ ổn định 3D của điểm theo dõi dài hạn. Tọa độ ảnh và độ sâu tại mỗi khung hình được đưa vào không gian 3D, sau đó quy về cùng hệ tọa độ bằng camera. Độ phân tán robust của cùng một quỹ đạo điểm, được chuẩn hóa theo độ sâu trung vị của cảnh, phản ánh mức biến dạng 3D. Chỉ số chỉ tổng hợp các quỹ đạo đủ dài và đạt điều kiện về độ tin cậy, khả năng quan sát và tính nhất quán.

Nhóm thứ hai là sai số chiếu lại dài hạn. Một điểm 3D từ khung hình nguồn được chiếu tới các khung hình đích cách nhau nhiều khoảng thời gian, rồi so sánh với vị trí điểm theo dõi quan sát được. Khác với TAE chủ yếu tổng hợp giữa các khung hình liên tiếp [10], chỉ số này phân tích sai số theo khoảng cách thời gian và độ dài quỹ đạo điểm, nhờ đó kiểm tra sai lệch tích lũy.

Nhóm thứ ba là độ trôi tỉ lệ-độ lệch theo thời gian. Sau bước căn chỉnh chung theo clip, các tham số affine tối ưu cục bộ của từng khung hình hoặc cửa sổ ngắn chỉ được dùng để chẩn đoán. Độ biến thiên và xu hướng của các tham số này cho biết hình dạng tương đối có thể ổn định nhưng tỉ lệ hoặc độ lệch đang thay đổi bất thường.

Các chỉ số được kiểm chứng bằng bốn phép phân tích. Thứ nhất, độ chính xác xếp hạng đo khả năng sắp đúng độ sâu gốc, lỗi nhẹ, vừa và nặng. Thứ hai, hệ số Spearman đo quan hệ đơn điệu giữa chỉ số và cường độ lỗi. Thứ ba, tương quan giữa giao thức VGGT và giao thức ground truth đo độ tin cậy của tín hiệu thay thế. Thứ tư, so sánh với AbsRel, RMSE và TAE xác định giá trị bổ sung của điểm theo dõi dài hạn và chỉ số trôi. Kết quả được báo cáo theo loại lỗi, bộ dữ liệu, độ dài clip và tỉ lệ vùng động, kèm trung vị và khoảng biến thiên thay vì chỉ một giá trị trung bình.

Tiêu chí thành công sơ bộ được xác định trước khi chạy thí nghiệm: độ chính xác xếp hạng đạt ít nhất 80% và Spearman trung vị đạt ít nhất 0,7 đối với tối thiểu ba trong năm loại lỗi trên cả hai bộ dữ liệu chính; tương quan giữa giao thức VGGT và ground truth đạt ít nhất 0,6. Các ngưỡng này được dùng để kết luận chỉ số có đủ ổn định cho bước hiệu chỉnh hay không, thay vì chỉ chọn các ví dụ thuận lợi.

### 6.4. Hiệu chỉnh ở thời điểm kiểm thử và giảm rủi ro

Sau khi chỉ số được kiểm chứng, đề tài thực hiện một thử nghiệm thích nghi tại thời điểm kiểm thử có phạm vi giới hạn. Với nghịch đảo độ sâu đầu vào \(x_t=1/D_t\), đầu ra được hiệu chỉnh bằng biến đổi affine \(x'_t=a_tx_t+b_t\), sau đó chuyển về độ sâu \(D'_t\). Các tham số \(a_t,b_t\) được tối ưu từ chỉ số hình học dùng VGGT, đồng thời có ràng buộc trơn theo thời gian và ràng buộc gần phép căn chỉnh chung của clip. Trọng số của mô hình độ sâu và VGGT được giữ cố định. Cách thiết kế này giới hạn số tham số cần tối ưu, có đối chứng rõ ràng và trực tiếp xử lý hiện tượng trôi tỉ lệ-độ lệch.

Ground truth chỉ được dùng sau tối ưu để đo AbsRel, RMSE, TAE và các chỉ số tham chiếu trước-sau. Một hiệu chỉnh được xem là có ích khi làm giảm ít nhất 5% trung vị sai số hình học tham chiếu, đồng thời AbsRel không tăng quá 5%. Nếu chỉ số VGGT giảm nhưng chỉ số ground truth tăng, trường hợp đó được ghi nhận là trường hợp thất bại, không được xem là cải thiện.

Các rủi ro chính gồm sai camera hoặc điểm theo dõi từ VGGT, nhầm quy ước camera, vật thể động, che khuất, dữ liệu khác miền và chi phí tính toán. Biện pháp giảm rủi ro gồm kiểm thử đơn vị phép chiếu bằng dữ liệu tổng hợp, đối chiếu camera VGGT với camera ground truth, lọc điểm theo dõi theo độ tin cậy và kiểm tra thuận-nghịch, đánh giá riêng vùng tĩnh/vùng động, dùng thống kê robust, ưu tiên clip ngắn và lưu kết quả trung gian.

## 7. Kết quả mong đợi

Kết quả mong đợi của đề tài bao gồm:

1. **Một quy trình đánh giá có thể tái lập** gồm chuẩn hóa camera và độ sâu, căn chỉnh tỉ lệ-độ lệch chung theo clip, lọc điểm theo dõi và ba nhóm chỉ số: ổn định điểm 3D, chiếu lại dài hạn và trôi tỉ lệ-độ lệch.
2. **Một bộ kết quả kiểm chứng định lượng** trên TUM RGB-D và Sintel, gồm độ chính xác xếp hạng, hệ số Spearman theo mức lỗi, mức tương quan giữa giao thức VGGT và ground truth, cùng so sánh với AbsRel, RMSE và TAE.
3. **Một phân tích điều kiện sử dụng và trường hợp thất bại**, thể hiện ảnh hưởng của độ dài clip, loại lỗi, độ tin cậy của VGGT, vùng động và che khuất bằng bảng, biểu đồ và hình chiếu lại.
4. **Một thử nghiệm thích nghi tại thời điểm kiểm thử có đối chứng trước-sau**, trong đó chỉ tối ưu tham số affine theo thời gian. Kết quả báo cáo đồng thời thay đổi của chỉ số VGGT và chỉ số ground truth để tránh kết luận cải thiện chỉ dựa trên tín hiệu được tối ưu.

## 8. Tài liệu tham khảo

[1]. Jianyuan Wang, Minghao Chen, Nikita Karaev, Andrea Vedaldi, Christian Rupprecht, David Novotný: VGGT: Visual Geometry Grounded Transformer. CVPR 2025: 5294-5306.

[2]. Sili Chen, Hengkai Guo, Shengnan Zhu, Feihu Zhang, Zilong Huang, Jiashi Feng, Bingyi Kang: Video Depth Anything: Consistent Depth Estimation for Super-Long Videos. CVPR 2025: 22831-22840.

[3]. Lihe Yang, Bingyi Kang, Zilong Huang, Zhen Zhao, Xiaogang Xu, Jiashi Feng, Hengshuang Zhao: Depth Anything V2. NeurIPS 2024.

[4]. Lihe Yang, Bingyi Kang, Zilong Huang, Xiaogang Xu, Jiashi Feng, Hengshuang Zhao: Depth Anything: Unleashing the Power of Large-Scale Unlabeled Data. CVPR 2024: 10371-10381.

[5]. Zhengqi Li, Richard Tucker, Forrester Cole, Qianqian Wang, Linyi Jin, Vickie Ye, Angjoo Kanazawa, Aleksander Holynski, Noah Snavely: MegaSaM: Accurate, Fast and Robust Structure and Motion from Casual Dynamic Videos. CVPR 2025: 10486-10496.

[6]. Andreas Geiger, Philip Lenz, Raquel Urtasun: Are we ready for autonomous driving? The KITTI vision benchmark suite. CVPR 2012: 3354-3361.

[7]. Daniel J. Butler, Jonas Wulff, Garrett B. Stanley, Michael J. Black: A Naturalistic Open Source Movie for Optical Flow Evaluation. ECCV 2012: 611-625.

[8]. Jürgen Sturm, Nikolas Engelhard, Felix Endres, Wolfram Burgard, Daniel Cremers: A Benchmark for the Evaluation of RGB-D SLAM Systems. IROS 2012: 573-580.

[9]. Angela Dai, Angel X. Chang, Manolis Savva, Maciej Halber, Thomas A. Funkhouser, Matthias Nießner: ScanNet: Richly-Annotated 3D Reconstructions of Indoor Scenes. CVPR 2017: 2432-2443.

[10]. Honghui Yang, Di Huang, Wei Yin, Chunhua Shen, Haifeng Liu, Xiaofei He, Binbin Lin, Wanli Ouyang, Tong He: Depth Any Video with Scalable Synthetic Data. ICLR 2025.
