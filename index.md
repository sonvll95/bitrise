# Chào bạn tới với bài hướng dẫn cấu hình và cài đặt CI/CD bằng Bitrise cho iOS Project

Bài viết này mình sẽ hướng dẫn mọi người cơ bản về cấu hình Bitrise CI cho iOS Project. Để tự động run & build iOS project từ Bitbucket. Và đối tượng hướng tới cho bài này là các bạn:

* Các bạn chưa biết gì về CI
* Các bạn iOS Dev muốn sài các tools auto
* Các bạn team lead dự án
* Các bạn muốn tự vọc vạch lúc rảnh

Trước khi bắt đầu thì nắm một số khái niệm trước nha. 😎

CI là Continuous Integration.

> Nó là phương pháp phát triển phần mềm yêu cầu các thành viên của team tích hợp công việc của họ thường xuyên, mỗi ngày ít nhất một lần. Mỗi tích hợp được “build” tự động (bao gồm cả test) nhằm phát hiện lỗi nhanh nhất có thể. Cả team nhận thấy rằng cách tiếp cận này giảm thiểu vấn đề tích hợp và cho phép phát triển phần mềm nhanh hơn.

CD là Continuous Delivery (tạm dịch là chuyển giao liên tục)

> lại nâng cao hơn một chút, bằng cách triển khai tất cả thay đổi về code (đã được build và test) đến môi trường testing hoặc staging. Continuous Delivery cho phép developer tự động hóa phần testing bên cạnh việc sử dụng unit test, kiểm tra phần mềm qua nhiều thước đo trước khi triển khai cho khách hàng (production). Những bài test này bao gồm UI testing, load testing, integration testing, API testing… Nó tự động hoàn toàn quy trình release phần mềm.

