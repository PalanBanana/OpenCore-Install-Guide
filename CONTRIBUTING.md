# 가이드 지원하기

**참고**: 이것은 Acidanthera가 공식적으로 승인하지 않은 커뮤니티 운영 가이드입니다. 이 가이드에 대한 문제로 Acidanthera를 괴롭히지 마십시오.

가이드를 지원하고 싶으신가요? 음 네가 도울 수 있는 몇 가지 방법이 있어!

[[Toc]]

참고: 재정적으로 기부하고 싶은 사람들을 위해, 우리는 진심으로 감사하지만 우리는 비영리 단체입니다. 우리는 돈을 벌기 위해가 아니라 가르치기 위해 이것을 한다. 남은 돈이 있다면 자선 단체에 기부하는 것을 적극 권장합니다. [캐나다 크론과 대장염](https://crohnsandcolitis.donorportal.ca/Donation/DonationDetails.aspx? L=en-CA&G=159&F=1097&T=GENER)는 마음에 드는 것이 없다면 우리가 추천하는 것입니다.

## 문제를 통해 기여하기

문제를 통해 기여하는 것은 매우 간단하지만 몇 가지 규칙이 있습니다.

* 이슈 탭은 가이드 이슈 전용으로 유지되며, **개인적인 해킨토시 이슈는 없습니다**. 설치 문제를 논의할 장소가 아닙니다.

* 오타나 더 나은 설명을 위해, 그것이 어느 페이지에 있었는지 표시해 주세요. 이러한 문제가 어디에 있는지에 대한 스캐빈저 헌트를 하지 않아도 감사하겠습니다.

여기에서 버그 트래커를 찾을 수 있습니다: [버그 트래커](https://github.com/dortania/bugtracker)

## PR을 통한 기여

PR을 통해 기여할 때 몇 가지 지침:

* 머리를 쓰세요 (제발).

* 제출물을 교정하세요.

* 풀 리퀘스트가 맞지 않거나 부정확한 정보가 있다고 생각되면 거부될 수 있습니다. 우리는 일반적으로 그것이 거부된 이유를 말하거나 수정을 요청할 것입니다.

* 우리는 또한 당신이 제공하는 정보가 유효한지 더 쉽게 확인할 수 있도록 더 큰 커밋에 대한 출처에 감사할 것입니다.

* 이미지는 `../images/` 폴더 아래의 저장소에서 로컬로 호스팅되어야 합니다.

* 귀하의 PR은 마크다운 린트를 통해 실행되어야 하며 모든 문제가 해결되어야 합니다.

* 일반적으로, 가능하면 "비 Acidanthera" 도구를 사용하지 않도록 노력하세요. 일반적으로 우리는 타사 도구의 사용을 피하고 싶습니다. 그렇지 않으면 불가능하다면 연결할 수 있습니다.

* 명시적으로 금지된 도구:

* 유니비스트, 멀티비스트 그리고 케스트비스트

* 더 많은 정보는 여기에서 찾을 수 있습니다: [Tonymacx86-stance](https://github.com/khronokernel/Tonymcx86-stance)

* 트랜스맥

* 지루한 USB 드라이브를 만드는 것에 대해 알고 있다

* 니레쉬 설치자

* 우리는 가이드와 함께 불법 복제를 피하고 싶습니다

### 기여하는 방법

커밋을 테스트하고 올바르게 포맷되었는지 확인하는 가장 좋은 방법은 Node.js를 다운로드한 다음 `npm install`을 실행하여 종속성을 설치하는 것입니다. `npm run dev`를 실행하면, 당신이 만든 변경 사항을 보기 위해 연결할 수 있는 로컬 웹 서버를 설정할 것입니다. `npm test`는 서식 및 맞춤법 검사에 대한 오류를 귀하에게 던질 것입니다. `markdownlint`가 자동으로 린팅을 수정하려고 시도하도록 하려면 `npm run fix-lint`를 실행하세요.

간단한 단계별:

* [이 저장소를 포크](https://github.com/dortania/OpenCore-Install-Guide/fork/)

* 필요한 도구를 설치하세요:

* [Node.js](https://nodejs.org/)

* 변경하세요.

* 사이트 구축:

* `npm install` (필요한 모든 플러그인을 설치하기 위해)

* `npm run dev` (사이트 미리보기)

* `http://localhost:8080`에서 찾을 수 있습니다.

* 체크 린팅 및 맞춤법 검사:

* `npm 테스트`

* `npm run lint` 및 `npm run spellcheck` (테스트를 개별적으로 실행하기 위해)

* `npm run fix-lint` (잠재적인 문제를 해결하기 위해)

* 기본 맞춤법 검사에서 지원되지 않는 단어의 경우, [dictionary.txt](./dictionary/dictionary.txt)에 추가하고 `npm run sort-dict`를 실행하십시오.

### 팁

기여를 조금 더 쉽게 만드는 몇 가지 도구:

* [비주얼 스튜디오 코드](https://code.visualstudio.com)

* [Typora](https://typora.io) 실시간 가격 인하 렌더링을 위해.

* [TextMate](https://macromates.com) 쉽고 강력한 대량 찾기/대체.

* [GitHub Desktop](https://desktop.github.com) 더 사용자 친화적인 GUI를 위해.

## 번역을 통한 기여

도르타니아의 가이드는 주로 영어를 기반으로 하지만, 우리는 세상에 다른 언어가 많고 모든 사람이 영어를 유창하게 구사하는 것은 아니라는 것을 알고 있습니다. 만약 당신이 우리의 가이드를 다른 언어로 번역하는 것을 돕고 싶다면, 우리는 기꺼이 당신을 지원할 것입니다.

명심해야 할 주요 사항:

* 번역은 전용 포크여야 하며 Dortania의 가이드에 병합되지 않습니다.

* 포크는 그들이 도르타니아의 번역이며 공식적이지 않다는 것을 나타내야 한다

* 포크는 또한 우리의 [라이센스](LICENSE.md)를 준수해야 합니다.

위의 내용을 충족하면 문제없이 번역을 자유롭게 호스팅할 수 있습니다! Dortania의 사이트는 [GitHub Actions](https://github.com/features/actions)를 사용하여 [VuePress](https://vuepress.vuejs.org)로 제작되었으며 마지막으로 [GitHub Pages](https://pages.github.com)에서 호스팅되므로 자신의 번역을 호스팅하는 데 비용이 들지 않습니다.

번역이나 호스팅에 대한 질문이나 우려 사항이 있으시면 언제든지 [Bugtracker](https://github.com/dortania/bugtracker)에 문의하십시오.

현재 알려진 번역:

* [InyextcionES](https://github.com/InyextcionES/OpenCore-Install-Guide)(스페인어)

* [macOS86](https://macos86.gitbook.io/guida-opencore/)(이탈리아어, 더 이상 유지 관리되지 않음)

* [테크노팟](https://www.technopat.net/sosyal/konu/opencore-ile-macos-kurulum-rehberi.963661/)(터키어)

* [ThrRip](https://github.com/ThrRip/OpenCore-Install-Guide)(중국어, 더 이상 유지 관리되지 않음)

* [sumingyd](https://github.com/sumingyd/OpenCore-Install-Guide)(중국어)

* [시주로](https://github.com/shijuro/OpenCore-Install-Guide)(러시아어)

* [viOpenCore](https://github.com/viOpenCore/OpenCore-Install-Guide)(베트남어)

그리고 이러한 번역은 저자의 선호도, 번역 변경 및 인적 오류의 영향을 받는다는 점에 유의하십시오. 그들은 더 이상 공식 도르타니아 가이드가 아니기 때문에 읽을 때 이것을 명심하세요.
