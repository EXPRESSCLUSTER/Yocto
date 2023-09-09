# Build Embedded OS

## Environment
- Ubuntu Server 22.04
- CPU: 4 cores
- RAM: 16 GiB
- HDD: 1 TiB

## How to Build
1. Install the following packages.
   ```sh
   sudo apt install gawk wget git diffstat unzip texinfo gcc build-essential chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev python3-subunit mesa-common-dev zstd liblz4-tool file locales
   ```
1. Clone the repository.
   ```sh
   git clone https://git.yoctoproject.org/poky
   ```
1. Move the poky directory.
   ```sh
   cd poky/
   ```
1. If you have a proxy server, set the following environment variables.
   ```sh
   export http_proxy='http://<IP Address>:<port>' \
   export https_proxy='http://<IP Address>:<port>' \
   export ftp_proxy='http://<IP Address>:<port>' \
   export ALL_PROXY='socks://<IP Address>:<port>' \
   export all_proxy='socks://<IP Address>:<port>'
   ```
1. 
   ```sh
   source oe-init-build-env
   ```
1. Build embedded OS.
   ```sh
   nohup bitbake core-image-minimal &
   ```
   - It takes some hours.
1. Run the embedded OS that you built.
   ```sh
   ./scripts/runqemu qemux86-64 nographic
   ```
1. Enjoy!
   ```
   (snip)
   Poky (Yocto Project Reference Distro) 4.2+snapshot-fb80dc894d8f9e8c02c30c59f981296e2a68f4fd qemux86-64 /dev/ttyS0
   
   qemux86-64 login: root
   root@qemux86-64:~# uname -r
   6.4.14-yocto-standard
   ```

## To Do
- If you run runqemu, qemu-system-x86_64 is executed with the following options. I would like to change some network parameters to match environment.
  ```sh
  /home/clpuser/work/GitHub/poky/build/tmp/work/x86_64-linux/qemu-helper-native/1.0/recipe-sysroot-native/usr/bin/qemu-system-x86_64 \
  -device virtio-net-pci,netdev=net0,mac=52:54:00:12:34:02 \
  -netdev tap,id=net0,ifname=tap0,script=no,downscript=no \
  -object rng-random,filename=/dev/urandom,id=rng0 \
  -device virtio-rng-pci,rng=rng0 \
  -drive file=/home/clpuser/work/GitHub/poky/build/tmp/deploy/images/qemux86-64/core-image-minimal-qemux86-64.rootfs-20230909173604.ext4,if=virtio,format=raw \
  -usb \
  -device usb-tablet \
  -usb \
  -device usb-kbd \
  -cpu IvyBridge \
  -machine q35,i8042=off \
  -smp 4 \
  -m 256 \
  -serial mon:stdio \
  -serial null \
  -nographic \
  -kernel /home/clpuser/work/GitHub/poky/build/tmp/deploy/images/qemux86-64/bzImage \
  -append root=/dev/vda rw ip=192.168.7.2::192.168.7.1:255.255.255.0::eth0:off:8.8.8.8 net.ifnames=0 console=ttyS0 console=ttyS1 oprofile.timer=1 tsc=reliable no_timer_check rcupdate.rcu_expedited=1
  ```

## Link
- https://docs.yoctoproject.org/4.2.3/brief-yoctoprojectqs/index.html#yocto-project-quick-build