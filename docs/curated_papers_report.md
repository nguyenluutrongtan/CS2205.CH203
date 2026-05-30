# DANH SÁCH BÀI BÁO KHOA HỌC TIÊU BIỂU (A* CONFERENCES 2025-2026)
## Lĩnh vực NLP/LLM, Computer Vision & Multimodal Learning

Bản báo cáo này tổng hợp 15 nghiên cứu tiêu biểu thuộc các hội nghị khoa học hàng đầu thế giới (A* như CVPR, ICLR, ICCV, NeurIPS) trong giai đoạn **2025-2026**. Các bài báo được lựa chọn dựa trên 3 tiêu chí cốt lõi:
1. **Uy tín học thuật:** Thuộc các hội nghị A*, đạt giải thưởng lớn hoặc có đóng góp đột phá.
2. **Tính trực quan, dễ tiếp cận:** Luồng ý tưởng rõ ràng, không bị quá nặng bởi toán học lý thuyết phức tạp.
3. **Tiềm năng phát triển (Extensibility):** Có mã nguồn mở hoàn chỉnh, cấu trúc mô-đun và rất thuận tiện để làm nền tảng phát triển tiếp các đề tài mới.

---

## 📊 Bảng Tổng Quan Các Bài Báo Khoa Học

| Tên Bài Báo | Hội Nghị | Phân Loại | Đặc Điểm Nổi Bật | Mã Nguồn |
| :--- | :---: | :---: | :--- | :---: |
| **GSM-Symbolic** | (ICLR 2025) | NLP/LLM | Đánh giá độ bền bỉ lập luận toán học của LLM | [Có] |
| **PlanSearch** | (ICLR 2025) | NLP/LLM | Thuật toán sinh code qua ngôn ngữ tự nhiên | [Có] |
| **Test-Time Compute** | (ICLR 2025 - Oral) | NLP/LLM | Chiến lược phân bổ tính toán động lúc suy luận | [Có] |
| **SORRY-Bench** | (ICLR 2025) | NLP/LLM | Khung đánh giá an toàn AI thực tiễn | [Có] |
| **Gated Attention** | (NeurIPS 2025 - Best) | NLP/LLM | Cải tiến kiến trúc Transformer không đổi | [Có] |
| **VGGT** | (CVPR 2025 - Best) | Computer Vision | Ước lượng hình học 3D một lượt feed-forward | [Có] |
| **MegaSaM** | (CVPR 2025 - Hon. Mention) | Computer Vision | Học sâu SLAM từ video thực tế chứa động | [Có] |
| **Video Depth Anything** | (CVPR 2025 - Highlight) | Computer Vision | Ước lượng độ sâu nhất quán thời gian cho video | [Có] |
| **3D Student Splatting** | (CVPR 2025 - Hon. Mention) | Computer Vision | Tối ưu hóa 3D Gaussian Splatting giảm tham số | [Có] |
| **Ref-Gaussian** | (ICLR 2025) | Computer Vision | Tái tạo bề mặt gương phản xạ bằng Gaussian | [Có] |
| **Eagle** | (ICLR 2025) | Multimodal | Kết hợp đa dạng Visual Encoders gọn nhẹ | [Có] |
| **VLM2Vec** | (ICLR 2025) | Multimodal | Biến đổi VLM thành mô hình nhúng đa năng | [Có] |
| **LLaVA-CoT** | (ICCV 2025) | Multimodal | Suy luận hình ảnh đa bước có cấu trúc | [Có] |
| **FLAIR** | (CVPR 2025) | Multimodal | Nhúng cục bộ đặc trưng CLIP định hướng văn bản | [Có] |
| **OmniGen** | (CVPR 2025) | Multimodal | Mô hình sinh/sửa ảnh thống nhất chuỗi-sang-chuỗi | [Có] |

---

## 📑 Chi Tiết Từng Lĩnh Vực

### 1. Phân Khúc 1: NLP & Large Language Models (NLP/LLM)

> [!NOTE]
> Các bài báo NLP/LLM giai đoạn 2025-2026 tập trung giải quyết các bài toán mang tính thực tiễn cao: độ bền bỉ trong suy luận toán học, cải tiến kiến trúc cốt lõi đạt hiệu năng tối ưu, và kỹ thuật tính toán tại thời điểm suy luận (Test-Time Compute).

#### 📄 [GSM-Symbolic: Understanding the Limitations of Mathematical Reasoning in Large Language Models](https://openreview.net/forum?id=p5nK2xGq2X)
*   **Hội nghị:** (ICLR 2025)
*   **Link thay thế:** [arXiv](https://arxiv.org/abs/2410.05229)
*   **Đánh giá khả năng nghiên cứu phát triển:**
    *   **Dễ hiểu:** Ý tưởng cốt lõi cực kỳ trực quan và thực tế. Thay vì đánh giá các mô hình ngôn ngữ lớn trên một bộ dữ liệu toán học tĩnh (GSM8K) dễ bị rò rỉ dữ liệu (contamination), tác giả tạo ra một công cụ sinh ký hiệu (symbolic generator) để tạo các biến thể câu hỏi (thay đổi tên, số hoặc thông tin gây nhiễu). Bài báo không dùng toán lý thuyết nặng nề, giúp người đọc nắm bắt nhanh.
    *   **Phát triển tiếp:** Các mẫu sinh câu hỏi và mã nguồn được mở hoàn toàn. Ý tưởng này có thể dễ dàng mở rộng sang các nhiệm vụ lập luận khác (lập luận logic, viết mã nguồn, hoặc ngôn ngữ khác như tiếng Việt) để đánh giá độ bền bỉ (robustness) của các mô hình LLM hiện nay.

#### 📄 [Planning in Natural Language Improves LLM Search for Code Generation (PlanSearch)](https://openreview.net/forum?id=Vv81a5V1vJ)
*   **Hội nghị:** (ICLR 2025)
*   **Link thay thế:** [arXiv](https://arxiv.org/abs/2409.03733)
*   **Đánh giá khả năng nghiên cứu phát triển:**
    *   **Dễ hiểu:** Giải quyết vấn đề thiếu sự đa dạng (diversity) khi các mô hình LLM sinh code (lặp lại các lỗi tương tự nhau khi tăng số lượng sample). Thay vì tìm kiếm trực tiếp trên không gian mã nguồn phức tạp, PlanSearch dịch chuyển không gian tìm kiếm sang các ý tưởng viết bằng ngôn ngữ tự nhiên (viết ra các quan sát, lập kế hoạch bằng lời trước khi sinh code), rất giống với cách con người giải quyết bài toán lập trình.
    *   **Phát triển tiếp:** PlanSearch cung cấp một thuật toán tìm kiếm trên không gian ý tưởng rất tường minh và có mã nguồn mở trên GitHub. Hướng nghiên cứu này cực kỳ tiềm năng để áp dụng cho các tác vụ lập luận khác ngoài sinh code (như giải toán, viết luận), hoặc tích hợp với các bộ kiểm thử tự động (automatic verifiers) nhằm nâng cao tỷ lệ chính xác.

#### 📄 [Scaling LLM Test-Time Compute Optimally Can be More Effective than Scaling Parameters for Reasoning](https://openreview.net/forum?id=9t1D4nLd1F)
*   **Hội nghị:** (ICLR 2025 - Oral)
*   **Link thay thế:** [arXiv](https://arxiv.org/abs/2408.03314)
*   **Đánh giá khả năng nghiên cứu phát triển:**
    *   **Dễ hiểu:** Trả lời một câu hỏi vô cùng thực tế: Thay vì huấn luyện một mô hình cực lớn, liệu chúng ta có thể đạt hiệu năng tương đương bằng cách cho mô hình nhỏ "suy nghĩ" nhiều hơn tại thời điểm suy luận (inference-time)? Khái niệm về các chiến lược như "Best-of-N" hay tìm kiếm dựa trên bộ kiểm chứng (verifier-based search) được trình bày hệ thống, phản ánh đúng xu thế phát triển của các dòng mô hình như OpenAI o1/o3.
    *   **Phát triển tiếp:** Bài báo đặt nền móng thực nghiệm vững chắc cho việc tối ưu hóa tài nguyên tính toán lúc suy luận (test-time compute). Các hướng phát triển tiếp theo bao gồm việc thiết kế các bộ kiểm chứng (verifiers) hiệu quả hơn, áp dụng cho các bài toán đa bước (multi-turn), hoặc tối ưu hóa chiến lược phân bổ tính toán động cho từng loại câu hỏi khó/dễ.

#### 📄 [SORRY-Bench: Systematically Evaluating Large Language Model Safety Refusal Behaviors](https://openreview.net/forum?id=YfKNaRktan)
*   **Hội nghị:** (ICLR 2025)
*   **Link thay thế:** [arXiv](https://arxiv.org/abs/2406.14598)
*   **Đánh giá khả năng nghiên cứu phát triển:**
    *   **Dễ hiểu:** Đề tài về an toàn AI (AI Safety) rất gần gũi và thiết thực. Bài báo đề xuất một bộ benchmark để đánh giá xem mô hình LLM từ chối các yêu cầu độc hại tốt đến đâu mà không bị "nhạy cảm quá đà" (từ chối cả các yêu cầu lành tính). Phương pháp thiết kế bộ phân loại gồm 45 lớp và việc sử dụng các mô hình nhỏ (7B) được tinh chỉnh để làm bộ đánh giá tự động (evaluator) có quy trình cực kỳ rõ ràng, thực tiễn.
    *   **Phát triển tiếp:** Mã nguồn, bộ dữ liệu và các mô hình đánh giá đều được công bố công khai trên GitHub và Hugging Face. Đây là cơ sở tuyệt vời để các nhóm nghiên cứu mở rộng sang việc đánh giá an toàn cho các ngôn ngữ bản địa (như tiếng Việt), thử nghiệm các phương pháp tấn công jailbreak mới, hoặc phát triển các kỹ thuật căn chỉnh an toàn (safety alignment) hiệu quả hơn.

#### 📄 [Gated Attention for Large Language Models: Non-linearity, Sparsity, and Attention-Sink-Free](https://openreview.net/forum?id=e7VbVNzDkl)
*   **Hội nghị:** (NeurIPS 2025 - Best Paper Award)
*   **Link thay thế:** [arXiv](https://arxiv.org/abs/2505.06708)
*   **Đánh giá khả năng nghiên cứu phát triển:**
    *   **Dễ hiểu:** Bài báo tập trung vào một thay đổi kiến trúc rất cụ thể và trực quan—thêm một cổng sigmoid (sigmoid gate) ngay sau phép toán Scaled Dot-Product Attention (SDPA). Cách giải thích và phân tích về "attention sink" (các token đầu tiên nhận quá nhiều chú ý thừa) rất rõ ràng và dễ tiếp cận mà không yêu cầu quá nhiều kiến thức toán học phức tạp.
    *   **Phát triển tiếp:** Vì đây là một cải tiến về mặt kiến trúc dạng mô-đun (modular architecture), các nhà nghiên cứu có thể dễ dàng tích hợp cổng chú ý này vào các kiến trúc Transformer hiện có (như LLaMA, Mistral) hoặc kết hợp với các kỹ thuật nén mô hình khác (như quantization, pruning) để đánh giá hiệu năng. Nhóm tác giả cũng đã mở nguồn mã nguồn và các checkpoint mô hình trên Hugging Face.

---

### 2. Phân Khúc 2: Computer Vision (CV)

> [!TIP]
> Lĩnh vực Computer Vision năm 2025 chứng kiến những cải tiến vượt bậc về ước lượng hình học 3D thời gian thực, phục dựng cảnh tĩnh và động tốc độ cao (như các cải tiến đột phá từ 3D Gaussian Splatting) và xử lý video nhất quán.

#### 📄 [VGGT: Visual Geometry Grounded Transformer](https://arxiv.org/abs/2503.11651)
*   **Hội nghị:** (CVPR 2025 - Best Paper Award)
*   **Đánh giá khả năng nghiên cứu phát triển:**
    *   **Dễ hiểu:** VGGT đề xuất một kiến trúc Transformer thuần túy cực kỳ trực quan, giúp ước lượng trực tiếp các thuộc tính hình học 3D (thông số camera, bản đồ độ sâu, luồng điểm 3D) từ hình ảnh chỉ bằng một lượt feed-forward duy nhất. Bài báo đã loại bỏ hoàn toàn các bước tối ưu hóa hậu kỳ phức tạp, giảm thời gian xử lý xuống hàng giây, giúp luồng đi của thuật toán đơn giản hóa hơn rất nhiều.
    *   **Phát triển tiếp:** Được Meta nghiên cứu và mở nguồn mã nguồn đầy đủ, giúp cộng đồng dễ dàng kế thừa, tùy biến hoặc tích hợp trực tiếp vào các hệ thống robotics, thực tế ảo (AR/VR) và mô hình sinh 3D thế hệ mới.

#### 📄 [MegaSaM: Accurate, Fast and Robust Structure and Motion from Casual Dynamic Videos](https://arxiv.org/abs/2412.04463)
*   **Hội nghị:** (CVPR 2025 - Best Paper Honorable Mention)
*   **Đánh giá khả năng nghiên cứu phát triển:**
    *   **Dễ hiểu:** Giải quyết một bài toán vô cùng thực tế: ước lượng quỹ đạo camera và bản đồ độ sâu từ các video quay tự do chứa vật thể chuyển động. Thay vì phụ thuộc vào các thuật toán SLAM/SfM truyền thống phức tạp, MegaSaM đề xuất một khung SLAM học sâu đơn giản và hiệu quả cao bằng cách tối ưu hóa độ sâu và luồng quang học nhất quán. Bài viết mạch lạc, rõ ràng.
    *   **Phát triển tiếp:** Mã nguồn và các script suy diễn được mở nguồn đầy đủ trên GitHub, cung cấp nền tảng lý tưởng cho các nghiên cứu mở rộng trong lĩnh vực camera tracking, dựng hình 3D động và robotics.

#### 📄 [Video Depth Anything: Consistent Depth Estimation for Super-Long Videos](https://arxiv.org/abs/2501.12375)
*   **Hội nghị:** (CVPR 2025 - Highlight Paper)
*   **Đánh giá khả năng nghiên cứu phát triển:**
    *   **Dễ hiểu:** Là bài báo nổi bật kế thừa trực tiếp từ sự thành công của Depth Anything V2. Bài báo giải quyết vấn đề không nhất quán thời gian (temporal inconsistency) khi ước lượng độ sâu cho video dài bằng chiến lược suy diễn dựa trên khung hình khóa (key-frame) và một hàm loss nhất quán thời gian cực kỳ tối giản. Phương pháp này loại bỏ hoàn toàn các tiên nghiệm hình học phức tạp.
    *   **Phát triển tiếp:** Code được mở nguồn đầy đủ kèm theo các mô hình pre-trained chạy thời gian thực (30 FPS), rất dễ dàng để tích hợp hoặc mở rộng cho các bài toán xử lý video nâng cao khác như tạo hiệu ứng chiều sâu thời gian thực, robot định vị.

#### 📄 [3D Student Splatting and Scooping](https://arxiv.org/abs/2503.10148)
*   **Hội nghị:** (CVPR 2025 - Best Paper Honorable Mention)
*   **Đánh giá khả năng nghiên cứu phát triển:**
    *   **Dễ hiểu:** Bài báo đưa ra một cải tiến mang tính cách mạng cho 3D Gaussian Splatting (3DGS). Thay vì dùng phân phối Gauss tiêu chuẩn và chỉ cộng thêm mật độ (splatting), nhóm tác giả đề xuất sử dụng phân phối Student's t linh hoạt hơn, đồng thời cho phép cả mật độ âm (scooping - trừ bớt mật độ). Ý tưởng toán học và hình học này rất tường minh và trực quan, giúp tăng vượt trội độ sắc nét và giảm tới 82% số lượng tham số.
    *   **Phát triển tiếp:** Mã nguồn được tác giả chia sẻ công khai trên GitHub, là một hướng đi đột phá nhưng cực kỳ dễ tiếp cận để phát triển tiếp các biến thể tối ưu dung lượng và tốc độ của 3DGS phục vụ các thiết bị di động.

#### 📄 [Reflective Gaussian Splatting (Ref-Gaussian)](https://arxiv.org/abs/2412.19282)
*   **Hội nghị:** (ICLR 2025)
*   **Đánh giá khả năng nghiên cứu phát triển:**
    *   **Dễ hiểu:** Giải quyết điểm yếu chí mạng của 3DGS truyền thống khi tái tạo các vật thể có bề mặt phản xạ gương bóng. Bài báo đề xuất mô hình kết xuất trì hoãn dựa trên vật lý (deferred rendering - BRDF) kết hợp với cơ chế phản xạ liên kết Gaussian tinh gọn mà không cần dùng đến Monte Carlo phức tạp. Các khái niệm hình học và vật lý trong bài báo được giải thích rõ ràng và có tính trực quan cao.
    *   **Phát triển tiếp:** Mã nguồn mở của dự án giúp các nhà nghiên cứu dễ dàng ứng dụng và mở rộng sang các nghiên cứu về mô phỏng vật liệu động, chiếu sáng và thực tế ảo.

---

### 3. Phân Khúc 3: Multimodal Learning (Đa phương thức)

> [!IMPORTANT]
> Học đa phương thức (Multimodal Learning) là phân khúc nóng nhất trong năm 2025-2026. Các bài nghiên cứu tập trung mạnh mẽ vào khả năng tối ưu hóa kiến trúc visual encoders (nhiều bộ mã hóa đồng thời), thiết lập embedding đa phương thức cho tìm kiếm thông minh, và suy luận chuỗi tư duy hình ảnh (Vision Chain-of-Thought).

#### 📄 [Eagle: Exploring The Design Space for Multimodal LLMs with Mixture of Encoders](https://arxiv.org/abs/2408.15998)
*   **Hội nghị:** (ICLR 2025)
*   **Đánh giá khả năng nghiên cứu phát triển:**
    *   **Dễ hiểu:** Tập trung vào một câu hỏi kiến trúc cực kỳ cơ bản và trực quan: Làm thế nào để kết hợp nhiều visual encoder khác nhau (như CLIP, ConvNeXt, SAM) và độ phân giải khác nhau một cách hiệu quả nhất. Phương pháp đề xuất rất đơn giản: chỉ cần "nối" (concatenate) các visual token lại với nhau mà không cần các bộ trộn (mixer) phức tạp, giúp người đọc dễ nắm bắt bản chất.
    *   **Phát triển tiếp:** Ý tưởng thiết kế mô hình dạng "Mixture-of-Encoders" cực kỳ dễ tái cấu trúc và mở rộng. Mã nguồn mở của NVIDIA được cấu trúc rõ ràng, cho phép các nhà nghiên cứu dễ dàng cắm-rút (plug-and-play) các vision encoder mới hoặc tự phát triển các cấu hình visual experts riêng để tối ưu hóa hiệu năng cho các tác vụ chuyên biệt (như OCR, phân tích tài liệu).

#### 📄 [VLM2Vec: Training Vision-Language Models for Massive Multimodal Embedding Tasks](https://arxiv.org/abs/2410.05160)
*   **Hội nghị:** (ICLR 2025)
*   **Đánh giá khả năng nghiên cứu phát triển:**
    *   **Dễ hiểu:** Bài báo giải quyết vấn đề biến đổi các mô hình VLM tự hồi quy (như Phi-3.5-V, LLaVA) thành một mô hình nhúng (embedding) đa phương thức đa năng thông qua phương pháp huấn luyện tương phản (contrastive learning) quen thuộc. Luồng xử lý dữ liệu và mục tiêu tối ưu rất trực quan, dễ dàng liên hệ với các mô hình text embedding truyền thống.
    *   **Phát triển tiếp:** Mã nguồn mở đầy đủ (cung cấp bởi TIGER-AI-Lab) đi kèm với bộ benchmark MMEB rất toàn diện gồm 36 bộ dữ liệu. Kiến trúc này cực kỳ thích hợp để mở rộng: bạn có thể dễ dàng thay thế backbone VLM bằng các mô hình mới hơn hoặc huấn luyện thêm trên các tập dữ liệu nhúng tùy chỉnh cho các ứng dụng thực tế như tìm kiếm đa phương thức (multimodal retrieval) hay RAG.

#### 📄 [LLaVA-CoT: Let Vision Language Models Reason Step-by-Step](https://arxiv.org/abs/2411.10440)
*   **Hội nghị:** (ICCV 2025)
*   **Đánh giá khả năng nghiên cứu phát triển:**
    *   **Dễ hiểu:** Thay vì áp dụng cơ chế suy luận chuỗi suy nghĩ (Chain-of-Thought - CoT) một cách phức tạp, bài báo chia quá trình suy luận hình ảnh thành các bước rõ ràng, mô phỏng tư duy con người: Tóm tắt (Summarization) -> Diễn giải trực quan (Visual Interpretation) -> Suy luận logic (Logical Reasoning) -> Kết luận (Conclusion). Cách tiếp cận này trực quan, có định dạng cấu trúc rất rõ ràng.
    *   **Phát triển tiếp:** Nhóm nghiên cứu cung cấp tập dữ liệu chất lượng cao LLaVA-CoT-100k cùng mã nguồn huấn luyện và suy luận chi tiết. Đây là nền tảng hoàn hảo để nghiên cứu sâu hơn về suy luận đa bước trên VLMs (như thiết kế lại quy trình suy luận, áp dụng các kỹ thuật học tăng cường - RLHF/DPO để tối ưu hóa chuỗi suy nghĩ hình ảnh, hoặc mở rộng sang các bài toán tác nhân - Agent).

#### 📄 [FLAIR: VLM with Fine-grained Language-informed Image Representations](https://arxiv.org/abs/2412.03561)
*   **Hội nghị:** (CVPR 2025)
*   **Đánh giá khả năng nghiên cứu phát triển:**
    *   **Dễ hiểu:** Tập trung giải quyết điểm yếu cố hữu của CLIP: chỉ căn chỉnh ảnh-văn bản ở cấp độ toàn cục (global) dẫn đến mất chi tiết cục bộ. Giải pháp đề xuất là thêm một module "text-conditioned attention pooling" rất trực quan lên trên các local image tokens để trích xuất các đặc trưng chi tiết theo ngữ cảnh văn bản tương ứng.
    *   **Phát triển tiếp:** Phương pháp này hoạt động như một tiện ích mở rộng gọn nhẹ cho CLIP mà không cần huấn luyện lại từ đầu toàn bộ mô hình lớn. Với mã nguồn mở đầy đủ trên GitHub, các nhà nghiên cứu có thể dễ dàng tích hợp FLAIR vào các hệ thống thị giác máy tính hiện tại để cải thiện hiệu năng của các tác vụ downstream đòi hỏi độ chính xác cao như phân vùng ngữ nghĩa zero-shot (zero-shot semantic segmentation) hay tìm kiếm ảnh chi tiết.

#### 📄 [OmniGen: Unified Image Generation](https://arxiv.org/abs/2409.11340)
*   **Hội nghị:** (CVPR 2025)
*   **Đánh giá khả năng nghiên cứu phát triển:**
    *   **Dễ hiểu:** OmniGen đơn giản hóa thế giới tạo ảnh phức tạp (vốn đang bị phân mảnh bởi ControlNet, IP-Adapter, v.v.) bằng cách đề xuất một mô hình diffusion thống nhất duy nhất. Nó xử lý mọi tác vụ sinh ảnh, chỉnh sửa ảnh, sinh ảnh theo mẫu (subject-driven) dưới dạng bài toán chuỗi-sang-chuỗi (sequence-to-sequence) trực quan: nhận đầu vào là ảnh/chữ bất kỳ và sinh ra ảnh đầu ra.
    *   **Phát triển tiếp:** Mã nguồn mở của dự án cực kỳ thân thiện với cộng đồng (được hỗ trợ rộng rãi bao gồm cả các giao diện trực quan như ComfyUI). Nhờ loại bỏ nhu cầu thiết kế các module phụ trợ phức tạp, nhà phát triển có thể dễ dàng tinh chỉnh (fine-tune) mô hình này trên các tập dữ liệu cá nhân hóa hoặc mở rộng khả năng điều khiển sinh ảnh bằng các loại điều kiện đầu vào mới chỉ thông qua việc thay đổi định dạng prompt.
