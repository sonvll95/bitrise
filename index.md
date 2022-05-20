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

## Hoạt động của CI
Một kịch bản CI bắt đầu bằng việc developer commit code lên repository (github chẳng hạn). Bất kỳ thay đổi nào cũng sẽ trigger một vòng đời CI. Các bước trong một kịch bản CI thường như sau:

1. Đầu tiên, developer commit code lên repo.
1. CI server giám sát repo và kiểm tra xem liệu có thay đổi nào trên repo hay không (liên tục, chẳng hạn mỗi phút 1 lần)
1. Ngay khi commit xảy ra, CI server phát hiện repo có thay đổi, nên nó nhận code mới nhất từ repo và sau đó build, chạy unit và integration test
1. CI server sẽ sinh ra các feedback và gửi đến các member của project
1. CI server tiếp tục chờ thay đổi ở repo

Mỗi lần developer làm xong task, họ phải chạy một private build (tức là chạy phần mềm trên local trước), check choác cẩn thận và commit code lên repo khi đã thấy ổn. Bước này xảy ra thường xuyên và ở bất kỳ thời điểm nào trong ngày. Việc build tích hợp sẽ không xảy ra khi những thay đổi này chưa ảnh hưởng đến repo (kiểu như bạn commit mà chưa được merge vậy).

Một trong những tuyên ngôn của CI là “**Build Software at Every Change**”. Mục đích là để tránh những câu kiểu như “Ớ, phần này chạy trên máy em bình thường mà” 😄 Vấn đề sẽ được tìm ra sớm bằng cách build thường xuyên, và để CI hoạt động hiệu quả trong project, tốt nhất là developer phải commit code thường xuyên hơn. Đợi hơn một ngày để commit code lên repo có thể khiến việc tích hợp tốn thời gian và code mình dùng có thể không phải là code mới nhất.

### Lợi ích của việc sử dụng CI sẽ là:

* Giảm thiểu rủi ro nhờ việc phát hiện lỗi và fix sớm, tăng chất lượng phần mềm nhờ việc tự động test và inspect (đây cũng là một trong những lợi ích của CI, code được inspect tự động dựa theo config đã cài đặt, đảm bảo coding style, chẳng hạn một function chỉ được dài không quá 10 dòng code …)
* Giảm thiểu những quy trình thủ công lặp đi lặp lại (build css, js, migrate, test…), thay vì đó là build tự động, chạy test tự động
* Sinh ra phần mềm có thể deploy ở bất kì thời gian, địa điểm

### Tại sao lại là Bitrise CI?
Có rất nhiều dịch vụ và open source cho CI. Trong khuôn khổ bài này thì mình chỉ hướng dẫn về Bitrise CI. Các CI khác thì áp dụng tương tự, nếu bạn đã hiểu rõ về bản chất hoạt động của CI là như thế nào.

Sự lựa chọn ở đây là do iOS. Vì iOS chạy được trên nền tảng OS là MAC OS. Các dịch vụ CI khác thì ít hỗ trợ MAC OS hoặc nếu có thì đều thu phí hoặc bị giới hạn. Với Bitrise CI thì chúng ta sẽ dùng được với iOS Project. Ngoài ra, về cấu hình Bitrise CI cũng khá là cơ bản, Bitrise CI bao gồm UI và workflow giúp các bạn config step by step dễ dàng hơn, nếu bạn thành tạo được rồi thì có thể cấu hình các CI khác một cách nhanh chóng.

Có 2 loại dịch vụ của Bitrise-CI

[Free : Bitrise - Mobile Continuous Integration and Delivery 
](https://www.bitrise.io/)

[Trả phí :  Bitrise Plans and Pricing(các bạn tham khảo các bản trả phí ở đây nhé)](https://www.bitrise.io/pricing)

Bạn truy cập vào link free và tiến hành đăng nhập với account Bitbucket của bạn. Công ty mình thì sử dụng **Bitbucket** để lưu trữ và quản lý source code.

## OK, khái niệm vậy được rồi đi config thôi!
**B1**: Sau khi đăng nhập thì nó sẽ có giao diện như vầy:

![cicd_image_1](https://user-images.githubusercontent.com/90396637/169445689-f2b953d5-890a-41d1-8a94-0a5170756877.png)

Phần latest build sẽ hiện những bản build gần nhất, nếu không có thì nó sẽ hiện trống thôi. Các bạn chọn Add new app để thêm project mới nhé. Trong phần Add new app thì có 2 kiểu: Add bằng web UI or CLI. Các bạn chọn web UI cho dễ dàng hơn nhé, còn CLI là cho các bạn thích sự khổ dâm của terminal.

**B2**: Tiếp theo các bạn chọn dự án mình cần config.

![cicd_2](https://user-images.githubusercontent.com/90396637/169446616-eaf9e066-d41f-4534-b6ab-7fef903be5a1.png)

**B3**: Sau đó các bạn chọn các setting phù hợp cho dự án, cái này ez game nên mình lướt qua thôi:

Phần Branch selected các bạn chọn nhánh để bitrise scan nhánh đó nhé. Mục đích là để bất kể có thay đổi nào trên nhánh đó thì Bitrise đều trigger CI. Ở đây mình chọn develop.

![cicd_3](https://user-images.githubusercontent.com/90396637/169446622-6de443e3-da6c-464c-9533-e20a1251d1c0.png)

**B4**: Tiếp tục điền các thông tin configuration:

![cicd_4](https://user-images.githubusercontent.com/90396637/169446623-d76ede0c-035b-4091-b22f-468bf55b03fc.png)

**B5**: Sau khi xong xuôi thì Bitrise sẽ tự động run build **#1** đầu tiên cho bạn. Đa số build **#1** kiểu gì cũng fail vì mình chưa setup env cũng như codesigning cần thiết cho project mà. Đúng là lần đầu lúc nào cũng đau và rát. 

![cicd_5](https://user-images.githubusercontent.com/90396637/169446627-57cc99bf-fae2-481b-bbdd-48b5801ac5fd.png)

**B6**: Tiếp theo bạn cần Edit workflow để xóa đi một số step thừa không cần thiết cũng như setup lại env. 

![cicd_6](https://user-images.githubusercontent.com/90396637/169446629-bcaaafd3-640e-4fec-a93e-b31e5992c518.png)

Ở trên là hình ảnh của Workflow. Nó sẽ chạy step by step, gặp lỗi ở đâu thì sẽ dừng ở đó. Mình sẽ xóa 3 bước cuối. Vì project của mình không có unittest và phần cocoapods mình sẽ dùng bundle để quản lí nên mình sẽ setup lại script cho step đó. Phần Recreate blabla mình cũng chưa rõ là gì nhưng không sử dụng nên các bạn cứ xóa đi :D

**B7**: Tiếp theo mình thêm một step vào workflow, các bạn bấm vào dấu + sau đó search Certificate để Bitrise cài đặt Codesigning. Vì mình cần export ra ipa nên việc code signing là cần thiết cho máy ảo, nếu các bạn chỉ compile code thôi thì có thể bỏ qua bước này. 

![cicd_7](https://user-images.githubusercontent.com/90396637/169446630-a6f1315b-07b7-46ab-88ab-c77969d99658.png)

Sau khi add step thành công thì các bạn chọn qua phần CodeSigning để kéo provisioning và certificate vào nhé. 

![cicd_8](https://user-images.githubusercontent.com/90396637/169446631-a6950a66-4db4-4f61-a0d8-b6902641d7ee.png)

**B8**: Tiếp đến mình cần chạy các script để CI khởi tạo các env cần thiết như pod…

Các bạn làm giống b7 nhưng chọn search phần Script

![cicd_9](https://user-images.githubusercontent.com/90396637/169446632-6d2cb5f2-e641-442f-96b0-8ebf2ab0e6f1.png)

Ở bước này mình thêm đoạn script:

`set -e`: -e  Exit immediately if a command exits with a non-zero status.

`set -x`:  Đại khái là nó sẽ log command và print ra trong terminal để mình dễ điều tra nếu có lỗi.

`sudo systemsetup -settimezone Asia/Ho_Chi_Minh`: Chỉnh lại timezone của máy CI về giờ VN, mục đích là nhiều khi các bạn cần lấy giờ của bản build để set cho build number thì cái này sẽ chính xác. Còn không thì nó sẽ lấy theo múi giờ của máy CI (có thể là Mỹ hoặc nước nào đó) 

`bundle install`: Cài đặt bundle

`bundle exec pod install --repo-update`: Update pod mới nhất. 

![cicd_10](https://user-images.githubusercontent.com/90396637/169446634-c8b53314-f121-4c5d-8cec-854473da928b.png)

Giải thích chút về step này: Vì CI là một máy chủ ảo nên mình cũng cần phải thiết lập các env bằng cách chạy bundle giống như việc bạn làm ở local.

**B9**: Việc CI đã hoàn tất , giờ là CD thôi. Tức là distribution lên một bên thứ 3 nào đó để QC có thể install bản build đó và test. Hiện giờ thì có nhiều dịch vụ hỗ trợ distribution như TestFlight, Firebase distribution, Deploygate. Mình sẽ dùng fastlane để giúp việc distribution tự động dễ dàng hơn. Trước khi đưa lên CI thì các bạn cần test lại fastlane ở local và sure là nó work trước đã nhé. Bạn nào không cần distribution thì có thể bỏ qua step này :sunglasses: 

![cicd_11](https://user-images.githubusercontent.com/90396637/169446635-567c92ae-cee7-46f1-96e5-0b2a12fdd714.png)

Tiếp theo, các bạn tạo thêm một step Script để run cmd của fastlane. Step giống ở trên, mình thay đổi đoạn script để chạy fastlane lane theo config trong fastfile.

![cicd_12](https://user-images.githubusercontent.com/90396637/169446638-9ebf852c-7324-4b64-82a0-b17b84db944b.png)

Vậy là xong, sau khi hoàn tất các step thì cây workflow sẽ trông như thế này. Các bạn hoàn toàn có thể rename lại theo ý muốn của từng step. Như hình thì mình rename lại step setup env và Fastlane export để giúp dễ phân biệt hơn.


Rebuild lại xem thành quả nào!!!

![cicd_13](https://user-images.githubusercontent.com/90396637/169446640-869a248d-3431-4043-a443-3725f2aab21b.png)
![cicd_14](https://user-images.githubusercontent.com/90396637/169446641-c3e923d3-07e5-41a5-a5da-b6d186012491.png)


Sau **#25** build thì mình đã hoàn thành việc config CICD cho dự án, build thành công hay thất bại thì Bitrise đều mail về cho các thành viên trong team. Ngoài ra thì fastlane cũng success và đã notify lên slack cho QC vào test bản build. 

![cicd_15](https://user-images.githubusercontent.com/90396637/169446643-bb11a358-d426-45e9-83a6-ad516f7bfd57.png)

## Where to go from here?
Vậy là các bạn đã có thể config được một CICD ở mức độ basic. Nâng cao hơn thì các bạn có thể tự tìm hiểu thêm việc trigger build, dùng linterbot để review code, tự tạo tag khi merge vào branch release. Còn rất nhiều việc mà CICD có thể auto giúp cho dự án hoạt động linh hoạt hơn. Tuy nhiên thì cần thời gian tìm hiểu dành cho những bạn có sở thích vọc vạch.

## Tổng kết
Hiểu về CI, thì đơn giản như sau:

* Nó là 1 cái máy ảo ở một nơi nào đó
* Muốn chạy iOS Project thì máy ảo đó phải cài MAC OS  và cái Xcode
* Để build được iOS Project thì xem cấu hình project ổn không, phù hợp với Xcode và Simulator gì trên máy ảo.
* Để run được Simulator thì phải cài đặt các thư viện, nhất là các thư viện trong cocoapod
* Để run được trên CICD thì phải đảm bảo việc run hoặc compile ở local thành công.

**Chúc bạn thành công!**

## Tham khảo:
Viblo: [https://viblo.asia/p/ci-cd-va-devops-07LKXYXDZV4
](https://viblo.asia/p/ci-cd-va-devops-07LKXYXDZV4)

Fxstudio: [https://fxstudio.dev/cau-hinh-va-cai-dat-ci-cd-cho-ios-project/](https://fxstudio.dev/cau-hinh-va-cai-dat-ci-cd-cho-ios-project/)		
