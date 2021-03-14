# flex items + CSS Grid

2차원 행과 열의 레이아웃 시스템을 제공  
Flexible Box도 훌륭하지만 좀 더 직관적인 2차원 레이아웃을 짤 수 있게 해준다

## flex-items

flex container 내부 content들의 속성

- order : 각자 넣어줄 수 있음. 숫자가 클수록 순서가 밀림
- flex : item의 너비(width)를 설정하는 단축 속성
  - flex-basis : 공간 배분 전 설정하는 기본 너비. 단위로 줄 수 있음. 값이 auto이면 width나 height 값을 따라감
  - flex-grow : 증가 너비 비율. 여백을 비율로 나눠서 가져감. 낮을수록 증가할때 더 빡세게 늘어남
  - flex-shrink : 감소 너비 비율. 줄어들때 너비의 비율. 낮을수록 줄어들때 더 빡세게 줄어듬 => 근데 이거 계산이 좀 까다로운듯. Container의 너비가 줄어 Items의 너비에 영향을 미칠 경우, 영향을 미치기 시작한 지점부터 줄어든 거리 만큼 감소 너비 비율에 맞게 Item의 너비가 줄어듭니다.
  - 아이템이 가변 너비가 아닐 경우 효과가 없음
  - grow가 요소중 하나라도 0으로 설정이 안되있을 경우 컨테이너가 줄어듬에 따라 같이 줄어들지 않음(걍 고정됨)

```plain
flex: 증가너비 감소너비 기본너비;
```

## CSS grid

### container

- fr : 유연한 크기를 갖는 단위. 그리드 컨테이너 내의 공간 비율을 분수로 나타냄. 사용자가 계산해야할 부분을 fr을 통해 쉽고 유연하게 사용 가능. fr만큼의 크기 할당 - 남은 자유 공간의 전체분의 fr
- display : grid
- grid-template-rows : 명시적 행의 크기, 라인의 이름, 공간 비율, repeat

```css
/* 각 행의 크기를 정의합니다. */
.container {
  grid-template-rows: 100px 200px;
  /* grid-template-rows: [1 -3] 100px [2 -2] 200px [3 -1]; */
}
/* 동시에 각 라인의 이름도 정의할 수 있습니다. */
.container {
  grid-template-rows: [first] 100px [second] 200px [third];
}
/* 라인에 중복된 이름을 지정할 수 있습니다. => 숫자로 찾으면 되기는 함 */
.container {
  grid-template-rows: [row1-start] 100px [row1-end row2-start] 200px [row2-end];
}

.container {
  width: 400px;
  display: grid;
  /* 100px만큼 3개의 row 할당 */
  grid-template-rows: repeat(3, 100px);
  /* 3분의 1만큼 3개의 column할당 */
  grid-template-columns: repeat(3, 1fr);
}
```

- grid-template-columns : row랑 똑같은데 말그대로 column의 경우
- grid-template-areas : 지정된 그리드 영역이름을 참조해 그리드 템플릿을 생성. grid-item 속성임. 시맨틱하다는게 신기한 특징

```css
.container {
  display: grid;
  grid-template-rows: repeat(3, 100px);
  grid-template-columns: repeat(3, 1fr);
  grid-template-areas:
    "header header header"
    "main main aside"
    "footer footer footer";
}
header {
  grid-area: header;
}
main {
  grid-area: main;
}
aside {
  grid-area: aside;
}
footer {
  grid-area: footer;
}

/* 빈칸은 마침표 혹은 None으로 */
.container {
  display: grid;
  grid-template-rows: repeat(4, 100px);
  grid-template-columns: repeat(3, 1fr);
  grid-template-areas:
    "header header header"
    "main . ."
    "main . aside"
    "footer footer footer";
}
header {
  grid-area: header;
}
main {
  grid-area: main;
}
aside {
  grid-area: aside;
}
footer {
  grid-area: footer;
}
```

- grid-template : rows, columns, areas 단축 속성. 되게 직관적이다;;

```css
.container {
  display: grid;
  grid-template:
    "header header header" 80px
    "main main aside" 350px
    "footer footer footer" 130px
    / 2fr 100px 1fr;
}
header {
  grid-area: header;
}
main {
  grid-area: main;
}
aside {
  grid-area: aside;
}
footer {
  grid-area: footer;
}
```

- row-gap : 각 행과 행 사이의 간격, gutter, 그리드 선의 크기 지정
- column-gap : 각 열과 열 사이의 간격, gutter
- gap : 각 행과행 열과열 사이의 간격

```css
.container {
  display: grid;
  grid-template-rows: repeat(2, 150px);
  grid-template-columns: repeat(3, 1fr);
  gap: 20px 10px;
}
/* 하나의 값으로 통일할 수 있습니다. */
.container {
  gap: 10px; /* row-gap: 10px; + column-gap: 10px; */
}
/* 하나의 값만 적용하고자 한다면 다음과 같이 사용할 수 있습니다. */
.container {
  gap: 10px 0; /* row-gap */
  gap: 0 10px; /* column-gap */
}
```

- grid-auto-rows/grid-auto-columns : 암시적 행과 열의 크기를 정의. 명시적으로 지정한 컨테이너의 행 바깥의 행의 크기를 지정해줌. 굉장히 동적. 사실 명시적으로 선언 안하고 이것만 선언하고 자식 elem은 마음대로 넣어도 될듯?
- grid-auto-flow : 배치하지 않은 아이템을 어떤 방식의 자동배치 알고리즘으로 처리할지 정의. row, column, row dense, column dense
- align-content : 수직 정렬. flex랑 같
- justify-content : 수평 정렬. flex랑 같

```css
.container {
  place-content: <align-content> <justify-content>;
}
```

- align-items : 그리드 한 칸 안에서의 item 수직 정렬
- justify-items : 그리드 아이템들을 수평 정렬

```css
.container {
  place-items: <align-items> <justify-items>;
}
```
