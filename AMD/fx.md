# Bulldozer(15h) 및 Jaguar(16h)

| 지원 | 버전 |
| :--- | :--- |
| 초기 macOS 지원 | macOS 10.13, High Sierra |
| 마지막 지원 OS | macOS 12 Monterey |
| 참고 | Ventura 정보는 [macOS 13 Ventura](../extras/ventura.md#dropped-cpu-support)를 참조하세요. |

## 시작점

config.plist를 만드는 것이 어려워 보일 수 있지만 그렇지 않습니다. 시간이 좀 걸릴 뿐이지만 이 가이드에서는 모든 것을 구성하는 방법을 알려드리므로 추위에 떨지 않을 ​​것입니다. 즉, 문제가 있는 경우 구성 설정을 검토하여 올바른지 확인하세요. OpenCore에서 주의해야 할 주요 사항:

* **모든 속성을 정의해야 합니다.** 기본적으로 OpenCore에서 사용할 수 있는 것이 없으므로 **명시적으로 그렇게 하지 않는 한 섹션을 삭제하지 마세요**. 가이드에 옵션이 언급되지 않으면 기본값으로 두세요.
* **Sample.plist는 있는 그대로 사용할 수 없습니다.** 시스템에 맞게 구성해야 합니다.
* **구성기를 사용하지 마세요.** 이들은 OpenCore의 구성을 거의 따르지 않으며 Mackie와 같은 일부 구성기조차도 Clover 속성을 추가하고 plist를 손상시킵니다!

이제 이 모든 것을 통해 필요한 도구에 대한 간단한 상기

* [ProperTree](https://github.com/corpnewt/ProperTree)
* 범용 plist 편집기
* [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)
* SMBIOS 데이터 생성
* [Sample/config.plist](https://github.com/acidanthera/OpenCorePkg/releases)
* 이전 섹션에서 다음을 얻는 방법을 참조하세요. [config.plist 설정](../config.plist/README.md)
* [AMD 커널 패치](https://github.com/AMD-OSX/AMD_Vanilla)
* AMD 하드웨어에서 macOS를 부팅하는 데 필요(나중에 저장하고 아래에서 사용 방법을 살펴보겠습니다)
* AMD Family 15h, 16h, 17h 및 19h 지원

::: 경고

읽기 OpenCore를 설정하기 전에 이 가이드를 두 번 이상 읽고 올바르게 설정했는지 확인하세요. 이미지가 항상 최신이 아니므로 이미지 아래의 텍스트를 읽어보세요. 아무것도 언급되지 않았다면 기본값으로 두세요.

:::

## ACPI

![ACPI](../images/config/AMD/acpi-fx.png)

### 추가

::: 팁 정보

여기서 시스템에 SSDT를 추가합니다. 이는 **macOS 부팅**에 매우 중요하며 [USB 맵](https://dortania.github.io/OpenCore-Post-Install/usb/), [지원되지 않는 GPU 비활성화](../extras/spoof.md) 등과 같은 여러 용도가 있습니다. 그리고 저희 시스템에서는 **부팅에도 필요합니다**. 여기에서 찾을 수 있는 가이드: [**ACPI 시작하기**](https://dortania.github.io/Getting-Started-With-ACPI/)

| 필요한 SSDT | 설명 |
| :--- | :--- |
| **[SSDT-EC-USBX](https://dortania.github.io/Getting-Started-With-ACPI/)** | 임베디드 컨트롤러와 USB 전원을 모두 수정합니다. 자세한 내용은 [ACPI 시작하기 가이드](https://dortania.github.io/Getting-Started-With-ACPI/)를 참조하세요. |

생성된 `DSDT.aml`을 여기에 **추가해서는 안 됩니다**. 펌웨어에 이미 들어 있습니다. 따라서 있는 경우 `config.plist`와 EFI/OC/ACPI에서 해당 항목을 제거합니다.

DSDT 덤프, SSDT 만들기, 컴파일에 대해 더 자세히 알아보고 싶은 분은 [**ACPI 시작하기**](https://dortania.github.io/Getting-Started-With-ACPI/) **페이지를 참조하세요.** 컴파일된 SSDT는 **.aml** 확장자(Assembled)를 가지며 `EFI/OC/ACPI` 폴더로 이동하고 `ACPI -> Add`에서 **반드시** 설정에 지정해야 합니다.

:::

### 삭제

이렇게 하면 특정 ACPI 테이블이 로드되지 않습니다. 우리는 이를 무시할 수 있습니다.

### 패치

이 섹션을 사용하면 OpenCore를 통해 ACPI의 일부(DSDT, SSDT 등)를 동적으로 수정할 수 있습니다. 우리에게 패치는 SSDT에서 처리합니다. 이것은 훨씬 더 깔끔한 솔루션으로, 이를 통해 OpenCore로 Windows 및 기타 OS를 부팅할 수 있습니다.

### 엉뚱한 점

ACPI와 관련된 설정은 이러한 엉뚱한 점에 대한 용도가 없으므로 여기의 모든 것을 기본값으로 둡니다.

## Booter

![Booter](../images/config/config-universal/aptio-iv-booter.png)

이 섹션은 AptioMemoryFix.efi를 대체하는 OpenRuntime을 사용한 boot.efi 패치와 관련된 엉뚱한 점에 대해 설명합니다.

### MmioWhitelist

이 섹션은 일반적으로 무시되는 macOS로의 패스스루를 허용하며, `DevirtualiseMmio`와 함께 사용할 때 유용합니다.

### 엉뚱한 점

::: 팁 정보
boot.efi 패치 및 펌웨어 수정과 관련된 설정입니다. 저희는 기본값으로 둡니다.
:::
::: 세부 정보 더 자세한 정보

* **AvoidRuntimeDefrag**: 예
* 날짜, 시간, NVRAM, 전원 제어 등과 같은 UEFI 런타임 서비스를 수정합니다.
* **EnableSafeModeSlide**: 예
* 안전 모드에서 슬라이드 변수를 사용할 수 있도록 합니다.
* **EnableWriteUnprotector**: 예
* CR0 레지스터에서 쓰기 보호를 제거하는 데 필요합니다.
* **ProvideCustomSlide**: 예
* 슬라이드 변수 계산에 사용됩니다. 그러나 이 엉뚱한 동작의 필요성은 디버그 로그의 `OCABC: N/256 슬라이드 값만 사용할 수 있습니다!` 메시지에 의해 결정됩니다. 로그에 `OCABC: 모든 슬라이드를 사용할 수 있습니다! ProvideCustomSlide를 비활성화할 수 있습니다!` 메시지가 있는 경우 `ProvideCustomSlide`를 비활성화할 수 있습니다.
* **SetupVirtualMap**: 예
* Gigabyte 보드에서 초기 커널 패닉을 해결하는 데 필요한 가상 주소에 대한 SetVirtualAddresses 호출을 수정합니다.

:::

## DeviceProperties

![DeviceProperties](../images/config/config-universal/DP-no-igpu.png)

### 추가

맵에서 장치 속성을 설정합니다.

기본적으로 Sample.plist에는 오디오에 대한 이 섹션이 설정되어 있으며, boot-args 섹션에서 레이아웃 ID를 설정하여 설정할 것이므로 `Add` 섹션에서 `PciRoot(0x0)/Pci(0x1b,0x0)`를 제거하는 것도 좋습니다.

요약하자면, 이 섹션을 사용하지 않으므로 여기에서 모든 PciRoot를 삭제합니다.

### 삭제

맵에서 장치 속성을 제거합니다. 우리는 이것을 무시할 수 있습니다.

## 커널

| 커널 | 커널 패치 |
| :--- | :--- |
| ![커널](../images/config/AMD/kernel.png) | ![](../images/config/AMD/kernel-patch.png) |

### 추가

여기서 어떤 kext를 로드할지, 어떤 특정 순서로 로드할지, 각 kext가 어떤 아키텍처를 위한 것인지 지정합니다. 기본적으로 ProperTree에서 한 대로 두는 것이 좋지만 32비트 CPU의 경우 아래를 참조하세요.

::: 세부 정보 더 자세한 정보

유의해야 할 가장 중요한 사항은 다음과 같습니다.

* 로드 순서
* 모든 플러그인은 종속성 *다음에* 로드해야 합니다.
* 즉, Lilu와 같은 kext는 VirtualSMC, AppleALC, WhateverGreen 등보다 **반드시** 앞에 와야 합니다.

[ProperTree](https://github.com/corpnewt/ProperTree) 사용자는 **Cmd/Ctrl + Shift + R**을 실행하여 각 kext를 수동으로 입력하지 않고도 모든 kext를 올바른 순서로 추가할 수 있습니다.

* **Arch**
* 이 kext에서 지원하는 아키텍처
* 현재 지원되는 값은 `Any`, `i386`(32비트), `x86_64`(64비트)입니다.
* **BundlePath**
* kext 이름
* 예: `Lilu.kext`
* **Enabled**
* 설명이 필요 없음, kext를 활성화하거나 비활성화
* **ExecutablePath**
* 실제 실행 파일의 경로는 kext 내에 숨겨져 있으며, 마우스 오른쪽 버튼을 클릭하고 `패키지 내용 표시`를 선택하면 kext의 경로를 확인할 수 있습니다. 일반적으로 `Contents/MacOS/Kext`이지만 일부는 `Plugin` 폴더 아래에 숨겨진 kext가 있습니다. plist 전용 kext는 이 값을 채울 필요가 없습니다.
* 예: `Contents/MacOS/Lilu`
* **MinKernel**
* kext가 주입될 가장 낮은 커널 버전입니다. 가능한 값은 아래 표를 참조하세요.
* 예: OS X 10.8의 경우 `12.00.00`
* **MaxKernel**
* kext가 주입될 가장 높은 커널 버전입니다. 가능한 값은 아래 표를 참조하세요.
* 예: OS X 10.7의 경우 `11.99.99`
* **PlistPath**
* kext 내에 숨겨진 `info.plist` 경로
* 예: `Contents/Info.plist`

::: 세부 정보 커널 지원 표

| OS X 버전 | MinKernel | MaxKernel |
| :--- | :--- | :--- |
| 10.4 | 8.0.0 | 8.99.99 |
| 10.5 | 9.0.0 | 9.99.99 |
| 10.6 | 10.0.0 | 10.99.99 |
| 10.7 | 11.0.0 | 11.99.99 |
| 10.8 | 12.0.0 | 12.99.99 |
| 10.9 | 13.0.0 | 13.99.99 |
| 10.10 | 14.0.0 | 14.99.99 |
| 10.11 | 15.0.0 | 15.99.99 |
| 10.12 | 16.0.0 | 16.99.99 |
| 10.13 | 17.0.0 | 17.99.99 |
| 10.14 | 18.0.0 | 18.99.99 |
| 10.15 | 19.0.0 | 19.99.99 |
| 11 | 20.0.0 | 20.99.99 |
| 12 | 21.0.0 | 21.99.99 |
| 13 | 22.0.0 | 22.99.99 |
| 14 | 23.0.0 | 23.99.99 |
| 15 | 24.0.0 | 24.99.99 |

:::

### 에뮬레이트

::: 팁 정보

펜티엄 및 셀러론과 같은 지원되지 않는 CPU를 스푸핑하고 지원되지 않는 CPU(예: AMD CPU)에서 CPU 전원 관리를 비활성화하는 데 필요합니다.

| Quirk | Enabled |
| :--- | :--- |
| DummyPowerManagement | YES |

:::

::: 세부 정보 더 자세한 정보

* **Cpuid1Mask**: 비워두세요
* 가짜 CPUID 마스크
* **Cpuid1Data**: 비워두세요
* 가짜 CPUID 항목
* **DummyPowerManagement**: YES
* NullCPUPowerManagement의 새로운 대안으로, 네이티브 전원 관리가 없는 모든 AMD CPU 기반 시스템에 필요합니다.
* **MinKernel**: 비워두세요
* 위 패치가 삽입되는 가장 낮은 커널 버전이며, 값을 지정하지 않으면 모든 버전의 macOS에 적용됩니다. 가능한 값은 아래 표를 참조하세요
* 예: OS X 10.8의 경우 `12.00.00`
* **MaxKernel**: 비워두세요
* 위 패치가 삽입되는 가장 높은 커널 버전이며, 값을 지정하지 않으면 모든 버전의 macOS에 적용됩니다. 가능한 값은 아래 표를 참조하세요
* 예: OS X 10.7의 경우 `11.99.99`

::: 세부 정보 커널 지원 표

| OS X 버전 | MinKernel | MaxKernel |
| :--- | :--- | :--- |
| 10.4 | 8.0.0 | 8.99.99 |
| 10.5 | 9.0.0 | 9.99.99 |
| 10.6 | 10.0.0 | 10.99.99 |
| 10.7 | 11.0.0 | 11.99.99 |
| 10.8 | 12.0.0 | 12.99.99 |
| 10.9 | 13.0.0 | 13.99.99 |
| 10.10 | 14.0.0 | 14.99.99 |
| 10.11 | 15.0.0 | 15.99.99 |
| 10.12 | 16.0.0 | 16.99.99 |
| 10.13 | 17.0.0 | 17.99.99 |
| 10.14 | 18.0.0 | 18.99.99 |
| 10.15 | 19.0.0 | 19.99.99 |
| 11 | 20.0.0 | 20.99.99 |
| 12 | 21.0.0 | 21.99.99 |
| 13 | 22.0.0 | 22.99.99 |

:::

### 강제

시스템 볼륨에서 kext를 로드하는 데 사용되며, 특정 kext가 캐시에 없는 이전 운영 체제(예: 10.6의 IONetworkingFamily)에만 해당됩니다.

저희는 무시할 수 있습니다.

### 차단

특정 kext가 로드되는 것을 차단합니다. 저희에게는 해당되지 않습니다.

### 패치

* 커널의 아키텍처 유형을 설정합니다. `Auto`, `i386`(32비트), `x86_64`(64비트) 중에서 선택할 수 있습니다.
* 32비트 커널(예: 10.4 및 10.5)이 필요한 이전 OS를 부팅하는 경우 `Auto`로 설정하고 macOS가 SMBIOS에 따라 결정하도록 하는 것이 좋습니다. 지원되는 값은 아래 표를 참조하세요.
* 10.4-10.5 — `x86_64`, `i386` 또는 `i386-user32`
* `i386-user32`는 32비트 사용자 공간을 참조하므로 32비트 CPU는 이것을 사용해야 합니다(또는 SSSE3가 없는 CPU)
* `x86_64`는 여전히 32비트 커널 공간을 갖지만 10.4/5에서는 64비트 사용자 공간을 보장합니다.
* 10.6 — `i386`, `i386-user32` 또는 `x86_64`
* 10.7 — `i386` 또는 `x86_64`
* 10.8 이상 — `x86_64`

* **KernelCache**: 자동
* 커널 캐시 유형을 설정합니다. 주로 디버깅에 유용하므로 최상의 결과를 위해 `자동`을 권장합니다. 지원

:::

## 기타

![기타](../images/config/config-universal/misc.png)

### 부팅

::: 팁 정보

| Quirk | 사용 가능 | 주석 |
| :--- | :--- | :--- |
| HideAuxiliary | YES | macOS 복구 및 기타 보조 항목을 표시하려면 스페이스바를 누르세요 |

:::

::: 세부 정보 더 자세한 정보

* **HideAuxiliary**: YES
* 이 옵션은 선택기에서 macOS 복구 및 도구와 같은 보충 항목을 숨깁니다. 보조 항목을 숨기면 다중 디스크 시스템에서 부팅 성능이 향상될 수 있습니다. 선택기에서 스페이스바를 눌러 이러한 항목을 표시할 수 있습니다.

:::

### 디버그

::: 팁 정보

OpenCore 부팅 문제를 디버깅하는 데 도움이 됩니다.

| Quirk | 사용 가능 |
| :--- | :--- |
| AppleDebug | 예 |
| ApplePanic | 예 |
| DisableWatchDog | 예 |
| Target | 67 |

:::

::: 세부 정보 더 자세한 정보

* **AppleDebug**: 예
* 디버깅에 유용한 boot.efi 로깅을 활성화합니다. 참고: 이 기능은 10.15.4 이상에서만 지원됩니다.
* **ApplePanic**: 예
* 커널 패닉을 디스크에 기록하려고 시도합니다.
* **DisableWatchDog**: 예
* UEFI 워치독을 비활성화합니다. 조기 부팅 문제를 해결하는 데 도움이 될 수 있습니다.
* **DisplayLevel**: `2147483650`
* 더 많은 디버그 정보를 표시하고 OpenCore의 디버그 버전이 필요합니다.
* **SysReport**: 아니요
* ACPI 테이블 덤프와 같은 디버깅에 도움이 됩니다.
* 이 기능은 OpenCore의 DEBUG 버전으로 제한됩니다.
* **Target**: `67`
* 더 많은 디버그 정보를 표시하고 OpenCore의 디버그 버전이 필요합니다.

이러한 값은 [OpenCore 디버깅](../troubleshooting/debug.md)에서 계산된 값을 기반으로 합니다.

:::

### 보안

::: 팁 정보

보안은 설명이 필요 없을 정도로 명확합니다. **건너뛰지 마세요**. 다음을 변경할 예정입니다.

| Quirk | Enabled | Comment |
| :--- | :--- | :--- |
| AllowSetDefault | YES | |
| BlacklistAppleUpdate | YES | |
| ScanPolicy | 0 | |
| SecureBootModel | Default | OpenCore가 SMBIOS에 해당하는 올바른 값을 자동으로 설정하도록 `Default`로 두세요. 다음 페이지에서 이 설정에 대해 자세히 설명합니다. |
| Vault | Optional | 이것은 단어이며, 이 설정을 생략하는 것은 선택 사항이 아닙니다. Optional로 설정하지 않으면 후회하게 될 것입니다. 대소문자를 구분합니다. |

:::

::: 세부 정보 더 자세한 정보

* **AllowSetDefault**: YES
* `CTRL+Enter` 및 `CTRL+Index`를 사용하여 선택기에서 기본 부팅 장치를 설정할 수 있도록 허용
* **ApECID**: 0
* 개인화된 보안 부팅 식별자를 수집하는 데 사용되며, 현재 이 기능은 macOS 설치 프로그램의 버그로 인해 신뢰할 수 없으므로 기본값으로 두는 것이 좋습니다.
* **AuthRestart**: NO
* FileVault 2에 대한 인증된 재시작을 활성화하여 재부팅 시 비밀번호가 필요하지 않습니다. 보안 위험으로 간주될 수 있으므로 선택 사항
* **BlacklistAppleUpdate**: YES
* 펌웨어 업데이트를 차단하는 데 사용되며 macOS Big Sur가 더 이상 `run-efi-updater` 변수를 사용하지 않으므로 추가 보호 수준으로 사용됨

* **DmgLoading**: Signed
* 서명된 DMG만 로드되도록 함
* **ExposeSensitiveData**: `6`
* 더 많은 디버그 정보를 표시하며 OpenCore의 디버그 버전이 필요함
* **Vault**: `Optional`
* 볼트를 다루지 않으므로 무시할 수 있음, **이 설정을 Secure로 설정하면 부팅되지 않음**
* 이것은 단어이며 이 설정을 생략하는 것은 선택 사항이 아닙니다. `Optional`로 설정하지 않으면 후회할 것입니다. 대소문자를 구분합니다.
* **ScanPolicy**: `0`
* `0`을 사용하면 사용 가능한 모든 드라이브를 볼 수 있습니다. 자세한 내용은 [보안](https://dortania.github.io/OpenCore-Post-Install/universal/security.html) 섹션을 참조하세요. **이 값을 기본값으로 설정하면 USB 장치를 부팅하지 않습니다.**
* **SecureBootModel**: 기본값
* macOS에서 Apple의 보안 부팅 기능을 제어합니다. 자세한 내용은 [보안](https://dortania.github.io/OpenCore-Post-Install/universal/security.html) 섹션을 참조하세요.
* 참고: 사용자는 이미 설치된 시스템에서 OpenCore를 업그레이드하면 조기 부팅 오류가 발생할 수 있습니다. 이를 해결하려면 여기를 참조하세요. [OCB에 멈춤: LoadImage 실패 - 보안 위반](/troubleshooting/extended/kernel-issues.md#stuck-on-ocb-loadimage-failed-security-violation)

:::

### 직렬

직렬 디버깅에 사용(모든 것을 기본값으로 둡니다).

### 도구

셸과 같은 OC 디버깅 도구를 실행하는 데 사용되며, ProperTree의 스냅샷 기능이 이를 추가합니다.

### 항목

OpenCore에서 자연스럽게 찾을 수 없는 불규칙한 부팅 경로를 지정하는 데 사용됩니다.

여기서는 다루지 않습니다. 자세한 내용은 [Configuration.pdf](https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/Configuration.pdf)의 8.6을 참조하세요.

## NVRAM

![NVRAM](../images/config/config-universal/nvram.png)

### 추가

::: 팁 4D1EDE05-38C7-4A6A-9CC6-4BCCA8B38C14

OpenCore의 UI 크기 조정에 사용되며 기본값이 우리에게 적합합니다. 자세한 내용은 심층 섹션을 참조하세요.

:::

::: 세부 정보 더 자세한 정보

부터 경로, 주로 UI 수정에 사용

* **DefaultBackgroundColor**: boot.efi에서 사용하는 배경색
* `00000000`: Syrah Black
* `BFBFBF00`: Light Gray

:::

::: 팁 4D1FDA02-38C7-4A6A-9CC6-4BCCA8B30102

OpenCore의 NVRAM GUID, 주로 RTCMemoryFixup 사용자에게 관련됨

:::

::: 세부 정보 더 자세한 정보

* **rtc-blacklist**: <>
* RTCMemoryFixup과 함께 사용하려면 자세한 내용은 여기를 참조하세요: [RTC 쓰기 문제 수정](https://dortania.github.io/OpenCore-Post-Install/misc/rtc.html#finding-our-bad-rtc-region)
* 대부분 사용자는 이 섹션을 무시할 수 있습니다.

:::

::: 팁 7C436110-AB2A-4BBB-A880-FE41995C9F82

시스템 무결성 보호 비트마스크

* **일반 용도 부팅 인수**:

| 부팅 인수 | 설명 |
| :--- | :--- |
| **-v** | 이렇게 하면 자세한 모드가 활성화되어 Apple 로고와 진행률 표시줄 대신 부팅할 때 스크롤되는 모든 비하인드 스토리 텍스트를 표시합니다. 부팅 프로세스를 자세히 살펴보고 문제, 문제 kext 등을 식별하는 데 도움이 되므로 모든 해킨토셔에게 매우 중요합니다. |
| **debug=0x100** | 이렇게 하면 macOS의 워치독이 비활성화되어 커널 패닉 시 재부팅을 방지하는 데 도움이 됩니다. 이렇게 하면 *바라건대* 유용한 정보를 얻고 빵가루를 따라 문제를 해결할 수 있습니다. |
| **keepsyms=1** | 이것은 debug=0x100에 대한 동반 설정으로, OS가 커널 패닉에 대한 심볼도 인쇄하도록 지시합니다. 이를 통해 패닉 자체의 원인에 대한 더 유용한 통찰력을 얻을 수 있습니다. |
| **npci=0x3000** | 이것은 `kIOPCIConfiguratorPFM64` 및 `gIOPCITunnelledKey`와 관련된 일부 PCI 디버깅을 비활성화합니다. 이것은 BIOS에서 Above 4G Decoding을 활성화하는 것의 대안입니다. BIOS에 없는 경우가 아니면 이것을 사용하지 마십시오. PCI 레인과 관련된 IRQ 충돌이 있어서 `[PCI 구성 시작]`에 갇힐 때 필요합니다. [출처](https://opensource.apple.com/source/IOPCIFamily/IOPCIFamily-370.0.2/IOPCIBridge.cpp.auto.html) |

* **GPU별 부팅 인수**:

| 부팅 인수 | 설명 |
| :--- | :--- |
| **agdpmod=pikera** | 일부 Navi GPU(RX 5000 및 6000 시리즈)에서 보드 ID 검사를 비활성화하는 데 사용되며, 이 옵션이 없으면 검은색 화면이 표시됩니다. **Navi가 없는 경우 사용하지 마세요** (예: Polaris 및 Vega 카드는 사용하면 안 됨) |
| **-radcodec** | 공식적으로 지원되지 않는 AMD GPU(스푸핑)가 하드웨어 비디오 인코더를 사용하도록 허용하는 데 사용됨 |
| **radpg=15** | 일부 전원 게이팅 모드를 비활성화하는 데 사용되며 AMD Cape Verde 기반 GPU를 올바르게 초기화하는 데 도움이 됨 |
| **unfairgva=1** | 지원되는 AMD GPU에서 하드웨어 DRM 지원을 수정하는 데 사용됨 |
| **nvda_drv_vrl=1** | macOS Sierra 및 High Sierra에서 Maxwell 및 Pascal 카드에서 NVIDIA의 웹 드라이버를 활성화하는 데 사용됨 |

* **csr-active-config**: `00000000`
* '시스템 무결성 보호'(SIP) 설정. 일반적으로 복구 파티션을 통해 `csrutil`로 변경하는 것이 좋습니다.
* csr-active-config는 기본적으로 `00000000`으로 설정되어 시스템 무결성 보호를 활성화합니다. 다양한 값을 선택할 수 있지만 전반적으로 최상의 보안 관행을 위해 이 기능을 활성화하는 것이 좋습니다. 자세한 내용은 문제 해결 페이지에서 확인할 수 있습니다. [SIP 비활성화](../troubleshooting/extended/post-issues.md#disabling-sip)

* **run-efi-updater**: `No`
* Apple의 펌웨어 업데이트 패키지가 설치되고 부팅 순서가 깨지는 것을 방지하는 데 사용됩니다. 이 기능은 이러한 펌웨어 업데이트(Mac용)가 작동하지 않으므로 중요합니다.

* **prev-lang:kbd**: <>
* `lang-COUNTRY:keyboard` 형식의 비라틴어 키보드에 필요하며, 지정할 수는 있지만 비워두는 것이 좋습니다(**샘플 구성의 기본값은 러시아어입니다**):
* 미국: `en-US:0`(`656e2d55533a30` in HEX)
* 전체 목록은 [AppleKeyboardLayouts.txt](https://github.com/acidanthera/OpenCorePkg/blob/master/Utilities/AppleKeyboardLayouts/AppleKeyboardLayouts.txt)에서 찾을 수 있습니다.
* 힌트: `prev-lang:kbd`를 문자열로 변경하여 HEX로 변환하는 대신 `en-US:0`을 직접 입력할 수 있습니다.
* 힌트 2: `prev-lang:kbd`를 빈 변수(예: `<>`)로 설정하면 처음 부팅할 때 언어 선택기가 대신 나타납니다.

| 키 | 유형 | 값 |
| :--- | :--- | :--- |
| prev-lang:kbd | 문자열 | en-US:0 |

:::

### 삭제

::: 팁 정보

NVRAM 변수를 강제로 다시 씁니다. `Add`는 NVRAM에 이미 있는 값을 **덮어쓰지 않으므로** `boot-args`와 같은 값은 그대로 두어야 합니다. 우리는 다음을 변경할 것입니다.

| Quirk | 사용 가능 |
| :--- | :--- |
| WriteFlash | 예 |

:::

::: 세부 정보 더 자세한 정보

* **LegacySchema**
* NVRAM 변수를 할당하는 데 사용되며 `OpenVariableRuntimeDxe.efi`와 함께 사용됩니다. 기본 NVRAM이 없는 시스템에만 필요합니다.

* **WriteFlash**: 예
* 추가된 모든 변수에 대한 플래시 메모리 쓰기를 활성화합니다.

:::

## PlatformInfo

![PlatformInfo](../images/config/config-universal/iMacPro-smbios.png)

::: 팁 정보

SMBIOS 정보를 설정하기 위해 CorpNewt의 [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) 애플리케이션을 사용하겠습니다.

이 예에서는 MacPro7,1 SMBIOS를 선택하지만 일부 SMBIOS는 다른 것보다 특정 GPU에서 더 잘 작동합니다.

* MacPro7,1: AMD Polaris 이상
* MacPro7,1은 macOS 10.15, Catalina 이상에서만 사용할 수 있습니다.
* iMacPro1,1: NVIDIA Maxwell 및 Pascal 또는 AMD Polaris 이상
* High Sierra 또는 Mojave가 필요한 경우 사용하고, 그렇지 않은 경우 MacPro7,1을 사용합니다.
* iMac14,2: NVIDIA Maxwell 및 Pascal
* NVIDIA GPU로 웹 드라이버를 설치한 후 iMacPro1,1에서 검은색 화면이 나타나는 경우 사용합니다.
* MacPro6,1: AMD GCN GPU(지원되는 HD 및 R5/R7/R9 시리즈)

GenSMBIOS를 실행하고 MacSerial을 다운로드하려면 옵션 1을 선택하고 SMBIOS를 선택하려면 옵션 3을 선택합니다. 이렇게 하면 다음과 비슷한 출력이 나옵니다.

```sh
###############################################################
# MacPro7,1 SMBIOS 정보 #
###################################################################

유형: MacPro7,1
일련 번호: F5KZV0JVP7QM
보드 일련 번호: F5K9518024NK3F7JC
SmUUID: 535B897C-55F7-4D65-A8F4-40F4B96ED394
Apple ROM: 001D4F0D5E22
```

순서는 `제품 | 일련번호 | 보드 일련번호(MLB)`입니다.

`유형` 부분은 Generic -> SystemProductName으로 복사됩니다.

`일련번호` 부분은 Generic -> SystemSerialNumber로 복사됩니다.

`보드 일련번호` 부분은 Generic -> MLB로 복사됩니다.

`SmUUID` 부분은 Generic -> SystemUUID로 복사됩니다.

`Apple ROM` 부분은 Generic -> ROM으로 복사됩니다.

Generic -> ROM을 Apple ROM(실제 Mac에서 덤프), NIC MAC 주소 또는 임의의 MAC 주소(6개의 임의 바이트일 수 있음, 이 가이드에서는 `11223300 0000`을 사용하겠습니다. 설치 후 [Fixing iServices](https://dortania.github.io/OpenCore-Post-Install/universal/iservices.html) 페이지를 따라 실제 MAC 주소를 찾는 방법을 따르세요)

**잘못된 일련번호가 필요하다는 것을 상기하세요! [Apple의 Check Coverage 페이지](https://checkcoverage.apple.com)에서 일련 번호를 입력하면 "이 일련 번호에 대한 적용 범위를 확인할 수 없습니다."와 같은 메시지가 표시됩니다.**

**자동**: 예

* DataHub, NVRAM 및 SMBIOS 섹션 대신 일반 섹션을 기반으로 PlatformInfo를 생성합니다.

:::

### 일반

::: 세부 정보 더 자세한 정보

* **AdviseFeatures**: 아니요
* EFI 파티션이 Windows 드라이브의 첫 번째가 아닌 경우에 사용됩니다.

* **MaxBIOSVersion**: 아니요
* 주로 정품 Mac에 적용되는 Big Sur+의 펌웨어 업데이트를 방지하기 위해 BIOS 버전을 최대로 설정합니다.

* **ProcessorType**: `0`
* 자동 유형 감지를 위해 `0`으로 설정하지만, 원하는 경우 이 값을 재정의할 수 있습니다. [AppleSmBios.h](https://github.com/acidanthera/OpenCorePkg/blob/master/Include/Apple/IndustryStandard/AppleSmBios.h)에서 가능한 값을 확인하세요.

* **SpoofVendor**: YES
* 공급업체 필드를 Acidanthera로 바꿉니다. 대부분의 경우 Apple을 공급업체로 사용하는 것은 일반적으로 안전하지 않습니다.

* **SystemMemoryStatus**: Auto
* SMBIOS 정보에서 메모리가 납땜되었는지 여부를 설정합니다. 순전히 미용적이므로 `Auto`를 권장합니다.

* **UpdateDataHub**: YES
* Data Hub 필드 업데이트

* **UpdateNVRAM**: YES
* NVRAM 필드 업데이트

* **UpdateSMBIOS**: YES
* SMBIOS 필드 업데이트

* **UpdateSMBIOSMode**: Create
* 새로 할당된 EfiReservedMemoryType으로 테이블을 교체하고 `CustomSMBIOSGuid`가 필요한 Dell 노트북에서 `Custom`을 사용합니다. quirk
* `CustomSMBIOSGuid` quirk를 활성화하여 `Custom`으로 설정하면 "비 Apple" OS에 대한 SMBIOS 주입을 비활성화할 수도 있지만 Bootcamp 호환성을 깨므로 이 방법을 권장하지 않습니다. 사용 시 위험을 감수하세요

:::

## UEFI

![UEFI](../images/config/config-universal/aptio-v-uefi.png)

**ConnectDrivers**: YES

* .efi 드라이버를 강제로 적용하고, NO로 변경하면 추가된 UEFI 드라이버가 자동으로 연결됩니다. 이렇게 하면 부팅 속도가 약간 빨라질 수 있지만 모든 드라이버가 스스로 연결되는 것은 아닙니다. 예를 들어 특정 파일 시스템 드라이버가 로드되지 않을 수 있습니다.

### 드라이버

여기에 .efi 드라이버를 추가하세요.

여기에 있는 드라이버는 다음과 같아야 합니다.

* HfsPlus.efi
* OpenRuntime.efi

::: 세부 정보 더 자세한 정보

| 키 | 유형 | 설명 |
| :--- | :--- | :--- |
| 경로 | 문자열 | `OC/Drivers` 디렉터리의 파일 경로 |
| LoadEarly | 부울 | NVRAM 설정 전에 드라이버를 조기에 로드합니다. 레거시 NVRAM을 사용하는 경우 `OpenRuntime.efi` 및 `OpenVariableRuntimeDxe.efi`에만 활성화해야 합니다. |
| 인수 | 문자열 | 일부 드라이버는 여기에 지정된 추가 인수를 허용합니다. |

:::

### APFS

기본적으로 OpenCore는 macOS Big Sur 이상에서만 APFS 드라이버를 로드합니다. macOS Catalina 이하를 부팅하는 경우 새 최소 버전/날짜를 설정해야 할 수 있습니다.
이를 설정하지 않으면 OpenCore가 macOS 파티션을 찾지 못할 수 있습니다!

macOS Sierra 이하에서는 APFS 대신 HFS를 사용합니다. 이전 버전의 macOS를 부팅하는 경우 이 섹션을 건너뛸 수 있습니다.

::: 팁 APFS 버전

최소 버전을 변경하는 경우 MinVersion과 MinDate를 모두 설정해야 합니다.

| macOS 버전 | 최소 버전 | 최소 날짜 |
| :------------ | :---------- | :------- |
| High Sierra(`10.13.6`) | `748077008000000` | `20180621` |
| Mojave(`10.14.6`) | `945275007000000` | `20190820` |
| Catalina(`10.15.4`) | `1412101001000000` | `20200306` |
| 제한 없음 | `-
