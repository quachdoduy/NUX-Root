# NUX-Root / Iproute2 Linux CLi
Notes on project implementation with Linux.

[![Lang EN](https://img.shields.io/badge/lang-en-yellow)](Iproute2-CLi.md)
[![Lang VI](https://img.shields.io/badge/lang-vi-green)](Iproute2-CLi.vi.md)
[![Home](https://img.shields.io/badge/Main-blue)](../README.md)<br/>
[![GitHub stars](https://img.shields.io/github/stars/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=red)](https://github.com/quachdoduy/NUX-Root/stargazers)
[![GitHub watchers](https://img.shields.io/github/watchers/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=blue)](https://github.com/quachdoduy/NUX-Root/watchers)<br/>
[![donate with paypal](https://img.shields.io/badge/Like_it%3F-Donate!-green?logo=githubsponsors&logoColor=orange&style=flat)](https://paypal.me/quachdoduy)
[![donate with buymeacoffe](https://img.shields.io/badge/Like_it%3F-Donate!-blue?logo=githubsponsors&logoColor=orange&style=flat)](https://buymeacoffee.com/quachdoduy)

# TABLE OF CONTENTS


# PREFACE
- Hầu hết các bản phân phối Linux và hầu hết UNIX hiện đang sử dụng các lệnh `arp`, `ifconfig` và `route` đáng kính. Mặc dù các công cụ này hoạt động, nhưng chúng cho thấy một số hành vi không mong muốn trong Linux 2.2 trở lên. Ví dụ, đường hầm GRE là một phần không thể thiếu của định tuyến ngày nay, nhưng yêu cầu các công cụ hoàn toàn khác.
- Với iproute2, đường hầm là một phần không thể thiếu của bộ công cụ.
- Các hạt nhân Linux 2.2 trở lên bao gồm một hệ thống con mạng được thiết kế lại hoàn toàn. Mã mạng mới này mang lại hiệu suất Linux và một bộ tính năng có ít đối thủ cạnh tranh trong lĩnh vực hệ điều hành nói chung. Trên thực tế, mã định tuyến, lọc và phân loại mới có nhiều tính năng hơn mã do nhiều bộ định tuyến, tường lửa và sản phẩm định hình lưu lượng chuyên dụng cung cấp.
- Khi các khái niệm mạng mới được phát minh, mọi người đã tìm ra cách để dán chúng lên trên khuôn khổ hiện có trong các hệ điều hành hiện có. Việc liên tục phân lớp các phần tử thừa này đã dẫn đến mã mạng chứa đầy hành vi kỳ lạ, giống như hầu hết các ngôn ngữ của con người. Trước đây, Linux đã mô phỏng cách xử lý nhiều thứ này của SunOS, điều này không lý tưởng.
- Khung làm việc mới này giúp thể hiện rõ ràng các tính năng trước đây nằm ngoài khả năng của Linux.

*[Lên đầu trang](#nux-root--iproute2-linux-cli)*