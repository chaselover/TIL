### CSS에서 사용하는 unit! 백분율, ems, rems,vh!

픽셀 이외의 단위를 사용할 떄마다 그것이 궁극적으로 px로 변한다는걸 알아야한다.

예측과 제어를 위해.

총 6단계

1. Declared value(author declared) - 선언된 값(140px, 66%)
2. Cascaded value(after the cascade) - cascading으로 결정된 값 66% / font-size브라우저기본값16px
3. Specified value(defaulting, if there is no cascaded value) 
   - 선언이 안된경우 initial value를 줌(padding의 경우 아무것도 원하지 않기 때문에 0px이 부여됨.)
   - 부모에게서 상속 받을 수도 있음.
4. Computed value(convertimg relative values to absolute) - 상대 단위가 있는 값 즉 px로 변환됨.
5. Used value(final calculations, based on layout) 
   - 렌더링된 레이아웃에 따라 다름. 부모가 280px이면 여기의 66%인 184.8px로 가치가 부여됨
6. Actual value(browser and device restricttions) - 너무 구체적인 값은 표시가 불가능. 반올림해서 185로 표시.



---



### Units!

font의 % -> 부모요소의 computed된 font-size보다 150% 크다는 이야기.

lengths의 %(높이 패딩, 여백..) -> parents의 width보다 10%라는 뜻.

font의 em(부모요소또는 현재 요소!!를 참조로 사용)

length의 em => padding의 2em은 현재의 글꼴크기인 24px의 두배. 48px

font의 rem(루트 글꼴 크기를 참조로 사용.)



!em과 rem을 사용하는 이유! 반응형 레이아웃을 위함(브라우저의 루트글꼴크기는 16px 거기의 10rem은 160)

-> root속성 등의 변경, hw에 대해 유연하게 대처함.



vh -> view port 높이의 ?%

vw-> view port 높이의 ?%



> Each property has an initial value.
>
> browser has a root font size
>
> percentages and relative values are always converted to pixels
>
> 글골은 부모 글꼴 기준
>
> 너비는 부노 너비 기준
>
> em은 부모
>
> rem은 root
>
> vh, vw는 뷰포트기준.

---

### 상속

모든 CSS 속성에는 값이 있어야한다.

cascade value가 먼저.

없다면 상속 받을 값이 있나 찾아봄.(line-height property specification)

없다면 그냥 deault value가 initial value로 주어짐.



> 상속에 대해 알아야 하는건 부모에서 자식에 이르는 일부 특정 속성에 대해 값을 전달한다는 것. 
>
> 개발자는 상속을 통해 더 적은 코드를 작성 할 수 있다, 유지관리에 용이하다는 뜻이다.



> 즉 우리는 상속되는 것이 무엇인지 기억해야한다(선언된 값이 아니라 속성의 계산된 값이 상속된다.)
>
> inherit 키워드를 사용해 강제로 상속 받을수도있고
>
> initial 키워드르 통해 강제로 초기값을 할당 받을수도 있다.



---



### REM을 사용해야 하는 이유!

: 반응형 웹에서 모바일 장치에 페이지를 표시할 때 수백개의 코드를 작성하는 대신 미디어 쿼리에서 전역설정 하나만 변경 하면 된다.(전역 글꼴 크기.)



*,

*::after,

*::before {

 margin: 0;

 padding: 0;

 box-sizing: inherit;

}

유니버셜 선택자에서 after과 before도 설정해주어 모든 환경에서 우리가 원하는 결과가 보이도록 통제.

이거 안하면 비포 애프터 설정할때마다 이상해짐.