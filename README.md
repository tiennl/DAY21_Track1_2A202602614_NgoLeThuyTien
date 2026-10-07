# Lab 21 — Phân tích rủi ro AI qua case study thực tế

- Họ và tên: Ngô Lê Thuỷ Tiên
- MSSV / mã học viên: 2A202602614
- Lớp: Track 1 - H201
- Ngành đã chọn: HR / tuyển dụng (AI sàng lọc CV, đánh giá và hỗ trợ tuyển ứng viên)

### 1. Industry Risk Snapshot

| Nội dung | Đánh giá của tôi và lý do |
| --- | --- |
| Những tác hại chính có thể xảy ra | **Phân biệt đối xử** theo giới tính, tuổi, chủng tộc, khuyết tật khi AI học từ dữ liệu tuyển dụng lịch sử vốn đã thiên lệch. <br/>**Mất cơ hội việc làm** của ứng viên đủ năng lực bị loại tự động mà không biết lý do. <br/>**Thiếu minh bạch và không khiếu nại được** (ứng viên không biết mình bị máy chấm). <br/>**Rủi ro pháp lý, uy tín** cho doanh nghiệp và nhà cung cấp phần mềm. <br/>**Thiệt hại cho doanh nghiệp**: bỏ lỡ ứng viên tốt, giảm đa dạng nhân sự. |
| Mức độ high-stakes | **Cao.** <br/>Quyết định tuyển dụng ảnh hưởng trực tiếp đến thu nhập, sinh kế và cơ hội nghề nghiệp. <br/>Một hệ thống sàng lọc được dùng cho hàng nghìn nhà tuyển dụng thì một lỗi thiên lệch nhân lên thành hàng triệu quyết định (xem case 3). <br/>EU AI Act (Phụ lục III) xếp AI dùng trong tuyển dụng và quản lý lao động vào nhóm **rủi ro cao**. <br/>Luật Trí tuệ nhân tạo của Việt Nam (thông qua 10/12/2025, hiệu lực 01/03/2026) cũng quản lý theo mức rủi ro dựa trên tác động tới quyền con người. <br/>Tuy nhiên, Danh mục hệ thống AI có rủi ro cao ban hành kèm Quyết định 33/2026/QĐ-TTg hiện gồm 6 lĩnh vực (giáo dục, dân tộc và tôn giáo, y tế, ngân hàng, tố tụng, giao thông vận tải) và **chưa có mục riêng cho tuyển dụng / nhân sự**. <br/>*Nhận định của tôi:* về mặt tác động, AI sàng lọc ứng viên vẫn nên được quản lý như hệ thống rủi ro cao (giống cách EU làm), dù pháp luật Việt Nam hiện chưa xếp nó vào danh mục này. |
| Dữ liệu nhạy cảm có thể được sử dụng | **Dữ liệu trực tiếp:** ngày sinh/tuổi, giới tính, dân tộc, tình trạng sức khỏe/khuyết tật, ảnh và video khuôn mặt, giọng nói (phỏng vấn video), địa chỉ, tình trạng hôn nhân. <br/>**Dữ liệu proxy** gián tiếp suy ra thuộc tính nhạy cảm: tên trường (trường nữ sinh), câu lạc bộ, khoảng trống trong lịch sử làm việc (có thể liên quan khuyết tật, thai sản), năm tốt nghiệp (suy ra tuổi). <br/>Việc thu thập, xử lý dữ liệu cá nhân của ứng viên tại Việt Nam chịu sự điều chỉnh của Luật Bảo vệ dữ liệu cá nhân số 91/2025/QH15 (hiệu lực 01/01/2026) và Nghị định 356/2025/NĐ-CP. |
| Nhu cầu human review | **Cao.** <br/>(1) **Trước khi triển khai:** đội HR, pháp chế và data kiểm tra bias audit theo từng nhóm (tỷ lệ chọn theo giới, tuổi...), rà soát các trường đầu vào và quy tắc lọc cứng. <br/>(2) **Trong vận hành:** recruiter xem lại các hồ sơ bị AI loại, ít nhất theo mẫu ngẫu nhiên. AI chỉ nên xếp hạng/gợi ý, không tự động từ chối. <br/>(3) **Sau quyết định:** ứng viên có kênh yêu cầu người thật xem lại. <br/>**Lý do:** lỗi thiên lệch thường **vô hình** với từng ứng viên, chỉ phát hiện khi có người nhìn số liệu tổng hợp. |

### 2. Case study 1 — Amazon: công cụ AI chấm CV thiên lệch giới tính

#### Brief Case

- Tổ chức / sản phẩm AI: Amazon, công cụ chấm điểm CV nội bộ dùng machine learning (không công bố tên).
- Thời gian, địa điểm / bối cảnh: Phát triển từ 2014 tại Edinburgh (Scotland) cho tuyển dụng của Amazon. Phát hiện vấn đề năm 2015, giải tán nhóm đầu 2017. Reuters công bố tháng 10/2018.
- AI được dùng để làm gì: Đọc CV và chấm điểm ứng viên từ 1 đến 5 sao để tự động tìm "người giỏi nhất", chủ yếu cho vị trí kỹ sư phần mềm và kỹ thuật.
- Vấn đề hoặc sự kiện đáng chú ý: Mô hình học từ CV nộp vào Amazon trong **10 năm**, phần lớn là của nam giới. Kết quả là nó **trừ điểm CV có chữ "women's"** (ví dụ "women's chess club captain") và **hạ điểm tốt nghiệp của hai trường đại học dành cho nữ**. Amazon sửa để các từ đó trung lập nhưng không bảo đảm mô hình không tìm cách phân biệt khác, nên đã bỏ dự án.
- Số liệu có nguồn: Dữ liệu huấn luyện là CV trong **10 năm**. Thang điểm **1–5 sao**. Vấn đề phát hiện năm **2015**, nhóm giải tán đầu **2017** (Reuters, 10/10/2018). Reuters không công bố số ứng viên bị ảnh hưởng.
- Nguồn: "Amazon scraps secret AI recruiting tool that showed bias against women" — Jeffrey Dastin, Reuters — 10/10/2018 — https://www.reuters.com/article/us-amazon-com-jobs-automation-insight-idUSKCN1MK08G (bản đăng lại: https://www.hrreporter.com/focus-areas/recruitment-and-staffing/amazon-scraps-secret-ai-recruiting-tool-that-showed-bias-against-women/287026). Hồ sơ AI Incident Database: https://incidentdatabase.ai/reports/611
- Phân biệt bằng chứng và nhận định:
  - **Nguồn xác nhận:** mô hình thiên lệch theo giới, học từ dữ liệu 10 năm chủ yếu là nam, Amazon đã bỏ công cụ. Theo Reuters, recruiter **có xem** gợi ý của công cụ nhưng **không chỉ dựa vào** xếp hạng đó.
  - **Nhận định của tôi / chưa rõ:** không có bằng chứng công khai rằng ứng viên cụ thể nào bị loại vì công cụ. Vì vậy tác hại với ứng viên là **nguy cơ**, không phải hậu quả đã chứng minh.

#### Harm Map Worksheet

| Trường | Phân tích của tôi |
| --- | --- |
| High-risk moment | Bước sàng lọc đầu vào: AI chấm sao hàng loạt CV cho vị trí kỹ thuật, recruiter dùng điểm này để quyết định xem hồ sơ nào trước. |
| Stakeholder bị ảnh hưởng | Ứng viên nữ (trực tiếp); recruiter và Amazon (rủi ro pháp lý, uy tín, mất ứng viên giỏi); ngành công nghệ nói chung (củng cố mất cân bằng giới). |
| Failure mode | **Bias / thiên lệch** do học lại mẫu lịch sử; mô hình bám vào **proxy** của giới tính (từ "women's", tên trường nữ). |
| Layer bắt đầu lỗi | **Model** (gồm dữ liệu huấn luyện): mô hình học từ dữ liệu thiên lệch nên tự rút ra đặc trưng phân biệt giới. <br/>Không phải lỗi giao diện hay grounding. |
| Harm xảy ra là gì? | **Đã xảy ra (có nguồn):** mô hình cho điểm thấp hơn với CV có dấu hiệu là nữ. Amazon mất công phát triển khoảng 3 năm và bị ảnh hưởng uy tín khi Reuters đưa tin. <br/>**Nguy cơ (nhận định):** nếu công cụ được dùng rộng, ứng viên nữ đủ năng lực bị xếp sau hoặc bị bỏ qua. |
| Harm lens | Allocative harm (phân bổ cơ hội việc làm không công bằng); phân biệt đối xử theo giới; rủi ro pháp lý/uy tín cho tổ chức. |
| Severity | **High.** Ảnh hưởng tới cơ hội việc làm là quyền lợi quan trọng. <br/>Không lên Critical vì không có bằng chứng hồ sơ bị loại hoàn toàn tự động. |
| Scale | **Lớn (tiềm năng).** Amazon nhận lượng CV rất lớn mỗi năm. <br/>Tuy nhiên Reuters không công bố số hồ sơ đã chấm, nên quy mô thực tế chưa xác định. |
| Probability | **Cao** nếu dùng: thiên lệch là hệ thống (xảy ra với mọi CV có đặc trưng đó), không phải lỗi ngẫu nhiên. |
| Frequency | **Liên tục** trong thời gian dùng: mỗi lần chấm CV có đặc trưng liên quan nữ giới đều có thể bị trừ điểm. |
| Vì sao? | Reuters (dựa trên 5 nguồn nội bộ) mô tả rõ cơ chế lỗi. <br/>Mức Severity/Probability đánh giá theo cơ chế đó. <br/>Giới hạn: không có số liệu về ứng viên bị ảnh hưởng thật, Amazon không công bố chi tiết mô hình. <br/>Bài học: xóa trường "giới tính" là chưa đủ vì mô hình vẫn tìm ra proxy. |

### 3. Case study 2 — iTutorGroup: phần mềm tự động loại ứng viên lớn tuổi (EEOC)

#### Brief Case

- Tổ chức / sản phẩm AI: iTutorGroup (iTutorGroup, Inc., Shanghai Ping An Intelligent Education Technology và Tutor Group Limited), phần mềm nhận hồ sơ ứng tuyển gia sư.
- Thời gian, địa điểm / bối cảnh: Hồ sơ nộp khoảng cuối 3 đến đầu 4/2020. Ứng viên ở Mỹ ứng tuyển dạy tiếng Anh online cho học viên ở Trung Quốc. EEOC (Ủy ban Cơ hội việc làm bình đẳng Hoa Kỳ) khởi kiện 5/2022, dàn xếp 8/2023.
- AI được dùng để làm gì: Tự động lọc hồ sơ ứng viên gia sư (công ty tuyển hàng nghìn gia sư mỗi năm).
- Vấn đề hoặc sự kiện đáng chú ý: Phần mềm được lập trình để **tự động từ chối ứng viên nữ từ 55 tuổi và nam từ 60 tuổi**. Vụ việc lộ ra khi một ứng viên nộp hai hồ sơ giống hệt nhau, chỉ khác ngày sinh. Hồ sơ ghi ngày sinh thật bị từ chối ngay, hồ sơ ghi ngày sinh trẻ hơn thì được mời phỏng vấn.
- Số liệu có nguồn: **Hơn 200** ứng viên đủ điều kiện ở Mỹ bị từ chối vì tuổi. Dàn xếp **$365,000** chia cho ứng viên bị từ chối. Thỏa thuận dàn xếp ký ngày **09/08/2023**. Đây được coi là vụ dàn xếp đầu tiên của EEOC liên quan AI trong tuyển dụng.
- Nguồn:
  - "The EEOC Settles Its First Lawsuit Alleging AI-Based Discrimination in Employment" — Baker Donelson — 2023 — https://www.bakerdonelson.com/the-eeoc-settles-its-first-lawsuit-alleging-ai-based-discrimination-in-employment
  - "US EEOC's first settlement in AI hiring discrimination" — Norton Rose Fulbright — 2023 — https://nortonrosefulbright.com/en/knowledge/publications/2ec12415/us-eeocs-first-settlement-in-ai-hiring-discrimination
- Phân biệt bằng chứng và nhận định:
  - **Nguồn xác nhận:** EEOC cáo buộc quy tắc loại theo tuổi, nêu con số hơn 200 ứng viên, và hai bên dàn xếp $365,000 kèm cam kết: ngừng hỏi ngày sinh trước khi mời làm việc, đào tạo chống phân biệt đối xử, mời ứng viên bị loại nộp lại.
  - **Lưu ý:** iTutorGroup **không thừa nhận sai phạm** khi dàn xếp, nên đây là cáo buộc đã được dàn xếp, không phải phán quyết của tòa.
  - **Nhận định của tôi:** "AI" ở đây thực chất là **quy tắc lọc cứng** do con người cài, không phải mô hình tự học. Case này cho thấy tự động hóa làm một quy tắc phân biệt đối xử chạy hàng loạt mà không ai kiểm tra.

#### Harm Map Worksheet

| Trường | Phân tích của tôi |
| --- | --- |
| High-risk moment | Ngay lúc nộp hồ sơ: phần mềm đọc ngày sinh và **tự động từ chối**, không có người xem. |
| Stakeholder bị ảnh hưởng | Ứng viên lớn tuổi (nữ từ 55, nam từ 60) ở Mỹ; iTutorGroup (tiền dàn xếp, nghĩa vụ tuân thủ, uy tín); học viên (mất gia sư đủ năng lực). |
| Failure mode | **Phân biệt đối xử trực tiếp** được mã hóa thành quy tắc tự động; thu thập dữ liệu nhạy cảm (ngày sinh) không cần thiết cho bước sàng lọc. |
| Layer bắt đầu lỗi | **Safety** (theo cách hiểu rộng là lớp kiểm soát/chính sách): không có cơ chế kiểm tra quy tắc lọc trước khi chạy, không có người duyệt quyết định từ chối. <br/>Không phải lỗi Model vì quy tắc do con người đặt. <br/>*Giới hạn:* nguồn không mô tả chi tiết kỹ thuật phần mềm. |
| Harm xảy ra là gì? | **Đã xảy ra (theo cáo buộc của EEOC, đã dàn xếp):** hơn 200 ứng viên đủ năng lực bị từ chối việc làm vì tuổi trong khoảng 3–4/2020. iTutorGroup trả $365,000. |
| Harm lens | Allocative harm (mất cơ hội việc làm, thu nhập); phân biệt đối xử theo tuổi (vi phạm ADEA theo cáo buộc); xâm phạm quyền riêng tư (dùng ngày sinh để sàng lọc). |
| Severity | **High.** Ứng viên bị từ chối hoàn toàn tự động, không có cơ hội được xem xét. |
| Scale | **Trung bình:** hơn 200 người được xác định trong khoảng vài tuần. <br/>Con số thật có thể lớn hơn vì quy tắc có thể đã chạy lâu hơn (nhận định, chưa có nguồn). |
| Probability | **Gần như chắc chắn** với mọi ứng viên thuộc nhóm tuổi đó, vì đây là quy tắc cứng. |
| Frequency | **Mỗi lần** có ứng viên thuộc nhóm tuổi nộp hồ sơ. |
| Vì sao? | Căn cứ vào nội dung vụ kiện và thỏa thuận dàn xếp do EEOC công bố. <br/>Giới hạn: không có phán quyết của tòa, công ty không thừa nhận sai phạm, không rõ quy tắc đã chạy bao lâu. <br/>Bài học: chỉ một bài test đơn giản (đổi ngày sinh) cũng phát hiện được lỗi. Doanh nghiệp nên tự làm loại test này trước khi triển khai. |

### 4. Case study 3 — Mobley v. Workday: vụ kiện AI sàng lọc ứng viên quy mô lớn (đang diễn ra)

#### Brief Case

- Tổ chức / sản phẩm AI: Workday, Inc., nền tảng tuyển dụng có công cụ AI chấm điểm, sắp xếp và gợi ý ứng viên (applicant recommendation system), được nhiều doanh nghiệp dùng.
- Thời gian, địa điểm / bối cảnh: Vụ kiện *Mobley v. Workday, Inc.*, Tòa Liên bang Quận Bắc California, số 23-cv-00770-RFL, nộp năm 2023. Nguyên đơn Derek Mobley là người Mỹ gốc Phi, trên 40 tuổi, có vấn đề về lo âu và trầm cảm. Ông cho biết đã ứng tuyển hơn 100 việc qua các công ty dùng Workday và đều bị từ chối.
- AI được dùng để làm gì: Chấm điểm, sắp xếp, xếp hạng hoặc sàng lọc ứng viên cho các nhà tuyển dụng là khách hàng của Workday.
- Vấn đề hoặc sự kiện đáng chú ý: Nguyên đơn **cáo buộc** công cụ AI của Workday có tác động bất lợi (disparate impact) đối với ứng viên theo **chủng tộc, tuổi và khuyết tật**, do được thiết kế phản ánh thiên lệch của nhà tuyển dụng và dữ liệu huấn luyện thiên lệch. Diễn biến tố tụng:
  - 07/2024: tòa bác yêu cầu bác đơn của Workday.
  - 16/05/2025: tòa **công nhận tạm thời vụ kiện tập thể** về phân biệt tuổi (ADEA) cho ứng viên từ 40 tuổi trở lên bị từ chối qua công cụ AI của Workday từ 09/2020.
  - 22/06/2026: tòa tiếp tục bác phần lớn yêu cầu bác đơn với các cáo buộc sửa đổi, cho phép lập luận về khuyết tật dựa trên proxy (ví dụ khoảng trống trong lịch sử làm việc).
- Số liệu có nguồn: Theo hồ sơ của chính Workday, khoảng **1,1 tỷ hồ sơ ứng tuyển bị từ chối** qua các công cụ phần mềm của họ trong giai đoạn liên quan. Nhóm vụ kiện tập thể có thể gồm **"hàng trăm triệu"** người (Proskauer, 2025). Nguyên đơn ứng tuyển **hơn 100** vị trí và đều bị từ chối.
- Nguồn:
  - "AI Bias Lawsuit Against Workday Reaches Next Stage as Court Grants Conditional Certification of ADEA Claim" — Proskauer (Law and the Workplace) — 06/2025 — https://www.proskauer.com/blog/ai-bias-lawsuit-against-workday-reaches-next-stage-as-court-grants-conditional-certification-of-adea-claim
  - Lệnh của tòa (07/2024) — https://clm.com/wp-content/uploads/2024/07/mobley-v.-workday-order.pdf
  - "The Class Action Weekly Wire – Episode 153" (phán quyết 22/06/2026) — Duane Morris — 25/06/2026 — https://blogs.duanemorris.com/classactiondefense/2026/06/25/the-class-action-weekly-wire-episode-153-california-federal-court-grants-in-part-and-denies-in-part-motion-to-dismiss-in-algorithmic-bias-suit/
- Phân biệt bằng chứng và nhận định:
  - **Nguồn xác nhận:** vụ kiện tồn tại, các mốc phán quyết thủ tục (bác yêu cầu bác đơn, công nhận tạm thời vụ kiện tập thể), con số 1,1 tỷ hồ sơ bị từ chối do Workday tự nêu.
  - **Chưa được chứng minh:** việc AI của Workday **thực sự** phân biệt đối xử. Tòa **chưa xét nội dung (merits)**. Các phán quyết hiện tại chỉ cho phép vụ kiện tiếp tục. Workday phản bác rằng tác động của công cụ khác nhau tùy khách hàng.
  - **Lưu ý về số liệu:** 1,1 tỷ là tổng số hồ sơ bị từ chối qua phần mềm, **không phải** số hồ sơ bị từ chối sai hay do thiên lệch.

#### Harm Map Worksheet

| Trường | Phân tích của tôi |
| --- | --- |
| High-risk moment | Khi AI của nhà cung cấp chấm điểm hoặc sàng lọc ứng viên cho **hàng nghìn nhà tuyển dụng cùng lúc**. <br/>Ứng viên bị từ chối (đôi khi rất nhanh sau khi nộp) mà không biết vì sao. |
| Stakeholder bị ảnh hưởng | Ứng viên từ 40 tuổi, ứng viên da đen, ứng viên khuyết tật (theo cáo buộc); doanh nghiệp dùng Workday (rủi ro pháp lý gián tiếp); Workday (trách nhiệm pháp lý của nhà cung cấp); thị trường lao động nói chung. |
| Failure mode | **Bias / disparate impact** (theo cáo buộc) do dữ liệu huấn luyện và proxy (ví dụ khoảng trống công việc); **thiếu minh bạch** trong quyết định tự động. |
| Layer bắt đầu lỗi | **Model** (theo cáo buộc của nguyên đơn: dữ liệu huấn luyện thiên lệch). <br/>**Chưa đủ bằng chứng** để khẳng định vì tòa chưa xét nội dung và chi tiết mô hình chưa công khai. |
| Harm xảy ra là gì? | **Chưa được chứng minh, mới là cáo buộc:** ứng viên trong nhóm được bảo vệ bị từ chối nhiều hơn. <br/>**Đã xảy ra (có nguồn):** Workday phải đối mặt vụ kiện tập thể có thể gồm hàng trăm triệu người. <br/>**Nguy cơ (nhận định):** nếu cáo buộc đúng, đây là thiên lệch lặp lại ở quy mô chưa từng có vì một mô hình dùng chung cho nhiều nhà tuyển dụng. |
| Harm lens | Allocative harm (cơ hội việc làm); phân biệt đối xử theo tuổi, chủng tộc, khuyết tật; thiếu minh bạch và trách nhiệm giải trình (ai chịu trách nhiệm: nhà tuyển dụng hay nhà cung cấp?). |
| Severity | **High** (tiềm năng). Ảnh hưởng tới sinh kế, nhưng mức độ thật phụ thuộc kết quả vụ kiện. |
| Scale | **Rất lớn (tiềm năng).** 1,1 tỷ hồ sơ bị từ chối qua phần mềm, nhóm vụ kiện tập thể có thể hàng trăm triệu người. <br/>Đây là quy mô của **cả hệ thống**, không phải số người bị hại đã chứng minh. |
| Probability | **Chưa xác định.** Tòa thấy cáo buộc đủ cơ sở để tiếp tục nhưng chưa có kết luận. <br/>Tôi đánh giá **trung bình** dựa trên cơ chế đã thấy ở case 1 (dữ liệu lịch sử thiên lệch dẫn tới mô hình thiên lệch). |
| Frequency | Nếu thiên lệch có thật thì **liên tục**, mỗi lần công cụ chấm hồ sơ. |
| Vì sao? | Căn cứ: hồ sơ tố tụng và phân tích của các hãng luật. <br/>Giới hạn: vụ kiện chưa kết thúc, không có audit độc lập công khai về mô hình. <br/>Bài học: trách nhiệm không chỉ thuộc doanh nghiệp tuyển dụng mà có thể kéo tới **nhà cung cấp AI**. Khi một mô hình phục vụ nhiều khách hàng, lỗi của nó cũng nhân lên theo. |

### 5. Tổng kết và bài học chung

| Case | Nguồn gốc lỗi | Mức bằng chứng về harm |
| --- | --- | --- |
| Amazon (2018) | Mô hình học từ dữ liệu lịch sử thiên lệch | Đã xác nhận mô hình thiên lệch; tác hại với ứng viên là nguy cơ |
| iTutorGroup (2023) | Quy tắc lọc cứng theo tuổi do người cài | Đã dàn xếp với EEOC ($365,000, hơn 200 người), công ty không thừa nhận sai phạm |
| Workday (2023 → nay) | Bị cáo buộc: mô hình và proxy thiên lệch | Vụ kiện đang diễn ra, chưa có kết luận |

Đề xuất cho doanh nghiệp dùng AI trong tuyển dụng (nhận định của tôi):
1. Không để AI **tự động từ chối**; AI chỉ gợi ý, người thật duyệt các trường hợp bị loại.
2. **Bias audit** định kỳ theo nhóm (giới, tuổi...) trước và sau khi triển khai. Test kiểu "đổi một thuộc tính, giữ nguyên phần còn lại" như case iTutorGroup.
3. Không thu thập dữ liệu nhạy cảm không cần thiết (ngày sinh, ảnh) ở bước sàng lọc. Rà soát các **proxy**.
4. Thông báo cho ứng viên khi có AI tham gia và cho phép yêu cầu người xem lại.
5. Hợp đồng với nhà cung cấp AI cần quy định rõ trách nhiệm, quyền audit và dữ liệu huấn luyện.

### Nguồn tham khảo bổ sung

- Quyết định số 33/2026/QĐ-TTg ban hành Danh mục hệ thống trí tuệ nhân tạo có rủi ro cao — Thủ tướng Chính phủ — 2026 — https://datafiles.chinhphu.vn/cpp/files/vbpq/2026/7/33-qdttg.signed.pdf
- Luật Bảo vệ dữ liệu cá nhân số 91/2025/QH15 — Quốc hội — 26/06/2025 — https://datafiles.chinhphu.vn/cpp/files/vbpq/2025/7/91qh.signed.pdf
- Nghị định số 356/2025/NĐ-CP quy định chi tiết một số điều và biện pháp thi hành Luật Bảo vệ dữ liệu cá nhân — Chính phủ — 31/12/2025 — https://datafiles.chinhphu.vn/cpp/files/vbpq/2026/01/356-nd.signed.pdf
- Quốc hội thông qua Luật Trí tuệ nhân tạo — Báo Đại biểu Nhân dân — 10/12/2025 — https://daibieunhandan.vn/quoc-hoi-thong-qua-luat-tri-tue-nhan-tao-10399959.html
- EU AI Act, Phụ lục III (hệ thống AI rủi ro cao, mục 4: việc làm, quản lý lao động) — https://artificialintelligenceact.eu/annex/3/
