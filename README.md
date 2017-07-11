# webrtc-build-scripts
Hướng dẫn build webrtc cho Android trên MacOs

* Cài đặt vagrant cho OSX

    https://www.vagrantup.com/downloads.html
    
* Clone repository này về máy.
* Dùng terminal chạy máy ảo vagrant lên bằng lệnh

    vagrant up
    
* Sau đó đăng nhập vào máy ảo bằng lệnh

    vagrant ssh
    
* Tiếp theo chạy lệnh trên máy ảo để cài đặt OpenJava

https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-get-on-ubuntu-16-04

    sudo apt-get install default-jre
    sudo apt-get install default-jdk
    
* Sau khi cài đặt thành công 2 gói Java. Tiếp đó chay thêm lệnh, nó sẽ tự động cài đặt các gói bổ xung cho việc build

    sudo /bin/bash ./install-build-deps-android.sh
    
* Có một lỗi sẽ xảy ra khi tải webrtc do không đủ bộ nhớ. Để khắc phục bạn sẽ cần phải làm thêm một vài thao tác. Hãy chú ý
 - Trước tiên, cần mở ứng dụng VirtualBox, Tắt máy ảo vagrant đang chạy.
 Mở thư mục đang lưu máy ảo vagrant, và tiến hành mở rộng dung lượng cho nó theo lệnh
 
    VBoxManage clonehd "tên file máy ảo hiện tại.vmdk" "tên máy ảo sẽ đổi.vdi" --format vdi
 
    Ex: VBoxManage clonehd "ubuntu-xenial-16.04-cloudimg.vmdk" "ubuntu-xenial-16.04-cloudimg.vdi" --format vdi
 
 tiến trình sẽ chạy từ 0-100%
 Sau đó bạn sẽ chạy lệnh để resize máy ảo đó bằng lệnh
 
    Ex: VBoxManage modifyhd ubuntu-xenial-16.04-cloudimg.vdi --resize 40000
    
 Sau khi mọi thay đổi thành công, bạn cần mở VirtualBox để cấu hình chạy máy ảo với máy ảo mới mà bạn và tiến hành thêm bộ nhớ
 
 Xem thêm hướng dẫn tại :https://tuhrig.de/resizing-vagrant-box-disk-space/
 Sau khi hoàn tất các bước trên, tiến hành chạy máy ảo bằng lệnh
 
  vagrant provision
  
 * Bây giờ bạn cần tải về bộ mã nguồn Webrtc về
 
    git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
 
    export PATH=`pwd`/depot_tools:"$PATH"
 
    $ mkdir webrtc-checkout
    $ cd webrtc-checkout
    $ fetch --nohooks webrtc_android
    $ gclient sync
 
Bây giờ tất cả các nguồn và phụ thuộc được tải xuống, bạn có thể: tạo các tệp tin ninja với GN, xây dựng và tạo thư viện webrtc theo định dạng .aar trong một bước sử dụng một tập lệnh khá tiện lợi mà Google đã phát triển:

    $ cd src
    $ tools_webrtc/android/build_aar.py
