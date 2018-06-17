전역등록



지역등록



### DOM 템플릿 구문 분석 경고

DOM을 템플릿으로 사용할 때 (예 : `el` 옵션을 사용하여 기존 콘텐츠가 있는 엘리먼트를 마운트하는 경우), Vue는 템플릿 콘텐츠만 가져올 수 있기 때문에 HTML이 작동하는 방식에 고유한 몇 가지 제한 사항이 적용됩니다. 이는 브라우저가 구문 분석과 정규화한 후에 작동합니다. 가장 중요한 것은`<ul>`,`<ol>`,`<table>`과`<select>`와 같은 일부 엘리먼트는 그 안에 어떤 엘리먼트가 나타날 수 있는지에 대한 제한을 가지고 있으며,`<option>`과 같이 특정 다른 엘리먼트 안에만 나타날 수 있습니다.

이러한 제한이 있는 엘리먼트가 있는 사용자 지정 컴포넌트를 사용하면 다음과 같은 문제가 발생할 수 있습니다.

```html
<table>
  <my-row>...</my-row>
</table>
```

사용자 지정 컴포넌트 `<my-row>` 는 잘못 된 컨텐츠가 되어, 결과적으로 렌더링시 에러를 발생시킵니다. 해결 방법은 `is` 특수 속성을 사용하는 것입니다 :

```html
<table>
  <tr is="my-row"></tr>
</table>
```

**다음 소스 중 하나에 포함되면 문자열 템플릿을 사용하는 경우에는 이러한 제한 사항이 적용되지 않습니다.**:

- `<script type="text/x-template">`
- JavaScript 인라인 템플릿 문자열
- `.vue` 컴포넌트

따라서 가능한 경우 항상 문자열 템플릿을 사용하는 것이 좋습니다.



### 컴포넌트 작성

Vue.js에서 부모-자식 컴포넌트 관계는 **props는 아래로, events 위로** 라고 요약 할 수 있습니다. 부모는 **props**를 통해 자식에게 데이터를 전달하고 자식은 **events**를 통해 부모에게 메시지를 보냅니다. 어떻게 작동하는지 보겠습니다. 



### Props

#### Props로 데이터 전달하기

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="example-2">
		  <child message="안녕하세요!"></child>
		</div>
		<script src="https://unpkg.com/vue/dist/vue.js"></script>
		<script>

			Vue.component('child', {
			  // props 정의
			  props: ['message'],
			  // 데이터와 마찬가지로 prop은 템플릿 내부에서 사용할 수 있으며
			  // vm의 this.message로 사용할 수 있습니다.
			  template: '<span>{{ message }}</span>'
			})

			new Vue({
				el: '#example-2'
			})

		</script>
	</body>
</html>

```

HTML 속성은 대소 문자를 구분하지 않으므로 문자열이 아닌 템플릿을 사용할 때 camelCased prop 이름에 해당하는 kebab-case(하이픈 구분)를 사용해야 합니다.

 

### 동적 props



### Prop 검증

컴포넌트가 받는 중인 prop에 대한 요구사항을 지정할 수 있습니다. 요구사항이 충족 되지 않으면 Vue에서 경고를 내보냅니다. 이 기능은 다른 사용자가 사용할 컴포넌트를 제작할 때 특히 유용합니다.

props를 문자열 배열로 정의하는 대신 유효성 검사 요구사항이 있는 객체를 사용할 수 있습니다.

```javascript
Vue.component('example', {
  props: {
    // 기본 타입 확인 (`null` 은 어떤 타입이든 가능하다는 뜻입니다)
    propA: Number,
    // 여러개의 가능한 타입
    propB: [String, Number],
    // 문자열이며 꼭 필요합니다
    propC: {
      type: String,
      required: true
    },
    // 숫자이며 기본 값을 가집니다
    propD: {
      type: Number,
      default: 100
    },
    // 객체/배열의 기본값은 팩토리 함수에서 반환 되어야 합니다.
    propE: {
      type: Object,
      default: function () {
        return { message: 'hello' }
      }
    },
    // 사용자 정의 유효성 검사 가능
    propF: {
      validator: function (value) {
        return value > 10
      }
    }
  }
})
```

`type`은 다음 네이티브 생성자 중 하나를 사용할 수 있습니다.

- String
- Number
- Boolean
- Function
- Object
- Array
- Symbol

또한, `type` 은 커스텀 생성자 함수가 될 수 있고, assertion은 `instanceof` 체크로 만들어 질 것입니다.

props 검증이 실패하면 Vue는 콘솔에서 경고를 출력합니다(개발 빌드를 사용하는 경우). props는 컴포넌트 인스턴스가 **생성되기 전**에 검증되기 때문에 `default` 또는 `validator` 함수 내에서 `data`, `computed` 또는 `methods`와 같은 인스턴스 속성을 사용할 수 없습니다.



### 사용자 정의 이벤트를 사용하여 폼 입력 컴포넌트 만들기

사용자 정의 이벤트는 `v-model` 에서 작동하는 사용자 정의 입력을 만드는데에도 사용할 수 있습니다. 기억하세요.

```
<input v-model="something">
```

위 문장은 아래와 같습니다.

```
<input
  v-bind:value="something"
  v-on:input="something = $event.target.value">
```

컴포넌트와 함께 사용하면 다음과 같이 간단해집니다.

```
<custom-input
  :value="something"
  @input="value => { something = value }">
</custom-input>
```

따라서 `v-model`을 사용하는 컴포넌트는 (2.2.0버전 이상에서 설정을 조작할 수 있습니다.)

- `value` prop를 가집니다.
- 새로운 값으로 `input` 이벤트를 내보냅니다.

매우 간단한 통화 입력을 사용하는 모습을 보겠습니다.

```
<currency-input v-model="price"></currency-input>
```



### 비 부모-자식간 통신

때로는 두 컴포넌트가 서로 통신 할 필요가 있지만 서로 부모/자식이 아닐 수도 있습니다. 간단한 시나리오에서는 비어있는 Vue 인스턴스를 중앙 이벤트 버스로 사용할 수 있습니다.

```
var bus = new Vue()
```

```
// 컴포넌트 A의 메소드
bus.$emit('id-selected', 1)
// 컴포넌트 B의 created 훅
bus.$on('id-selected', function (id) {
  // ...
})
```

보다 복잡한 경우에는 전용 [상태 관리 패턴](https://kr.vuejs.org/v2/guide/state-management.html)을 고려해야합니다



### 슬롯



### 동적 컴포넌트

같은 마운트 포인트를 사용하고 예약된 `<component>` 엘리먼트를 사용하여 여러 컴포넌트 간에 동적으로 트랜지션하고 `is` 속성에 동적으로 바인드 할 수 있습니다. 

```
var vm = new Vue({
  el: '#example',
  data: {
    currentView: 'home'
  },
  components: {
    home: { /* ... */ },
    posts: { /* ... */ },
    archive: { /* ... */ }
  }
})
```

```
<component v-bind:is="currentView">
  <!-- vm.currentView가 변경되면 컴포넌트가 변경됩니다! -->
</component>
```

원하는 경우 컴포넌트 객체에 직접 바인딩 할 수도 있습니다.

```
var Home = {
  template: '<p>Welcome home!</p>'
}

var vm = new Vue({
  el: '#example',
  data: {
    currentView: Home
  }
})
```



### 단방향 데이터 흐름

모든 props는 하위 속성과 상위 속성 사이의 **단방향** 바인딩을 형성합니다. 상위 속성이 업데이트되면 하위로 흐르게 되지만 그 반대는 안됩니다. 이렇게하면 하위 컴포넌트가 실수로 부모의 상태를 변경하여 앱의 데이터 흐름을 추론하기 더 어렵게 만드는 것을 **방지할 수** 있습니다.

일반적으로 prop을 변경시키고 싶은 유혹을 불러 일으킬 수있는 두 가지 경우가 있습니다.

1. 이 prop는 초기 값을 전달 하는데만 사용되며 하위 컴포넌트는 이후에 이를 로컬 데이터 속성으로 사용하기만 합니다.
2. prop는 변경되어야 할 원시 값으로 전달됩니다.

이러한 사용 사례에 대한 적절한 대답은 다음과 같습니다.

1. prop의 초기 값을 초기 값으로 사용하는 로컬 데이터 속성을 정의 하십시오.

   ```
   props: ['initialCounter'],
   data: function () {
     return { counter: this.initialCounter }
   }
   ```

2. prop 값으로 부터 계산된 속성을 정의 합니다.

   ```
   props: ['size'],
   computed: {
     normalizedSize: function () {
       return this.size.trim().toLowerCase()
     }
   }
   ```

자바 스크립트의 객체와 배열은 참조로 전달되므로 prop가 배열이나 객체인 경우 하위 객체 또는 배열 자체를 부모 상태로 변경하면 부모 상태에 **영향을 줍니다**.





### 재사용 가능한 컴포넌트 제작하기

컴포넌트를 작성할 때 나중에 다른 곳에서 다시 사용할 것인지에 대한 여부를 명심하는 것이 좋습니다. 일회용 컴포넌트가 단단히 결합 되어도 상관 없지만 재사용 가능한 컴포넌트는 깨끗한 공용 인터페이스를 정의 해야하며 사용된 컨텍스트에 대한 가정을 하지 않아야합니다.

Vue 컴포넌트의 API는 prop, 이벤트 및 슬롯의 세 부분으로 나뉩니다.

- **Props** 는 외부 환경이 데이터를 컴포넌트로 전달하도록 허용합니다.
- **이벤트**를 통해 컴포넌트가 외부 환경에서 사이드이펙트를 발생할 수 있도록 합니다.
- **슬롯** 을 사용하면 외부 환경에서 추가 컨텐츠가 포함 된 컴포넌트를 작성할 수 있습니다.

`v-bind` 와 `v-on` 을 위한 전용 약어문을 사용하여 의도를 명확하고 간결하게 템플릿에 전달할 수 있습니다.

```
<my-component
  :foo="baz"
  :bar="qux"
  @event-a="doThis"
  @event-b="doThat"
>
  <img slot="icon" src="...">
  <p slot="main-text">Hello!</p>
</my-component>
```









## 컴포넌트 props를 원시 자료형으로 사용하기

Vue.js는 다양한 속성을 통해 복잡한 JavaScript 객체를 전달할 수 있지만 **컴포넌트 옵션을 가능한 원시 자료형으로 유지하는 것이 좋습니다**. 즉, [JavaScript 원시 값](https://developer.mozilla.org/ko/docs/Glossary/Primitive) (string, number, boolean) 및 함수만을 사용하고 복잡한 Object는 피하는게 좋습니다.

### 왜 그렇게 하나요?

- 개별적으로 `prop` 속성을 사용함으로써 컴포넌트는 명확한 API를 가집니다.
- 원시 자료형과 함수만을 `props` 값으로 사용하면 컴포넌트 API가 네이티브 HTML(5) 엘리먼트의 API와 유사해집니다.
- 개별적으로 `prop` 속성을 사용하면 다른 개발자가 컴포넌트 인스턴스로 전달되는 내용을 쉽게 이해할 수 있습니다.
- 복잡한 Object를 전달할 때 전달한 Object의 속성과 메서드가 실제로 사용자 정의 컴포넌트에서 사용되는지 분명하지 않습니다. 이렇게 사용하면 코드를 리팩토링하기가 어려워지고 코드가 더러워 질 수 있습니다.

### 어떻게 하나요?

원시 자료형 혹은 함수를 사용하여 `props` 컴포넌트 속성을 사용하세요.

```
<!-- 권장합니다 -->
<range-slider
  :values="[10, 20]"
  min="0"
  max="100"
  step="5"
  :on-slide="updateInputs"
  :on-end="updateResults">
</range-slider>

<!-- 피하세요! -->
<range-slider :config="complexConfigObject"></range-slider>
```





## 컴포넌트 props를 잘 사용하기

Vue.js에서는 컴포넌트 props가 당신의 API 입니다. 강력하고 예측가능한 API를 만들면 다른 개발자가 컴포넌트를 쉽게 사용할 수 있습니다.

컴포넌트 props는 사용자 정의 HTML 어트리뷰트에 의해 전달됩니다. 이러한 어트리뷰트의 값은 Vue.js 일반 문자열(`:attr="value"` 또는 `v-bind:attr="value"`)이거나 누락됩니다. 당신은 이러한 경우를 고려하여 **당신의 컴포넌트 props를 잘 사용해야 합니다**.

### 왜 그렇게 하나요?

컴포넌트 props를 잘 사용하면 당신의 컴포넌트는 항상 작동할 것 입니다(방어적 프로그래밍). 나중에 다른 개발자가 당신이 생각하지 못한 방법으로 컴포넌트를 사용하더라도 마찬가지 입니다.

### 어떻게 하나요?

- props에 기본 값을 사용하세요.
- 값을 [validate](https://kr.vuejs.org/v2/guide/components.html#Prop-%EA%B2%80%EC%A6%9D) 하기위해 `type` 옵션을 사용하세요. **[1\*]**
- 중복된 `props`가 있는지 확인하세요.

```html
<template>
  <input type="range" v-model="value" :max="max" :min="min">
</template>
<script type="text/javascript">
  export default {
    props: {
      max: {
        type: Number, // [1*] 'max' prop은 Number로만 사용할 수 있습니다.
        default() { return 10; },
      },
      min: {
        type: Number,
        default() { return 0; },
      },
      value: {
        type: Number,
        default() { return 4; },
      },
    },
  };
</script>
```



[컴포넌트스타일가이드](http://vuejs.kr/jekyll/update/2017/03/13/vuejs-component-style-guide/#%EB%AA%A8%EB%93%88-%EA%B8%B0%EB%B0%98-%EA%B0%9C%EB%B0%9C)

[컴포넌트란?](https://kr.vuejs.org/v2/guide/components.html#%EB%8F%99%EC%A0%81-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8)