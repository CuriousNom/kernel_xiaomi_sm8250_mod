# About this repo
This repo (`android14-lineage21-mod` branch) is based on [Lineage OS 21 xiaomi sm8250 kernel source](https://github.com/LineageOS/android_kernel_xiaomi_sm8250).

Originally this repo (`android12-stable-mod` or `android14-stable-mod` branch) is fork from [UtsavBalar1231's repo](https://github.com/UtsavBalar1231/kernel_xiaomi_sm8250), but the `android14-stable` branch has freeze problem when the device sleep for a while or wake up. So now I switched to lineage21 codebase. The MIUI features code and some parts of the drivers is still copied from UtsavBalar1231's branch.

So still Thanks to [@UtsavBalar1231](https://github.com/UtsavBalar1231/)!

The main purpose of maintaining and building this kernel is to fix [this battery stuck at 1% problem](https://github.com/liyafe1997/Xiaomi-fix-battery-one-percent), and provide KernelSU integrated pre-built image(flashable anykernel3 zip). Also provides a more intuitive and easy-to-use build script and build guide that allow you to try to build by yourself.

(The devices affected by the "1% battery bug" are: alioth, apollo, lmi, thyme, umi, pipa. Because they all use the PM8150, aka Qualcomm fuel gauge GEN4. For the other devices are not affected by that bug, you can use this kernel for KernelSU purpose, as a replacement of the orginal stock kernel. Also, as the people tested, this kernel NoKernelSU version is good for applying [APatch](https://github.com/bmax121/APatch)).

The pre-built kernel image/zip is built on the `android14-lineage21-mod` branch. It should works on both stock MIUI and third-party AOSP based Android 11-14 ROMs. You are welcomed to give feedback (Issues/Pull Requests)!

Note: The zip does not include the `dtbo.img` and it will not replace your `dtbo` partition. It is recommanded to keep the stock `dtbo` or the `dtbo` from your third-party rom (If the builder comfirmed it works well). Since there are some problems with the `dtbo.img` which built from this source, one of them is the screen will suddently goes to the highest brightness when shut try to shut off the screen in the lock screen. If you had flashed any other third-party kernels, and you got some weird problem, you should keep an eye to check your `dtbo` has been replaced or not. 

Supported devices:
| Code Name | Device Name                          |
|-----------|--------------------------------------|
| psyche    | Xiaomi Mi 12X                        |
| thyme     | Xiaomi Mi 10S                        |
| umi       | Xiaomi Mi 10                         |
| munch     | Poco F4 / Redmi K40S                 |
| lmi       | Redmi K30 Pro                        |
| cmi       | Xiaomi Mi 10 Pro                     |
| cas       | Xiaomi Mi 10 Ultra                   |
| apollo    | Xiaomi Mi 10T / Redmi K30S Ultra     |
| alioth    | Xiaomi Mi 11X / POCO F3 / Redmi K40  |
| elish     | Xiaomi Pad 5 Pro                     |
| enuma     | Xiaomi Pad 5 Pro 5G                  |
| dagu      | Xiaomi Pad 5 Pro 12.4                |
| pipa      | Xiaomi Pad 6                         |

Other Features/Improvement of this Kernel:
1. Support USB Serial (CH340/FTDI/PL2303/OTI6858/TI/SPCP8X5/QT2/UPD78F0730/CP210X).
2. Support EROFS.
3. F2FS realtime discard enabled for better TRIM the flash.
4. Support CANBus and USB CAN adapter (like CANable).
5. Support LZ4, LZ4HC, ZSTD compression algorithms for ZRAM.

# How to build
1. Prepair the basic build environment. 

    You have to have the basic common toolchains, such as `git`, `make`, `curl`, `bison`, `flex`, `zip`, etc, and some other packages.
    In Debian/Ubuntu, you can
    ```
    sudo add-apt-repository universe
    sudo apt update
    sudo apt install bc binutils-dev bison build-essential ca-certificates ccache clang cmake cpio curl file flex git libelf-dev libssl-dev lld       make ninja-build python3-dev texinfo u-boot-tools xz-utils zlib1g-dev
    ```
    And also, you have to have `python` (only `python3` is not enough). you can install the apt package `python-is-python3`.

    In RHEL/RPM based OS, you can
    ```
    sudo yum groupinstall 'Development Tools'
    sudo yum install wget bc openssl-devel ccache
    ```

    Notice: `ccache` is enabled in `build.sh` for speed up the compiling. `CCACHE_DIR` has been set as `$HOME/.cache/ccache_mikernel` in `build.sh`. If you don't like you can remove or modify it.

2. Download [ZYC-clang] compiler toolchain or any latest toolchain

    You have to have `aarch64-linux-gnu`, `arm-linux-gnueabi`, `clang`. [ZYC Clang](https://github.com/ZyCromerZ/Clang.git) is a good prebuilt clang cross compiler toolchain.


3. Build

    Build Kernel: 
    ```
    bash build.sh TARGET_DEVICE
    ```

    For example, build for pipa (Xiaomi Pad 6):
    ```
    bash build.sh pipa
    ````

    And also, here is a `buildall.sh` can build for all supported models at once.


