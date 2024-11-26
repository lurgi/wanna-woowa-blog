## 🔗 [Good Abstraction, Bad Abstraction](https://frontendatscale.com/issues/2/)

### 🗓️ 번역 날짜: 2024.08.12

### 🧚 번역한 크루: 소하(최소연)

---

안녕하세요,

벌써 8월이 다 되어간다는 게 믿겨지시나요? 맞습니다. 1년 중 8번째 달이죠. 시간이 이렇게 빨리 흘러가다니요?

이번 주 이슈에서는 추상화, 디자인 패턴, 그리고 WET 코드베이스(맞습니다, wet입니다.)에 대해 이야기해 보겠습니다. 바로 시작해 보시죠.

#### SOFTWARE DESIGN

## 좋은 추상화, 나쁜 추상화

<img width="500px" src="https://embed.filekitcdn.com/e/5XfA8yFEtpiQK9jSAsyMnq/eNfbCPVKZX6vcXLFwgNova"/>

초창기 경력 시절, 저는 추상화란 그저 DRY(반복하지 않기)를 유지하는 것이라고 생각했습니다. 비슷해 보이는 두 개의 코드 블록을 볼 때마다, 모든 사용 사례를 처리할 수 있는 단일 함수를 만들어 중복을 제거하려고 최선을 다했습니다.

하지만 이 접근 방식에는 문제가 있습니다. 잘 확장되지 않는다는 것입니다.

요구사항이 변하고 코드베이스가 진화함에 따라, 이 모델은 점차 무너지기 시작합니다. 만약 우리가 완전히 다른 사용 사례에 맞추기 위해 함수를 변경하기 시작하면, 코드 재사용을 위한 우리의 선의의 시도는 빠르게 잘못된 추상화로 변하게 됩니다.

그렇다면 좋은 추상화를 만드는 비밀은 무엇일까요? 간단히 말해, 재사용성에 대한 관심을 줄이고 추상화가 진정으로 하는 일, 즉 복잡성을 관리하고 캡슐화하는 데 더 집중하는 것입니다. 그리고 이를 위한 훌륭한 방법은 좋은 추상화가 갖춰야 할 세 가지 기본적인 특성을 살펴보는 것입니다.

1. 좋은 추상화는 깊은 모듈(Deep Modules)로 구성됩니다.
2. 좋은 추상화는 중요하지 않은 세부 사항을 숨깁니다.
3. 좋은 추상화는 단일 수준(Single Level)에서 작동합니다.

이제 이에 대해 이야기해 보겠습니다.

## 좋은 추상화는 깊은 모듈로 구성된다.

**깊은** 모듈은 간단한 인터페이스를 통해 *강력한 기능*을 제공하는 모듈입니다. 인터페이스를 추상화의 비용, 기능을 그 *이점*이라고 생각할 수 있습니다. 깊은 모듈은 더 적은 비용으로 더 많은 기능을 제공하기 때문에 더 나은 추상화를 만듭니다.

<img width="500px" src="https://embed.filekitcdn.com/e/5XfA8yFEtpiQK9jSAsyMnq/4dsMJBKajWbpKpntGZhVYV"/>

반면에, 얕은 모듈은 기능에 비해 상대적으로 복잡한 인터페이스를 가진 모듈입니다. 함수가 너무 적게 수행하는 경우(예: "pass-through" 메서드) 이는 얕은 추상화를 다루고 있다는 것을 의미할 수 있습니다.

<img width="500px" src="https://embed.filekitcdn.com/e/5XfA8yFEtpiQK9jSAsyMnq/npFn4sZa3YucuEGN6hrtmw"/>

깊은 모듈을 선호한다고 해서 큰 함수를 목표로 해야 한다는 것은 아닙니다. 함수가 몇 줄의 코드만 가지고 있지만 인터페이스를 통해 노출하는 것보다 더 많은 복잡성을 캡슐화한다면 여전히 깊은 것으로 간주합니다.

이전 호에서 이야기했던 **'소프트웨어 디자인의 철학'**이라는 책에서 깊은 모듈 개념을 기억하실 수도 있습니다. 놓쳤다면 [여기](https://charca.ck.page/posts/frontend-at-scale-it-s-all-about-complexity)서 읽어보실 수 있습니다.

## 좋은 추상화는 중요하지 않은 세부 사항을 숨긴다.

추상화는 _중요하지 않은_ 구현 세부 사항을 숨겨야 하지만 중요한 세부 사항은 드러내야 합니다.

다음은 예시입니다:

<img width="500px" src="https://embed.filekitcdn.com/e/5XfA8yFEtpiQK9jSAsyMnq/4sAzv5qFmrv5AZQKtq33KD"/>

이 함수는 **axios** config 객체를 매개변수로 받는다는 것을 알 수 있습니다. 이것 자체로는 문제가 없지만, 이 특정한 경우에는 함수가 네트워크 요청을 하기 위해 axios를 사용한다는 사실은 추상화 사용자가 신경 쓸 필요가 없는 구현 세부 사항입니다.

또 한 가지 눈치채셨을 수도 있는 것은 사용자가 로그인되어 있을 때는 비동기적으로 동작하지만 로그인되어 있지 않을 때는 동기적으로 동작한다는 점입니다. 이것도 구현 세부 사항이지만 추상화 사용자에게 더 명확해야 하는 중요한 세부 사항입니다.

왜 그 세부 사항이 중요할까요? 장바구니에 항목을 추가한 후 바로 장바구니의 총 금액을 계산한다고 가정해 보세요. 동기 또는 비동기 동작이 호출되었는지에 따라 완전히 다른 결과를 얻게 됩니다.

그 함수의 대안 버전은 다음과 같이 보일 수 있습니다:

<img width="500px" src="https://embed.filekitcdn.com/e/5XfA8yFEtpiQK9jSAsyMnq/rTBb7piG1aaykuCteDNfaG"/>

여기서는 단순화를 위해 동일한 함수 내에서 axios 구성 객체를 구성하고 있습니다. 실제 사용 사례에서는 이를 별도로 처리하고 싶겠지만, 요점은 그게 아닙니다.

주목해야 할 중요한 점은 사용 가능한 정확한 구성 옵션을 지정하여 추상화의 누수를 수정했다는 것입니다. 이제 이 함수의 구현을 axios 대신 **fetch**를 사용하도록 변경해도 인터페이스는 전혀 변경되지 않습니다. 또한 이제 함수는 항상 비동기적으로 작동하므로 사용자에게 더 일관되고 명확한 동작을 제공합니다.

## 좋은 추상화는 단일 수준에서 작동한다.

추상화 수준을 혼합하면 모듈이 더 예측하기 어렵고 다루기 어려워집니다.

데이터베이스를 업데이트하는 함수는 DOM도 업데이트해서는 안 됩니다. 이들은 각각 분리되어야 하는 두 가지 다른 책임입니다. 선택한 프레임워크에서 이 두 가지 동작을 동일한 모듈에 공존할 수 있도록 허용하더라도, 로직을 더 작고 집중된 함수로 분리하여 각 추상화의 관심사를 분리하는 것이 항상 좋은 아이디어입니다.

**단일 책임 원칙**도 언급할 가치가 있습니다. 동일한 추상화 수준에서 작동하도록 보장하는 좋은 방법은 각 모듈에 [변경해야 할 이유를 하나](https://en.wikipedia.org/wiki/Single-responsibility_principle)만 제공하는 것입니다.

---

이것이 모든 함수가 "좋은 추상화 체크리스트"의 모든 항목에 체크해야 한다는 것을 의미할까요? 반드시 그렇지는 않습니다.

이것들은 규칙이 아닌 지침으로 취급되어야 합니다. 그리고 제 바람은 여러분이 이 지침을 사용하여 추상화를 완벽이 아닌 _더 좋게_ 만드는 것입니다.

결국 우리는 완벽한 추상화를 만드는 것이 아니라 문제를 해결하는 것이 목적입니다. 그리고 기존의 추상화가 관리 가능한 수준의 복잡성으로 문제를 해결할 수 있다면 이미 중요한 일을 제대로 하고 있는 것입니다.

#### 시청 리스트

## WET 코드베이스

<img width="500px" src="https://embed.filekitcdn.com/e/5XfA8yFEtpiQK9jSAsyMnq/7awJXEn6uPbPt68sWuQMNn"/>

추상화에 대한 에세이를 막 마친 것을 알고 있으며 주제를 바꾸고 싶을 수도 있습니다. 하지만 조금만 더 기다려주세요. 이번 주제는 그만한 가치가 있다고 약속드립니다.

[댄 아브람오프](https://x.com/dan_abramov)는 2019년 디콘스트(수십 년 전처럼 느껴지지만)에서 이 강연을 했지만 그가 공유하는 아이디어는 진정으로 시대를 초월합니다.

댄은 우리가 만드는 추상화가 여전히 의미가 있는지 고려하지 않고 너무 DRY에만 집중하는 위험에 대해 이야기하고, 추상화를 다룰 때 염두에 두어야 할 몇 가지 핵심 요소를 공유합니다:

- **추상화의 이점:** 의도에 집중, 코드 재사용, 버그 피하기
- **추상화의 비용:** 우연한 결합, 추가 간접성, 관성(팀을 위한)
- **책임감 있게 추상화하는 팁:** 구체적인 코드 테스트, 계층 추가 지연, 필요한 경우 항상 추상화 인라인 준비

댄은 이 주제에 대해 더 깊이 파고들려면 다음에 무엇을 봐야 하는지에 대한 추천으로 강연을 마무리합니다. 이 모든 강연의 열렬한 팬으로서 댄의 추천에 **+1**을 드리며 여러분도 시청 목록에 추가하시길 권장합니다:

- [샌디 메츠](https://twitter.com/sandimetz)의 ['모든 작은 것들'](https://www.youtube.com/watch?v=8bZh5LMaSmE)
- [세바스티안 마크바게](https://twitter.com/sebmarkbage)의 ['최소 API 표면 영역'](https://youtu.be/4anAwXYqLG8)
- [청 루](https://twitter.com/_chenglou)의 ['추상화 스펙트럼'](https://youtu.be/mVVNJKv9esE)

#### 아키텍처 스낵

## 확인해 볼 만한 링크

<img width="500px" src="https://embed.filekitcdn.com/e/5XfA8yFEtpiQK9jSAsyMnq/gpKBE266X4QPbeDxfBQixH"/>

#### [​React 18이 애플리케이션 성능을 향상하는 방법](https://vercel.com/blog/how-react-18-improves-application-performance)​

[​리디아 할리​](https://twitter.com/lydiahallie)

리디아 할리의 이 아름답게 설명된 기사에서는 Transitions, Suspense, **React 서버 컴포넌트**와 같은 동시성 기능이 애플리케이션 성능을 향상하는 방법을 배웁니다.

---

#### [​대규모 소프트웨어 개발은 어떤 모습일까요?​](https://www.youtube.com/watch?v=Dl-BdxNRUqs)

[​웹 개발 코디](https://twitter.com/webdevcody)​

혼자 또는 소규모 팀으로 작업하는 데 익숙한 경우 대규모 기업의 전형적인 소프트웨어 개발 라이프사이클(**SDLC**)에 대한 이 개요는 매우 유익합니다.

---

#### [​가능한 한 지루한 아키텍처를 고수하세요​](https://addyosmani.com/blog/boring-architecture/)

[​애디 오사니​](https://twitter.com/addyosmani)

최신 라이브러리와 프레임워크로 앱을 구축하는 것은 항상 매력적이지만 사용자에게 가치를 제공하는 데 관심이 있다면 **검증되고 테스트된**(즉, 지루한) 기술을 고수하는 것이 성공의 가장 좋은 기회를 제공합니다.

---

#### [​React에서 라우팅 계층을 구축하고 필요한 이유​](https://semaphoreci.com/blog/routing-layer-react)

[​안토넬로 자니니](https://twitter.com/antozanini95), [댄 애커슨​](https://twitter.com/danackerson)

React 앱의 **클라이언트 측 라우팅** 로직이 감당할 수 없게 된 경우 이 기사는 라우팅 계층을 사용하여 복잡성을 관리하는 몇 가지 아이디어를 제공할 수 있습니다.

---

#### [​Nuxt 서버 구성 요소 가이드​](https://roe.dev/blog/nuxt-server-components)

[​다니엘 로우​](https://twitter.com/danielcroe)

Nuxt는 다른 프레임워크와는 다른 **서버 컴포넌트** 및 섬 아키텍처 접근 방식을 취합니다. Vue 메타프레임워크에서 이러한 기능이 어떻게 작동하는지 잘 모르는 경우 이 기사는 훌륭한 개요입니다.

#### 서재

## 자바스크립트 디자인 패턴 학습

<img width="500px" src="https://embed.filekitcdn.com/e/5XfA8yFEtpiQK9jSAsyMnq/a974mFsysEfs835KCKPuqD"/>

해결하려는 문제에 적합한 추상화를 찾는 것은 종종 올바른 디자인 패턴을 선택하는 것과 같습니다. 하지만 선택할 수 있는 패턴이 너무 많고 프로그래밍 언어에 따라 구현 방식이 크게 다르기 때문에 말처럼 쉽지 않습니다.

그래서 [애디 오사니](https://twitter.com/addyosmani)의 고전적인 책인 ['자바스크립트 디자인 패턴 학습'](https://www.goodreads.com/book/show/144365156-learning-javascript-design-patterns)의 개정판이 나온다는 소식을 듣고 매우 기뻤습니다. 이 개정판은 언어의 최신 기능을 사용하도록 업데이트되었으며 지난 10년 동안 등장한 새로운 패턴도 일부 포함되어 있습니다.

260페이지 정도의 분량으로 다음을 살펴봅니다:

- **고전**: Gang of Four 책에서 친숙할 수 있는 생성 패턴, 구조 패턴, 행동 패턴
- **새로운 업데이트**: 비동기 자바스크립트를 다루는 패턴, MV\* 및 모듈 패턴에 대한 업데이트된 섹션
- **렌더링 및 React 전용**: React 코드베이스를 구성하고 Islands 아키텍처 및 Next.js와 함께하는 React 서버 구성 요소와 같은 모던 렌더링 패턴을 포함한 패턴

**오늘은 여기까지입니다, 여러분!** 끝까지 함께 해주셔서 감사합니다. 뉴스레터가 마음에 드셨다면 친구나 동료들과 공유해 주시면 큰 힘이 될 것입니다. (마음에 들지 않았다면 적에게 공유하는 건 어떠신가요?)

**저에게 피드백이 있으신가요?** 트위터나 이 이메일에 직접 답장하여 자유롭게 연락해 주세요. 모든 댓글을 읽고 감사드립니다.

**누가 이 이메일을 전달해 주었나요?** 먼저 그들이 얼마나 대단한지 말해주고, 다음 호를 바로 받아보시려면 뉴스레터 구독을 고려해 보세요.

좋은 한 주 보내세요 👋

- 맥스