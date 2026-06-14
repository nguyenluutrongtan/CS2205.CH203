# ĐỀ CƯƠNG NGHIÊN CỨU

## 1. Tên đề tài tiếng Việt

**Đánh giá và hiệu chỉnh tính nhất quán hình học 3D cho ước lượng độ sâu video bằng VGGT**

## 2. Tên đề tài tiếng Anh

**VGGT-Guided Evaluation and Test-Time Adaptation for 3D-Consistent Video Depth Estimation**

## 3. Tóm tắt

Mặc dù các mô hình ước lượng độ sâu từ video hiện đại ngày càng tạo ra kết quả ổn định hơn giữa các khung hình, tính nhất quán về hình học 3D vẫn chưa được đánh giá đầy đủ. Khi camera di chuyển, bản đồ độ sâu có thể bị trôi tỉ lệ, sai lệch vị trí hoặc làm biến dạng vật thể mà các chỉ số trên từng khung hình như AbsRel hay RMSE không phản ánh hết. Trong khi đó, phương pháp đánh giá bằng sai số chiếu lại thường cần biết trước thông tin camera và chủ yếu xem xét các cặp khung hình liền kề. Để giải quyết khoảng trống này, đề tài đề xuất một khung đánh giá sử dụng VGGT như nguồn thông tin hình học hỗ trợ, không phải chuẩn tham chiếu tuyệt đối. Ba nhóm chỉ số bổ sung cho nhau được xây dựng, gồm độ ổn định vị trí 3D của cùng một điểm qua nhiều khung hình, sai số chiếu lại giữa các khung hình cách xa nhau về thời gian, và mức biến động của tỉ lệ và độ lệch theo thời gian. Khung đánh giá còn kiểm tra độ tin cậy của thông tin do VGGT cung cấp, qua đó cho phép phân tích cấu trúc 3D trên các quỹ đạo điểm dài hạn thay vì chỉ xem xét từng cặp khung hình. Phương pháp dự kiến được kiểm chứng trên TUM RGB-D và Sintel bằng nhiều dạng sai lệch hình học được tạo có kiểm soát, từ nhẹ đến nặng. Các chỉ số đề xuất sẽ được đối chiếu với thông tin hình học chuẩn và so sánh trực tiếp với TAE về khả năng phản ánh mức độ sai lệch. Ngoài ra, nghiên cứu thử nghiệm hiệu chỉnh tỉ lệ và độ lệch theo thời gian ở đầu ra mà không cập nhật trọng số mô hình. Kết quả kỳ vọng là một quy trình có thể tái lập để phát hiện, giải thích và giảm những sai lệch hình học khó nhận biết nếu chỉ xem xét độ ổn định giữa các khung hình.

## 4. Giới thiệu

Ước lượng độ sâu từ video là một bài toán quan trọng trong thị giác máy tính, có thể được ứng dụng trong robot, xe tự hành, thực tế tăng cường, dựng lại không gian 3D và định vị camera. Với mỗi khung hình trong video, mô hình cần dự đoán khoảng cách từ camera đến các điểm trong cảnh.

Depth Anything và Depth Anything V2 đã cải thiện đáng kể chất lượng ước lượng độ sâu từ ảnh [3], [4], trong khi Video Depth Anything hướng đến duy trì sự ổn định giữa các khung hình video [2]. Tuy nhiên, ổn định về mặt hình ảnh chưa đồng nghĩa với nhất quán về mặt hình học. Bản đồ độ sâu có thể thay đổi trơn tru theo thời gian nhưng vẫn bị trôi tỉ lệ, sai lệch khi camera di chuyển hoặc làm biến dạng cùng một vật thể trong không gian 3D.

![Minh họa Video Depth Anything](assets/proposal/video_depth_anything_teaser.png)

*Hình 1. Minh họa kết quả của Video Depth Anything trên video dài. Mô hình hướng tới việc tạo bản đồ độ sâu ổn định theo thời gian. Nguồn: Video Depth Anything [2].*

Các chỉ số như AbsRel và RMSE chủ yếu đo sai số trên từng khung hình. TAE mở rộng việc đánh giá sang tính nhất quán theo thời gian bằng cách chiếu độ sâu giữa hai khung hình liên tiếp khi thông tin camera đã được biết trước [10]; chỉ số này cũng được Video Depth Anything sử dụng để so sánh [2]. Các công trình như MegaSaM đã khai thác đồng thời cấu trúc và chuyển động trong video có vật thể động [5], nhưng tập trung vào tái tạo cảnh hơn là đánh giá độc lập kết quả của một mô hình độ sâu. Vì vậy, đề tài đặt ra câu hỏi liệu camera và điểm theo dõi từ một mô hình hình học tổng quát có thể thay thế một phần dữ liệu chuẩn (ground truth) để phát hiện sai lệch 3D hay không, đặc biệt khi các điểm được theo dõi qua nhiều khung hình, và mức độ tin cậy của cách tiếp cận này là bao nhiêu.

VGGT có khả năng đồng thời ước lượng camera, độ sâu, điểm 3D và điểm theo dõi từ nhiều ảnh [1]. Do đó, đề tài sử dụng các đầu ra này như tín hiệu hỗ trợ để xây dựng chỉ số đánh giá, thay vì xem VGGT là chuẩn tham chiếu tuyệt đối. Để kiểm tra tính khách quan, cùng một nhóm chỉ số sẽ được tính theo hai giao thức: một giao thức dùng thông tin hình học chuẩn làm tham chiếu độc lập và một giao thức dùng thông tin do VGGT ước lượng. Mức tương quan giữa hai giao thức cho biết khi nào tín hiệu VGGT đủ tin cậy để hỗ trợ đánh giá.

![Kiến trúc VGGT](assets/proposal/vggt_architecture.png)

*Hình 2. Kiến trúc tổng quát của VGGT. Mô hình nhận nhiều ảnh đầu vào và dự đoán đồng thời thông tin camera, bản đồ độ sâu, bản đồ điểm 3D và điểm theo dõi. Nguồn: VGGT [1].*

Ba câu hỏi nghiên cứu được đặt ra: (1) các chỉ số đề xuất có thay đổi tương ứng với mức độ sai lệch hình học hay không; (2) kết quả đánh giá bằng tín hiệu VGGT có nhất quán với kết quả dựa trên dữ liệu chuẩn và các chỉ số đánh giá hiện có hay không; và (3) việc hiệu chỉnh tỉ lệ và độ lệch theo thời gian bằng tín hiệu VGGT có làm giảm sai lệch hình học mà không ảnh hưởng đáng kể đến độ chính xác của bản đồ độ sâu hay không. Đầu vào của đề tài là các đoạn video ngắn, bản đồ độ sâu dự đoán và thông tin hình học hỗ trợ; đầu ra gồm các chỉ số, bảng so sánh, biểu đồ độ nhạy, hình minh họa và kết quả trước và sau hiệu chỉnh.

## 5. Mục tiêu

Mục tiêu tổng quát của đề tài là xây dựng và kiểm chứng một quy trình sử dụng tín hiệu VGGT để đánh giá và hiệu chỉnh nhẹ tính nhất quán hình học 3D của bản đồ độ sâu video.

Các mục tiêu cụ thể gồm:

1. Xây dựng quy trình chuẩn hóa bản đồ độ sâu, thông tin camera, điểm theo dõi và độ tin cậy từ VGGT; trong đó, tỉ lệ và độ lệch được căn chỉnh thống nhất cho toàn bộ đoạn video, còn các điểm không hợp lệ, bị che khuất hoặc thuộc vùng động được xử lý riêng.
2. Đề xuất và kiểm chứng ba nhóm chỉ số về độ ổn định vị trí 3D, sai số chiếu lại dài hạn và độ trôi của tỉ lệ và độ lệch; đồng thời so sánh kết quả giữa giao thức dùng VGGT, giao thức dùng dữ liệu chuẩn và các chỉ số đánh giá AbsRel, RMSE, TAE.
3. Xây dựng thử nghiệm thích nghi tại thời điểm kiểm thử (test-time adaptation) ở mức đầu ra bằng biến đổi affine theo thời gian; đánh giá định lượng sự thay đổi của các chỉ số hình học và độ chính xác độ sâu trước và sau hiệu chỉnh.

## 6. Nội dung và phương pháp

### 6.1. Phạm vi, đầu vào và đầu ra

Đề tài tập trung đánh giá tính nhất quán hình học 3D của bản đồ độ sâu video trên các bộ dữ liệu có sẵn, không xây dựng bộ dữ liệu mới và không huấn luyện lại mô hình ước lượng độ sâu từ đầu. Video Depth Anything được chọn làm mô hình độ sâu chính; Depth Anything V2 có thể được dùng để so sánh với phương pháp xử lý từng khung hình độc lập [2], [3]. VGGT chỉ cung cấp thông tin hình học hỗ trợ và không được xem là dữ liệu chuẩn [1].

Đầu vào gồm các đoạn video từ 8 đến 64 khung hình, ảnh màu, bản đồ độ sâu cần đánh giá, thông tin camera và điểm theo dõi do VGGT cung cấp, cùng dữ liệu chuẩn chỉ dùng cho việc kiểm chứng. TUM RGB-D và Sintel là hai bộ dữ liệu kiểm chứng chính: TUM RGB-D cung cấp dữ liệu RGB-D và quỹ đạo camera trong cảnh thực [8], còn Sintel cung cấp độ sâu, camera và luồng quang học tổng hợp [7]. KITTI hoặc ScanNet chỉ được sử dụng bổ sung nếu thời gian và tài nguyên tính toán cho phép [6], [9].

Đầu ra gồm mã nguồn thực nghiệm, ba nhóm chỉ số, bảng so sánh hai giao thức đánh giá, biểu đồ thể hiện độ nhạy theo mức sai lệch, hình minh họa điểm 3D và kết quả chiếu lại, cùng kết quả trước và sau bước hiệu chỉnh affine.

### 6.2. Quy trình nghiên cứu

![Sơ đồ phương pháp đề xuất](assets/proposal/proposal_pipeline_diagram.png)

*Hình 3. Sơ đồ tổng quát của đề tài. Hai giao thức đánh giá lần lượt sử dụng tín hiệu VGGT và dữ liệu chuẩn độc lập. Nhánh hiệu chỉnh chỉ sử dụng tín hiệu VGGT; dữ liệu chuẩn chỉ được dùng để kiểm chứng kết quả.*

Quy trình nghiên cứu gồm bốn bước chính. Thứ nhất, các công trình liên quan được khảo sát để xác định giả thuyết, chỉ số đánh giá so sánh và các yếu tố cần kiểm soát. TAE được dùng để so sánh sai số chiếu lại giữa hai khung hình liên tiếp khi thông tin camera đã được biết trước [10]. Phương pháp đề xuất mở rộng việc đánh giá sang các điểm được theo dõi qua nhiều khung hình, nhiều khoảng cách thời gian và trường hợp thông tin camera phải được ước lượng.

Thứ hai, các đoạn video được lựa chọn dựa trên mức chuyển động của camera, mức độ che khuất và tỉ lệ vùng có vật thể động. Dữ liệu trung gian được đưa về cùng độ phân giải, quy ước camera và hệ tọa độ. Với độ sâu tương đối, một cặp tham số tỉ lệ và độ lệch được ước lượng chung cho toàn bộ đoạn video trong không gian nghịch đảo độ sâu, sau đó được giữ cố định khi đánh giá. Việc căn chỉnh riêng từng khung hình không được sử dụng vì có thể loại bỏ chính hiện tượng trôi tỉ lệ cần đo. Trong giao thức tham chiếu, phép căn chỉnh sử dụng các điểm ảnh có dữ liệu chuẩn hợp lệ; trong giao thức VGGT, phép căn chỉnh sử dụng các điểm có độ tin cậy cao do VGGT cung cấp. Dữ liệu chuẩn không tham gia giao thức VGGT hoặc bước hiệu chỉnh.

Thứ ba, nghiên cứu tạo các phiên bản bản đồ độ sâu chứa sai lệch bằng những phép biến đổi có tham số, gồm trôi tỉ lệ và độ lệch theo thời gian, nhiễu theo thời gian, trễ khung hình, biến dạng cục bộ và làm trơn quá mức. Mỗi dạng sai lệch được tạo ở ít nhất ba mức độ, từ nhẹ đến nặng. Thử nghiệm được phân tích riêng cho cảnh tĩnh và vùng có vật thể động. Những điểm có độ tin cậy thấp, bị che khuất, nằm ngoài ảnh hoặc không được theo dõi đủ lâu sẽ bị loại. Vùng động và vùng che khuất được xác định từ nhãn hoặc luồng quang học nếu có; trong trường hợp không có nhãn, nghiên cứu sử dụng kiểm tra thuận-nghịch và các thống kê ít nhạy với giá trị ngoại lệ.

Thứ tư, cùng một nhóm chỉ số được tính theo hai giao thức. Giao thức tham chiếu sử dụng thông tin camera chuẩn và các điểm tương ứng thu được từ luồng quang học hoặc từ phép chiếu sử dụng độ sâu chuẩn; giao thức hỗ trợ sử dụng camera, điểm theo dõi và độ tin cậy do VGGT cung cấp. Kết quả được trình bày bằng bảng, biểu đồ và hình minh họa để phân tích độ nhạy của chỉ số, mức độ thống nhất giữa hai giao thức và những trường hợp phương pháp hoạt động chưa tốt.

### 6.3. Chỉ số đánh giá và kiểm chứng

Nhóm thứ nhất đo độ ổn định vị trí 3D của các điểm được theo dõi qua nhiều khung hình. Tọa độ trên ảnh và độ sâu tại mỗi khung hình được dùng để đưa điểm vào không gian 3D, sau đó quy đổi về cùng một hệ tọa độ bằng thông tin camera. Độ phân tán của các vị trí thuộc cùng một quỹ đạo, sau khi được chuẩn hóa theo độ sâu trung vị của cảnh, phản ánh mức biến dạng 3D. Chỉ số chỉ tổng hợp những quỹ đạo đủ dài và đáp ứng yêu cầu về độ tin cậy, khả năng quan sát và tính nhất quán.

Nhóm thứ hai đo sai số chiếu lại trên nhiều khoảng thời gian. Một điểm 3D từ khung hình nguồn được chiếu sang các khung hình đích, sau đó vị trí chiếu được so sánh với vị trí quan sát của điểm tương ứng. Khác với TAE chủ yếu xem xét các khung hình liên tiếp [10], chỉ số này phân tích sai số theo khoảng cách thời gian và độ dài quỹ đạo, qua đó phát hiện sai lệch tích lũy.

Nhóm thứ ba đo mức trôi của tỉ lệ và độ lệch theo thời gian. Sau bước căn chỉnh chung cho toàn bộ đoạn video, các tham số affine tối ưu trên từng khung hình hoặc cửa sổ thời gian ngắn chỉ được dùng để phân tích. Mức biến thiên và xu hướng của các tham số này cho biết trường hợp hình dạng tương đối vẫn ổn định nhưng tỉ lệ hoặc độ lệch thay đổi bất thường.

Các chỉ số được kiểm chứng theo bốn hướng. Thứ nhất, tỉ lệ xếp đúng thứ tự cho biết chỉ số có phân biệt được độ sâu gốc với các mức sai lệch nhẹ, vừa và nặng hay không. Thứ hai, hệ số tương quan Spearman đo mức độ tương quan đơn điệu giữa chỉ số và cường độ sai lệch. Thứ ba, tương quan giữa giao thức VGGT và giao thức tham chiếu cho biết độ tin cậy của tín hiệu VGGT. Thứ tư, việc so sánh với AbsRel, RMSE và TAE giúp xác định giá trị bổ sung của quỹ đạo điểm dài hạn và chỉ số đo độ trôi. Kết quả được báo cáo theo dạng sai lệch, bộ dữ liệu, độ dài đoạn video và tỉ lệ vùng động; mỗi kết quả gồm giá trị trung vị và khoảng biến thiên thay vì chỉ có giá trị trung bình.

Tiêu chí thành công sơ bộ được xác định trước khi tiến hành thí nghiệm: tỉ lệ xếp đúng thứ tự đạt ít nhất 80% và hệ số Spearman trung vị đạt ít nhất 0,7 đối với tối thiểu ba trong năm dạng sai lệch trên cả hai bộ dữ liệu chính; hệ số tương quan giữa giao thức VGGT và giao thức tham chiếu đạt ít nhất 0,6. Các ngưỡng này được dùng để quyết định chỉ số có đủ ổn định cho bước hiệu chỉnh hay không, tránh việc chỉ lựa chọn những ví dụ cho kết quả thuận lợi.

### 6.4. Hiệu chỉnh ở thời điểm kiểm thử và giảm rủi ro

Sau khi các chỉ số được kiểm chứng, đề tài thực hiện một thử nghiệm thích nghi tại thời điểm kiểm thử với phạm vi giới hạn. Với nghịch đảo độ sâu đầu vào \(x_t=1/D_t\), kết quả được hiệu chỉnh bằng biến đổi affine \(x'_t=a_tx_t+b_t\), sau đó chuyển trở lại thành độ sâu \(D'_t\). Các tham số \(a_t,b_t\) được tối ưu theo chỉ số hình học dựa trên VGGT, đồng thời chịu ràng buộc về độ trơn theo thời gian và mức sai lệch so với phép căn chỉnh chung của toàn bộ đoạn video. Trọng số của mô hình độ sâu và VGGT được giữ cố định. Thiết kế này giới hạn số tham số cần tối ưu, tạo điều kiện so sánh rõ ràng và tập trung trực tiếp vào hiện tượng trôi tỉ lệ và độ lệch.

Dữ liệu chuẩn chỉ được sử dụng sau quá trình tối ưu để tính AbsRel, RMSE, TAE và các chỉ số tham chiếu trước và sau hiệu chỉnh. Một phép hiệu chỉnh được xem là có ích khi làm giảm trung vị của sai số hình học tham chiếu ít nhất 5%, đồng thời không làm AbsRel tăng quá 5%. Nếu chỉ số dựa trên VGGT giảm nhưng chỉ số tham chiếu tăng, kết quả đó được ghi nhận là trường hợp thất bại và không được xem là cải thiện.

Các rủi ro chính gồm sai số trong camera hoặc điểm theo dõi do VGGT cung cấp, nhầm lẫn quy ước camera, vật thể động, che khuất, chênh lệch miền dữ liệu và chi phí tính toán. Các biện pháp giảm rủi ro gồm kiểm thử riêng phép chiếu bằng dữ liệu tổng hợp, đối chiếu camera do VGGT ước lượng với camera chuẩn, lọc điểm theo dõi theo độ tin cậy và kiểm tra thuận-nghịch, đánh giá riêng vùng tĩnh và vùng động, sử dụng thống kê ít nhạy với ngoại lệ, ưu tiên các đoạn video ngắn và lưu kết quả trung gian.

## 7. Kết quả mong đợi

Kết quả mong đợi của đề tài bao gồm:

1. **Một quy trình đánh giá có thể tái lập**, gồm chuẩn hóa camera và độ sâu, căn chỉnh tỉ lệ và độ lệch chung cho toàn bộ đoạn video, lọc điểm theo dõi và tính ba nhóm chỉ số: độ ổn định vị trí 3D, sai số chiếu lại dài hạn và độ trôi của tỉ lệ và độ lệch.
2. **Một bộ kết quả kiểm chứng định lượng** trên TUM RGB-D và Sintel, gồm tỉ lệ xếp đúng thứ tự, hệ số Spearman theo mức sai lệch, mức tương quan giữa giao thức VGGT và giao thức tham chiếu, cùng kết quả so sánh với AbsRel, RMSE và TAE.
3. **Một phân tích về điều kiện sử dụng và trường hợp thất bại**, thể hiện ảnh hưởng của độ dài đoạn video, dạng sai lệch, độ tin cậy của VGGT, vùng động và che khuất thông qua bảng, biểu đồ và hình minh họa kết quả chiếu lại.
4. **Một thử nghiệm thích nghi tại thời điểm kiểm thử với kết quả so sánh trước và sau hiệu chỉnh**, trong đó chỉ các tham số affine theo thời gian được tối ưu. Kết quả sẽ báo cáo đồng thời sự thay đổi của chỉ số dựa trên VGGT và chỉ số tham chiếu để tránh kết luận cải thiện chỉ từ tín hiệu được tối ưu.

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
