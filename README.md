---

집: 진실

heroImage: /dortania-logo-clear.png

heroText: Dortania의 OpenCore 설치 가이드

actionText: 시작하기→

actionLink: prerequisites.md

메타:

- 이름: 설명

내용: 현재 지원되는 버전 1.0.2

---

# OpenCore란 무엇이며 이 가이드는 누구를 위한 것입니다.

OpenCore는 우리가 "부트 로더"라고 부르는 것입니다. 특히 SMBIOS, ACPI 테이블 및 kexts와 같은 macOS에 대한 새로운 데이터를 주입하여 macOS용 시스템을 준비하는 데 사용하는 복잡한 소프트웨어입니다. 이 도구가 Clover와 같은 다른 도구와 다른 점은 보안과 품질을 염두에 두고 설계되어 [시스템 무결성 보호](https://support.apple.com/ko-ca/HT204899) 및 [FileVault](https://support.apple.com/en-ca/HT204837)와 같은 실제 Mac에서 찾을 수 있는 많은 보안 기능을 사용할 수 있다는 것입니다. 더 심층적인 모습은 여기에서 찾을 수 있습니다: [왜 OpenCore가 Clover와 다른 사람들보다 더 많은가](why-oc.md)

이 가이드는 특히 두 가지 주요 사항에 중점을 둡니다.

* X86 기반 PC에 macOS 설치하기

* 당신의 해킹이 작동하게 만드는 것을 가르쳐 줍니다.

이 때문에, 당신은 구글을 읽고, 배우고, 심지어 사용할 것으로 기대될 것입니다. 이것은 간단한 원클릭 설치 설정이 아닙니다.

OpenCore는 아직 새롭고 현재 베타 버전임을 기억하십시오. 꽤 안정적이며, 거의 모든 면에서 클로버보다 훨씬 안정적이지만, 여전히 자주 업데이트되고 있기 때문에 구성 덩어리가 꽤 자주 변경됩니다(즉, 오래된 것을 대체하는 새로운 기이함).

마지막으로, 문제가 있는 사람들은 [r/Hackintosh subreddit](https://www.reddit.com/r/hackintosh/)와 [r/Hackintosh Discord](https://discord.gg/u8V7N5C)를 모두 방문하여 더 많은 도움을 구할 수 있습니다.
