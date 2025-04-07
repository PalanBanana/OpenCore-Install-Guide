# 파일 수집하기

이 섹션은 macOS 부팅을 위한 기타 파일을 수집하는 것입니다. 시작하기 전에 하드웨어를 잘 알고 있기를 기대하며 여기에서 깊이 잠수하지 않을 것이기 때문에 이전에 Hackintosh를 만들 수 있기를 바랍니다.

> 내 하드웨어가 지원되는지 알아내는 가장 좋은 방법은 무엇입니까?

macOS가 부팅에 필요한 사항에 대한 더 나은 통찰력을 얻으려면 [**하드웨어 제한 페이지**](macos-limits.md)를 참조하십시오. Clover와 OpenCore 간의 하드웨어 지원은 매우 유사합니다.

> 내가 가지고 있는 하드웨어를 알아내는 몇 가지 방법이 있나요?

이전 페이지를 참조하십시오: [하드웨어 찾기](./find-hardware.md)

[[Toc]]

## 펌웨어 드라이버

펌웨어 드라이버는 UEFI 환경에서 OpenCore에서 사용하는 드라이버입니다. 그들은 주로 OpenCore의 패치 기능을 확장하거나 OpenCore 선택기에서 다른 유형의 드라이브를 보여줌으로써 기계를 부팅하는 데 필요합니다. HFS 드라이브).

* **위치 참고**: 이 파일들은 **반드시** `EFI/OC/Drivers/` 아래에 배치되어야 합니다.

### 범용

::: 팁 필수 드라이버

대부분의 시스템의 경우, 시작하고 실행하려면 2개의 `.efi` 드라이버만 있으면 됩니다.

* [HfsPlus.efi](https://github.com/acidanthera/OcBinaryData/blob/master/Drivers/HfsPlus.efi)(<span style="color:red">필수</span>)

* HFS 볼륨(예: macOS 설치 프로그램 및 복구 파티션/이미지)을 보는 데 필요합니다. **다른 HFS 드라이버를 혼합하지 마세요**

* 샌디 브리지 및 이전(저가형 아이비 브리지(i3 및 셀러론)뿐만 아니라)의 경우, 아래의 레거시 섹션을 참조하십시오.

* [OpenRuntime.efi](https://github.com/acidanthera/OpenCorePkg/releases)(<span style="color:red">필수</span>)

* [AptioMemoryFix.efi](https://github.com/acidanthera/AptioFixPkg)의 대체품, NVRAM 수정 및 더 나은 메모리 관리를 위한 boot.efi 패치를 돕기 위해 OpenCore의 확장으로 사용됩니다.

* 이것은 우리가 이전에 다운로드한 OpenCorePkg에 번들로 제공되었습니다.

:::

### 레거시 사용자

위의 것 외에도, 하드웨어가 UEFI(2011년 및 이전 시대)를 지원하지 않는 경우 다음이 필요합니다. 4개 모두 필요하지 않을 수 있으므로 각 항목에 세심한 주의를 기울이십시오.

* [OpenUsbKbDxe.efi](https://github.com/acidanthera/OpenCorePkg/releases)

* DuetPkg**을 실행하는 **레거시 시스템의 OpenCore 선택기에 사용됨, [UEFI(Ivy Bridge 및 최신)에서는 권장되지 않으며 심지어 유해합니다)](https://applelife.ru/threads/opencore-obsuzhdenie-i-ustanovka.2944066/page-176#post-856653)

* [HfsPlusLegacy.efi](https://github.com/acidanthera/OcBinaryData/blob/master/Drivers/HfsPlusLegacy.efi)

* RDRAND 명령 지원이 부족한 시스템에 사용되는 HfsPlus의 레거시 변형. 이것은 일반적으로 샌디 브리지와 더 오래된 것(저가 아이비 브리지(i3와 셀러론))에서 볼 수 있습니다.

* 이것을 HfsPlus.efi와 섞지 말고, 하드웨어에 따라 하나를 선택하세요

* [OpenPartitionDxe](https://github.com/acidanthera/OpenCorePkg/releases)

* OS X 10.7에서 10.9까지 복구를 부팅하는 데 필요

* 이 파일은 EFI/OC/Drivers 아래의 OpenCorePkg와 함께 번들로 제공됩니다.

* 참고: OpenDuet 사용자(즉, UEFI가 없는)는 이 드라이버가 내장되어 있으며 필요하지 않습니다.

* OS X 10.10, Yosemite 및 이후 버전에는 필요하지 않습니다.

이 파일들은 당신의 EFI의 드라이버 폴더에 들어갈 것입니다.

::: 세부 사항 32비트 세부 사항

32비트 CPU를 가지고 있는 사람들은 이 드라이버들도 갖고 싶을 것입니다.

* [HfsPlus32](https://github.com/acidanthera/OcBinaryData/blob/master/Drivers/HfsPlus32.efi)

* HfsPlusLegacy의 대안이지만 32비트 CPU의 경우 다른 HFS .efi 드라이버와 혼합하지 마십시오.

:::

## 펙츠

Kext는 **k**ernel **ext**ension입니다. 이것을 macOS용 드라이버로 생각할 수 있습니다. 이 파일들은 EFI의 Kexts 폴더로 이동합니다.

* **윈도우 및 리눅스 참고 사항**: Kexts는 OS의 일반 폴더처럼 보일 것입니다. **설치 중인 폴더에 .kext 확장자가 보이는지 다시 확인하십시오(그리고 누락된 경우 수동으로 추가하지 마십시오).

* 만약 어떤 kext에도 `.dSYM` 파일이 포함되어 있다면, 당신은 그것을 간단히 삭제할 수 있습니다. 그것들은 디버깅 목적으로만 사용됩니다.

* **위치 참고 사항**: 이 파일들은 **반드시** `EFI/OC/Kexts/` 아래에 배치되어야 합니다.

아래 나열된 대부분의 kext는 [build repo](http://dortania.github.io/builds/)에서 **사전 컴파일된**을 찾을 수 있습니다. 여기 Kexts는 새로운 커밋이 있을 때마다 컴파일됩니다.

### 필수품

::: 팁 필수 Kexts

아래 2가 없으면 어떤 시스템도 부팅할 수 없습니다.

* [Lilu](https://github.com/acidanthera/Lilu/releases)(<span style="color:red">필수</span>)

* AppleALC, WhateverGreen, VirtualSMC 및 기타 여러 kext에 필요한 많은 프로세스를 패치하는 kext. 릴루가 없으면, 그들은 작동하지 않을 것이다.

* Lilu는 Mac OS X 10.4부터 지원하지만 많은 플러그인은 최신 버전에서만 작동합니다.

* [VirtualSMC](https://github.com/acidanthera/VirtualSMC/releases)(<span style="color:red">필수</span>)

* 실제 Mac에서 볼 수 있는 SMC 칩을 에뮬레이트합니다. 이 macOS가 없으면 부팅되지 않습니다.

* Mac OS X 10.4 이상 필요

:::

### VirtualSMC 플러그인

아래 플러그인은 부팅할 필요가 없으며 하드웨어 모니터링과 같은 추가 기능을 시스템에 추가할 뿐입니다. 달리 명시되지 않는 한, 이 플러그인들은 VirtualSMC와 함께 제공됩니다.

::: 팁

VirtualSMC는 10.4를 지원하지만 플러그인은 최신 버전이 필요할 수 있습니다.

:::

* SMCProcessor.kext

* 인텔 CPU 온도 모니터링에 사용됨

* AMD CPU 기반 시스템용이 아닙니다.

* Mac OS X 10.7 이상 필요

* [SMCAMD 프로세서](https://github.com/trulyspinach/SMCAMD 프로세서)

* AMD Zen 기반 시스템에서 CPU 온도를 모니터링하는 데 사용됨

* **활성 개발 중, 잠재적으로 불안정**

* AMDRyzenCPUPowerManagement가 필요합니다 ([AMD CPU Specific Kexts](ktext.md#amd-cpu-specific-kexts) 참조)

* macOS 10.13 이상 필요

* [SMCRadeonSensors](https://github.com/ChefKissInc/SMCRadeonSensors)

* AMD GPU 시스템에서 GPU 온도를 모니터링하는 데 사용됩니다.

* macOS 10.14 이상 필요

* SMCSuperIO.kext

* 팬 속도 모니터링에 사용됨

* AMD CPU 기반 시스템용이 아닙니다.

* Mac OS X 10.6 이상 필요

* SMCLightSensor.kext

* 노트북의 주변광 센서에 사용됨

* ** 주변광 센서(예: 데스크탑)가 없는 경우 사용하지 마십시오. 그렇지 않으면 문제가 발생할 수 있습니다**

* Mac OS X 10.6 이상 필요

* SMCBatteryManager.kext

* 노트북의 배터리 판독값을 측정하는 데 사용됨

* **데스크탑에서 사용하지 마세요**

* Mac OS X 10.4 이상 필요

* SMCDellSensors.kext

* 시스템 관리 모드(SMM)를 지원하는 Dell 컴퓨터에서 팬을 더 세밀하고 모니터링하고 제어할 수 있습니다.

* **지원되는 Dell 기계가 없는 경우 사용하지 마십시오**, 주로 Dell 노트북이 이 kext의 이점을 누릴 수 있습니다.

* Mac OS X 10.7 이상 필요

### 그래픽

* [WhateverGreen](https://github.com/acidanthera/WhateverGreen/releases)(<span style="color:red">필수</span>)

* 그래픽 패치, DRM 수정, 보드 ID 확인, 프레임 버퍼 수정 등에 사용됩니다. 모든 GPU는 이 kext의 이점을 누릴 수 있습니다.

* 포함된 SSDT-PNLF.dsl 파일은 노트북과 AIO에만 필요합니다. 자세한 내용은 [ACPI 시작하기](https://dortania.github.io/Getting-Started-With-ACPI/)를 참조하십시오.

* Mac OS X 10.6 이상 필요

### 오디오

* [AppleALC](https://github.com/acidanthera/AppleALC/releases)

* AppleHDA 패치에 사용되어 대부분의 온보드 사운드 컨트롤러를 지원합니다.

* AppleALCU.kext는 디지털 오디오만 지원하는 AppleALC의 다소한 버전입니다. 하지만 디지털 오디오 전용 시스템에서는 AppleALC.kext를 계속 사용할 수 있습니다.

* AMD 15h/16h는 AppleALC에 문제가 있을 수 있으며 Ryzen/Threadripper 시스템은 마이크를 거의 지원하지 않습니다.

* OS X 10.4 이상 필요

::: 레거시 오디오 Kext 세부 사항

10.7 이상을 부팅하려는 사람들은 대신 다음 kexts를 선택하고 싶을 수 있습니다.

* [VoodooHDA](https://sourceforge.net/projects/voodoohda/)

* OS X 10.6 이상 필요

* [VoodooHDA-FAT](https://github.com/khronokernel/Legacy-Kexts/blob/master/FAT/Zip/VoodooHDA.kext.zip)

* 위와 비슷하지만 32비트 및 64비트 커널을 지원하므로 OS X 10.4-5 부팅 및 32비트 CPU에 적합합니다.

:::

### 이더넷

여기서 우리는 당신이 당신의 시스템에 어떤 이더넷 카드를 가지고 있는지 알고 있다고 가정할 것입니다. 제품 사양 페이지에 네트워크 카드의 유형이 나열될 가능성이 높다는 것을 상기시켜 드립니다.

* [IntelMausi](https://github.com/acidanthera/IntelMausi/releases)

* 대부분의 인텔 NIC에 필요하며, I211을 기반으로 하는 칩셋은 SmallTreeIntel82576 kext가 필요합니다.

* 인텔의 82578, 82579, I217, I218 및 I219 NIC가 공식적으로 지원됩니다.

* OS X 10.9 이상이 필요하며, 10.6-10.8 사용자는 이전 OS 대신 IntelSnowMausi를 사용할 수 있습니다.

* [AppleIGB](https://github.com/donatengit/AppleIGB/releases)

* macOS Monterey 이상에서 실행되는 I211 NIC에 필요합니다.

* 일부 NIC에 불안정성 문제가 있을 수 있습니다. Big Sur에 머물고 SmallTree를 사용하는 것이 좋습니다.

* 인텔 NIC를 실행하는 대부분의 AMD 보드에 필요

* macOS 12 이상 필요

* [SmallTreeIntel82576](https://github.com/khronokernel/SmallTree-I211-AT-patch/releases)

* SmallTree kext를 기반으로 하지만 I211을 지원하기 위해 패치된 Big Sur까지의 macOS 버전에서 실행되는 I211 NIC에 필요합니다(macOS 12 [Monterey](./extras/monterey.md#ethernet) 이상에서는 작동하지 않음)

* 인텔 NIC를 실행하는 대부분의 AMD 보드에 필요

* OS X 10.9-12(v1.0.6), macOS 10.13-14(v1.2.5), macOS 10.15+(v1.3.0)가 필요합니다.

* [AtherosE2200Ethernet](https://github.com/Mieze/AtherosE2200Ethernet/releases)

* 아테로와 킬러 NIC에 필요

* OS X 10.8 이상이 필요합니다.

* 참고: Atheros Killer E2500 모델은 실제로 Realtek 기반입니다. 이러한 시스템의 경우 대신 [RealtekRTL8111](https://github.com/Mieze/RTL8111_driver_for_OS_X/releases)를 사용하십시오.

* [RealtekRTL8111](https://github.com/Mieze/RTL8111_driver_for_OS_X/releases)

* Realtek의 기가비트 이더넷용

* 버전 v2.2.0 이하의 경우 OS X 10.8 이상, 버전 v2.2.2의 경우 macOS 10.2 이상, 버전 v2.3.0 이상의 경우 macOS 10.14 이상이 필요합니다.

* **참고:** 때때로 최신 버전의 kext가 이더넷에서 제대로 작동하지 않을 수 있습니다. 이 문제가 발생하면 이전 버전을 사용해 보십시오.

* [루시RTL8125이더넷](https://www.insanelymac.com/forum/files/file/1004-lucyrtl8125ethernet/)

* Realtek의 2.5Gb 이더넷용

* macOS 10.15 이상 필요

* 인텔의 I225-V NIC의 경우, 패치는 데스크톱 [Comet Lake DeviceProperties](config.plist/comet-lake.md#deviceproperties) 섹션에 언급되어 있습니다.

* macOS 13 이상의 경우, I225-V NIC를 지원하는 kext가 삭제되고 대신 DriverKit DEXT로 대체되었습니다. 이 DEXT는 작동하는 VT-d가 필요하므로 이전 kext를 재사용하는 것이 좋습니다: [AppleIntelI210Ethernet](extra-files/AppleIntelI210Ethernet.kext.zip)

* 몬터레이와 그 이상은 걱정할 필요가 없다

* macOS 10.15 이상 필요

* 인텔의 I350 NIC의 경우, 패치는 HEDT [Sandy and Ivy Bridge-E DeviceProperties](config-HEDT/ivy-bridge-e.md#deviceproperties) 섹션에 언급되어 있습니다. Kext는 필요하지 않습니다.

* OS X 10.10 이상 필요

::: 레거시 이더넷 Kexts에 대한 세부 정보

레거시 macOS 설치 또는 구형 PC 하드웨어와 관련이 있습니다.

* [AppleIntele1000e](https://github.com/chris1111/AppleIntelE1000e/releases)

* 주로 10/100MBe 기반 인텔 이더넷 컨트롤러와 관련

* 10.6 이상 필요

* [RealtekRTL8100](https://www.insanelymac.com/forum/files/file/259-realtekrtl8100-binary/)

* 주로 10/100MBe 기반 Realtek 이더넷 컨트롤러와 관련이 있습니다.

* macOS 10.12 이상 버전(v2.0.0 이상) 필요

* [BCM5722D](https://github.com/chris1111/BCM5722D/releases)

* 주로 BCM5722 기반 브로드컴 이더넷 컨트롤러와 관련이 있습니다.

* OS X 10.6 이상 필요

:::

또한 특정 NIC는 실제로 macOS에서 기본적으로 지원됩니다.

::: 네이티브 이더넷 컨트롤러에 대한 세부 정보

#### 아쿠아티아 시리즈

```md

# AppleEthernetAquantiaAqtion.kext

Pci1d6a,1 = Aquantia AQC107

Pci1d6a,d107 = Aquantia AQC107

Pci1d6a,7b1 = Aquantia AQC107

Pci1d6a,80b1 = Aquantia AQC107

Pci1d6a,87b1 = Aquantia AQC107

Pci1d6a,88b1 = Aquantia AQC107

Pci1d6a,89b1 = Aquantia AQC107

Pci1d6a,91b1 = Aquantia AQC107

Pci1d6a,92b1 = Aquantia AQC107

Pci1d6a,c0 = Aquantia AQC113

Pci1d6a,4c0 = Aquantia AQC113

```

**참고**: 많은 Aquantia NIC에 제공되는 일부 오래된 펌웨어로 인해 Linux/Windows에서 펌웨어를 업데이트하여 macOS와 호환되는지 확인해야 할 수 있습니다.

#### 인텔 시리즈

```md

# 애플인텔8254XEthernet.kext

Pci8086,1096 = 인텔 80003ES2LAN

Pci8086,100f = 인텔 82545EM

Pci8086,105e = 인텔 82571EB/82571GB

# AppleIntelI210Ethernet.kext

Pci8086,1533 = 인텔 I210

Pci8086,15f2 = Intel I225LM(macOS 10.15에 추가됨)

# 인텔82574L.kext

Pci8086,104b = 인텔 82566DC

Pci8086,10f6 = 인텔 82574L

```

#### 브로드컴 시리즈

```md

# AppleBCM5701Ethernet.kext

Pci14e4,1684 = 브로드컴 BCM5764M

Pci14e4,16b0 = Broadcom BCM57761

Pci14e4,16b4 = Broadcom BCM57765

Pci14e4,1682 = 브로드컴 BCM57762

Pci14e4,1686 = 브로드컴 BCM57766

```

:::

### USB

* USBToolBox ([tool](https://github.com/USBToolBox/tool) 및 [kext](https://github.com/USBToolBox/kext))

* Windows 및 macOS용 USB 매핑 도구.

* 포트 제한 문제를 피하기 위해 macOS를 설치하기 전에 USB 포트를 매핑하는 것이 좋습니다.

* 특징

* Windows 및 macOS에서 매핑 지원(Linux 지원 진행 중)

* USBToolBox kext 또는 기본 Apple kexts(AppleUSBHostMergeProperties)를 사용하여 지도를 만들 수 있습니다.

* 다양한 매칭 방법을 지원합니다.

* 컴패니언 포트 지원(윈도우)

* [XHCI-지원되지 않음](https://github.com/RehabMan/OS-X-USB-Inject-All)

* 네이티브가 아닌 USB 컨트롤러에 필요

* AMD CPU 기반 시스템은 이것이 필요하지 않습니다.

* 이것을 필요로 하는 일반적인 칩셋:

* H370

* B360

* H310

* Z390 (모하비 및 최신에서는 필요하지 않음)

* X79

* X99

* ASRock 인텔 보드 (그러나 B460/Z490+ 보드는 필요하지 않습니다)

### WiFi 및 Bluetooth

#### 네이티브가 아닌 Bluetooth 카드

* [BlueToolFixup](https://github.com/acidanthera/BrcmPatchRAM/releases)

* 타사 카드를 지원하기 위해 macOS 12+ Bluetooth 스택을 패치합니다.

* 모든 비 네이티브(Apple Broadcom, Intel 등이 아닌) Bluetooth 카드에 필요함

* [BrcmPatchRAM](#broadcom) zip에 포함됨

* **macOS 11 및 이전 버전에서는 사용하지 마십시오**

#### 인텔

* [AirportItlwm](https://github.com/OpenIntelWireless/itlwm/releases)

* 다양한 Intel 무선 카드에 대한 지원을 추가하고 IO80211Family 통합 덕분에 복구 시 기본적으로 작동합니다.

* macOS 10.13 이상이 필요하며 올바르게 작동하려면 Apple의 보안 부팅이 필요합니다.

* [Itlwm](https://github.com/OpenIntelWireless/itlwm/releases)

* Apple의 보안 부팅을 활성화할 수 없는 시스템에 대한 AirportItlwm의 대안

* [Heliport](https://github.com/OpenIntelWireless/HeliPort/releases)가 필요합니다.

* 이더넷 카드로 취급되며 헬리포트를 통해 Wi-Fi에 연결해야 합니다.

* **macOS 복구에서는 작동하지 않음**

* [IntelBluetoothFirmware](https://github.com/OpenIntelWireless/IntelBluetoothFirmware/releases)

* Intel 무선 카드와 페어링할 때 macOS에 Bluetooth 지원 추가

* macOS에서 버그를 패치하는 것 외에도 IntelBTPatcher(포함)를 사용하십시오.

* macOS 10.13 이상 필요

* macOS 10.13부터 11까지에서는 IntelBluetoothInjector(포함)도 필요합니다.

::: 세부 사항 AirportItlwm 활성화에 대한 추가 정보

OpenCore에서 AirportItlwm 지원을 활성화하려면 다음을 해야 합니다.

* `Misc -> Security -> SecureBootModel`을 `Default` 또는 다른 유효한 값으로 설정하여 활성화합니다.

* 이 내용은 이 가이드의 뒷부분과 설치 후 가이드에서 논의됩니다. [Apple Secure Boot](https://dortania.github.io/OpenCore-Post-Install/universal/security/applesecureboot.html)

* SecureBootModel을 활성화할 수 없는 경우에도 IO80211Family를 강제로 삽입할 수 있습니다 (**매우 권장되지 않음**)

* config.plist의 `Kernel -> Force`에서 다음을 설정하세요 (이 가이드의 뒷부분에서 논의됨):

![](./Images/ktext-md/force-io80211.png)

:::

#### 브로드컴

* [AirportBrcmFixup](https://github.com/acidanthera/AirportBrcmFixup/releases)

* Apple/Fenvi가 아닌 Broadcom 카드를 패치하는 데 사용됨, **Intel, Killer, Realtek 등에서는 작동하지 않음**

* OS X 10.10 이상 필요

* Big Sur의 경우 AirPortBrcm4360 드라이버에 대한 추가 단계는 [Big Sur Known Issues](./extras/big-sur#known-issues)를 참조하십시오.

* [BrcmPatchRAM](https://github.com/acidanthera/BrcmPatchRAM/releases)

* Broadcom Bluetooth 칩셋에 펌웨어를 업로드하는 데 사용되며, 모든 비 Apple/Non-Fenvi Airport 카드에 필요합니다.

* BrcmFirmwareData.kext와 페어링

* 10.15+용 BrcmPatchRAM3 (BrcmBluetoothInjector와 페어링해야 함)

* 10.11-10.14용 BrcmPatchRAM2

* 10.8-10.10을 위한 BrcmPatchRAM

* macOS 10.11부터 macOS 11까지는 BrcmBluetoothInjector(포함)도 필요합니다.

::: 세부 사항 BrcmPatchRAM 로드 순서

`커널 -> 추가`의 순서는 다음과 같아야 한다:

1. BrcmBluetoothInjector (필요한 경우)

2. Brcm펌웨어데이터

3. BrcmPatchRAM3(또는 BrcmPatchRAM2/BrcmPatchRAM)

BlueToolFixup은 Lilu 이후 어디에나 있을 수 있습니다.

그러나 ProperTree가 당신을 위해 이것을 처리할 것이므로, 당신은 걱정할 필요가 없습니다.

:::

### AMD CPU 특정 kexts

* [XLNCUSBFIX](https://github.com/dortania/OpenCore-Install-Guide/blob/master/extra-files/XLNCUSBFix.kext.zip)

* AMD FX 시스템에 대한 USB 수정, Ryzen에는 권장되지 않음

* macOS 10.13 이상 필요

* [VoodooHDA](https://sourceforge.net/projects/voodoohda/)

* Ryzen 시스템을 위한 FX 시스템용 오디오 및 전면 패널 마이크+오디오 지원, AppleALC와 혼합하지 마십시오. 오디오 품질은 Zen CPU의 AppleALC보다 눈에 띄게 나쁩니다.

* OS X 10.6 이상 필요

* macOS 11.3 이상에서 이 kext를 사용하는 것은 macOS 파일 시스템을 수정하고 SIP를 비활성화해야 하기 때문에 권장되지 않습니다.

* [AMDRyzenCPUPowerManagement](https://github.com/trulyspinach/SMCAMDProcessor)

* Ryzen 시스템을 위한 CPU 전원 관리

* **활성 개발 중, 잠재적으로 불안정**

* macOS 10.13 이상 필요

### 엑스트라

* [AppleMCEReporterDisabler](https://github.com/acidanthera/bugtracker/files/3703498/AppleMCEReporterDisabler.kext.zip)

* AMD 시스템의 경우 macOS 12.3 및 이후 버전, 듀얼 소켓 Intel 시스템의 경우 macOS 10.15 및 이후 버전에 필요합니다.

* 영향을 받는 SMBIOS:

* 맥프로6,1

* 맥프로7,1

* iMacPro1,1

* [CpuTscSync](https://github.com/lvs1974/CpuTscSync/releases)

* 일부 인텔의 HEDT 및 서버 마더보드에서 TSC를 동기화하는 데 필요하며, 이 macOS가 없으면 매우 느리거나 부팅할 수 없습니다.

* **AMD CPU에서는 작동하지 않습니다**

* OS X 10.8 이상이 필요합니다.

* [NVMeFix](https://github.com/acidanthera/NVMeFix/releases)

* Apple이 아닌 NVMe에서 전원 관리 및 초기화 수정에 사용됨

* macOS 10.14 이상 필요

* [SATA-지원되지 않음](https://github.com/khronokernel/Legacy-Kexts/blob/master/Injectors/Zip/SATA-unsupported.kext.zip)

* macOS에서 SATA 드라이브를 보는 데 문제가 있는 노트북과 관련된 다양한 SATA 컨트롤러에 대한 지원을 추가합니다. 우리는 먼저 이것 없이 테스트하는 것을 권장합니다.

* Big Sur+ 참고: [CtlnaAHCIPort](https://github.com/dortania/OpenCore-Install-Guide/blob/master/extra-files/CtlnaAHCIPort.kext.zip)는 바이너리 자체에서 수많은 컨트롤러가 삭제되기 때문에 대신 사용해야 합니다.

* 카탈리나 이상은 걱정할 필요가 없습니다

* [CPUTopologyRebuild](https://github.com/b00t0x/CpuTopologyRebuild)

* Alder Lake의 이질적인 코어 구성을 최적화하는 실험적인 Lilu 플러그인. **Alder Lake CPU 전용**

* [제한 이벤트](https://github.com/acidanthera/RestrictEvents)

* macOS의 다양한 기능을 패치합니다. 자세한 내용은 [README](https://github.com/acidanthera/RestrictEvents#boot-arguments)를 참조하십시오.

* [EmeraldSDHC](https://github.com/acidanthera/EmeraldSDHC)

* eMMC 지원을 위한 macOS 커널 확장. 현재 최대 HS200 속도로 eMMC/MMC 카드만 지원합니다. 이 드라이버는 현재 진행 중인 작업이며 일부 기기에서 성능이 저하되거나 작동하지 않을 수 있습니다. SD 카드는 현재 지원되지 않습니다.

::: 레거시 SATA Kexts 세부 사항

* [AppleIntelPIIXATA.kext](https://github.com/dortania/OpenCore-Legacy-Patcher/blob/d20d9975c144728da7ae2543d65422f53dabaa2d/payloads/Kexts/Misc/AppleIntelPIIXATA-v1.0.0.zip)

* 구형 Core 2 Duo/Quad 및 Pentium 4 시스템을 위한 레거시 IDE 및 ATA kext. 이 kext가 macOS 10.15(카탈리나)에서 삭제되기 때문에 macOS 11(Big Sur) 및 최신 버전에 필요합니다.

* [AHCIPortInjector](https://github.com/khronokernel/Legacy-Kexts/blob/master/Injectors/Zip/AHCIPortInjector.kext.zip)

* 레거시 SATA/AHCI 인젝터, 주로 Penryn 시대의 오래된 기계와 관련이 있습니다.

* [ATAPortInjector](https://github.com/khronokernel/Legacy-Kexts/blob/master/Injectors/Zip/ATAPortInjector.kext.zip)

* 레거시 ATA 인젝터, 주로 IDE 및 ATA 장치와 관련이 있습니다(즉, BIOS에 AHCI 옵션이 없는 경우)

* macOS 11(Big Sur) 및 이후 버전을 사용할 때 포함되어야 하는 AppleIntelPIIXATA.kext에 의존합니다.

:::

### 노트북 입력

어떤 종류의 키보드와 트랙패드를 가지고 있는지 알아보려면 Windows의 장치 관리자 또는 Linux의 `dmesg | grep -i input`을 확인하십시오.

::: 경고

대부분의 노트북 키보드는 PS2입니다! I2C, USB 또는 SMBus 트랙패드가 있더라도 VoodooPS2를 잡고 싶을 것입니다.

:::

#### PS2 키보드/트랙패드

* [VoodooPS2](https://github.com/acidanthera/VoodooPS2/releases)

* 다양한 PS2 키보드, 마우스 및 트랙패드와 함께 작동

* MT2(Magic Trackpad 2) 기능을 사용하려면 macOS 10.11 및 이후 버전이 필요합니다.

* [RehabMan의 VoodooPS2](https://bitbucket.org/RehabMan/os-x-voodoo-ps2-controller/downloads/)

* PS2 키보드, 마우스 및 트랙패드가 있는 구형 시스템의 경우 또는 VoodooInput을 사용하고 싶지 않은 경우

* macOS 10.6+ 지원

#### SMBus 트랙패드

* [VoodooRMI](https://github.com/VoodooSMBus/VoodooRMI/releases)

* Synaptics SMBus 트랙패드가 있는 시스템의 경우

* MT2 기능을 사용하려면 macOS 10.11 이상이 필요합니다.

* Acidanthera의 VoodooPS2에 따라 다름

* [VoodooSMBus](https://github.com/VoodooSMBus/VoodooSMBus/releases)

* ELAN SMBus 트랙패드가 있는 시스템의 경우

* 현재 macOS 10.14 이상을 지원합니다.

#### I2C/USB HID 기기

* [VoodooI2C](https://github.com/VoodooI2C/VoodooI2C/releases)

* macOS 10.11+ 지원

* 플러그인이 I2C 트랙패드와 대화할 수 있도록 I2C 컨트롤러에 부착

* 아래 플러그인을 사용하는 USB 장치는 여전히 VoodooI2C가 필요합니다

* 아래에 표시된 하나 이상의 플러그인과 페어링되어야 합니다.

::: 팁 VoodooI2C 플러그인

| 연결 유형 | 플러그인 | 메모 |

| :--- | :--- | :--- |

| 멀티터치 HID | VoodooI2CHID | I2C/USB 터치스크린 및 트랙패드와 함께 사용 가능 |

| ELAN 독점 | VoodooI2CElan | ELAN1200+는 대신 VoodooI2CHID가 필요합니다 |

| FTE1001 터치패드 | VoodooI2CFTE | |

| Atmel 멀티터치 프로토콜 | VoodooI2CAtmelMXT | |

| Synaptics HID | [VoodooRMI](https://github.com/VoodooSMBus/VoodooRMI/releases) | I2C Synaptic 트랙패드 (I2C 모드에만 VoodooI2C 필요) |

| Alps HID | [AlpsHID](https://github.com/blankmac/AlpsHID/releases) | USB 또는 I2C Alps 트랙패드와 함께 사용할 수 있습니다. 대부분 Dell 노트북과 일부 HP EliteBook 모델에서 볼 수 있습니다.

:::

#### 기타

* [ECEnabler](https://github.com/1Revenger1/ECEnabler/releases)

* 많은 기기에서 배터리 상태를 읽는 것을 수정합니다(8비트 이상의 EC 필드를 읽을 수 있음)

* OS X 10.7 이상 지원(10.4 - 10.6에서는 필요하지 않음)

* [BrightnessKeys](https://github.com/acidanthera/BrightnessKeys/releases)

* 밝기 키를 자동으로 수정합니다.

지원되는 kexts의 전체 목록은 [Kexts.md](https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/Kexts.md)를 참조하십시오.

## SSDT

그래서 당신은 AcpiSamples 폴더에 있는 모든 SSDT를 보고 그것들이 필요한지 궁금해합니다. 필요한 SSDT는 플랫폼별이기 때문에 config.plist의 **특정 ACPI 섹션**에서 필요한 SSDT를 검토할 것입니다. 구성이 필요한 일부 시스템 특정과 함께 지금부터 선택할 SSDT 목록을 제공하면 쉽게 길을 잃을 수 있습니다.

[ACPI 시작하기](https://dortania.github.io/Getting-Started-With-ACPI/)에는 다른 플랫폼에서 컴파일하는 것을 포함하여 SSDT에 대한 확장 섹션이 있습니다.

필요한 SSDT의 빠른 TL;DR(이것은 소스 코드이며, .aml 파일로 컴파일해야 합니다):

### 데스크탑

| 플랫폼 | **CPU** | **EC** | **AWAC** | **NVRAM** | **USB** |

| :-------: | :-----: | :----: | :-----: | :-------: | :-----: |

| 펜린 | 해당 없음 | [SSDT-EC](https://dortania.github.io/Getting-Started-With-ACPI/Universal/ec-fix.html) | 해당 없음 | 해당 없음 | 해당 없음 |

| 린필드와 클라크데일 | ^^ | ^^ | ^^ | ^^ | ^^ |

| SandyBridge | [CPU-PM](https://dortania.github.io/OpenCore-Post-Install/universal/pm.html#sandy-and-ivy-bridge-power-management) (설치 후 실행) | ^^ | ^^ | ^^ | | ^^ |

| 아이비 브리지 | ^^ | ^^ | ^^ | ^^ | ^^ |

| Haswell | [SSDT-PLUG](https://dortania.github.io/Getting-Started-With-ACPI/Universal/plug.html) | ^^ | ^^ | ^^ | ^^ |

| 브로드웰 | ^^ | ^^ | ^^ | ^^ | ^^ |

| 스카이레이크 | ^^ | [SSDT-EC-USBX](https://dortania.github.io/Getting-Started-With-ACPI/Universal/ec-fix.html) | ^^ | ^^ | ^^ |

| 카비 호수 | ^^ | ^^ | ^^ | ^^ | ^^ |

| 커피 레이크 | ^^ | ^^ | [SSDT-AWAC](https://dortania.github.io/Getting-Started-With-ACPI/Universal/awac.html) | [SSDT-PMC](https://dortania.github.io/Getting-Started-With-ACPI/Universal/nvram.html) | ^^ |

| 혜성 호수 | ^^ | ^^ | ^^ | N/A | [SSDT-RHUB](https://dortania.github.io/Getting-Started-With-ACPI/Universal/rhub.html) |

| AMD (15/16h) | 해당 없음 | ^^ | 해당 없음 | ^^ | 해당 없음 |

| AMD (17/19h) | [B550 및 A520용 SSDT-CPUR](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-CPUR.aml) | ^^ | ^^ | ^^ | ^^ | | ^^ |

### 하이엔딩 데스크탑

| 플랫폼 | **CPU** | **EC** | **RTC** | **PCI** |

| :-------: | :-----: | :----: | :-----: | :-----: |

| 네할렘과 웨스트미어 | 해당 없음 | [SSDT-EC](https://dortania.github.io/Getting-Started-With-ACPI/Universal/ec-fix.html) | 해당 없음 | 해당 없음 |

| 샌디 브리지-E | ^^ | ^^ | ^^ | [SSDT-UNC](https://dortania.github.io/Getting-Started-With-ACPI/Universal/unc0) |

| 아이비 브리지-E | ^^ | ^^ | ^^ | ^^ |

| Haswell-E | [SSDT-PLUG](https://dortania.github.io/Getting-Started-With-ACPI/Universal/plug.html) | [SSDT-EC-USBX](https://dortania.github.io/Getting-Started-With-ACPI/Universal/ec-fix.html) | [SSDT-RTC0-RANGE](https://dortania.github.io/Getting-Started-With-ACPI/Universal/awac.html) | ^^ |

| 브로드웰-E | ^^ | ^^ | ^^ | ^^ |

| 스카이레이크-X | ^^ | ^^ | ^^ | 해당 없음 |

### 노트북

| 플랫폼 | **CPU** | **EC** | **백라이트** | **I2C 트랙패드** | **AWAC** | **USB** | **IRQ** |

| :-------: | :-----: | :----: | :----------------: | :-------------: | :-----: | :-----: | :-----: |

| Clarksfield and Arrandale | N/A | [SSDT-EC](https://dortania.github.io/Getting-Started-With-ACPI/Universal/ec-fix.html) | [SSDT-PNLF](https://dortania.github.io/Getting-Started-With-ACPI/Laptops/backlight.html) | N/A | N/A | N/A | [IRQ SSDT](https://dortania.github.io/Getting-Started-With-ACPI/Universal/irq.html) |

| 샌디브릿지 | [CPU-PM](https://dortania.github.io/OpenCore-Post-Install/universal/pm.html#sandy-and-ivy-bridge-power-management) (설치 후 실행) | ^^ | ^^ | ^^ | ^^ | | ^^ | ^^ |

| 아이비 브리지 | ^^ | ^^ | ^^ | ^^ | ^^ | ^^ |

| Haswell | [SSDT-PLUG](https://dortania.github.io/Getting-Started-With-ACPI/Universal/plug.html) | ^^ | ^^ | [SSDT-GPI0](https://dortania.github.io/Getting-Started-With-ACPI/Laptops/trackpad.html) | ^^ | ^^ | ^^ |

| 브로드웰 | ^^ | ^^ | ^^ | ^^ | ^^ | ^^ | ^^ |

| 스카이레이크 | ^^ | [SSDT-EC-USBX](https://dortania.github.io/Getting-Started-With-ACPI/Universal/ec-fix.html) | ^^ | ^^ | ^^ | ^^ | N/A |

| 카비 호수 | ^^ | ^^ | ^^ | ^^ | ^^ | ^^ |

| 커피 레이크(8세대)와 위스키 레이크 | ^^ | ^^ | [SSDT-PNLF](https://dortania.github.io/Getting-Started-With-ACPI/Laptops/backlight.html) | ^^ | [SSDT-AWAC](https://dortania.github.io/Getting-Started-With-ACPI/Universal/awac.html) | ^^ | ^^ |

| 커피 호수 (9세대) | ^^ | ^^ | ^^ | ^^ | ^^ | ^^ | ^^ |

| 혜성 호수 | ^^ | ^^ | ^^ | ^^ | ^^ | ^^ | ^^ |

| 아이스 레이크 | ^^ | ^^ | ^^ | ^^ | ^^ | [SSDT-RHUB](https://dortania.github.io/Getting-Started-With-ACPI/Universal/rhub.html) | ^^ |

계속:

| 플랫폼 | **NVRAM** | **IMEI** |

| :-------: | :-------: | :------: |

| 클락스필드와 아랜데일 | 해당 없음 | 해당 없음 |

| 샌디 브리지 | ^^| [SSDT-IMEI](https://dortania.github.io/Getting-Started-With-ACPI/Universal/imei.html) |

| 아이비 브리지 | ^^ | ^^ |

| 하스웰 | ^^ | 해당 없음 |

| 브로드웰 | ^^ | ^^ |

| 스카이레이크 | ^^ | ^^ |

| 카비 호수 | ^^ | ^^ |

| 커피 호수(8세대)와 위스키 호수 | ^^ | ^^ |

| 커피 호수 (9세대) | [SSDT-PMC](https://dortania.github.io/Getting-Started-With-ACPI/Universal/nvram.html) | ^^ |

| 혜성 호수 | 해당 없음 | ^^ |

| 얼음 호수 | ^^ | ^^ |

# 이제 이 모든 것이 끝났으니 [ACPI 시작하기](https://dortania.github.io/Getting-Started-With-ACPI/)로 가세요.rarely have mic support
  * Requires OS X 10.4 or newer
  
::: details Legacy Audio Kext

For those who plan to boot 10.7 and older may want to opt for these kexts instead:

* [VoodooHDA](https://sourceforge.net/projects/voodoohda/)
  * Requires OS X 10.6 or newer
  
* [VoodooHDA-FAT](https://github.com/khronokernel/Legacy-Kexts/blob/master/FAT/Zip/VoodooHDA.kext.zip)
  * Similar to the above, however supports 32 and 64-Bit kernels so perfect for OS X 10.4-5 booting and 32-Bit CPUs

:::

### Ethernet

Here we're going to assume you know what ethernet card your system has, reminder that product spec pages will most likely list the type of network card.

* [IntelMausi](https://github.com/acidanthera/IntelMausi/releases)
  * Required for the majority of Intel NICs, chipsets that are based off of I211 will need the SmallTreeIntel82576 kext
  * Intel's 82578, 82579, I217, I218 and I219 NICs are officially supported
  * Requires OS X 10.9 or newer, 10.6-10.8 users can use IntelSnowMausi instead for older OSes
* [AppleIGB](https://github.com/donatengit/AppleIGB/releases)
  * Required for I211 NICs running on macOS Monterey and above
  * Might have instability issues on some NICs, recommended to stay on Big Sur and use SmallTree
  * Required for most AMD boards running Intel NICs
  * Requires macOS 12 and above
* [SmallTreeIntel82576](https://github.com/khronokernel/SmallTree-I211-AT-patch/releases)
  * Required for I211 NICs running on macOS versions up to Big Sur, based off of the SmallTree kext but patched to support I211 (doesn't work on macOS 12 [Monterey](./extras/monterey.md#ethernet) or above)
  * Required for most AMD boards running Intel NICs
  * Requires OS X 10.9-12(v1.0.6), macOS 10.13-14(v1.2.5), macOS 10.15+(v1.3.0)
* [AtherosE2200Ethernet](https://github.com/Mieze/AtherosE2200Ethernet/releases)
  * Required for Atheros and Killer NICs
  * Requires OS X 10.8 or newer
  * Note: Atheros Killer E2500 models are actually Realtek based, for these systems please use [RealtekRTL8111](https://github.com/Mieze/RTL8111_driver_for_OS_X/releases) instead
* [RealtekRTL8111](https://github.com/Mieze/RTL8111_driver_for_OS_X/releases)
  * For Realtek's Gigabit Ethernet
  * Requires OS X 10.8 and up for versions v2.2.0 and below, macOS 10.12 and up for version v2.2.2, macOS 10.14 and up for versions v2.3.0 and up
  * **NOTE:** Sometimes the latest version of the kext might not work properly with your Ethernet. If you see this issue, try older versions.
* [LucyRTL8125Ethernet](https://www.insanelymac.com/forum/files/file/1004-lucyrtl8125ethernet/)
  * For Realtek's 2.5Gb Ethernet
  * Requires macOS 10.15 or newer
* For Intel's I225-V NICs, patches are mentioned in the desktop [Comet Lake DeviceProperties](config.plist/comet-lake.md#deviceproperties) section.
  * For macOS 13 and above, the kext supporting I225-V NICs was dropped and replaced with a DriverKit DEXT instead. This DEXT requires working VT-d, so we recommended reusing the older kext: [AppleIntelI210Ethernet](extra-files/AppleIntelI210Ethernet.kext.zip)
    * Monterey and older need not concern
  * Requires macOS 10.15 or newer
* For Intel's I350 NICs, patches are mentioned in the HEDT [Sandy and Ivy Bridge-E DeviceProperties](config-HEDT/ivy-bridge-e.md#deviceproperties) section. No kext is required.
  * Requires OS X 10.10 or newer

::: details Legacy Ethernet Kexts

Relevant for either legacy macOS installs or older PC hardware.

* [AppleIntele1000e](https://github.com/chris1111/AppleIntelE1000e/releases)
  * Mainly relevant for 10/100MBe based Intel Ethernet controllers
  * Requires 10.6 or newer
* [RealtekRTL8100](https://www.insanelymac.com/forum/files/file/259-realtekrtl8100-binary/)
  * Mainly relevant for 10/100MBe based Realtek Ethernet controllers
  * Requires macOS 10.12 or newer with v2.0.0+
* [BCM5722D](https://github.com/chris1111/BCM5722D/releases)
  * Mainly relevant for BCM5722 based Broadcom Ethernet controllers
  * Requires OS X 10.6 or newer

:::

And also keep in mind certain NICs are actually natively supported in macOS:

::: details Native Ethernet Controllers

#### Aquantia Series

```md
# AppleEthernetAquantiaAqtion.kext
pci1d6a,1    = Aquantia AQC107
pci1d6a,d107 = Aquantia AQC107
pci1d6a,7b1  = Aquantia AQC107
pci1d6a,80b1 = Aquantia AQC107
pci1d6a,87b1 = Aquantia AQC107
pci1d6a,88b1 = Aquantia AQC107
pci1d6a,89b1 = Aquantia AQC107
pci1d6a,91b1 = Aquantia AQC107
pci1d6a,92b1 = Aquantia AQC107
pci1d6a,c0   = Aquantia AQC113
pci1d6a,4c0  = Aquantia AQC113
```

**Note**: Due to some outdated firmware shipped on many Aquantia NICs, you may need to update the firmware in Linux/Windows to ensure it's macOS-compatible.

#### Intel Series

```md
# AppleIntel8254XEthernet.kext
pci8086,1096 = Intel 80003ES2LAN
pci8086,100f = Intel 82545EM
pci8086,105e = Intel 82571EB/82571GB

# AppleIntelI210Ethernet.kext
pci8086,1533 = Intel I210
pci8086,15f2 = Intel I225LM (Added in macOS 10.15)

# Intel82574L.kext
pci8086,104b = Intel 82566DC
pci8086,10f6 = Intel 82574L

```

#### Broadcom Series

```md
# AppleBCM5701Ethernet.kext
pci14e4,1684 = Broadcom BCM5764M
pci14e4,16b0 = Broadcom BCM57761
pci14e4,16b4 = Broadcom BCM57765
pci14e4,1682 = Broadcom BCM57762
pci14e4,1686 = Broadcom BCM57766
```

:::

### USB

* USBToolBox ([tool](https://github.com/USBToolBox/tool) and [kext](https://github.com/USBToolBox/kext))
  * USB mapping tool for Windows and macOS.
  * It is highly advisable to map your USB ports before you install macOS to avoid any port limit issues
  * Features
    * Supports mapping from Windows and macOS (Linux support in progress)
    * Can build a map using either the USBToolBox kext or native Apple kexts (AppleUSBHostMergeProperties)
    * Supports multiple ways of matching
    * Supports companion ports (on Windows)

* [XHCI-unsupported](https://github.com/RehabMan/OS-X-USB-Inject-All)
  * Needed for non-native USB controllers
  * AMD CPU based systems don't need this
  * Common chipsets needing this:
    * H370
    * B360
    * H310
    * Z390 (not needed on Mojave and newer)
    * X79
    * X99
    * ASRock Intel boards (B460/Z490+ boards do not need it however)

### WiFi and Bluetooth

#### Non-Native Bluetooth Cards

* [BlueToolFixup](https://github.com/acidanthera/BrcmPatchRAM/releases)
  * Patches the macOS 12+ Bluetooth stack to support third-party cards
  * Needed for all non-native (non-Apple Broadcom, Intel, etc) Bluetooth cards
  * Included in the [BrcmPatchRAM](#broadcom) zip
  * **Do not use on macOS 11 and earlier**

#### Intel

* [AirportItlwm](https://github.com/OpenIntelWireless/itlwm/releases)
  * Adds support for a large variety of Intel wireless cards and works natively in recovery thanks to IO80211Family integration
  * Requires macOS 10.13 or newer and requires Apple's Secure Boot to function correctly
* [Itlwm](https://github.com/OpenIntelWireless/itlwm/releases)
  * Alternative to AirportItlwm for systems where Apple's Secure Boot cannot be enabled
  * Requires [Heliport](https://github.com/OpenIntelWireless/HeliPort/releases)
  * It will be treated as an Ethernet card, and you will have to connect to Wi-Fi via Heliport
  * **Does not work in macOS recovery**
* [IntelBluetoothFirmware](https://github.com/OpenIntelWireless/IntelBluetoothFirmware/releases)
  * Adds Bluetooth support to macOS when paired with an Intel wireless card
  * Use IntelBTPatcher (included) in addition to patch bugs in macOS
  * Requires macOS 10.13 or newer
  * On macOS 10.13 through 11, you also need IntelBluetoothInjector (included)

::: details More info on enabling AirportItlwm

To enable AirportItlwm support with OpenCore, you'll need to either:

* Enable `Misc -> Security -> SecureBootModel` by either setting it as `Default` or some other valid value
  * This is discussed both later on in this guide and in the post-install guide: [Apple Secure Boot](https://dortania.github.io/OpenCore-Post-Install/universal/security/applesecureboot.html)
* If you cannot enable SecureBootModel, you can still force inject IO80211Family (**highly discouraged**)
  * Set the following under `Kernel -> Force` in your config.plist (discussed later in this guide):
  
![](./images/ktext-md/force-io80211.png)

:::

#### Broadcom

* [AirportBrcmFixup](https://github.com/acidanthera/AirportBrcmFixup/releases)
  * Used for patching non-Apple/non-Fenvi Broadcom cards, **will not work on Intel, Killer, Realtek, etc**
  * Requires OS X 10.10 or newer
  * For Big Sur see [Big Sur Known Issues](./extras/big-sur#known-issues) for extra steps regarding AirPortBrcm4360 drivers.
* [BrcmPatchRAM](https://github.com/acidanthera/BrcmPatchRAM/releases)
  * Used for uploading firmware on Broadcom Bluetooth chipset, required for all non-Apple/non-Fenvi Airport cards.
  * To be paired with BrcmFirmwareData.kext
    * BrcmPatchRAM3 for 10.15+ (must be paired with BrcmBluetoothInjector)
    * BrcmPatchRAM2 for 10.11-10.14
    * BrcmPatchRAM for 10.8-10.10
  * On macOS 10.11 through macOS 11, you also need BrcmBluetoothInjector (included)

::: details BrcmPatchRAM Load order

The order in `Kernel -> Add` should be:

1. BrcmBluetoothInjector (if needed)
2. BrcmFirmwareData
3. BrcmPatchRAM3 (or BrcmPatchRAM2/BrcmPatchRAM)

BlueToolFixup can be anywhere after Lilu.

However ProperTree will handle this for you, so you need not concern yourself

:::

### AMD CPU Specific kexts

* [XLNCUSBFIX](https://github.com/dortania/OpenCore-Install-Guide/blob/master/extra-files/XLNCUSBFix.kext.zip)
  * USB fix for AMD FX systems, not recommended for Ryzen
  * Requires macOS 10.13 or newer
* [VoodooHDA](https://sourceforge.net/projects/voodoohda/)
  * Audio for FX systems and front panel Mic+Audio support for Ryzen system, do not mix with AppleALC. Audio quality is noticeably worse than AppleALC on Zen CPUs
  * Requires OS X 10.6 or newer
  * Using this kext on macOS 11.3 and above is not recommended as you need to modify the macOS filesystem and disable SIP
* [AMDRyzenCPUPowerManagement](https://github.com/trulyspinach/SMCAMDProcessor)
  * CPU power management for Ryzen systems
  * **Under active development, potentially unstable**
  * Requires macOS 10.13 or newer

### Extras

* [AppleMCEReporterDisabler](https://github.com/acidanthera/bugtracker/files/3703498/AppleMCEReporterDisabler.kext.zip)
  * Required on macOS 12.3 and later on AMD systems, and on macOS 10.15 and later on dual-socket Intel systems.
  * Affected SMBIOSes:
    * MacPro6,1
    * MacPro7,1
    * iMacPro1,1
* [CpuTscSync](https://github.com/lvs1974/CpuTscSync/releases)
  * Needed for syncing TSC on some of Intel's HEDT and server motherboards, without this macOS may be extremely slow or even unbootable.
  * **Does not work on AMD CPUs**
  * Requires OS X 10.8 or newer
* [NVMeFix](https://github.com/acidanthera/NVMeFix/releases)
  * Used for fixing power management and initialization on non-Apple NVMe
  * Requires macOS 10.14 or newer
* [SATA-Unsupported](https://github.com/khronokernel/Legacy-Kexts/blob/master/Injectors/Zip/SATA-unsupported.kext.zip)
  * Adds support for a large variety of SATA controllers, mainly relevant for laptops which have issues seeing the SATA drive in macOS. We recommend testing without this first.
  * Big Sur+ Note: [CtlnaAHCIPort](https://github.com/dortania/OpenCore-Install-Guide/blob/master/extra-files/CtlnaAHCIPort.kext.zip) will need to be used instead due to numerous controllers being dropped from the binary itself
    * Catalina and older need not concern
* [CPUTopologyRebuild](https://github.com/b00t0x/CpuTopologyRebuild)
  * An experimental Lilu plugin that optimizes Alder Lake's heterogeneous core configuration. **Only for Alder Lake CPUs**
* [RestrictEvents](https://github.com/acidanthera/RestrictEvents)
  * Patch various functions of macOS, see [the README](https://github.com/acidanthera/RestrictEvents#boot-arguments) for more info
* [EmeraldSDHC](https://github.com/acidanthera/EmeraldSDHC)
  * macOS kernel extension for eMMC support. Currently only supports eMMC/MMC cards at up to HS200 speeds.  This driver is currently a work in progress and may experience poor performance or be nonfunctional on some devices. SD cards are currently not supported at this time.

::: details Legacy SATA Kexts

* [AppleIntelPIIXATA.kext](https://github.com/dortania/OpenCore-Legacy-Patcher/blob/d20d9975c144728da7ae2543d65422f53dabaa2d/payloads/Kexts/Misc/AppleIntelPIIXATA-v1.0.0.zip)
  * Legacy IDE and ATA kext for older Core 2 Duo/Quad and Pentium 4 systems. Needed for macOS 11 (Big Sur) and newer as this kext was dropped in macOS 10.15 (Catalina)
* [AHCIPortInjector](https://github.com/khronokernel/Legacy-Kexts/blob/master/Injectors/Zip/AHCIPortInjector.kext.zip)
  * Legacy SATA/AHCI injector, mainly relevant for older machines of the Penryn era
* [ATAPortInjector](https://github.com/khronokernel/Legacy-Kexts/blob/master/Injectors/Zip/ATAPortInjector.kext.zip)
  * Legacy ATA injector, mainly relevant for IDE and ATA devices (ie. when no AHCI option is present in the BIOS)
  * Is dependent on AppleIntelPIIXATA.kext, which needs to be included when using macOS 11 (Big Sur) and newer
  
:::

### Laptop Input

To figure out what kind of keyboard and trackpad you have, check Device Manager in Windows or `dmesg | grep -i input` in Linux

::: warning

Most laptop keyboards are PS2! You will want to grab VoodooPS2 even if you have an I2C, USB, or SMBus trackpad.

:::

#### PS2 Keyboards/Trackpads

* [VoodooPS2](https://github.com/acidanthera/VoodooPS2/releases)
  * Works with various PS2 keyboards, mice, and trackpads
  * Requires macOS 10.11 or newer for MT2 (Magic Trackpad 2) functions
* [RehabMan's VoodooPS2](https://bitbucket.org/RehabMan/os-x-voodoo-ps2-controller/downloads/)
  * For older systems with PS2 keyboards, mice, and trackpads, or when you don't want to use VoodooInput
  * Supports macOS 10.6+

#### SMBus Trackpads

* [VoodooRMI](https://github.com/VoodooSMBus/VoodooRMI/releases)
  * For systems with Synaptics SMBus trackpads
  * Requires macOS 10.11 or newer for MT2 functions
  * Depends on Acidanthera's VoodooPS2
* [VoodooSMBus](https://github.com/VoodooSMBus/VoodooSMBus/releases)
  * For systems with ELAN SMBus Trackpads
  * Supports macOS 10.14 or newer currently

#### I2C/USB HID Devices

* [VoodooI2C](https://github.com/VoodooI2C/VoodooI2C/releases)
  * Supports macOS 10.11+
  * Attaches to I2C controllers to allow plugins to talk to I2C trackpads
  * USB devices using the below plugins still need VoodooI2C
  * Must be paired with one or more plugins shown below:

::: tip VoodooI2C Plugins

| Connection type | Plugin | Notes |
| :--- | :--- | :--- |
| Multitouch HID | VoodooI2CHID | Can be used with I2C/USB Touchscreens and Trackpads |
| ELAN Proprietary | VoodooI2CElan | ELAN1200+ require VoodooI2CHID instead |
| FTE1001 touchpad | VoodooI2CFTE | |
| Atmel Multitouch Protocol | VoodooI2CAtmelMXT | |
| Synaptics HID | [VoodooRMI](https://github.com/VoodooSMBus/VoodooRMI/releases) | I2C Synaptic Trackpads (Requires VoodooI2C ONLY for I2C mode) |
| Alps HID | [AlpsHID](https://github.com/blankmac/AlpsHID/releases) | Can be used with USB or I2C Alps trackpads. Mostly seen on Dell laptops and some HP EliteBook models |

:::

#### Misc

* [ECEnabler](https://github.com/1Revenger1/ECEnabler/releases)
  * Fixes reading battery status on many devices (Allows reading EC fields over 8 bits long)
  * Supports OS X 10.7 and above (not needed on 10.4 - 10.6)
* [BrightnessKeys](https://github.com/acidanthera/BrightnessKeys/releases)
  * Fixes brightness keys automatically

Please refer to [Kexts.md](https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/Kexts.md) for a full list of supported kexts

## SSDTs

So you see all those SSDTs in the AcpiSamples folder and wonder whether you need any of them. For us, we will be going over what SSDTs you need in **your specific ACPI section of the config.plist**, as the SSDTs you need are platform specific. With some even system specific where they need to be configured and you can easily get lost if I give you a list of SSDTs to choose from now.

[Getting started with ACPI](https://dortania.github.io/Getting-Started-With-ACPI/) has an extended section on SSDTs including compiling them on different platforms.

A quick TL;DR of needed SSDTs(This is source code, you will have to compile them into a .aml file):

### Desktop

| Platforms | **CPU** | **EC** | **AWAC** | **NVRAM** | **USB** |
| :-------: | :-----: | :----: | :------: | :-------: | :-----: |
| Penryn | N/A | [SSDT-EC](https://dortania.github.io/Getting-Started-With-ACPI/Universal/ec-fix.html) | N/A | N/A | N/A |
| Lynnfield and Clarkdale | ^^ | ^^ | ^^ | ^^ | ^^ |
| SandyBridge | [CPU-PM](https://dortania.github.io/OpenCore-Post-Install/universal/pm.html#sandy-and-ivy-bridge-power-management) (Run in Post-Install) | ^^ | ^^ | ^^ | ^^ |
| Ivy Bridge | ^^ | ^^ | ^^ | ^^ | ^^ |
| Haswell | [SSDT-PLUG](https://dortania.github.io/Getting-Started-With-ACPI/Universal/plug.html) | ^^ | ^^ | ^^ | ^^ |
| Broadwell | ^^ | ^^ | ^^ | ^^ | ^^ |
| Skylake | ^^ | [SSDT-EC-USBX](https://dortania.github.io/Getting-Started-With-ACPI/Universal/ec-fix.html) | ^^ | ^^ | ^^ |
| Kaby Lake | ^^ | ^^ | ^^ | ^^ | ^^ |
| Coffee Lake | ^^ | ^^ | [SSDT-AWAC](https://dortania.github.io/Getting-Started-With-ACPI/Universal/awac.html) | [SSDT-PMC](https://dortania.github.io/Getting-Started-With-ACPI/Universal/nvram.html) | ^^ |
| Comet Lake | ^^ | ^^ | ^^ | N/A | [SSDT-RHUB](https://dortania.github.io/Getting-Started-With-ACPI/Universal/rhub.html) |
| AMD (15/16h) | N/A | ^^ | N/A | ^^ | N/A |
| AMD (17/19h) | [SSDT-CPUR for B550 and A520](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-CPUR.aml) | ^^ | ^^ | ^^ | ^^ |

### High End Desktop

| Platforms | **CPU** | **EC** | **RTC** | **PCI** |
| :-------: | :-----: | :----: | :-----: | :-----: |
| Nehalem and Westmere | N/A | [SSDT-EC](https://dortania.github.io/Getting-Started-With-ACPI/Universal/ec-fix.html) | N/A | N/A |
| Sandy Bridge-E | ^^ | ^^ | ^^ | [SSDT-UNC](https://dortania.github.io/Getting-Started-With-ACPI/Universal/unc0) |
| Ivy Bridge-E | ^^ | ^^ | ^^ | ^^ |
| Haswell-E | [SSDT-PLUG](https://dortania.github.io/Getting-Started-With-ACPI/Universal/plug.html) | [SSDT-EC-USBX](https://dortania.github.io/Getting-Started-With-ACPI/Universal/ec-fix.html) | [SSDT-RTC0-RANGE](https://dortania.github.io/Getting-Started-With-ACPI/Universal/awac.html) | ^^ |
| Broadwell-E | ^^ | ^^ | ^^ | ^^ |
| Skylake-X | ^^ | ^^ | ^^ | N/A |

### Laptop

| Platforms | **CPU** | **EC** | **Backlight** | **I2C Trackpad** | **AWAC** | **USB** | **IRQ** |
| :-------: | :-----: | :----: | :-----------: | :--------------: | :------: | :-----: | :-----: |
| Clarksfield and Arrandale | N/A | [SSDT-EC](https://dortania.github.io/Getting-Started-With-ACPI/Universal/ec-fix.html) | [SSDT-PNLF](https://dortania.github.io/Getting-Started-With-ACPI/Laptops/backlight.html) | N/A | N/A | N/A | [IRQ SSDT](https://dortania.github.io/Getting-Started-With-ACPI/Universal/irq.html) |
| SandyBridge | [CPU-PM](https://dortania.github.io/OpenCore-Post-Install/universal/pm.html#sandy-and-ivy-bridge-power-management) (Run in Post-Install) | ^^ | ^^ | ^^ | ^^ | ^^ | ^^ |
| Ivy Bridge | ^^ | ^^ | ^^ | ^^ | ^^ | ^^ | ^^ |
| Haswell | [SSDT-PLUG](https://dortania.github.io/Getting-Started-With-ACPI/Universal/plug.html) | ^^ | ^^ | [SSDT-GPI0](https://dortania.github.io/Getting-Started-With-ACPI/Laptops/trackpad.html) | ^^ | ^^ | ^^ |
| Broadwell | ^^ | ^^ | ^^ | ^^ | ^^ | ^^ | ^^ |
| Skylake | ^^ | [SSDT-EC-USBX](https://dortania.github.io/Getting-Started-With-ACPI/Universal/ec-fix.html) | ^^ | ^^ | ^^ | ^^ | N/A |
| Kaby Lake | ^^ | ^^ | ^^ | ^^ | ^^ | ^^ | ^^ |
| Coffee Lake (8th Gen) and Whiskey Lake | ^^ | ^^ | [SSDT-PNLF](https://dortania.github.io/Getting-Started-With-ACPI/Laptops/backlight.html) | ^^ | [SSDT-AWAC](https://dortania.github.io/Getting-Started-With-ACPI/Universal/awac.html) | ^^ | ^^ |
| Coffee Lake (9th Gen) | ^^ | ^^ | ^^ | ^^ | ^^ | ^^ | ^^ |
| Comet Lake | ^^ | ^^ | ^^ | ^^ | ^^ | ^^ | ^^ |
| Ice Lake | ^^ | ^^ | ^^ | ^^ | ^^ | [SSDT-RHUB](https://dortania.github.io/Getting-Started-With-ACPI/Universal/rhub.html) | ^^ |

Continuing:

| Platforms | **NVRAM** | **IMEI** |
| :-------: | :-------: | :------: |
| Clarksfield and Arrandale | N/A | N/A |
| Sandy Bridge | ^^| [SSDT-IMEI](https://dortania.github.io/Getting-Started-With-ACPI/Universal/imei.html) |
| Ivy Bridge | ^^ | ^^ |
| Haswell | ^^ | N/A |
| Broadwell | ^^ | ^^ |
| Skylake | ^^ | ^^ |
| Kaby Lake | ^^ | ^^ |
| Coffee Lake (8th Gen) and Whiskey Lake | ^^ | ^^ |
| Coffee Lake (9th Gen) | [SSDT-PMC](https://dortania.github.io/Getting-Started-With-ACPI/Universal/nvram.html) | ^^ |
| Comet Lake | N/A | ^^ |
| Ice Lake | ^^ | ^^ |

# Now with all this done, head to [Getting Started With ACPI](https://dortania.github.io/Getting-Started-With-ACPI/)
