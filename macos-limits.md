# 하드웨어 제한

macOS에서는 설치에 들어가기 전에 알아야 할 수많은 하드웨어 제한이 있습니다. 이는 Apple에서 지원하는 하드웨어의 양이 제한되어 있기 때문에 Apple이나 커뮤니티에서 만든 패치에 의해 제한을 받습니다.

확인해야 할 주요 하드웨어 섹션은 다음과 같습니다.

[[toc]]

이 주제에 대한 자세한 가이드는 여기에서 확인하세요.

* [GPU 구매자 가이드](https://dortania.github.io/GPU-Buyers-Guide/)
* GPU가 지원되는지와 실행할 수 있는 macOS 버전을 확인하세요.
* [무선 구매자 가이드](https://dortania.github.io/Wireless-Buyers-Guide/)
* WiFi 카드가 지원되는지 확인하세요.
* [안티 하드웨어 구매자 가이드](https://dortania.github.io/Anti-Hackintosh-Buyers-Guide/)
* 피해야 할 사항과 하드웨어가 빠질 수 있는 함정에 대한 전반적인 가이드입니다.

## CPU 지원

CPU 지원에 대한 세부 사항은 다음과 같습니다.

* 32비트 및 64비트 CPU가 모두 지원됩니다.
* 그러나 이를 위해서는 OS가 아키텍처를 지원해야 합니다. 아래 CPU 요구 사항 섹션을 참조하세요.
* 인텔 데스크톱 CPU가 지원됩니다.
* 이 가이드에서는 Yonah부터 Comet Lake까지 지원합니다.
* 인텔 하이엔드 데스크톱 및 서버 CPU.
* 이 가이드에서는 Nehalem부터 Cascade Lake X까지 지원합니다.
* 인텔의 Core "i" 및 Xeon 시리즈 노트북 CPU
* 이 가이드에서는 Arrandale부터 Ice Lake까지 지원합니다.
* Mobile Atoms, Celeron 및 Pentium CPU는 지원되지 않습니다.
* AMD의 Desktop Bulldozer(15h), Jaguar(16h) 및 Ryzen(17h) CPU
* 노트북 CPU는 **지원되지 않습니다.**
* macOS의 모든 기능이 AMD에서 지원되는 것은 아닙니다. 아래를 참조하세요.

**자세한 내용은 여기를 참조하세요: [Anti-Hardware Buyers Guide](https://dortania.github.io/Anti-Hackintosh-Buyers-Guide/)**

::: 세부 정보 CPU 요구 사항

아키텍처 요구 사항

* 32비트 CPU는 10.4.1에서 10.6.8까지 지원됩니다.
* 10.7.x는 64비트 사용자 공간이 필요하므로 32비트 CPU는 10.6으로 제한됩니다.
* 64비트 CPU는 10.4.1에서 현재

SSE 요구 사항:

* SSE3는 모든 Intel 버전의 OS X/macOS에 필수
* SSSE3는 모든 64비트 버전의 OS X/macOS에 필수
* SSSE3가 없는 CPU(예: 특정 64비트 펜티엄)의 경우 32비트 사용자 공간(`i386-user32`)을 실행하는 것이 좋습니다.
* SSE4는 macOS 10.12 이상에 필수
* SSE4.2는 macOS 10.14 이상에 필수
* SSE4.1 CPU는 [telemetrap.kext](https://forums.macrumors.com/threads/mp3-1-others-sse-4-2-emulation-to-enable-amd-metal-driver.2206682/post-28447707)에서 지원
* 최신 AMD 드라이버도 Metal 지원을 위해 SSE4.2가 필요합니다. 이를 해결하려면 여기를 참조하세요. [MouSSE: SSE4.2 에뮬레이션](https://forums.macrumors.com/threads/mp3-1-others-sse-4-2-emulation-to-enable-amd-metal-driver.2206682/)

펌웨어 요구 사항:

* OS X 10.4.1~10.4.7은 EFI32(즉, OpenCore의 IA32(32비트) 버전)가 필요합니다.
* OS X 10.4.8~10.7.5는 EFI32와 EFI64를 모두 지원합니다.
* OS X 10.8 이상은 EFI64(즉, OpenCore의 x64(64비트) 버전)가 필요합니다.
* OS X 10.7~10.9는 복구 파티션을 부팅하기 위해 OpenPartitionDxe.efi가 필요합니다.

커널 요구 사항:

* OS X 10.4 및 10.5는 32비트 커널 공간만 지원하기 때문에 32비트 kext가 필요합니다.
* OS X 10.6 및 10.7은 32비트와 64비트 커널 공간을 모두 지원합니다.
* OS X 10.8 이상은 64비트 커널 공간만 지원하기 때문에 64비트 kext가 필요합니다.
* `lipo -archs`를 실행하여 kext가 지원하는 아키텍처를 확인합니다(바이너리 자체에서 실행해야 하며 .kext 번들은 실행하지 않아야 합니다)

코어/스레드 수 제한:

* OS X 10.10 이하에서는 24개 이상의 스레드로 부팅되지 않을 수 있습니다(`mp_cpus_call_wait() timeout` 패닉으로 확인)
* OS X 10.11 이상은 64개 스레드 제한이 있습니다.
* `cpus=` 부팅 인수를 해결 방법으로 사용하거나 하이퍼스레딩을 비활성화할 수 있습니다.

특별 참고 사항:

* Lilu 및 플러그인은 작동하려면 10.8 이상이 필요합니다.
* 이전 버전의 OS X에서는 FakeSMC를 실행하는 것이 좋습니다.
* OS X 10.6 이하에서는 RebuildAppleMemoryMap을 활성화해야 합니다.
* 이는 초기 커널을 해결하기 위한 것입니다.

:::

::: 세부 정보 Intel CPU 지원 차트

Vanilla 커널 기반 지원(즉, 수정 없음):

| CPU 세대 | 초기 지원 | 마지막 지원 버전 | 참고 | CPUID |
| :--- | :--- | :--- | :--- | :--- |
| [Pentium 4](https://en.wikipedia.org/wiki/Pentium_4) | 10.4.1 | 10.5.8 | 개발 키트에서만 사용 | 0x0F41 |
| [Yonah](https://en.wikipedia.org/wiki/Yonah_(microprocessor)) | 10.4.4 | 10.6.8 | 32비트 | 0x0006E6 |
| [콘로](https://en.wikipedia.org/wiki/Conroe_(마이크로프로세서)), [메롬](https://en.wikipedia.org/wiki/Merom_(마이크로프로세서)) | 10.4.7 | 10.11.6 | SSE4 없음 | 0x0006F2 |
| [펜린](https://en.wikipedia.org/wiki/Penryn_(마이크로아키텍처)) | 10.4.10 | 10.13.6 | SSE4.2 없음 | 0x010676 |
| [네할렘](https://en.wikipedia.org/wiki/Nehalem_(마이크로아키텍처)) | 10.5.6 | <span style="color:green"> 현재 </span> | 해당 없음 | 0x0106A2 |
| [린필드](https://en.wikipedia.org/wiki/Lynnfield_(마이크로프로세서)), [클락스필드](https://en.wikipedia.org/wiki/Clarksfield_(마이크로프로세서)) | 10.6.3 | ^^ | iGPU 지원 없음 10.14+ | 0x0106E0 |
| [웨스트미어, 클라크데일, 애런데일](https://en.wikipedia.org/wiki/Westmere_(microarchitecture)) | 10.6.4 | ^^ | ^^ | 0x0206C0 |
| [샌디 브릿지](https://en.wikipedia.org/wiki/샌디_브릿지) | 10.6.7 | ^^ | ^^ | 0x0206A0(M/H) |
| [아이비 브릿지](https://en.wikipedia.org/wiki/아이비_브릿지_(microarchitecture)) | 10.7.3 | ^^ | iGPU 지원 안 함 12+ | 0x0306A0(M/H/G) |
| [아이비 브릿지-E5](https://en.wikipedia.org/wiki/아이비_브릿지_(microarchitecture)) | 10.9.2 | ^^ | 없음 | 0x0306E0 |
| [Haswell](https://en.wikipedia.org/wiki/Haswell_(마이크로아키텍처)) | 10.8.5 | ^^ | ^^ | 0x0306C0(S) |
| [브로드웰](https://en.wikipedia.org/wiki/Broadwell_(마이크로아키텍처)) | 10.10.0 | ^^ | ^^ | 0x0306D4(U/Y) |
| [스카이레이크](https://en.wikipedia.org/wiki/Skylake_(마이크로아키텍처)) | 10.11.0 | ^^ | ^^ | 0x0506e3(H/S) 0x0406E3(U/Y) |
| [카비레이크](https://en.wikipedia.org/wiki/Kaby_Lake) | 10.12.4 | ^^ | ^^ | 0x0906E9(H/S/G) 0x0806E9(U/Y) |
| [커피레이크](https://en.wikipedia.org/wiki/Coffee_Lake) | 10.12.6 | ^^ | ^^ | 0x0906EA(S/H/E) 0x0806EA(U)|
| [앰버](https://en.wikipedia.org/wiki/Kaby_Lake#List_of_8th_ Generation_Amber_Lake_Y_processors), [위스키](https://en.wikipedia.org/wiki/Whiskey_Lake_(마이크로아키텍처)), [코멧 레이크](https://en.wikipedia.org/wiki/Comet_Lake_(마이크로프로세서)) | 10.14.1 | ^^ | ^^ | 0x0806E0(U/Y) |
| [Comet Lake](https://en.wikipedia.org/wiki/Comet_Lake_(microprocessor)) | 10.15.4 | ^^ | ^^ | 0x0906E0(S/H)|
| [Ice Lake](https://en.wikipedia.org/wiki/Ice_Lake_(microprocessor)) | ^^ | ^^ | ^^ | 0x0706E5(U) |
| [Rocket Lake](https://en.wikipedia.org/wiki/Rocket_Lake) | ^^ | ^^ | Comet Lake CPUID 필요 | 0x0A0671 |
| [Tiger Lake](https://en.wikipedia.org/wiki/Tiger_Lake_(microprocessor)) | <span style="color:red"> 없음 </span> | <span style="color:red"> 없음 </span> | <span style="color:red"> 테스트 안 함 </span> | 0x0806C0(U) |

:::

::: macOS의 AMD CPU 제한 사항

불행히도 macOS의 많은 기능은 AMD와 다른 많은 기능이 부분적으로 손상되어 전혀 지원되지 않습니다. 여기에는 다음이 포함됩니다.

* AppleHV에 의존하는 가상 머신
* 여기에는 VirtualBox, VMWare, Parallels, Docker, Android Studio 등이 포함됩니다.
* VirtualBox 6, VMware 10 및 Parallels 13.1.0은 자체 하이퍼바이저를 지원하지만 이러한 오래된 VM 소프트웨어를 사용하면 큰 보안 위협이 발생합니다.
* Adobe 지원
* Adobe 제품군의 대부분은 Intel의 Memfast 명령어 집합에 의존하여 AMD CPU에서 충돌이 발생합니다.
* 충돌을 방지하기 위해 RAW 지원과 같은 기능을 비활성화할 수 있습니다. [Adobe 수정](https://gist.github.com/naveenkrdy/26760ac5135deed6d0bb8902f6ceb6bd)
* 32비트 지원
* Mojave 이하에서 여전히 32비트 소프트웨어에 의존하는 경우 Vanilla 패치는 32비트 명령어를 지원하지 않는다는 점에 유의하세요.
* 해결 방법은 [사용자 정의 커널](https://files.amd-osx.com/?dir=Kernels), 하지만 iMessage 지원이 손실되고 이러한 커널에 대한 지원이 제공되지 않습니다.
* 많은 앱의 안정성 문제
* 오디오 기반 앱은 문제가 가장 발생하기 쉽습니다. 즉, Logic Pro
* DaVinci Resolve도 산발적인 문제가 있는 것으로 알려져 있습니다.

:::

## GPU 지원

GPU 지원은 시중에 거의 무한한 양의 GPU가 있기 때문에 훨씬 더 복잡해졌지만, 일반적인 세부 사항은 다음과 같습니다.

* AMD의 GCN 기반 GPU는 최신 버전의 macOS에서 지원됩니다.
* AMD APU는 지원되지 않습니다.
* Polaris 시리즈의 AMD [Lexa 기반 코어](https://www.techpowerup.com/gpu-specs/amd-lexa.g806)도 지원되지 않습니다.
* MSI Navi 사용자를 위한 특별 참고 사항: [설치 프로그램이 5700XT에서 작동하지 않음 #901](https://github.com/acidanthera/bugtracker/issues/901)
* 이 문제는 더 이상 macOS 11(Big Sur)에서 발생하지 않습니다.
* NVIDIA의 GPU 지원은 복잡합니다.
 * [Maxwell(9XX)](https://en.wikipedia.org/wiki/GeForce_900_series) 및 [Pascal(10XX)](https://en.wikipedia.org/wiki/GeForce_10_series) GPU는 macOS 10.13: High Sierra로 제한됩니다.
 * [NVIDIA의 Turing(20XX,](https://en.wikipedia.org/wiki/GeForce_20_series)[16XX)](https://en.wikipedia.org/wiki/GeForce_16_series) GPU는 **모든 macOS 버전에서 지원되지 않습니다**
 * [NVIDIA Ampere(30XX)](https://en.wikipedia.org/wiki/GeForce_30_series) GPU는 **모든 macOS 버전에서 지원되지 않습니다**
 * [엔비디아의 Kepler(6XX,](https://en.wikipedia.org/wiki/GeForce_600_series)[7XX)](https://en.wikipedia.org/wiki/GeForce_700_series) GPU는 macOS 11까지 지원됩니다: Big Sur
* Intel의 [GT2+ 티어](https://en.wikipedia.org/wiki/Intel_Graphics_Technology) 시리즈 iGPU
* Ivy Bridge부터 Ice Lake iGPU 지원은 이 가이드에서 다룹니다
* GMA 시리즈 iGPU에 대한 정보는 여기에서 찾을 수 있습니다: [GMA 패치](https://dortania.github.io/OpenCore-Post-Install/gpu-patching/)
* GT2는 iGPU 티어를 말하며, Pentium, Celeron 및 Atom에서 찾을 수 있는 로우엔드 GT1 iGPU는 macOS에서 지원되지 않습니다

그리고 **이산형 노트북에 대한 중요 참고 사항 GPU**:

* 개별 GPU의 90%는 macOS에서 지원하지 않는 구성(전환 가능한 그래픽)으로 연결되어 있기 때문에 작동하지 않습니다. NVIDIA 개별 GPU의 경우 일반적으로 Optimus라고 합니다.
* 이러한 개별 GPU는 내부 디스플레이를 위한 것이므로 일반적으로 이를 비활성화하고 전원을 끄는 것이 좋습니다(이 가이드의 후반부에서 다룹니다).
* 그러나 일부 경우 개별 GPU는 작동할 수도 있고 작동하지 않을 수도 있는 외부 출력(HDMI, 미니 디스플레이포트 등)에 전원을 공급합니다. 작동하는 경우 카드를 켜고 실행해야 합니다.
* 그러나 전환 가능한 그래픽이 없는 노트북이 드물게 있으므로 개별 카드를 사용할 수 있지만(macOS에서 지원하는 경우) 배선 및 설정으로 인해 일반적으로 문제가 발생합니다.

**지원되는 GPU의 전체 목록은 [GPU 구매자 가이드](https://dortania.github.io/GPU-Buyers-Guide/)를 참조하세요.**

::: 세부 정보 Intel GPU 지원 차트

| GPU 세대 | 초기 지원 | 마지막 지원 버전 | 참고 사항 |
| :--- | :--- | :--- | :--- |
| [3세대 GMA](https://en.wikipedia.org/wiki/List_of_Intel_graphics_processing_units#Third_generation) | 10.4.1 | 10.7.5 | [32비트 커널 및 패치 필요](https://dortania.github.io/OpenCore-Post-Install/gpu-patching/legacy-intel/) |
| [4세대 GMA](https://en.wikipedia.org/wiki/List_of_Intel_graphics_processing_units#Gen4) | 10.5.0 | ^^ | ^^ |
| [Arrandale(HD Graphics)](https://en.wikipedia.org/wiki/List_of_Intel_graphics_processing_units#Gen5) | 10.6.4 | 10.13.6 | LVDS만 지원되고 eDP 및 외부 출력은 지원되지 않음 |
| [샌디 브릿지(HD 3000)](https://en.wikipedia.org/wiki/인텔_그래픽_처리_단위_목록#Gen6) | 10.6.7 | ^^ | 없음 |
| [아이비 브릿지(HD 4000)](https://en.wikipedia.org/wiki/인텔_그래픽_처리_단위_목록#Gen7) | 10.7.3 | 11.7.x | ^^ |
| [하스웰(HD 4XXX, 5XXX)](https://en.wikipedia.org/wiki/인텔_그래픽_처리_단위_목록#Gen7) | 10.8.5 | 12.6.x | ^^ |
| [브로드웰(5XXX, 6XXX)](https://en.wikipedia.org/wiki/인텔_그래픽_처리_단위_목록#Gen8) | 10.10.0 | ^^ | ^^ |
| [스카이레이크(HD 5XX)](https://en.wikipedia.org/wiki/List_of_Intel_graphics_processing_units#Gen9) | 10.11.0 | ^^ | ^^ |
| [캐비레이크(HD 6XX)](https://en.wikipedia.org/wiki/List_of_Intel_graphics_processing_units#Gen9) | 10.12.4 | <span style="color:green"> 현재 </span> | ^^ |
| [커피레이크(UHD 6XX)](https://en.wikipedia.org/wiki/List_of_Intel_graphics_processing_units#Gen9) | 10.13.6 | ^^ | ^^ |
| [코멧레이크(UHD 6XX)](https://en.wikipedia.org/wiki/List_of_Intel_graphics_processing_units#Gen9) | 10.15.4 | ^^ | ^^ |
| [아이스 레이크(Gx)](https://en.wikipedia.org/wiki/List_of_Intel_graphics_processing_units#Gen11) | 10.15.4 | ^^ | boot-args에 `-igfxcdc` 및 `-igfxdvmt`가 필요합니다 |
| [타이거 레이크(Xe)](https://en.wikipedia.org/wiki/Intel_Xe) | <span style="color:red"> 해당 없음 </span> | <span style="color:red"> 해당 없음 </span> | <span style="color:red"> 사용 가능한 드라이버가 없습니다 </span> |
| [Rocket Lake](https://en.wikipedia.org/wiki/Rocket_Lake) | <span style="color:red"> 없음 </span> | <span style="color:red"> 없음 </span> | <span style="color:red"> 드라이버가 없습니다 </span> |

:::

::: 세부 정보 AMD GPU 지원 차트

| GPU 세대 | 초기 지원 | 마지막 지원 버전 | 참고 |
| :--- | :--- | :--- | :--- |
| [X800](https://en.wikipedia.org/wiki/Radeon_X800_series) | 10.3.x | 10.7.5 | 32비트 커널 필요 |
| [X1000](https://en.wikipedia.org/wiki/Radeon_X1000_series) | 10.4.x | ^^ | 없음 |
| [TeraScale](https://en.wikipedia.org/wiki/TeraScale_(마이크로아키텍처)) | 10.4.x | 10.13.6 | ^^ |
| [TeraScale 2/3](https://en.wikipedia.org/wiki/TeraScale_(마이크로아키텍처)) | 10.6.x | ^^ | ^^ |
| [GCN 1](https://en.wikipedia.org/wiki/Graphics_Core_Next) | 10.8.3 | 12.6.x | ^^ |
| [GCN 2/3](https://en.wikipedia.org/wiki/Graphics_Core_Next) | 10.10.x | ^^ | ^^ |
| [Polaris 10](https://en.wikipedia.org/wiki/Radeon_RX_400_series), [20](https://en.wikipedia.org/wiki/Radeon_RX_500_series) | 10.12.1 | <span style="color:green"> 현재 </span> | ^^ |
| [Vega 10](https://en.wikipedia.org/wiki/Radeon_RX_Vega_series) | 10.12.6 | ^^ | ^^ |
| [Vega 20](https://en.wikipedia.org/wiki/Radeon_RX_Vega_series) | 10.14.5 | ^^ | ^^ |
| [Navi 10](https://en.wikipedia.org/wiki/Radeon_RX_5000_series) | 10.15.1 | ^^ | 부팅 인수에 `agdpmod=pikera`가 필요합니다 |
| [Navi 20](https://en.wikipedia.org/wiki/Radeon_RX_6000_series) | 11.4 | ^^ | <span style="color:yellow"> 현재 일부 Navi 21 모델만 작동 중입니다 </span> |

:::

::: 세부 정보 NVIDIA GPU 지원 차트

| GPU 세대 | 초기 지원 | 마지막 지원 버전 | 참고 |
| :--- | :--- | :--- | :--- |
| [GeForce 6](https://en.wikipedia.org/wiki/GeForce_6_series) | 10.2.x | 10.7.5 | 32비트 커널과 [NVCAP 패치](https://dortania.github.io/OpenCore-Post-Install/gpu-patching/nvidia-patching/) 필요 |
| [GeForce 7](https://en.wikipedia.org/wiki/GeForce_7_series) | 10.4.x | ^^ | [NVCAP 패치 필요](https://dortania.github.io/OpenCore-Post-Install/gpu-patching/nvidia-patching/) |
| [테슬라](https://en.wikipedia.org/wiki/Tesla_(microarchitecture)) | 10.4.x | 10.13.6 | ^^ |
| [테슬라 v2](https://en.wikipedia.org/wiki/Tesla_(microarchitecture)#Tesla_2.0) | 10.5.x | ^^ | ^^ |
| [페르미](https://en.wikipedia.org/wiki/Fermi_(microarchitecture)) | 10.7.x | ^^ | ^^ |
| [케플러](https://en.wikipedia.org/wiki/Kepler_(microarchitecture)) | 10.7.x | 11.7.x | 없음 |
| [케플러 v2](https://en.wikipedia.org/wiki/Kepler_(microarchitecture)) | 10.8.x | ^^ | ^^ |
| [맥스웰](https://en.wikipedia.org/wiki/맥스웰_(microarchitecture)) | 10.10.x | 10.13.6 | [NVIDIA 웹 드라이버 필요](https://www.nvidia.com/download/driverResults.aspx/149652/) |
| [Pascal](https://en.wikipedia.org/wiki/Pascal_(microarchitecture)) | 10.12.4 | ^^ | ^^ |
| [Turing](https://en.wikipedia.org/wiki/Turing_(microarchitecture)) | <span style="color:red"> 없음 </span> | <span style="color:red"> 없음 </span> | <span style="color:red"> 드라이버 없음 </span> |
| [Ampere](https://en.wikipedia.org/wiki/Ampere_(microarchitecture)) | ^^ | ^^ | ^^ |

:::

## 마더보드 지원

대부분의 경우 CPU만 지원된다면 모든 마더보드가 지원됩니다.

::: 세부 정보 MSI 500 시리즈 AMD 마더보드 참고

~~MSI 500 시리즈 AMD 마더보드(A520, B550, X570)는 예외입니다. 이 마더보드는 macOS Monterey 이상에서 문제가 있습니다.~~

* ~~PCIe 장치가 항상 제대로 열거되지 않습니다~~
* ~~Zen 3 지원을 위한 BIOS 업데이트가 부팅을 중단합니다~~

~~macOS Big Sur 또는 이전 버전이 이러한 마더보드에 권장됩니다~~

CaseySJ 덕분에 최신 버전의 AMD vanilla 패치에서 이 문제가 해결되었습니다!

:::

## 스토리지 지원

대부분의 경우 모든 SATA 기반 드라이브가 지원되고 대부분의 NVMe 드라이브도 지원됩니다. 몇 가지 예외가 있습니다.

* **Samsung PM981, PM991 및 Micron 2200S NVMe SSD**
* 이러한 SSD는 기본적으로 호환되지 않으므로(커널 패닉 발생) 이러한 커널 패닉을 해결하려면 [NVMeFix.kext](https://github.com/acidanthera/NVMeFix/releases)가 필요합니다. 이러한 드라이브는 NVMeFix.kext를 사용하더라도 부팅 문제를 일으킬 수 있습니다.
* 관련 사항으로, Samsung 970 EVO Plus NVMe SSD도 동일한 문제가 있었지만 펌웨어 업데이트에서 해결되었습니다. 업데이트(Samsung Magician을 통한 Windows 또는 부팅 가능한 ISO)를 [여기](https://www.samsung.com/semiconductor/minisite/ssd/download/tools/)에서 받으세요.
* 또한, HDD 가속을 위해 [Intel Optane Memory](https://www.intel.com/content/www/us/en/architecture-and-technology/optane-memory.html) 또는 [Micron 3D XPoint](https://www.micron.com/products/advanced-solutions/3d-xpoint-technology)를 사용하는 노트북은 macOS에서 지원되지 않습니다. 일부 사용자는 Catalina에서 읽기 및 쓰기 지원까지 성공했다고 보고했지만 잠재적인 부팅 문제를 방지하기 위해 드라이브를 제거하는 것이 좋습니다.
* Intel Optane Memory H10/H20 모델은 macOS에서 Optane 부분이 비활성화된 경우 호환됩니다. 자세한 내용은 [여기](https://blog.csdn.net/weixin_46341175/article/details/126626808)에서 확인할 수 있습니다([원문 중국어 출처](https://zhuanlan.zhihu.com/p/429073173)).

* **Intel 600p**
* 부팅할 수 없는 것은 아니지만 이 모델은 수많은 문제를 일으킬 수 있습니다. [Intel 600p NVMe 드라이브에 대한 수정 사항이 있나요? #1286](https://github.com/acidanthera/bugtracker/issues/1286)
* 660p 모델은 괜찮습니다.

## 유선 네트워킹

거의 모든 유선 네트워크 어댑터는 macOS에서 기본 제공 드라이버나 커뮤니티에서 만든 kext를 통해 어떤 형태로든 지원됩니다. 주요 예외:

* Intel I225-V 2.5Gb NIC
* 하이엔드 데스크톱 Comet Lake 보드에서 발견
* 장치 속성 필요: [출처](https://www.hackintosh-forum.de/forum/thread/48568-i9-10900k-gigabyte-z490-vision-d-er-läuft/?postID=606059#post606059) 및 [예](config.plist/comet-lake.md#deviceproperties)
* Intel I350 1Gb 서버 NIC
* 일반적으로 다양한 세대의 Intel 및 Supermicro 서버 보드에서 발견
* [장치 속성 필요](config-HEDT/ivy-bridge-e.md#deviceproperties)
* Intel 10Gb 서버 NIC
* [X520 및 X540]에 대한 해결 방법이 가능합니다. 칩셋](https://www.tonymacx86.com/threads/how-to-build-your-own-imac-pro-successful-build-extended-guide.229353/)
* Mellanox 및 Qlogic 서버 NIC

## 무선 네트워킹

노트북과 함께 제공되는 대부분의 WiFi 카드는 일반적으로 Intel/Qualcomm이므로 지원되지 않습니다. 운이 좋다면 지원되는 Atheros 카드가 있을 수 있지만 High Sierra까지만 지원됩니다.

가장 좋은 옵션은 지원되는 Broadcom 카드를 구입하는 것입니다. 권장 사항은 [WiFi 구매자 가이드](https://dortania.github.io/Wireless-Buyers-Guide/)를 참조하세요.

참고: Intel WiFi는 macOS에서 비공식적으로(타사 드라이버) 지원됩니다. 드라이버 및 지원되는 카드에 대한 자세한 내용은 [WiFi 구매자 가이드](https://dortania.github.io/Wireless-Buyers-Guide/)를 확인하세요.

## 기타

* **지문 센서**
* 현재 Touch ID 센서를 에뮬레이션할 방법이 없으므로 지문 센서는 작동하지 않습니다.
* **Windows Hello 얼굴 인식**
* 일부 노트북에는 I2C로 연결된(그리고 iGPU를 통해 사용되는) WHFR이 함께 제공되며 작동하지 않습니다.
* 일부 노트북에는 USB로 연결된 WHFR이 함께 제공되며 운이 좋으면 카메라 기능은 사용할 수 있지만 그 외의 기능은 사용할 수 없습니다.
* **Intel Smart Sound Technology**
* Intel SST가 있는 노트북은 아무것도 연결되어 있지 않으면 작동하지 않습니다(일반적으로 내장 마이크). 지원되지 않기 때문입니다. Windows의 장치 관리자에서 확인할 수 있습니다.
* **헤드폰 잭 콤보**
* 콤보 헤드폰 잭이 있는 일부 노트북은 이를 통해 오디오 입력을 받지 못할 수 있으며 내장 마이크나 USB를 통한 외부 오디오 입력 장치를 사용해야 합니다.
* **Thunderbolt USB-C 포트**
* (Hackintosh) Thunderbolt 지원은 현재 macOS에서 여전히 불확실하며, 대부분의 최신 노트북에 있는 Alpine Ridge 컨트롤러의 경우 더욱 그렇습니다. 컨트롤러의 전원을 켜두어 Thunderbolt와 USB-C 핫플러그가 작동하도록 하려는 시도가 있었지만, 커널 패닉 및/또는 절전 모드 이후 USB-C가 중단되는 대가를 치렀습니다. 포트의 USB-C 쪽을 사용하여 절전 모드로 전환하려면 부팅 시 플러그를 꽂고 플러그를 꽂아 두어야 합니다.
* 참고: 이는 USB-C 전용 포트에는 적용되지 않으며 Thunderbolt 3와 USB-C가 결합된 포트에만 적용됩니다.
* BIOS에서 Thunderbolt를 비활성화하면 이 문제가 해결됩니다.
