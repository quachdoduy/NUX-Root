# NUX-Root / KeepAlived Linux CLi
Notes on project implementation with Linux.

[![Lang EN](https://img.shields.io/badge/lang-en-yellow)](KeepAlived-CLi.md)
[![Lang VI](https://img.shields.io/badge/lang-vi-green)](KeepAlived-CLi.vi.md)
[![Home](https://img.shields.io/badge/Main-blue)](../README.md)<br/>
[![GitHub stars](https://img.shields.io/github/stars/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=red)](https://github.com/quachdoduy/NUX-Root/stargazers)
[![GitHub watchers](https://img.shields.io/github/watchers/quachdoduy/NUX-Root?logo=GitHub&style=flat&color=blue)](https://github.com/quachdoduy/NUX-Root/watchers)<br/>
[![donate with paypal](https://img.shields.io/badge/Like_it%3F-Donate!-green?logo=githubsponsors&logoColor=orange&style=flat)](https://paypal.me/quachdoduy)
[![donate with buymeacoffe](https://img.shields.io/badge/Like_it%3F-Donate!-blue?logo=githubsponsors&logoColor=orange&style=flat)](https://buymeacoffee.com/quachdoduy)

# TABLE OF CONTENTS

---

# PREFACE
- **Keepalived** là một phần mềm định tuyến được viết bằng C. Mục tiêu chính của dự án này là cung cấp các tiện ích đơn giản và mạnh mẽ để cân bằng tải và tính khả dụng cao cho hệ thống Linux và cơ sở hạ tầng dựa trên Linux. Khung cân bằng tải dựa trên mô-đun hạt nhân Linux Virtual Server (IPVS) nổi tiếng và được sử dụng rộng rãi cung cấp cân bằng tải Layer4. **Keepalived** triển khai một bộ kiểm tra để duy trì và quản lý nhóm máy chủ cân bằng tải một cách năng động và thích ứng theo tình trạng của chúng. Mặt khác, tính khả dụng cao đạt được bằng giao thức VRRP. VRRP là một khối cơ bản để chuyển đổi dự phòng bộ định tuyến. Ngoài ra, **Keepalived** triển khai một bộ móc vào máy trạng thái hữu hạn VRRP cung cấp các tương tác giao thức cấp thấp và tốc độ cao. Để cung cấp khả năng phát hiện lỗi mạng nhanh nhất, **Keepalived** triển khai giao thức BFD. Quá trình chuyển đổi trạng thái VRRP có thể tính đến gợi ý BFD để thúc đẩy quá trình chuyển đổi trạng thái nhanh. Các khung **Keepalived** có thể được sử dụng độc lập hoặc kết hợp với nhau để cung cấp cơ sở hạ tầng phục hồi.

**Keepalived là phần mềm miễn phí**; bạn có thể phân phối lại và/hoặc sửa đổi nó theo các điều khoản của Giấy phép Công cộng GNU do Free Software Foundation công bố; phiên bản 2 của Giấy phép hoặc (tùy bạn lựa chọn) bất kỳ phiên bản nào mới hơn.

## Tham khảo tài liệu gốc
- webpage: https://www.keepalived.org/
- github: https://github.com/acassen/keepalived/
- user guide: https://keepalived.readthedocs.io
- social: https://x.com/keepalived


*[Lên đầu trang](#nux-root--keepalived-linux-cli)*