# PART6: Sass(SCSS)
<a href="https://heropy.blog/2018/01/31/sass/"><참고문헌></a>

CSS Preprocessor CSS 전처리기의 일종.  

## Sass, SCSS 문법차이
```css
/*Sass:*/

.list
  width: 100px
  float: left
  li
    color: red
    background: url("./image.jpg")
    &:last-child
      margin-right: -10px
/*SCSS:*/

.list {
  width: 100px;
  float: left;
  li {
    color: red;
    background: url("./image.jpg");
    &:last-child {
      margin-right: -10px;
    }
  }
}
```
Sass는 선택자의 유효범위를 ‘들여쓰기’로 구분하고, SCSS는 {}로 범위를 구분합니다.

<a href="https://www.sassmeister.com/">SCSS 문법 테스트 사이트</a>

## 상위 선택자 참조(&)
```css
.btn{
  position: absolute;
  &.active{
    color: red
  }
}
//==>css
.btn.acrive{}

.list{
  li{
    &:last-child{
      //...
    }
  }
}
==> css
.list li:last-child{

}
```

## 중첩된 속성(:)
font-, margin- 등과 같이 동일한 네임 스페이스를 가지는 속성들을 다음과 같이 사용할 수 있습니다.
```scss
.box {
  font: {
    weight: bold;
    size: 10px;
    family: sans-serif;
  };
  margin: {
    top: 10px;
    left: 20px;
  };
  padding: {
    bottom: 40px;
    right: 30px;
  };
}
```
Compiled to:
```css
.box {
  font-weight: bold;
  font-size: 10px;
  font-family: sans-serif;
  margin-top: 10px;
  margin-left: 20px;
  padding-bottom: 40px;
  padding-right: 30px;
}
```

## 변수(Variables)
반복적으로 사용되는 값을 변수로 지정할 수 있습니다.
변수 이름 앞에는 항상 `$`를 붙입니다.   
`$변수이름: 속성값;`

- ### 변수 유효범위(Variable Scope)   
  `선언된 블록({}) 내에서만 유효범위를 가집니다.`
      
- ### 변수 재 할당(Variable Reassignment)
  변수에 변수를 할당할 수 있습니다.
  ```scss
  $red: #FF0000;
  $blue: #0000FF;

  $color-primary: $blue;
  $color-danger: $red;

  .box {
    color: $color-primary;
    background: $color-danger;
  }
  ```

- ### !global (전역 설정)

- ### !default (초깃값 설정)

- ### #{} (문자 보간)
#{}를 이용해서 코드의 어디든지 변수 값을 넣을 수 있습니다.

## 산술 연산(Operations)
나누기 연산의 주의사항
CSS는 속성 값의 숫자를 분리하는 방법으로 /를 허용하기 때문에 /가 나누기 연산으로 사용되지 않을 수 있습니다.   
예를 들어, font: 16px / 22px serif; 같은 경우 font-size: 16px와 line-height: 22px의 속성값 분리를 위해서 /를 사용합니다.
```scss
div{
  width: (100px/2) /*==> 50px*/
}
```

## 재활용(Mixins)
Sass Mixins는 스타일 시트 전체에서 재사용 할 CSS 선언 그룹 을 정의하는 아주 훌륭한 기능입니다.  
약간의 Mixin(믹스인)으로 다양한 스타일을 만들어낼 수 있습니다.

우선, Mixin은 두 가지만 기억하면 됩니다.
선언하기(@mixin)와 포함하기(@include) 입니다.  
만들어서(선언), 사용(포함)

## 키워드 인수
인수를 입력할 때 명시적으로 키워드(변수)를 입력하여 작성할 수 있습니다.   
별도의 인수 입력 순서를 필요로 하지 않아 편리하게 작성할 수 있습니다.  
```scss
@mixin 믹스인이름 {
  스타일;
}
@include 믹스인이름

//인수 사용 가능
@mixin 믹스인이름($매개변수) {
  스타일;
}
@include 믹스인이름(인수);

//EX
@mixin dash-line($width, $color) {
  border: $width dashed $color;
}

.box1 { @include dash-line(1px, red); }
.box2 { @include dash-line(4px, blue); }

//인수의 기본값 설정 (:)
@mixin 믹스인이름($매개변수: 기본값) {
  스타일;
}
//EX
@mixin dash-line($width: 1px, $color: black) {
  border: $width dashed $color;
}

.box1 { @include dash-line; }
.box2 { @include dash-line(4px); }
.box3 { @include dash-line(4px,red); }
//키워드 인수
.box3 { @include dash-line($color:red); }
```

## 반복문 (for문)
```scss
// through
// 종료 만큼 반복
@for $변수 from 시작 through 종료 {
  // 반복 내용
}

// to
// 종료 직전까지 반복
@for $변수 from 시작 to 종료 {
  // 반복 내용
}
//ex
@for $i from 1 through 3 {
  //through:nth-child css 문법이기 때문에
  //변수를 넣을려면 문자 보간 하여 기입 = #{$i}
  .through:nth-child(#{$i}) {
    width : 20px * $i
  }
}
```

## 함수(Functions)
함수와 Mixins은 거의 유사하지만 `반환되는 내용이 다름`.

Mixin은 위에서 살펴본 대로 지정한 스타일(Style)을 반환하는 반면,  
함수는 보통 연산된(Computed) 특정 값을 `@return` 지시어를 통해 반환
```scss
// Functions
@function 함수이름($매개변수) {
  @return 값
}

//함수는 함수이름으로 바로 사용합니다.
함수이름(인수)

//ex
@function columns($number:2,$columns:12)
{
  @return ($number / $columns)
}
.box_group {
  .box1{
    width:columns(8)
  }
}
```

## 색상 내장 함수
- mix($color1, $color2) : 두 개의 색을 섞습니다.
- lighten($color, $amount) : 더 밝은색을 만듭니다.
- `darken`($color, $amount) : 더 어두운색을 만듭니다.
- saturate($color, $amount) : 색상의 채도를 올립니다.
- desaturate($color, $amount) : 색상의 채도를 낮춥니다.
- grayscale($color) : 색상을 회색으로 변환합니다.
- invert($color) : 색상을 반전시킵니다.
- `rgba`($color, $alpha) : 색상의 투명도를 변경합니다.
- opacify($color, $amount) / fade-in($color, $amount) : 색상을 더 불투명하게 만듭니다.
- transparentize($color, $amount) / fade-out($color, $amount) : 색상을 더 투명하게 만듭니다.

## 가져오기(Import)

@import로 외부에서 가져온 Sass 파일은 모두 단일 CSS 출력 파일로 병합됩니다.  
또한, 가져온 파일에 정의된 모든 변수 또는 Mixins 등을 주 파일에서 사용할 수 있습니다.
```scss
//import 시 확장자 생략 가능 다중 import 가능 ,(쉼표)로 구분
@import "./sub","./sub2";
```

## 오버워치 캐릭터 선택 차 scss 리팩토링
리팩토링 : 결과의 변경 없이 코드의 구조를 재조정함.  
<a href="https://github.com/backSeungWook/Part6/blob/master/overwatch/main.scss/"> overwatch/main.scss </a>
