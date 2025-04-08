# Clover 및 기타보다 OpenCore가 나은 이유

이 섹션에는 커뮤니티가 OpenCore로 전환한 이유에 대한 간략한 설명이 포함되어 있으며, 커뮤니티에서 흔히 있는 몇 가지 오해를 해소하고자 합니다. macOS 머신만 원하는 사람은 이 페이지를 건너뛸 수 있습니다.

[[목차]]

## OpenCore 기능

* 더 많은 OS 지원!
* OpenCore는 이제 Clover와 Chameleon이 구현해야 했던 고통스러운 해킹 없이 더 많은 버전의 OS X와 ​​macOS를 기본적으로 지원합니다.
* 여기에는 10.4, Tiger, 심지어 최신 빌드인 13, Ventura까지 포함됩니다!
* 일반적으로 OpenCore 시스템은 불필요한 패치가 덜 적용되어 Clover를 사용하는 시스템보다 부팅 속도가 빠릅니다.
* 패치가 훨씬 더 정확할 수 있으므로 전반적인 안정성이 향상됩니다.
* [macOS 10.15.4 업데이트](https://www.reddit.com/r/hackintosh/comments/fo9bfv/macos_10154_update/)
* AMD OSX 패치는 모든 사소한 보안 업데이트로 업데이트할 필요가 없습니다.
* 여러 형태로 전반적인 보안이 향상됩니다.
* 시스템 무결성 보호(SIP)를 비활성화할 필요가 없습니다.
* 기본 제공 FileVault 2 지원
* [Vaulting](https://dortania.github.io/OpenCore-Post-Install/universal/security.html#Vault)을 통해 원치 않는 수정을 방지하는 EFI 스냅샷을 만들 수 있습니다.
* 진정한 보안 부팅 지원
* UEFI와 Apple의 변형 모두
* BootCamp 전환 및 부팅 장치 선택은 실제 Mac과 마찬가지로 시작 디스크에서 설정된 NVRAM 변수를 읽어 지원됩니다.
* `boot.efi`를 통한 부팅 단축키 지원 - 부팅 장치를 선택하려면 시작 시 `Option` 또는 `ESC`를 누르고, 복구를 시작하려면 `Cmd+R`을 누르고, NVRAM을 재설정하려면 `Cmd+Opt+P+R`을 누릅니다.

### 소프트웨어 지원

누군가가 다른 부트 로더에서 전환하고 싶어하는 가장 큰 이유는 실제로 소프트웨어 지원입니다.

* Kext가 더 이상 Clover를 테스트하지 않음:
* Kext에 버그가 있습니까? [Acidanthera](https://github.com/acidanthera) 조직(대부분의 인기 있는 kext 제작자)을 포함한 많은 개발자는 OpenCore가 아닌 이상 지원을 제공하지 않습니다.
* OpenCore에 병합되는 많은 펌웨어 드라이버:
* [APFS 지원](https://github.com/acidanthera/AppleSupportPkg)
* [FileVault 지원](https://github.com/acidanthera/AppleSupportPkg)
* [펌웨어 패치](https://github.com/acidanthera/AptioFixPkg)

## OpenCore의 단점

Clover의 기능 대부분은 실제로 OpenCore에서 약간의 엉뚱한 형태로 지원되지만 전환할 때는 OpenCore의 누락된 기능에 주의를 기울여야 합니다. 이는 사용자에게 영향을 미칠 수도 있고 미치지 않을 수도 있습니다.

* MBR 기반 운영 체제 부팅을 지원하지 않습니다.
* 해결 방법은 OpenCore에서 rEFInd를 한 번 체인 로드하는 것입니다.
* UEFI 기반 VBIOS 패치를 지원하지 않습니다. * macOS에서는 가능하지만
* 레거시 GPU에 대한 자동 DeviceProperty 주입을 지원하지 않습니다. * 예: InjectIntel, InjectNVIDIA 및 InjectAti
* 그러나 수동으로는 가능합니다. [GPU 패치](https://dortania.github.io/OpenCore-Post-Install/gpu-patching/)
* IRQ 충돌 패치를 지원하지 않습니다. * [SSDTTime](https://github.com/corpnewt/SSDTTime)으로 해결할 수 있습니다.
* 이전 CPU에 대한 P 및 C 상태 생성을 지원하지 않습니다. * 하드웨어 UUID 주입을 지원하지 않습니다. * Clover의 많은 XCPM 패치를 지원하지 않습니다. * 예: Ivy Bridge XCPM 패치
* 특정 드라이브 숨기기를 지원하지 않음
* OpenCore 메뉴 내에서 설정을 변경하는 것을 지원하지 않음
* PCIRoot UID 값을 패치하지 않음
* macOS 전용 ACPI 패치를 지원하지 않음

## 일반적인 오해

### OpenCore는 베타 버전이므로 불안정합니까?

짧은 답변: 아니요

긴 답변: 아니요

OpenCore의 버전 번호는 프로젝트의 품질을 나타내는 것이 아닙니다. 대신 프로젝트의 발판을 보여주는 방법입니다. Acidanthera는 여전히 전반적인 개선 및 더 많은 기능 지원을 포함하여 프로젝트와 관련하여 하고 싶은 일이 많습니다.

예를 들어, OpenCore는 UEFI 보안 부팅을 준수하는지 확인하기 위해 적절한 보안 감사를 거치며 이러한 엄격한 검토를 거쳐 이러한 지원을 받는 유일한 Hackintosh 부트로더입니다.

버전 0.6.1은 원래 OpenCore의 공식 릴리스로 설계되었는데, 적절한 UEFI/Apple 보안 부팅이 있었고, OpenCore가 공개 도구로 출시된 지 1주년이 되었습니다. 그러나 macOS Big Sur와 이를 지원하기 위한 OpenCore의 사전 링커 재작성 주변의 상황으로 인해 1.0.0을 1년 더 미루기로 결정했습니다.

현재 로드맵:

* 2019: 베타의 해
* 2020: 보안 부팅의 해
* 2021: 개선의 해

따라서 버전 번호를 방해물로 보지 말고, 기대해야 할 것으로 생각하세요.

### OpenCore는 항상 SMBIOS 및 ACPI 데이터를 다른 OS에 주입합니까?

기본적으로 OpenCore는 모든 OS가 ACPI 및 SMBIOS 정보와 관련하여 동등하게 처리되어야 한다고 가정합니다. 이러한 생각의 이유는 세 가지 부분으로 구성됩니다.

* 이렇게 하면 [BootCamp](https://dortania.github.io/OpenCore-Post-Install/multiboot/bootcamp.html)와 같이 적절한 멀티부팅 지원이 가능합니다.
* 제대로 만들어지지 않은 DSDT를 피하고 적절한 ACPI 관행을 장려합니다.
* Clover에서 흔히 볼 수 있는 정보가 여러 번 주입되는 에지 케이스를 피합니다.
* 즉, boot.efi를 부팅한 후 SMBIOS 및 ACPI 데이터 주입을 어떻게 처리해야 할까요? 그런 다음 쫓겨납니다. 변경 사항은 이미 메모리에 있으므로 실행 취소를 시도하는 것은 매우 위험할 수 있습니다. 이것이 Clover의 방법이 비난받는 이유입니다.

* 그러나 OpenCore에는 macOS가 SMBIOS 정보를 읽는 곳을 패치하여 SMBIOS 주입을 macOS로 제한할 수 있는 특이한 점이 있습니다. `CustomSMBIOSMode`를 `Custom`으로 설정한 `CustomSMIOSGuid` 특이한 점은 나중에 깨질 수 있으므로 특정 소프트웨어가 다른 OS에서 깨지는 경우에만 이 옵션을 권장합니다. 최상의 안정성을 위해 이러한 특이한 점을 비활성화하세요.

### OpenCore에 새로 설치가 필요합니까?

"Vanilla" 설치인 경우에는 전혀 필요하지 않습니다. 이는 OS가 시스템 볼륨에 타사 kext를 설치하거나 Apple에서 지원하지 않는 기타 수정 사항과 같이 어떤 식으로든 조작했는지 여부를 나타냅니다. 시스템이 Hackintool과 같은 타사 유틸리티나 사용자에 의해 심하게 손상되었을 경우 잠재적인 문제를 피하기 위해 새로 설치하는 것이 좋습니다.

Clover 사용자를 위한 특별 참고 사항: OpenCore로 설치할 때 NVRAM을 재설정하세요. Clover 변수 중 다수가 OpenCore 및 macOS와 충돌할 수 있습니다.

* 참고: Thinkpad 노트북은 OpenCore에서 NVRAM을 재설정한 후 반쯤 벽돌이 되는 것으로 알려져 있으므로 이러한 컴퓨터에서 BIOS를 업데이트하여 NVRAM을 재설정하는 것이 좋습니다.

### OpenCore는 제한된 버전의 macOS만 지원합니까?

OpenCore 0.6.2부터는 OS X 10.4까지 거슬러 올라가는 모든 Intel 버전의 macOS를 부팅할 수 있습니다! 하지만 적절한 지원은 하드웨어에 따라 달라지므로 직접 확인하세요. [하드웨어 제한](macos-limits.md)

::: 세부 정보 macOS 설치 갤러리

Acidanthera는 여러 버전을 테스트했고, 저도 오래된 HP DC 7900(Core2 Quad Q8300)에서 여러 버전의 OS X를 실행했습니다. 다음은 제가 테스트한 것의 작은 갤러리입니다. 테스트됨:

![](./images/installer-guide/legacy-mac-install-md/dumpster/10.4-Tiger.png)

![](./images/installer-guide/legacy-mac-install-md/dumpster/10.5-Leopard.png)

![](./images/installer-guide/legacy-mac-install-md/dumpster/10.6-Snow-Loepard.png)

![](./images/installer-guide/legacy-mac-install-md/dumpster/10.7-Lion.png)

![](./images/installer-guide/legacy-mac-install-md/dumpster/10.8-MountainLion.png)

![](./images/installer-guide/legacy-mac- install-md/dumpster/10.9-Mavericks.png)

![](./images/installer-guide/legacy-mac-install-md/dumpster/10.10-Yosemite.png)

![](./images/installer-guide/legacy-mac-install-md/dumpster/10.12-Sierra.png)

![](./images/installer-g uide/legacy-mac-install-md/dumpster/10.13-HighSierra.png)

![](./images/installer-guide/legacy-mac-install-md/dumpster/10.15-Catalina.png)

![](./images/installer-guide/legacy-mac-install-md/dumpster/11-Big-Sur.png)

:::

### OpenCore는 이전 하드웨어를 지원합니까?

현재로서는 OS 자체만 지원된다면 대부분의 Intel 하드웨어가 지원됩니다! 그러나 어떤 하드웨어가 어떤 버전의 OS X/macOS에서 지원되는지에 대한 자세한 내용은 [하드웨어 제한 사항 페이지](macos-limits.md)를 참조하세요.

현재 Intel의 Yonah 및 최신 시리즈 CPU는 OpenCore에서 제대로 테스트되었습니다.

### OpenCore는 Windows/Linux 부팅을 지원합니까?

OpenCore는 추가 구성 없이 Windows를 자동으로 감지합니다. OpenCore 0.7.3에서는 OpenLinuxBoot가 EFI 드라이버로 OpenCore에 추가되어 Linux 파티션을 자동으로 감지합니다. 이를 위해서는 배포판에서 사용한 형식에 따라 [ext4_x64.efi](https://github.com/acidanthera/OcBinaryData/blob/master/Drivers/ext4_x64.efi) 또는 [btrfs_x64.efi](https://github.com/acidanthera/OcBinaryData/blob/master/Drivers/btrfs_x64.efi)가 필요합니다. 부트로더의 경로나 이름이 불규칙한 모든 OS의 경우 BlessOverride 섹션에 추가하기만 하면 됩니다.

### 해킨토싱의 합법성

해킨토싱은 법적으로 불분명한 영역에 있습니다. 주로 불법은 아니지만 실제로 EULA를 위반하고 있기 때문입니다. 이것이 불법이 아닌 이유:

* 우리는 [Apple의 서버](https://github.com/acidanthera/OpenCorePkg/blob/0.6.9/Utilities/macrecovery/macrecovery.py#L125)에서 macOS를 직접 다운로드하고 있습니다.
* 우리는 비영리 단체로서 교육 및 개인적 사용을 위해 이를 수행하고 있습니다.
* Hackintosh를 업무에 사용하거나 재판매하려는 사람은 [Psystar 사례](https://en.wikipedia.org/wiki/Psystar_Corporation)와 해당 지역 법률을 참조해야 합니다.

EULA는 macOS를 정품 Mac 또는 정품 Mac에서 실행되는 가상 머신에만 설치해야 한다고 명시하지만([섹션 2B-i 및 2B-iii](https://www.apple.com/legal/sla/docs/macOSBigSur.pdf)), 이를 전면적으로 금지하는 시행 가능한 법률은 없습니다. 그러나 macOS 설치 프로그램을 다시 패키징하고 수정하는 사이트는 잠재적으로 [DMCA 삭제](https://en.wikipedia.org/wiki/Digital_Millennium_Copyright_Act)와 같은 문제를 일으킬 위험이 있습니다.

* **참고**: 이는 법적 조언이 아니므로 직접 적절한 평가를 내리고 우려 사항이 있는 경우 변호사와 상의하십시오.

### macOS는 NVIDIA GPU를 지원합니까?\

최신 버전의 macOS에서 NVIDIA 지원과 관련된 문제로 인해 많은 사용자가 macOS가 NVIDIA GPU를 지원하지 않았다는 결론을 내렸습니다. Apple은 Monterey Beta 7이 출시될 때까지 NVIDIA GPU가 있는 Mac(예: Kepler dGPU가 있는 2013년 MacBook Pro)을 지원했습니다. 지원을 다시 제공하기 위한 커뮤니티에서 만든 패치가 있지만 SIP(시스템 무결성 보호)를 비활성화해야 하며 macOS의 중요한 보안 기능이 비활성화됩니다.

다른 문제는 최신 NVIDIA GPU와 관련이 있는데, Apple이 해당 GPU가 있는 컴퓨터를 더 이상 제공하지 않아 Apple에서 공식적인 OS 지원을 받지 못했기 때문입니다. 대신 사용자는 타사 드라이버에 대해 NVIDIA에 의존해야 했습니다. Apple의 새로 도입된 보안 부팅 문제로 인해 더 이상 웹 드라이버를 지원할 수 없었고 NVIDIA는 최신 플랫폼에 이를 게시할 수 없어 Mac OS 10.13, High Sierra로 제한되었습니다.

OS 지원에 대한 자세한 내용은 여기를 참조하세요: [GPU 구매자 가이드](https://dortania.github.io/GPU-Buyers-Guide/)
