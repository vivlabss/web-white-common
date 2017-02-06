자바스크립트 MVC
https://www.youtube.com/watch?v=GuS-JF3W82g

View
UI 논리 로직 - UI에 어떤 행위를 시킬 것인지 결정하는 논리 로직
프리젠테이션 로직 - UI를 실제로 변경하는 로직

둘을 구분해서 개발하자


Controller
입력 분석 로직 - 행위, 좌표, 상황 등을 고려해 어떤 요청인지 판단하는 로직
요청 처리 로직 - 요청에 필요한 행위를 실제로 구현하는 로직

지금은 대부분 요청 처리 로직이다


어떤 MVC 형태가 자바스크립트에 가장 적합한가?
뷰를 패시브하게 구현하면, 최대한 가볍게 구현 가능하고 가벼워진 뷰는 테스트를 용이하게 할 수 있다
그리고 모델의 상태 변화에 대한 구독과 UI 논리로직을 콘트롤러에 집중시킨다
테스트의 용이성을 높이거나 코드가 혼란스러워질 가능성을 최소화 하기 위해선 패시브 뷰 형태가 가장 무난하다
(UI 논리 로직과 프리젠테이션 로직의 분리)

패시브 뷰가 테스트에 유용한가?
콘트롤러는 뷰의 인터페이스와 관계를 가지고, 뷰는 최소한의 프리젠테이션 로직만 구현함으로써 뷰를 목으로 대체할 수 있다

변하는 것이 변하지 않는 것에 의존되어야 한다
도메인 -> 비즈니스 -> 뷰 순으로 구현되어야 한다
변할 가능성이 적은 것부터 개발을 해나가야 문제가 없다
모델링(도메인 객체) -> 도메인 객체를 사용하는 비즈니스 로직, 콘트롤러 -> 목 데이터로 테스트 -> 목이 아닌 실제 뷰 개발

사용자 요청 처리는 뷰가? 콘트롤러가?
사용자의 요청은 뷰가 아닌 모델과 뷰의 참조가 자유로운 콘트롤러에서 해야 적절하다
뷰는 소극적이므로 사용자 요청이 모델의 자원을 필요로 하는 경우 대응할 수 없다

엘리먼트 셀렉팅은 어디서?
뷰에서 엘리멘트 셀렉팅을 하는 것이 좋다
프리젠테이션에 필요한 리소스는 엘리먼트이며, 이를 적극적으로 사용하는 것은 뷰이다
콘트롤러는 엘리먼트를 통해 사용자 요청을 리스닝하는 것외엔 사용하지 않는다 (이벤트 리스너)
또한 셀렉팅 로직을 분산시키지 않는 것이 좋다, 분산시킨다면 마크업 변화 대응이 힘들어진다

UI 논리로직은 콘트롤러에서만 구현한다, 뷰는 프리젠테이션 로직만을 구현할 뿐 어떠한 UI 논리로직은 있어선 안 된다

HTML은 뷰가 아닌가?
HTML, DOM, 자바스크립트 뷰 객체 중 어느 것을 뷰로 보느냐에 따라 설계가 완전히 달라진다
HTML을 뷰로 보면, 자바스크립트 뷰 객체가 없어지므로, 엘리먼트 셀렉팅, 요청 처리, 프리젠테이션, UI 논리 모두 담당해야 한다 (이런 것을 뷰콘트롤러라고 함)

HTML은 템플릿으로 보는게 적합하다, 하지만 DOM 객체는 뷰로 보는게 좋다, 최소 단위의 ElementView다,
자바스크립트 구현시 생성하는 뷰 클래스는 이 여러 개의 ElementView를 조합한 큰 범위의 뷰다

이런 식으로 구현하는게 항상 좋은가?
우리가 개발할 때는 패시브 뷰가 늘 효율적이라고 생각하지만,
다중 뷰, 다중 모델일 때는 슈퍼바이징 콘트롤러와 비슷한 형태가 구현 복잡도를 낮추는 면에서 좋을 수 있다