################################################################################
1. How to Build
        - get Toolchain
                get the proper toolchain packages from AOSP or CodeSourcery or ETC.
                (Download link : https://opensource.samsung.com/uploadSearch?searchValue=toolchain )
                Please decompress in 'kernel_platform' folder
                (toolchain path : kernel_platform\prebuilts, kernel_platform\prebuilts-master)

        - Set and export target config
                1. target config
                        BUILD_TARGET=
                        export MODEL=$(echo ${BUILD_TARGET} | cut -d'_' -f1)
                        export PROJECT_NAME=${MODEL}
                        export REGION=$(echo ${BUILD_TARGET} | cut -d'_' -f2)
                        export CARRIER=$(echo ${BUILD_TARGET} | cut -d'_' -f3)
                        export TARGET_BUILD_VARIANT= user
                        
                2. Chipset common config
                        CHIPSET_NAME=
                        export ANDROID_BUILD_TOP=$(pwd)
                        export TARGET_PRODUCT=gki
                        export TARGET_BOARD_PLATFORM=gki

                        export ANDROID_PRODUCT_OUT=${ANDROID_BUILD_TOP}/out/target/product/${MODEL}
                        export OUT_DIR=${ANDROID_BUILD_TOP}/out/msm-kernel-${CHIPSET_NAME}-${TARGET_PRODUCT}

                        # for Lcd(techpack) driver build
                        export KBUILD_EXTRA_SYMBOLS="/home/dpi/qb5_8814/workspace/P4_1716/android/out/vendor/qcom/opensource/mmrm-driver/Module.symvers                                                                         /home/dpi/qb5_8814/workspace/P4_1716/android/out/vendor/qcom/opensource/mm-drivers/hw_fence/Module.symvers                                                                         /home/dpi/qb5_8814/workspace/P4_1716/android/out/vendor/qcom/opensource/mm-drivers/sync_fence/Module.symvers                                                                         /home/dpi/qb5_8814/workspace/P4_1716/android/out/vendor/qcom/opensource/mm-drivers/msm_ext_display/Module.symvers                                                                         /home/dpi/qb5_8814/workspace/P4_1716/android/out/vendor/qcom/opensource/securemsm-kernel/Module.symvers                                                                         "
                        # for Audio(techpack) driver build
                        export MODNAME=audio_dlkm

                        export KBUILD_EXT_MODULES="../vendor/qcom/opensource/mm-drivers/msm_ext_display                           ../vendor/qcom/opensource/mm-drivers/sync_fence                           ../vendor/qcom/opensource/mm-drivers/hw_fence                           ../vendor/qcom/opensource/mmrm-driver                           ../vendor/qcom/opensource/securemsm-kernel                           ../vendor/qcom/opensource/display-drivers/msm                           ../vendor/qcom/opensource/audio-kernel                           ../vendor/qcom/opensource/camera-kernel                           "

        - Start to trigger build
                3. build kernel
                        RECOMPILE_KERNEL=1 ./kernel_platform/build/android/prepare_vendor.sh sec ${TARGET_PRODUCT}


2. Output files
        - Kernel : arch/${__arch_name}/boot/Image
        - module : drivers/*/*.ko

3. How to Clean
        Change to OUTPUT_DIR folder
        EX) /home/dpi/qb5_8814/workspace/P4_1716/android/out/target/product/a05s/out
        $ make clean
################################################################################
How to build Module for Platform
- It is only for modules are needed to using Android build system.
- Please check its own install information under its folder for other module.

[Step to build]
1. Get android open source.
    : version info - Android 14.0
    ( Download site : http://source.android.com )

2. Copy module that you want to build - to original android open source
   If same module exist in android open source, you should replace it. (no overwrite)
   
  # It is possible to build all modules at once.
  
3. You should add module name to 'PRODUCT_PACKAGES' in 'build/make/target/product/base_system.mk' as following case.
	case 1) libexifa : should add 'libexifa.camera.samsung' to PRODUCT_PACKAGES
	case 2) libjpega : should add 'libjpega.camera.samsung' to PRODUCT_PACKAGES
	case 3) keyutils : should add 'libknox_keyutils' to PRODUCT_PACKAGES
	

ex.) [build/make/target/product/base_system.mk] - add all module name for case 1 ~ 3 at once
    
# libexifa
PRODUCT_PACKAGES += \
    libexifa.camera.samsung
    
# libjpega
PRODUCT_PACKAGES += \
    libjpega.camera.samsung
    
# KeyUtils
PRODUCT_PACKAGES += \
    libknox_keyutils
   
4. To build files in the 'ztd/bpf_pros' folder, please add each module name to the 'required' property of 'system/bpf/bploder/Android.bp' 
   
5. excute build command
   ./build_64bit.sh

6. Note : 
   To download the source code of S/W listed below, please visit http://opensource.samsung.com and find "Mobile -> Mobile Application" menu, 
   and then, you will be able to download what you want. 
   You might save time in finding the right one by making use of the search keyword below. 
	- AREmoji.apk : "AREmoji"
	- MdecService.apk : "MdecService"
	- Notes40_Removable.apk : "Samsung Notes"
	- SamsungCalendar.apk : "SamsungCalendar"
	- VoiceNote_5.0.apk : "Voice Recorder"
	- SamsungMessages.apk : "Messaging"
	- HybridRadio.apk : "FMRadio"
	- SBrowser.apk : "SBrowser"
	- AvatarEmojiSticker.apk : "AvatarEmojiSticker"
	- Fmm.apk : "FMM"
