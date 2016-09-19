# Ghi chép về ứng dụng snap 

https://github.com/intelsdi-x/snap
http://snap-telemetry.io/

# Môi trường chuẩn bị

Ubuntu 14.04

# Các bước cài đặt

- Tải snap

    ```sh
    wget https://github.com/intelsdi-x/snap/releases/download/v0.16.1-beta/snap-v0.16.1-beta-linux-amd64.tar.gz

    tar -xvf snap-v0.16.1-beta-linux-amd64.tar.gz
    mv snap-v0.16.1-beta/bin/* /usr/local/bin/
    rm -rf snap-v0.16.1-beta
    ```

- Chạy thử

    ```sh
    snapd --plugin-trust 0 --log-level 1
    ```


    - Kết quả: http://prntscr.com/cjnzue
    
    - Thoát: tổ hợp phím `ctl + C`
    

##  Load plugin snap

- Tải plugin 

    ```sh

    wget https://github.com/intelsdi-x/snap/releases/download/v0.16.1-beta/snap-plugins-v0.16.1-beta-linux-amd64.tar.gz

    tar -xvf snap-plugins-v0.16.1-beta-linux-amd64.tar.gz
    mkdir -p ~/snap/plugins/
    mv snap-v0.16.1-beta/plugin/* ~/snap/plugins/
    rm -rf snap-v0.16.1-beta
    ```

- Mở cửa sổ thứ nhất để khởi động snap 

    ```sh
    snapd --plugin-trust 0 --log-level 1
    ```

- Mở cửa sổ thứ 2 để load các plugin

    ```sh
    cd ~/snap/plugins/
    curl -X POST -F plugin=@snap-plugin-collector-mock1 http://localhost:8181/v1/plugins
    curl -X POST -F plugin=@snap-plugin-processor-passthru http://localhost:8181/v1/plugins
    curl -X POST -F plugin=@snap-plugin-publisher-file http://localhost:8181/v1/plugins
    ```


    ```sh
    cd ~/snap/plugins/
    snapctl plugin load snap-plugin-collector-mock1
    snapctl plugin load snap-plugin-processor-passthru
    snapctl plugin load snap-plugin-publisher-file
    ```

- Kiểm tra lại các plugin

    ```sh
    snapctl plugin list
    ```

    - Kết quả: 
    
        ```sh
        root@u-14-ctl1:~/snap/plugins#  snapctl plugin list
        NAME             VERSION         TYPE            SIGNED          STATUS          LOADED TIME
        mock             1               collector       false           loaded          Mon, 19 Sep 2016 11:53:27 ICT
        passthru         1               processor       false           loaded          Mon, 19 Sep 2016 11:53:44 ICT
        file             2               publisher       false           loaded          Mon, 19 Sep 2016 11:53:50 ICT
        ```

###  `Còn tiếp`