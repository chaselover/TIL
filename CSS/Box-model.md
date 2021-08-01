## Rendering Tree 이후 Website rendering.

CSS의 시각적 서식모델은

* Dimensions of boxes: the box model
* Box type: inline, block, inline block
* Positioning schme: floats and positioning
* stacking context
* other elements in the rendertree(형제, 부모등)
* Viewport size, dimensions of images, etc.



### Box Model

웹 페이지의 레이아웃을 위해 알아야함.

요소가 어떻게 구성되는지 정의하는 요소 중 하나.

각각의 모든 요소는 웹페이지에서 사각형 상자로 볼수있다.

각 상자는 width height, padding 그리고 border, margin 이 있다.

* content: text, images, stc..

* Padding: 상자 내부에 공백을 생성한다.(context를 감싸는 영역.)

* Margin(여백) : 박스와 박스사이의 공간.

* Fill AREA : 채우기영역. 배경색과 배경이미지는 content영역을 벗어나 border까지 닿음.

  (padding까지 포함하는 영역)

* total width,height : border끝에서 끝까지. (border를 포함하는)



#### Block-level boxes(display: block)

* elements formatted visually as blocks
* 100% of parents width
* vertically, one after another(세로로 형식화되어 줄바꿈 만듬)
* Box-model applies as ahowed



#### Inline-Boxes(display: inline)

* Content is distributed in lines
* Occupies only content's space
* No line-breaks
* No height and widths
* Paddings and margins only horizontal (left and right)(수평패딩마진만 지정할 수 있다.)



#### Inline-block boxes(콘텐츠크기만 차지하는 block)

* mixed of block and inline
* Occupies only content's space

* No line-breaks

* Box-model applies as ahowed

  =>보통 block에 inline속성을 부여하고 싶을때 부여.



### 3. Positioning schemes : Normal flow, Absolute positioning, flow 

일반흐름, 부동 및 절대위치 지정. 상대위치, 절대위치 등등.

#### Normal flow(Default, position relative)

* Default positioning scheme
* Not floated
* Not Absolutely positioned
* Elements laid out according to their source order



### floats(float: left, right)

* Element is removd from the normal flow 

  정상적인 흐름에서 벗어나  왼쪽 오른쪽 끝으로 이동.(가장자리)

* text and inline elements will wrap around the floated element 

  텍스트나 다른 인라인 요소가 다른 float요소를 줄바꿈한다.

* the container will not adjust its ehight to the element

  요소에 맞게 높이를 조정함.(문제가 될 요소가 있음. -> 구체적인 수정사항을 써야함.)



#### absolute positioning(position: absolute, fixed)

* Element is removd from the normal flow

*  No impact on surrounding content or elements

* We use top,bottom, left and right to offset the element from its relatively positioned container

  탑, 바텀, 왼, 오른쪽으로 그 콘테이너에 상대적으로 위치시킨다.

---





### 4. Stacking Context

요소가 페이지에 랜더링됨. 스태킹 컨텍스트는 다른 방법으로 생성할 수 있다. z-index같은 속성.

스택을 형성하는 레이어 같음. 맨아래에 있는 레이어가 처음에, 스택의 상위요소가 맨위에 나와서 겹침.

z-index 3 relative, z-index 2 absolute, z-index 1 relative

z-index뿐만 아니라 불투명도값, 변환, 필터등 속성으로 stacking context를 생성할 수 있음.

