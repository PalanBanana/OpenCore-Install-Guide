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
| [웨스트미어, 클라크데일, 애런데일](https://en.w
