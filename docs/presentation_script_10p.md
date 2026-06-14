# Script thuyết trình 10 phút

Đề tài: **Đánh giá và hiệu chỉnh tính nhất quán hình học 3D cho ước lượng độ sâu video bằng VGGT**

Ghi chú: script này bám theo 10 slide hiện tại. Nếu cần thu video 5 phút theo yêu cầu nộp bài, xem phần rút gọn ở cuối file.

## Slide 1 - Trang bìa (khoảng 35 giây)

Kính chào thầy và các anh chị. Tôi là Nguyễn Lưu Trọng Tấn, mã số học viên 250201089. Hôm nay tôi xin trình bày đề cương nghiên cứu với đề tài: "Đánh giá và hiệu chỉnh tính nhất quán hình học 3D cho ước lượng độ sâu video bằng VGGT".

Tên tiếng Anh của đề tài là "VGGT-Guided Evaluation and Test-Time Adaptation for 3D-Consistent Video Depth Estimation". Vì đây là phần trình bày đề cương, nội dung sẽ tập trung vào vấn đề nghiên cứu, mục tiêu, phương pháp dự kiến và kết quả mong đợi, thay vì trình bày như một báo cáo kết quả đã hoàn tất.

## Slide 2 - Tóm tắt thông tin (khoảng 25 giây)

Đây là các thông tin chung của đồ án cuối kỳ môn CS2205, lớp CS2205.JAN2026. Các thông tin như link Github, link Youtube và ảnh đại diện sẽ được cập nhật trong bản nộp cuối cùng theo đúng mẫu của lớp.

Phần trình bày gồm bốn ý chính: bối cảnh và vấn đề nghiên cứu; giải pháp đề xuất với VGGT; mục tiêu, phạm vi và phương pháp; cuối cùng là kết quả mong đợi và tài liệu tham khảo.

## Slide 3 - Giới thiệu: Đặt vấn đề (khoảng 1 phút 15 giây)

Ước lượng độ sâu từ video là một bài toán quan trọng trong thị giác máy tính. Với mỗi khung hình, mô hình cần dự đoán khoảng cách từ camera đến các điểm trong cảnh. Thông tin này có thể phục vụ robot, xe tự hành, thực tế tăng cường, định vị camera và dựng lại không gian 3D.

Các mô hình gần đây như Depth Anything V2 và Video Depth Anything đã cải thiện đáng kể chất lượng ước lượng độ sâu. Đặc biệt, Video Depth Anything hướng tới kết quả ổn định theo thời gian, nghĩa là bản đồ độ sâu giữa các khung hình liên tiếp ít bị nhảy hoặc thay đổi đột ngột.

Tuy nhiên, ổn định về mặt hình ảnh chưa chắc đã đồng nghĩa với nhất quán về mặt hình học 3D. Một video có thể nhìn mượt, nhưng khi đưa độ sâu vào không gian 3D, kết quả vẫn có thể bị trôi tỉ lệ, sai lệch vị trí khi camera di chuyển, hoặc làm biến dạng cùng một vật thể qua nhiều khung hình.

Đây là giới hạn của các chỉ số đánh giá truyền thống như AbsRel hay RMSE, vì chúng chủ yếu đo sai số trên từng khung hình riêng lẻ. Chúng chưa trực tiếp trả lời câu hỏi: cùng một điểm trong thế giới 3D, khi xuất hiện qua nhiều frame, có được đặt vào một vị trí hình học nhất quán hay không. Vì vậy, đề tài tập trung vào đánh giá tính nhất quán hình học 3D của độ sâu video.

## Slide 4 - Giới thiệu: Giải pháp đề xuất (khoảng 1 phút 5 giây)

Để tiếp cận bài toán này, đề tài đề xuất sử dụng VGGT, tức Visual Geometry Grounded Transformer, như một nguồn tín hiệu hình học hỗ trợ.

VGGT có khả năng nhận nhiều ảnh đầu vào và dự đoán đồng thời camera, bản đồ độ sâu, bản đồ điểm 3D và các điểm theo dõi qua nhiều khung hình. Những đầu ra này phù hợp với bài toán, vì chúng giúp phân tích quan hệ hình học giữa các frame, thay vì chỉ đánh giá từng frame độc lập.

Điểm quan trọng là đề tài không xem VGGT là ground truth tuyệt đối. Nếu coi VGGT là chuẩn đúng, kết quả đánh giá có thể bị lệch theo chính sai số của VGGT. Vì vậy, VGGT chỉ được dùng như tín hiệu hỗ trợ để chẩn đoán hình học.

Để kiểm soát tính khách quan, cùng một nhóm chỉ số sẽ được tính theo hai giao thức. Giao thức thứ nhất dùng dữ liệu chuẩn khi có sẵn, ví dụ camera và độ sâu ground truth. Giao thức thứ hai dùng tín hiệu do VGGT ước lượng. Mức tương quan giữa hai giao thức sẽ cho biết VGGT có đủ tin cậy để hỗ trợ đánh giá hay không.

## Slide 5 - Mục tiêu nghiên cứu (khoảng 1 phút 15 giây)

Mục tiêu tổng quát của đề tài là xây dựng và kiểm chứng một quy trình dùng tín hiệu VGGT để đánh giá, và nếu phù hợp, hiệu chỉnh nhẹ tính nhất quán hình học 3D của bản đồ độ sâu video.

Mục tiêu cụ thể thứ nhất là chuẩn hóa đầu vào. Bản đồ độ sâu, thông tin camera, điểm theo dõi và độ tin cậy từ VGGT cần được đưa về cùng quy ước. Đặc biệt, tỉ lệ và độ lệch của độ sâu sẽ được căn chỉnh thống nhất cho cả đoạn video, thay vì căn chỉnh riêng từng khung hình, vì căn chỉnh riêng có thể làm mất hiện tượng trôi tỉ lệ cần đo.

Mục tiêu thứ hai là đề xuất và kiểm chứng ba nhóm chỉ số: độ ổn định vị trí 3D của cùng một điểm qua nhiều khung hình, sai số chiếu lại dài hạn, và độ trôi của tỉ lệ cùng độ lệch theo thời gian. Các chỉ số này sẽ được so sánh với AbsRel, RMSE và TAE.

Mục tiêu thứ ba là thử nghiệm test-time adaptation ở mức đầu ra. Đề tài không huấn luyện lại mô hình độ sâu và không cập nhật trọng số VGGT, mà chỉ tối ưu biến đổi affine theo thời gian trên đầu ra độ sâu để giảm sai lệch hình học.

## Slide 6 - Phạm vi, đầu vào và đầu ra (khoảng 55 giây)

Về phạm vi, đề tài tập trung đánh giá độ sâu video trên các bộ dữ liệu có sẵn, không xây dựng bộ dữ liệu mới và không huấn luyện lại mô hình từ đầu. Mô hình độ sâu chính là Video Depth Anything; Depth Anything V2 có thể được dùng để so sánh với hướng xử lý từng ảnh độc lập.

Hai bộ dữ liệu kiểm chứng chính là TUM RGB-D và Sintel. TUM RGB-D cung cấp dữ liệu RGB-D trong cảnh thực và quỹ đạo camera. Sintel là dữ liệu tổng hợp, có độ sâu, camera và thông tin chuyển động đầy đủ hơn, nên phù hợp để tạo và kiểm tra các sai lệch có kiểm soát.

Đầu vào gồm các đoạn video ngắn từ 8 đến 64 khung hình, bản đồ độ sâu cần đánh giá, và các đầu ra hình học của VGGT. Dữ liệu chuẩn, nếu có, chỉ dùng để kiểm chứng. Đầu ra gồm ba nhóm chỉ số, bảng so sánh hai giao thức, biểu đồ độ nhạy, minh họa chiếu lại và kết quả trước sau nếu có bước hiệu chỉnh.

## Slide 7 - Quy trình nghiên cứu (khoảng 1 phút 15 giây)

Quy trình nghiên cứu gồm bốn bước chính.

Bước thứ nhất là khảo sát công trình liên quan và xác định chỉ số so sánh. TAE là một mốc quan trọng vì đánh giá tính nhất quán thời gian thông qua sai số chiếu lại giữa hai khung hình. Tuy nhiên, đề tài muốn mở rộng sang quãng thời gian dài hơn và các quỹ đạo điểm dài hơn.

Bước thứ hai là tiền xử lý và chuẩn hóa dữ liệu. Các đoạn video sẽ được chọn theo mức chuyển động camera, mức che khuất và tỉ lệ vùng động. Độ sâu, camera và điểm theo dõi được đưa về cùng hệ tọa độ và độ phân giải. Với độ sâu tương đối, phép căn chỉnh tỉ lệ và độ lệch được ước lượng chung cho toàn bộ đoạn video.

Bước thứ ba là tạo các sai lệch có kiểm soát, gồm trôi tỉ lệ, trôi độ lệch, nhiễu thời gian, trễ khung hình, biến dạng cục bộ và làm trơn quá mức. Mỗi dạng sai lệch sẽ có nhiều mức độ từ nhẹ đến nặng, để kiểm tra xem chỉ số có phản ánh đúng mức lỗi hay không.

Bước thứ tư là tính cùng một nhóm chỉ số theo hai giao thức. Giao thức tham chiếu dùng camera và độ sâu chuẩn. Giao thức VGGT dùng camera, tracking và độ tin cậy do VGGT cung cấp. Kết quả sẽ được phân tích bằng bảng, biểu đồ và hình minh họa.

## Slide 8 - Chỉ số đánh giá và kiểm chứng (khoảng 1 phút 25 giây)

Ba nhóm chỉ số là phần trọng tâm của phương pháp.

Nhóm thứ nhất đo độ ổn định vị trí 3D. Nếu cùng một điểm vật lý được theo dõi qua nhiều khung hình, thì sau khi dùng độ sâu và camera để đưa điểm đó vào không gian 3D, các vị trí thu được phải gần nhau. Nếu các vị trí phân tán mạnh, bản đồ độ sâu có thể đang gây biến dạng hình học.

Nhóm thứ hai đo sai số chiếu lại dài hạn. Một điểm 3D từ khung hình nguồn được chiếu sang các khung hình đích, rồi so sánh với vị trí quan sát của điểm tương ứng. Điểm khác so với TAE là đề tài không chỉ xét frame liền kề, mà còn xét các khoảng cách thời gian xa hơn để phát hiện sai lệch tích lũy.

Nhóm thứ ba đo độ trôi tỉ lệ và độ lệch. Sau khi căn chỉnh chung cho toàn bộ video, đề tài phân tích biến động của các tham số affine trên từng frame hoặc cửa sổ thời gian ngắn. Nếu các tham số này thay đổi bất thường, đó có thể là dấu hiệu mô hình mất ổn định về tỉ lệ hoặc độ lệch.

Về kiểm chứng, đề tài đặt trước các tiêu chí định lượng. Tỉ lệ xếp đúng thứ tự cần đạt ít nhất 80 phần trăm, nghĩa là chỉ số phân biệt được mức sai lệch nhẹ, vừa và nặng. Spearman trung vị cần đạt từ 0,7 trở lên trên ít nhất ba trong năm dạng sai lệch. Ngoài ra, tương quan giữa giao thức VGGT và giao thức tham chiếu cần đạt từ 0,6 trở lên. Các ngưỡng này giúp đánh giá phương pháp một cách có hệ thống, thay vì chỉ dựa vào ví dụ trực quan.

## Slide 9 - Kết quả mong đợi (khoảng 1 phút 5 giây)

Kết quả mong đợi đầu tiên là một quy trình đánh giá có thể tái lập, bao gồm chuẩn hóa camera và độ sâu, lọc điểm theo dõi, tính ba nhóm chỉ số và báo cáo kết quả theo định dạng nhất quán.

Kết quả thứ hai là bộ kết quả kiểm chứng định lượng trên TUM RGB-D và Sintel, gồm bảng so sánh, biểu đồ độ nhạy theo mức sai lệch, và so sánh với AbsRel, RMSE và TAE. Điểm cần làm rõ là khi nào các chỉ số hình học 3D cung cấp thông tin bổ sung so với đánh giá từng frame.

Kết quả thứ ba là phân tích điều kiện sử dụng và giới hạn của phương pháp, đặc biệt với vùng động, che khuất, độ dài video và độ tin cậy của điểm theo dõi. Các trường hợp thất bại cũng sẽ được ghi nhận, vì chúng cho biết khi nào không nên tin vào giao thức hỗ trợ.

Kết quả cuối cùng là thử nghiệm test-time adaptation. Một hiệu chỉnh được xem là có ích khi giảm trung vị sai số hình học tham chiếu ít nhất 5 phần trăm, đồng thời không làm AbsRel tăng quá 5 phần trăm.

## Slide 10 - Tài liệu tham khảo và kết thúc (khoảng 45 giây)

Các tài liệu tham khảo chính gồm VGGT, Video Depth Anything, Depth Anything V2 và Depth Any Video với chỉ số TAE. Các công trình này lần lượt cung cấp nền tảng về hình học 3D tổng quát, ước lượng độ sâu video nhất quán thời gian, ước lượng độ sâu từ ảnh, và đánh giá sai số chiếu lại theo thời gian.

Tóm lại, đề tài tập trung vào một khoảng trống cụ thể: độ sâu video có thể mượt theo thời gian nhưng vẫn sai về hình học 3D. Hướng tiếp cận là dùng VGGT như tín hiệu hỗ trợ, không phải ground truth tuyệt đối, để xây dựng và kiểm chứng các chỉ số đánh giá 3D. Nếu thành công, đề tài sẽ tạo ra một quy trình có thể tái lập để phát hiện, giải thích và bước đầu giảm các sai lệch hình học khó nhận ra bằng các chỉ số từng frame.

Xin cảm ơn thầy và các anh chị đã lắng nghe.

## Bản rút gọn nếu cần 5 phút

Nếu cần thu video 5 phút, có thể cắt theo thứ tự ưu tiên sau:

1. Slide 2 chỉ nói một câu về thông tin chung.
2. Slide 3 giữ lại ý "ổn định hình ảnh khác nhất quán hình học 3D", bỏ bớt ví dụ ứng dụng.
3. Slide 7 rút gọn mỗi bước quy trình thành một câu.
4. Slide 8 chỉ giải thích ngắn ba nhóm chỉ số, không đọc đầy đủ ngưỡng kiểm chứng.
5. Slide 10 chỉ tóm tắt đóng góp và lời cảm ơn.

