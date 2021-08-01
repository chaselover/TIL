# CSS HTML중요.

* 반응형 디자인
  * fluid layouts
  * media queys
  * responsive images
  * correct units
  * desktop-first vs mobile-first
* 유지가능하고 확장 가능한 코드작성
  * Clean
  * easy-to-understand
  * growth
  * reusable
  * how to organize files
  * how to name classes
  * how to strucure HTML
* 웹 성능
  * 더 빠르고 크기를 작게 만드는 법.(사용자가 더 적은 데이터를 다운받아야함.
  * Less HTTP requests
  * less code
  * compress code
  * use a css preprocessor
  * Less images
  * Compress images

> 우리가 프로젝트를 하는동안 항상 자문해야될 것들임.



## 무대 뒤에서 어떤일이?

브라우저가 로딩을 시작할 때까지. http 요청, 올바른 도메인 이름 서비스.?



초기 html파일을 로드함.

* 그다음 로드된 html 코드를 가져와서 구문분석(parsing)함.(decode의 의미.)

* DOM(문서 객체 모델에는 디코딩된 html문서가 담김)

  

parsing된 HTML 헤더로 부터 CSS를 로딩

* CSS를 파싱함.

* 충돌하는 CSS 선언이 해결됨(cascade process로)
* 최종 CSS값을 처리함.(백분율 단위에서 실제 px단위로)(사용자의 기기에 따라 다르고 각각에따라 계산함)
* 이 모든것들이 DOM과 같은 CSS객체모델(CSSOM)트리 구조로 저장됨.



이들은 함께  Render tree를 만들고 페이지를 렌더링함.

visual formatting model을 랜더링 하려면 알고리즘이 많은 것들을 계산하고 사용함.

작업을 완료하면 마침내 웹사이트가 렌더링되고 화면에 페인팅을함.



---

##  CSS 파싱단계를 살펴보자

* CSS는 Selector과 Declaration block으로 나눠짐.(선택자와 선언블록)

* 선택자는 하나이상의 HTML요소를 선택함
* 선언은 각 CSS속성으로 구성됨.
* declare은 property와 declaration value로 나눠짐(각각의 선언의 key와 value)



### 첫단계는 충돌하는 CSS 선언의 해결

#### CASCADE로 다양한 Style Sheet의 충돌을 해결하고 결합함.

* 선언은 개발자 선언과 사용자 선언이 있고 브라우저 선언너이 있다.(a태그에 파란 텍스트 및줄 그어지는거)
* cascade는 이 모든것에서 충돌을 해결함.
  * Importance(weight)
    * 상충되는 선언의 중요성(선언된 위치에 따라)
      * 중요표시된 user 선언
      * 중요표시된 Author  선언 - !important 선언을 css  선언 가치 옆에 붙힐 수 있음.
      * 일반 Author 선언
      * 일반 사용자 선언
      * Default 브라우저 선언.
* 하지만 같은 중요성을 가지고있다면????
  * specificity(특성을 계산하고 비교하는 것.) 
  * 같은 중요성이면 아래의 스타일 갯수를 세어 가장 높은 점수를 얻은 선언박스에 우선권을 줌.
    * Inline styles
    * IDs
    * Classes,pseudo-classes,attribute
    * Elements,pseudo-elements(nav, div,...)
* 그래도 같은 중요성이라면 가장 마지막에 선언된 CSS선언이 다른 선언들을override하고 적용됨
  * source order



> CSS declarations marked with '!important' have highest priority!
>
> But only use !important as a last resource. (마지막 요소로 사용해야한다.)
>
> it's better to use correct specificities- more maintainable code
>
> 유지 가능한 코드에 대한 이야기. 중요한 것을 추가해서 덮는것은 장기적으로 더 큰 문제를 일으킨다.
>
> 더 쉽게 문제를 해결하도록 노력해야한다.



> Inline styles will alwyas have priority over style in external stylesheets
>
> 항상 스타일보다 우선시함(좋은 습관은 아님.)



> 또 1000개의 클래스보다 1개의 ID가 구체적이고
>
> 1000개의 elements보다 1개의 class가 있는 선택자가 더 구체적이다.
>
> 주목해야할것은 universal selector * 는 특이성값이(0,0,0,0)이다.
>
> 즉 우선권이 없다.



> 항상 선택자의 순서보다 구체적인 것에 의존하는것이 좋다. (id부터 class, element를 많이 달수록 우선순위를 가지니 구체적으로 작성하는게 좋다.(단순한 선택자 순서보다))
>
> -hover이라던가 뭔가 단다고 적용되는게 아니다. 무조건 보다 구체적인 선언박스가 적용된다.(hover같은건 무조건 가장 구체적인 선언박스를 복사해서 거기다 추가해야됨.)
>
> 언젠가 모든 CSS코드를 재정렬하는 경우 이 방법으로