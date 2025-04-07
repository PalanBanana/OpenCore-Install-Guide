# OpenCore 시작하기

OpenCore 기반 시스템을 만드는 데 뛰어들기 전에 몇 가지 사항을 살펴봐야 합니다.

## 필수 조건

1. <span style="color:red">_**[중요]**_</span> 시간과 인내심.
* 마감일이 있거나 중요한 작업이 있는 경우 이 작업을 시작하지 마세요. 해킨토시는 업무용 기계로 의존해서는 안 되는 것입니다.
2. <span style="color:red">_**[CRUCIAL]**_</span> **하드웨어 알아보기**
* CPU 이름과 세대
* GPU
* 스토리지 장치(HDD/SSD, NVMe/AHCI/RAID/IDE 구성)
* OEM에서 제공하는 경우 노트북/데스크톱 모델
* **이더넷 칩셋**
* WLAN/블루투스 칩셋
3. <span style="color:red">_**[CRUCIAL]**_</span> **명령줄에 대한 기본 지식과 터미널/명령 프롬프트 사용 방법**
* 이것은 [CRUCIAL]만이 아니라 이 가이드 전체의 기초입니다. 디렉토리로 `cd`를 하거나 파일을 삭제하는 방법을 모른다면 도와드릴 수 없습니다.
4. <span style="color:red">_**[CRUCIAL]**_</span> _**호환성**_ 섹션에서 볼 수 있듯이 호환되는 기기.
* [하드웨어 제한 사항 페이지](macos-limits.md)
5. <span style="color:red">_**[CRUCIAL]**_</span> 최소:
* macOS를 사용하여 USB를 만들 경우 16GB USB
* Windows 또는 Linux를 사용하여 USB를 만들 경우 4GB USB
6. <span style="color:red">_**[CRUCIAL]**_</span> **이더넷 연결**(WiFi 동글 없음, 이더넷 USB 어댑터는 macOS 지원에 따라 작동할 수 있음) 및 LAN 카드 모델을 알아야 함
* 실제 이더넷 포트 또는 호환되는 macOS 이더넷 동글/어댑터가 있어야 함. [호환 WiFi 카드](https://dortania.github.io/Wireless-Buyers-Guide/)가 있는 경우, 그것도 사용할 수 있습니다.
* 대부분의 WiFi 카드는 macOS에서 지원되지 않습니다.
* 이더넷을 사용할 수 없지만 Android 휴대전화가 있는 경우 Android 휴대전화를 WiFi에 연결한 다음 [HoRNDIS](https://joshuawise.com/horndis#available_versions)를 사용하여 USB를 사용하여 테더링할 수 있습니다.
7. <span style="color:red">_**[중요]**_</span> **적절한 OS 설치:**
* 다음 중 하나:
* macOS(최신 버전이 더 좋음)
* Windows(Windows 10, 1703 이상)
* Linux(Python 2.7 이상이 설치된 깨끗하고 제대로 작동하는 버전)
* Windows 또는 Linux 사용자의 경우 작업 중인 드라이브에 **15GB**의 여유 공간. Windows에서 OS 디스크(C:)에는 최소 **15GB**의 여유 공간이 있어야 합니다.
* macOS 사용자의 경우 시스템 드라이브에 **30GB**의 여유 공간이 있어야 합니다.
* 이 가이드에서 사용하는 대부분 도구에는 [Python](https://www.python.org/downloads/)도 설치되어 있어야 합니다.
8. <span style="color:red">_**[중요한]**_</span> **최신 BIOS 설치**
* 대부분의 경우 BIOS를 업데이트하면 macOS에 대한 최상의 지원이 제공됩니다.
* 예외는 MSI 500 시리즈 AMD 마더보드입니다. [마더보드 지원](macos-limits.md#motherboard-support)에서 자세히 알아보세요.
