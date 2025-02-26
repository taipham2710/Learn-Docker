# Docker cơ bản

## Giới thiệu
* Docker là dự án mã nguồn mở https://github.com/docker

## Docker được dùng để làm gì ?
* Tạo ra các container cho các ứng dụng phần mềm
* Sau khi dùng Docker build ra được các container thì Docker sẽ giúp vận chuyển các container đến các môi trường phát triển
* Hỗ trợ việc chia sẻ các container cho các lập trình viên khác

## Khẩu quyết của Docker
* ***"Build, ship and deply any application, anywhere"***
    * Build: đóng gói ứng dụng cho 1 container
    * Ship: vận chuyển container đó từ máy này sang máy khác
    * Deploy: triển khai và chạy các container
    * Any application: bất cứ ứng dụng nào chạy được trên môi trường Linux
    * Anywhere: từ môi trường phát triển trên laptop, đến máy chủ vật lý, máy ảo, cloud instance, ....
    * Đóng gói phần mềm dễ dàng
    * Component có thể deploy ngay lập tức 
    * Không cần cấu hình và cài đặt môi trường phát triển rườm rà
    * Image: component để triên khai ứng dụng bao gồm mã nguồn, thư viện framework, file, ...
    * Trừu tượng hóa giải pháp và đóng gói vào 1 image kèm dependencies

* ***"Batteries included but replaceable"***
    * 1 component có thể thay thể bằng 1 component khác bằng cách implement cùng 1 interface có sẵn
    * Docker framework được phân chia thành các mô-đun có khả năng mở rộng cao. Chính vì tính mở rộng cao này nên các component có thể được thay thế bằng các component tương tự, các tính năng tương tự thậm chí còn ưu việt hơn

## Một số thuật ngữ
* Image
    * Khuôn mẫu, lớp chứa các file cần thiết để tạo nên 1 container
    * Chứa những tài nguyên có sẵn
    * Không được tiếp cận vào CPU, memory, storage, ...
* Container
    * Được xem như 1 object tồn tại trên host với 1 IP
    * Được deploy, chạy, và xóa bỏ thông qua remote client
* Docker engine
    * Tạo vào chạy container
    * Chạy lệnh trong chế độ daemon
    * Linux trở thành máy chủ daemon
    * Container được deploy, chạy, xóa bỏ thông qua remote client

<div style="text-align: center;">
    <img src="https://www.tutorialspoint.com/virtualization2.0/images/applications.jpg" alt="Docker engine">
</div>

```
Qua hình bên ta thấy Docker engine nằm trên layer (hệ điều hành) và nằm dưới layer (thư viện và ứng dụng). Thư viện và ứng dụng ở đây tượng trưng là container mà docker engine đang quản lý.
```

* Docker daemon
    * Tiến trình chạy ngầm quản lý các container
* Docker client
    * Kiếm soát hầu hết các workflow của Docker
    * Giao tiếp với các máy chủ Docker thông qua daemon
<div style="text-align: center;">
    <img src="https://www.devopsschool.com/blog/wp-content/uploads/2023/12/image-195.png" alt="Docker daemon and docker client">
</div>

* Docker Hub (Registry)
    * Chứa các component Docker
    * Cho phép lưu, sử dụng, tìm kiếm image
    * Vai trò: **"ship"** trong **"build, ship, deploy"**
<div style="text-align: center;">
    <img src="https://3.bp.blogspot.com/-qAC-f6ceVho/U5rfqpzj3VI/AAAAAAAAC8w/Ta4pzEhm-8A/s1600/Screen+Shot+2014-06-13+at+8.34.10+pm.png" alt="Docker hub">
</div>

```
Docker Hub giúp việc vận chuyển các images từ lập trình viên này đến lập trình viên khác thông qua hệ thống quản lý Revision, tới các Integration Tests và tới các Deployment Platforms
```
## Điểm mạnh của Docker
* Deploy nhanh hơn
    * Docker images được build sử dụng hệ thống augmented file system
    * Thêm các layer bên trên root kernel
    * Dễ dàng tổng hợp các layer thành một
**=> Tạo ra 1 container mới sẽ nhanh chóng và gọn nhẹ**

    * Độc lập
        * Lỗi xảy ra với 1 container không ảnh hưởng các container khác
    * Cơ động
        * Tránh conflict môi trường
        * Trao đổi giữa các máy
        * Nhất quán khi chạy trên các máy khác nhau
    * Chụp ảnh hệ thống (snapshot)
        * Một khi 1 docker container đã chạy, chúng ta có thể lưu giữ trạng thái của nó bất cứ khi nào, lưu snapshot thành container hoặc image
        * Tag
        * Tạo container y hệt từ snapshot
    * Kiểm soát việc sử dụng tài nguyên (CPU, RAM, Storage, ...) . Vd: ***1 hành động chỉ cho phép bao nhiêu đó CPU, nếu lớn hơn mức quy định -> tắt nó đi***; ***định nghĩa hành động khi memory vượt mức quy định cho phép***

    * Đơn giản hóa sự phụ thuộc lẫn nhau giữa các ứng dụng (dependency)
        * Docker giản lược rất nhiều effort của các lập trình viên khi xác định dependency ở Dockerfile mà không cần thiết phải cài đặt từng thư viện một
    * Thuận tiện cho việc chia sẻ
        * Docker Hub (public/ private registry)
        * Dockerfile (lấy image về chạy dockerfile trên file image gốc)

## Một số lầm tưởng về Docker
1. ***Không*** phải công cụ quản lý thiết lập hay thiết lập tự động (Puppet, Chef, ...)
2. ***Không*** phải giải pháp ảo hóa phần cứng (VMWare, KVM, ...)
3. ***Không*** phải là một nền tảng điện toán đám mây (OpenStack, CloudStack, ...)
4. ***Không*** phải là một deployment framework (Capistrano, Fabric, ...)
5. ***Không*** phải là một công cụ quản lý workload (Mesos, Fleet, ...)
6. ***Không*** phải là một môi trường phát triển (Vagant, ...)

## Kernel
*  Kernel chạy trực tiếp trên phần cứng và có các nhiệm vụ khác nhau:
    * Nhận tin từ phần cứng, thông báo rằng 1 ổ đĩa mới vừa được kết nối, .... -> Phản hồi các thông điệp từ phần cứng
    * Khởi tạo và đặt lịch cho các chương trình
    * Quản lý và hệ thống các tác vụ
    * Truyền tin giữa các chương trình
    * Phân chia tài nguyên, bộ nhớ, CPU, mạng, ...
    * Tạo container bằng cách chỉnh thiết lập của kernel

## Docker
* Viết bằng ngôn ngữ Go (Go, 1 trong những ngôn ngữ dành cho hệ thống)
* Quản lý nhiều đặc tính của kernel, và dùng những đặc tính đó đưa ra định nghĩa về container và image
    * ***"cgroup" - "control group"***: nhóm các tiến trình, và bao lấy các tiến trình cùng nhóm trong 1 không gian ảo riêng => các container không can thiệp lẫn nhau được
    * ***"namespace"***: 1 đặc điểm của kernel linux cho phép chia tách các tầng network
    * ***"copy-on-write"***: định nghĩa image
* Hướng tiếp cận đã tồn tại vài năm trước khi có Docker
* Docker đơn giản hóa cho việc viết script cho các hệ thống phân tán

## Socket điều khiển của Docker
* Docker bao gồm 2 phần: client và server   
* Server nhận lệnh qua socket (qua mạng hoặc qua "file")
* Client thậm chí có thể được chạy bên trong Docker

### Chạy Docker ở máy Local
<div style="text-align: center;">
    <img src="https://images.viblo.asia/32b0627f-8d5c-4868-add8-dac242bd7953.png" alt="Docker hub">
</div>

```
1 Chương trình docker client kết nối tới Socket -> gửi lệnh đến Docker Server, tiến hành các lệnh theo yêu cầu: tạo container, xóa container, ...
```

### Chạy Docker ở bên trong client
<div style="text-align: center;">
    <img src="https://images.viblo.asia/8d74a186-f1bc-4f4a-a1d8-d2c5c49d130a.png" alt="Docker hub">
</div>

```
Cho phép chia sẻ socket bên trong container, gửi cùng 1 tin đi qua 1 socket tới server chạy trên host và làm các việc khác như bình thường
Và quan trọng, client có thể chạy ở bất cứ chỗ nào, và có thể kết nối tới server, làm bất cứ việc gì Docker có thể làm 
```