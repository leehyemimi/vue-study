## 1. 컴포넌트등록

### 전역등록

Vue는 사용자 지정 태그 이름에 대해 [W3C 규칙](http://www.w3.org/TR/custom-elements/#concepts)을 적용하지 않습니다 

**모두 소문자이어야 하고 하이픈을 포함해야합니다**

```html
<div id="app">
  <my-compoent></my-compoent>
</div>

<script>
	Vue.component('my-compoent',{
      template : '<div>전역컴포넌트</div>'
    })

    new Vue({
        el : '#app'
    })
</script>
```

[예제](https://jsfiddle.net/leemimi/eywraw8t/90200/)

### 지역등록

모든 컴포넌트를 전역으로 등록 할 필요는 없습니다. 컴포넌트를 `components` 인스턴스 옵션으로 등록함으로써 다른 인스턴스/컴포넌트의 범위에서만 사용할 수있는 컴포넌트를 만들 수 있습니다:

```html
<div id="app">
  <my-compoent></my-compoent>
</div>

<script>
	var Child = {
        template : '<div>지역컴포넌트</div>'
    }

    new Vue({
        el : '#app',
      components :{
        'my-compoent' : Child
      }
    })
</script>
```

[예제](https://jsfiddle.net/leemimi/eywraw8t/90260/)



## 2. props

#### Props로 데이터 전달하기

모든 컴포넌트 인스턴스에는 자체 **격리 된 범위** 가 있습니다. 데이터는 [`props` 옵션](https://kr.vuejs.org/v2/api/#props)을 사용하여 하위 컴포넌트로 전달 될 수 있습니다.

```html
<div id="app">
  <child message="안녕하세요!"></child>
</div>

<script>
    Vue.component('child',{
        props : ['message'],
      template : '<span>{{ message }}</span>'
    })

    new Vue({
        el : '#app'
    })
</script>
```

[예제](https://jsfiddle.net/leemimi/eywraw8t/90288/)



#### ★★★ camelCase vs. kebab-case (대소문자) ->  중요!!!!!!

HTML 속성은 대소 문자를 구분하지 않으므로 문자열이 아닌 템플릿을 사용할 때 camelCased prop 이름에 해당하는 **kebab-case(하이픈 구분)**를 사용해야 합니다.

```html
<sript>
  Vue.component('child', {
    // JavaScript는 camelCase
    props: ['myMessage'],
    template: '<span>{{ myMessage }}</span>'
  })
</sript>

<!-- HTML는 kebab-case -->
<child my-message="안녕하세요!"></child>

```



#### 동적 Props

정규 속성을 표현식에 바인딩하는 것과 비슷하게, `v-bind`를 사용하여 부모의 데이터에 props를 동적으로 바인딩 할 수 있습니다. 데이터가 상위에서 업데이트 될 때마다 하위 데이터로도 전달됩니다.

[예제1](https://jsfiddle.net/leemimi/eywraw8t/90504/)



#### Prop 검증

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



### Props가 아닌 속성

Props가 아닌 속성은 컴포넌트로 전달되지만 해당 props는 정의되지 않은 속성입니다.

class나 style에만 적용됨 **( addClass사용할때 유용함)**

```html
<child my-message="hello!" data-date-picker="activated"></child>
<child my-message="hello!" class="formParent"></child>
```

[예제](https://jsfiddle.net/leemimi/eywraw8t/90804/)



## 3. 이벤트

### v-on을 이용한 사용자 지정 이벤트

모든 Vue 인스턴스는 다음과 같은 이벤트 인터페이스를 구현합니다.

```vue
$on(eventName)을 사용하여 이벤트를 감지 하십시오.
$emit(eventName)을 사용하여 이벤트를 트리거 하십시오.
```

```html
<div id="counter-event-example">
  <p>{{ total }}</p>
  <button-counter v-on:increment="incrementTotal"></button-counter>
  <button-counter v-on:increment="incrementTotal"></button-counter>
</div>
```

```javascript
Vue.component('button-counter', {
  template: '<button v-on:click="incrementCounter">{{ counter }}</button>',
  data: function () {
    return {
      counter: 0
    }
  },
  methods: {
    incrementCounter: function () {
      this.counter += 1
      this.$emit('increment')
    }
  },
})

new Vue({
  el: '#counter-event-example',
  data: {
    total: 0
  },
  methods: {
    incrementTotal: function () {
      this.total += 1
    }
  }
})
```

[예제](https://jsfiddle.net/leemimi/eywraw8t/90832/)



### 사용자 정의 이벤트를 사용하여 폼 입력 컴포넌트 만들기

사용자 정의 이벤트는 `v-model` 에서 작동하는 사용자 정의 입력을 만드는데에도 사용할 수 있습니다. 기억하세요.

```html
<input v-model="something">

<input
  v-bind:value="something"
  v-on:input="something = $event.target.value">
  
<custom-input
  :value="something"
  @input="value => { something = value }">
</custom-input>
```

따라서 `v-model`을 사용하는 컴포넌트는 (2.2.0버전 이상에서 설정을 조작할 수 있습니다.)

- `value` prop를 가집니다.
- 새로운 값으로 `input` 이벤트를 내보냅니다.



#### 컴포넌트의 v-model 사용자 정의

기본적으로 컴포넌트의 `v-model`은 `value`를 보조 변수로 사용하고 `input`을 이벤트로 사용하지만 체크 박스와 라디오 버튼과 같은 일부 입력 타입은 다른 목적으로 `value` 속성을 사용할 수 있습니다. `model` 옵션을 사용하면 다음 경우에 충돌을 피할 수 있습니다

```javascript
Vue.component('my-checkbox', {
  model: {
    prop: 'checked',
    event: 'change'
  },
  props: {
    // 다른 목적을 위해 `value` prop를 사용할 수 있습니다.
    checked: Boolean,
    value: String
  },
  // ...
})
```

```html
<my-checkbox v-model="foo" value="some value"></my-checkbox>
```

아래와 같습니다

```html
<my-checkbox
  :checked="foo"
  @change="val => { foo = val }"
  value="some value">
</my-checkbox>
```