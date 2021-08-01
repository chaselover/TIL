## Clean CSS

* clean
* modular
* Reusable
* Ready for growth



### CSS 구축

1. Think : about Layout of your webpage or app before writimg code
2. Build : your layout in HTML and CSS with a consistent structure fornaming classes
3. Architect : Create a logical architecture for your CSS with files and filders



#### 1. Component-Driven Design(디자인에 대한 Thinking이 먼저.)

* **modular building blocks** that make up interfaces(컴포넌트 드리븐 디자인을 통해 페이지를 모듈식 구성.)
* held together by the **layout** of the page.
* **Re-useable** across a project, and between different projects
* **Independent**, allowing us to use them anywhere on the page(컴포넌트가 부모에 의존하면 안됨.)



-> ATOMIC DESIGN이랑 비슷함.



#### 2. Build - naming classes

##### BEM(Block Element Modifier) - layout을 표시하기 위한 멋진 깔금한 시스템.

.block {}

.block__element {}

.block__element--modifier {}

* BLOCK: standalone component that is meaningful on its own(재사용가능한 독립실행형 구성요소)

* ELEMENT: part of a block that has no standalone meaning

* MODIFIER: a different version of a block or an element



### 3. Architect(CSS가 존재할 파일 구조.)

ITCSS / S MAX / 7-1 pattern

#### The 7-1 pattern

* 7개의 다른 폴더(partial Sass 폴더)
* 1개의 main Sass파일. 다른 모든 파일들을 임포트해 CSS stylesheet를 compile하는.

##### 7 folders

* base/(하나의 파일이있는)
* components/(각 구성요소에 대한 레이아웃.)
* layout/(프로젝트 전체의 레이아웃을 정의하는)
* pages/(스타일이 있는 페이지)
* themes/(프로젝트의 특정 페이지에 대해 구현하는(다양한 시각적 테마))
* abstracts/(CSS를 출력하지 않는 코드(변수, 믹스인))
* vendors/(모든 3자 CSS가 가는곳)

