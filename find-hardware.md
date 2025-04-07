# 하드웨어 찾기

이 섹션은 대부분 현재 실행 중인 하드웨어를 찾는 방법에 대한 미니 가이드입니다. 하드웨어 사양을 얻기가 조금 더 어렵기 때문에 이것은 주로 노트북 및 사전 구축된 사용자와 관련이 있습니다. 이미 가지고 있는 하드웨어를 알고 있다면 이 페이지를 건너뛰고 [USB 만들기](./installer-guide/)로 이동할 수 있습니다.

이를 위해 Windows 또는 Linux가 설치되어 있다고 가정합니다.

[[Toc]]

## Windows를 사용하여 하드웨어 찾기

이를 위해 우리는 주로 2가지 옵션이 있습니다:

* 윈도우의 내장 장치 관리자

* [AIDA64](https://www.aida64.com/downloads)

GUI를 더 사용하기 쉽기 때문에 사양을 잡기가 훨씬 쉽기 때문에 AIDA64를 다운로드하고 실행하는 것이 좋습니다. 그러나 하드웨어 사양을 얻기 위한 두 가지 방법을 모두 보여 드리겠습니다.

### CPU 모델

| AIDA64 | 장치 관리자 |

|:---------------------------------------------------------------------|:------------------------------------------------------------------------|

| ![](./images/finding-hardware-md/cpu-model-aida64.png) | ![](./images/finding-hardware-md/cpu-model-devicemanager.png) |

### GPU 모델

| AIDA64 | 장치 관리자 |

|:---------------------------------------------------------------------|:------------------------------------------------------------------------|

| ![](./images/finding-hardware-md/GPU-model-aida64.png) | ![](./images/finding-hardware-md/GPU-model-devicemanager.png) |

### 칩셋 모델

| AIDA64 | 장치 관리자 |

|:------------------------------------------------------------------------|:----------------------------------------------------------------|

| ![](./images/finding-hardware-md/chipset-model-aida64.png) | ![](./images/finding-hardware-md/chipset-model-devicemanager.png) |

* 참고: 인텔 SOC 기반 CPU는 전용 칩이 아닌 동일한 다이에 이미 칩셋 및 기타 기능을 가지고 있습니다. 이것은 정확한 칩셋을 감지하는 것이 조금 더 어렵다는 것을 의미합니다.

### 키보드, 트랙패드 및 터치스크린 연결 유형

| 장치 관리자 |

|:--------------------------------------------------------------------------------|

| ![](./images/finding-hardware-md/trackpad-model-devicemanager.png) |

안타깝게도 AIDA64는 포인터 장치에 대한 유용한 정보를 제공하지 않으므로 이를 위해 DeviceManager를 사용하는 것이 좋습니다.

* 다음에서 이러한 장치를 찾을 수 있습니다.

* `휴먼 인터페이스 장치`

* `키보드`

* `쥐 및 기타 포인터 장치`

* 장치의 정확한 연결 유형을 보려면 포인터 장치를 선택한 다음 `보기 -> 연결별 장치`를 입력하십시오. 이것은 PS2, I2C, SMBus, USB 등을 통해 있는지 여부를 명확히 할 것입니다.

기기에 따라 여러 이름과 연결 아래에 표시될 수 있습니다. 주목해야 할 주요 사항:

::: 세부 사항 SMBus

이것들은 `Synaptics SMBus Driver` 또는 `ELAN SMBus Driver`와 같은 직선 PCI 장치로 나타날 것입니다.

* 시냅틱 장치는 PS2 아래에 `시냅틱스 PS2 장치`/`시냅틱스 포인팅 장치`와 PCI 아래에 `시냅틱스 SMBus 드라이버`로 표시됩니다.

![](./images/finding-hardware-md/Windows-SMBus-Device.png)

보시다시피, 왼쪽 이미지에 2개의 Synaptics 장치가 있지만, 자세히 살펴보면 상단 장치는 PS2이고 하단 장치는 SMBus입니다. 어느 모드에서든 트랙패드를 사용할 수 있지만, SMBus는 일반적으로 더 나은 제스처 지원과 정확성을 제공합니다.

:::

::: 세부 사항 USB

| 유형별 장치 | 연결별 장치 |

| :--- | :--- |

| ![](./images/finding-hardware-md/USB-trackpad-normal.png) | ![](./images/finding-hardware-md/USB-trackpad-by-connection.png)

연결 보기를 `연결별 장치`로 전환할 때 USB 아래에도 'PS2 호환 트랙패드'로 표시됩니다.

:::

::: 세부 사항 I2C

![](./Images/finding-hardware-md/i2c-trackpad.png)

이것들은 거의 항상 Microsoft HID 장치로 표시되지만 다른 트랙패드로도 나타날 수 있습니다. 하지만 그들은 항상 I2C 아래에 나타날 것이다.

:::

### 오디오 코덱

| AIDA64 | 장치 관리자 |

|:--------------------------------------------------------------------------------|:----------------------------------------------------------------|

| ![](./images/finding-hardware-md/audio-controller-aida64.png) | ![](./images/finding-hardware-md/audio-controller-aida64.png.png) |

특정 OEM이 장치 이름을 표시하는 방식 때문에 DeviceManager로 얻을 수 있는 가장 정확한 정보는 PCI ID(예: pci 14F1,50F4)를 통한 것입니다. 이것은 당신이 ID를 구글링하고 정확한 장치 ID를 알아내야 한다는 것을 의미하지만, AIDA64는 최종 사용자에게는 훨씬 더 쉬운 이름을 적절하게 제시할 수 있습니다.

### 네트워크 컨트롤러 모델

| AIDA64 | 장치 관리자 |

|:---------------------------------------------------------------------|:------------------------------------------------------------------------|

| ![](./images/finding-hardware-md/nic-model-aida64.png) | ![](./images/finding-hardware-md/nic-model-devicemanager.png) |

특정 OEM이 장치 이름을 제시하는 방식 때문에 장치 관리자로 얻을 수 있는 가장 정확한 정보는 PCI ID(예: `PCI\VEN_14E4&DEV_43A0`은 `14E4`의 공급 업체 ID와 `43A0`의 장치 ID에 해당합니다. 이것은 당신이 ID를 구글링하고 정확한 장치 ID를 알아내야 한다는 것을 의미합니다. 그러나 AIDA64는 이름을 적절하게 제시할 수 있어 훨씬 쉬울 수 있습니다.

### 드라이브 모델

| AIDA64 | 장치 관리자 |

|:--------------------------------------------------------|:------------------------------------------------------------------------|

| ![](./images/finding-hardware-md/disk-model-aida64.png) | ![](./images/finding-hardware-md/disk-model-devicemanager.png) |

OEM이 드라이브에 대한 세부 사항을 많이 제공하지 않기 때문에 어떤 드라이브가 표시된 이름과 일치하는지 구글링해야 합니다.

## 리눅스를 사용하여 하드웨어 찾기

리눅스를 사용하여 하드웨어를 찾기 위해, 우리는 몇 가지 도구를 사용할 것입니다:

* `pciutils`

* `dmidecode`

아래에서 터미널에서 실행할 명령 목록을 찾을 수 있습니다. 고맙게도 대부분의 Linux 배포판은 이러한 도구가 이미 설치되어 있습니다. 그렇지 않다면 배포자의 패키지 관리자에서 찾을 수 있습니다.

### CPU 모델

```쉿

Grep -i "모델 이름" /proc/cpuinfo

```

### GPU 모델

```쉿

Lspci | grep -i --color "vga\|3d\|2d"

```

### 칩셋 모델

```쉿

Dmidecode -t 베이스보드

```

### 키보드, 트랙패드 및 터치스크린 연결 유형

```쉿

Dmesg | grep -i 입력

```

### 오디오 코덱

```쉿

에이플레이 -l

```

### 네트워크 컨트롤러 모델

기본 정보:

```쉿

Lspci | grep -i 네트워크

```

더 심층적인 정보:

```쉿

Lshw -클래스 네트워크

```

### 드라이브 모델

```쉿

Lshw -클래스 디스크 -클래스 스토리지

```

## OCSysInfo를 사용하여 하드웨어 찾기

OCSysInfo를 얻고 실행하는 2가지 방법이 있습니다.

* [사전 컴파일된 바이너리](https://github.com/KernelWanderers/OCSysInfo/releases)

* [리포지토리]를 수동으로 복제하기(https://github.com/KernelWanderers/OCSysInfo)

::: 팁

가장 간단하고 쉬운 방법이기 때문에 [이진](https://github.com/KernelWanderers/OCSysInfo/releases)를 다운로드하는 것이 좋습니다.

저장소를 수동으로 복제하는 것에 대해 자세히 알고 싶다면 OCSysInfo [mini-guide](https://github.com/KernelWanderers/OCSysInfo/tree/main/mini-guide)를 확인할 수 있습니다.

:::

### 하드웨어 발견하기

::: 경고

노트북 사용자: 시작하기 전에 외부 USB 장치를 분리하는 것이 좋습니다. 이렇게 하면 모호하거나 불필요한 정보가 수집되어 혼란스러울 수 있습니다.

:::

응용 프로그램을 성공적으로 설치하고 실행하면 다음 화면이 표시됩니다.

![](./Images/finding-hardware-md/ocsysinfo-example.png)

여기에서 `d`를 입력하고 `ENTER`/`RETURN`을 누르면 비슷한 화면이 나타납니다.

![](./Images/finding-hardware-md/ocsysinfo-hwdisc.png)

### CPU 모델

![](./Images/finding-hardware-md/cpu-model-ocsysinfo.png)

CPU 모델 외에도 CPU의 코드명, 지원되는 최고 SSE 버전 및 SSSE3 가용성도 나열합니다.

### GPU 모델

![](./Images/finding-hardware-md/gpu-model-ocsysinfo.png)

이 경우 컴퓨터에는 두 개의 GPU가 있습니다.

* iGPU (인텔 UHD 그래픽 630)

* dGPU (AMD Radeon R9 390X)

모델 이름 외에도 GPU의 코드명, ACPI 및 PCI 경로도 나열되어 있으며, 이는 hackintosh 여정을 진행함에 따라 곧 유용할 수 있습니다.

### 키보드 및 트랙패드 연결 유형

::: SMBus 트랙패드 세부 정보

![](./Images/finding-hardware-md/id-smbus-ocsysinfo.png)

트랙패드: `SMBus` <br /> 키보드: `PS/2`

이미지 제공에 대한 크레딧: [ThatCopy](https://github.com/ThatCopy)

:::

::: 세부 사항 I2C 트랙패드

![](./Images/finding-hardware-md/id-i2c-ocsysinfo.png)

트랙패드: `I2C` <br /> 키보드: `PS/2`

이미지 제공에 대한 크레딧: [Mahas](https://github.com/Mahas1)

:::

::: 세부 사항 PS/2 트랙패드

![](./Images/finding-hardware-md/id-ps2-ocsysinfo.png)

트랙패드: `PS/2` <br /> 키보드: `PS/2`

이미지 제공에 대한 크레딧: [Tasty0](https://github.com/Tasty0)

:::

### 오디오 코덱

![](./Images/finding-hardware-md/audio-codec-ocsysinfo.png)

### 네트워크 모델

![](./Images/finding-hardware-md/network-model-ocsysinfo.png)

### 드라이브 모델

![](./Images/finding-hardware-md/drive-model-ocsysinfo.png)
