## 1.Vue.js란?

> Vue(/vjuː/ 로 발음, view 와 발음이 같습니다.).js 는 **프론트엔드 자바스크립트 프레임워크**입니다.
>
> 진입 장벽이 낮고, 유연하고, 성능이 우수하고, 유지 보수와 테스팅이 편리한 자바스크립트 프레임워크입니다. 뷰는 또한 점진적인 프레임워크를 지향합니다. 이는 웹 애플리케이션 전체를 프레임워크로 구조화하지 않아도 웹 애플리케이션의 작은 부분에만 적용하여 특정 화면에 더 나은 사용자 경험을 제공할 수 있는 걸 의미해요.
>
> Vue.js 의 경우엔 CDN 으로 불러와서 사용하기도 매우 간편하고, 원한다면 webpack 으로도 구성을 할 수 있습니다. 이 프레임워크의 엄청난 장점 중 하나는, 매뉴얼이 타 프레임워크 / 라이브러리에 비해서 **한글화**가 엄청 잘 되어있습니다. 정리도 아주 잘 되어있구요. 따라서 한국 개발자들이 새로 시작하기에 참 좋은 환경이라고 생각합니다. 



- [Vue.js 한국 사용자 모임](https://vuejs-kr.github.io/)


- [한국어가이드](https://kr.vuejs.org/v2/guide/installation.html) 
- [한국어 번역 프로젝트 목록](https://vuejs-kr.github.io/translated-in-korean/)
- [Vue.js API](https://kr.vuejs.org/v2/api/#search-form) 


### 호환성 정보

Vue는 ECMAScript 5 기능을 사용하기 때문에 IE8 이하 버전을 **지원하지 않습니다.** 

하지만 모든 [ECMAScript 5 호환 브라우저](http://caniuse.com/#feat=es5)를 지원합니다



### 릴리즈 노트

최신 안정 버전: 2.5.13

각 버전에 대한 자세한 릴리즈 노트는 [GitHub](https://github.com/vuejs/vue/releases)에서 보실 수 있습니다.



## 2.1 React 와의 비교

### **2.1.1 공통점**

- 가상 DOM 을 사용합니다
- 컴포넌트를 제공합니다
- 뷰에만 집중을 하고 있고, 라우터, 상태관리를 위해선 써드파티 라이브러리를 사용합니다.

### **2.1.2 성능의 차이**

[메뉴얼](https://kr.vuejs.org/v2/guide/comparison.html#%EC%84%B1%EB%8A%A5-%EB%B6%84%EC%84%9D)에 따르면 모든 시나리오에서 Vue 가 React 보다 우수한 성능을 발휘한다고 합니다.  10000 개의 컴포넌트를 100 번 렌더링하는 React 프로젝트와 Vue 프로젝트를 비교 했을 때 다음과 같은 결과가 나왔다고 하네요.

![성능비교](https://velopert.com/wp-content/uploads/2017/01/Screenshot-from-2017-01-21-13-52-46.png)

추가적으로, 리액트에서는 불필요한 업데이트를 방지할때는 `shouldComponentUpdate` 라는 메소드를 통해서 최적화를 하죠. Vue 에서는 컴포넌트의 종속성이 렌더링 중 자동으로 추적되어 시스템에서 다시 렌더링 해야하는 컴포넌트를 정확히 알고 있기 때문에 이 작업이 불필요 합니다.

그래서, Vue 프로젝트와 React 프로젝트가 최적화 되지 않았을때는, Vue 가 훨씬 더 빠르고, 만약에 최적화를 했다 하더라도 Vue가 React 보다 빠르다고 하네요.

애니메이션 부분에서도 좀 복잡한 애니메이션을 구현 할 때, Vue 에서 초당 10 프레임으로 진행이 될 때, React는 1프레임 정도로 진행이 된다고하네요 (이 부분은 개발모드일때만 해당이 되는 것 같습니다. 경고/오류 메시지를 처리하는 것 때문에 느려집니다)



### **2.1.3 JSX vs Template**

React 에서는 JSX 를 사용하죠? Vue 에선 템플릿을 사용합니다.

```vue
<template>
  <div class="list-container">
    <ul v-if="items.length">
      <li v-for="item in items">
        {{ item.name }}
      </li>
    </ul>
    <p v-else>No items found.</p>
  </div>
</template>
```

Vue 에서도 원한다면 [JSX 를 사용 할 수 있구요](https://kr.vuejs.org/v2/guide/render-function.html#JSX), 템플릿을 사용 할 때의 장점은 HTML 파일에서 바로 사용 할 수 있다는 점 입니다. 따라서 다른 HTML 템플릿에서도 사용 할 수 있죠 (예: Pug (이전 Jade), EJS, 장고템플릿 등..)



### **2.1.4 서버사이드 렌더링**(SSR)

Vue 에서도 [서버사이드 렌더링](https://kr.vuejs.org/v2/guide/ssr.html)이 지원됩니다. 리액트에 비하여 훨씬 개선되어있습니다. 그 이유는 스트리밍 서버사이드 렌더링이 지원 되기 때문인데요, 리액트의 서버사이드 렌더링의 문제점은 synchronous 하여 서버 렌더링 시 렌더링 코드가 진행되는 동안 이벤트 루프가 막히게 됩니다 (이 부분은 써드 파티 라이브러리를 사용하여 tweak 하여 사용하는 방법이 존재하긴 합니다) 이러한 서버사이드 렌더링은 만약에 서버의 사용률이 높을 경우 성능을 악화시키고 response 시간이 늘어나기도 하죠.

Vue 에서는 스트리밍 서버 사이드 렌더링이 지원되어 이벤트 루프가 막히지 않습니다. 따라서 유저들에게 더 빠르게 결과를 반환 할 수 있죠.



#### 서버사이드 렌더링(SSR)이란 무엇입니까?

Vue.js는 클라이언트 측 애플리케이션을 위한 프레임워크입니다. 기본적으로 Vue 컴포넌트는 브라우저에서 DOM을 생성 및 조작 후 출력합니다. 그러나 동일한 컴포넌트를 서버의 HTML 문자열로 렌더링한 후 직접 브라우저로 보내고 마지막으로 정적 마크업을 클라이언트의 상호작용하는 애플리케이션으로 "hydrate" 하는 것도 가능합니다.

서버에서 렌더링된 Vue.js 앱은 코드 대부분이 서버와 클라이언트 모두에서 실행하는 점에서 "같은 형태" **이며** "범용적"으로 여겨질 수 있습니다.



#### 왜 SSR을 사용하나요?

전통적인 SPA(싱글 페이지 애플리케이션)에 비해 SSR의 장점은 주로 아래에 있는 내용과 같습니다.

- 검색 엔진 크롤러는 완전히 렌더링 된 페이지를 직접 볼 수 있으므로 검색 엔진 최적화가 개선됩니다.

  현재 Google과 Bing은 동기식 자바스크립트 애플리케이션의 인덱싱을 할 수 있습니다. 여기서 동기식이 핵심 단어입니다. 앱이 로딩 스피너로 시작한 다음 Ajax를 이용해 컨텐츠를 가져오는 경우 크롤러가 완료될 때까지 기다리지 않습니다. <u>즉, SEO가 중요한 페이지에서 비동기적으로 콘텐츠를 가져오는 경우 SSR이 필요합니다.</u>

  <u>(이렇게 클라이언트 렌더링만 될 경우엔 검색엔진 크롤러가 여러분의 어플리케이션이 지닌 데이터들을 제대로 수집하지 못합니다. 하지만 그렇다고 해서 너무 걱정하지는 마세요. 검색엔진중 가장 중요한 구글 검색엔진은 크롤러에 자바스크립트 엔진이 내장되어 있어서 우리가 별도로 작업을 하지 않아도, 우리가 준비한 데이터를 제대로 렌더링해줍니다.</u>

  <u>하지만, 네이버, 다음 등의 검색엔진이 사이트를 제대로 크롤링 하게 지원해야한다면, 서버사이드 렌더링을 따로 구현하셔야 합니다</u>.)

- 컨텐츠 도달 시간 단축, 특히 느린 인터넷 또는 느린 장치의 경우에 서버 렌더링 마크업은 모든 자바 스크립트가 다운로드되어 실행될 때까지 기다릴 필요가 없으므로 사용자는 완전히 렌더링 된 페이지를 더 빨리 볼 수 있습니다. 일반적으로 사용자 경험이 향상되고 전환율과 직접적인 관련성이있는 애플리케이션의 경우 중요해질 수 있습니다.

하지만 SSR을 사용할 때 고려해야할 몇가지 단점이 있습니다.

- 개발 제약 사항이 있습니다. 브라우저의 특정 코드는 특정한 라이프사이클에서만 사용할 수 있습니다. 일부 외부 라이브러리는 서버에서 렌더링 된 애플리케이션에서 실행할 수 있도록 추가적인 처리가 필요할 수 있습니다.
- 설치 및 배포 요구사항을 보다 복잡하게 만들 수 있습니다. 정적 파일 서버에 배포할 수 있는 완전히 정적인 SPA와 달리 서버 사이드 렌더링 애플리케이션에서는 Node.js 서버를 실행할 수 있는 환경이 필요합니다.
- 더 많은 서버측 부하가 생깁니다. Node.js의 전체 애플리케이션 렌더링은 정적 파일을 제공하는 것 보다 CPU를 많이 사용하므로 트래픽이 많을 것으로 예상되면 서버 부하에 대비하고 캐싱 전략을 잘 짜야합니다.

앱에 SSR을 사용하기 전에 먼저 실제 필요한지 고민해야합니다. 보통 앱의 컨텐츠가 얼마나 빨리 도달해야 하는지에 달려 있습니다. 예를 들어, 초기 로드 시 추가적인 수백 밀리 초가 그다지 중요하지 않은 내부 대시 보드를 구축하는 경우 SSR은 불필요합니다. 그러나 시간에 따른 컨텐츠가 절대적으로 중요한 경우 SSR을 사용하면 최상의 초기 로드 성능을 얻을 수 있습니다.

[참고 : 클라이언트 사이드 렌더링 VS 서버 사이드 렌더링](http://asfirstalways.tistory.com/244) 

[Angular 2 대신에 Vue.js를 선택한 이유 (그리고 React를 선택하지 않은 이유)](https://joshua1988.github.io/web-development/translation/why-we-moved-from-angular2-to-vuejs/)

[다른프레임워크와비교](https://kr.vuejs.org/v2/guide/comparison.html)



## 3. Vue.js 시작하기

자, 이제 Vue.JS 를 프로젝트에 불러올 것입니다. 정말 간편한 방법으로는 상단의 Add Library 버튼을 눌러서 Vue.js 2.x 버전을 찾아서 추가를 하셔도 됩니다. 하지만, Vue는 업데이트가 꽤 잦게 일어나는 편이고, JSBin 에서 바로바로 반영이 되지 않기 때문에, Vue.js 메뉴얼에서 최신 버전의 CDN 주소를 불러오는게 더 좋습니다.

메뉴얼의 [설치방법](https://kr.vuejs.org/v2/guide/installation.html#CDN) 을 보시면 CDN 주소가 제공되는데 거기에있는 링크 중 그 중 unpkg 에서 제공하는 링크를 사용하면 됩니다. 다음 링크를 사용하면 그때 그때 최신 버전으로 불러와줍니다.

[https://unpkg.com/vue/dist/vue.js](https://unpkg.com/vue/dist/vue.js)

[예제](https://jsbin.com/fivomus/edit?html,js,output)



### 3.1. 선언적 렌더링

```vue
<div id="app">
  {{ message }}
</div>
```

```javascript
var app = new Vue({
  el: '#app',
  data: {
    message: '안녕하세요 Vue!'
  }
})
```

`결과 ` 안녕하세요 Vue!

우리는 이미 첫 Vue 앱을 만들었습니다! 문자열 템플릿을 렌더링하는 것과 매우 유사하지만 사실 Vue는 더 많은 작업을 합니다. 데이터와 DOM이 연결되어 이제 모든 것이 **반응형**입니다. 어떻게 알 수 있을까요? 브라우저의 JavaScript 콘솔을 열고 `app.message`를 다른 값으로 설정해 보십시오. 그에 따라 렌더링 된 예제를 볼 수 있습니다.

### 3.2. 디렉티브

디렉티브는 `v-` 접두사가 있는 특수 속성입니다. 디렉티브 속성 값은 **단일 JavaScript 표현식** 이 됩니다. (나중에 설명할 `v-for`는 예외입니다.) 디렉티브의 역할은 표현식의 값이 변경될 때 사이드이펙트를 반응적으로 DOM에 적용하는 것 입니다.  

```html
<div id="app-2">
  <span v-bind:title="message">
    내 위에 잠시 마우스를 올리면 동적으로 바인딩 된 title을 볼 수 있습니다!
  </span>
</div>
```

```javascript
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: '이 페이지는 ' + new Date() + ' 에 로드 되었습니다'
  }
})
```

내 위에 마우스를 올리면 동적으로 바인딩 된 title을 볼 수 있습니다!



### 3.3. Vue 인스턴스

<u>모든 Vue 앱은 `Vue` 함수로 새 **Vue 인스턴스**를 만드는 것부터 시작합니다.</u>

```javascript
var vm = new Vue({
  // 옵션
})

---------------------------------------
    
var vm = new Vue({
  template: ...,
  el: ...,
  methods: {

  },
  // ...
})
```

엄격히 [MVVM 패턴](https://en.wikipedia.org/wiki/Model_View_ViewModel)과 관련이 없지만 Vue의 디자인은 부분적으로 그것에 영감을 받았습니다. 컨벤션으로, Vue 인스턴스를 참조하기 위해 종종 변수 `vm`(ViewModel의 약자)을 사용합니다.

Vue 인스턴스를 인스턴스화 할 때는 데이터, 템플릿, 마운트할 엘리먼트, 메소드, 라이프사이클 콜백 등의 옵션을 포함 할 수있는 **options 객체**를 전달 해야합니다. 전체 옵션 목록은 [API reference](https://kr.vuejs.org/v2/api)에서 찾을 수 있습니다.

`Vue` 생성자는 미리 정의 된 옵션으로 재사용 가능한 **컴포넌트 생성자**를 생성하도록 확장 될 수 있습니다
Vue 앱은 `new Vue`를 통해 만들어진 `루트 Vue 인스턴스`로 구성되며 선택적으로 중첩이 가능하고 재사용 가능한 컴포넌트 트리로 구성됩니다. 

확장된 인스턴스를 만들수는 있으나 대개 템플릿에서 사용자 지정 엘리먼트로 선언적으로 작성하는 것이 좋습니다. 나중에 [컴포넌트 시스템](https://kr.vuejs.org/v2/guide/components.html)에 대해 자세히 설명합니다. 지금은 <u>모든 Vue 컴포넌트가 본질적으로 확장된 Vue 인스턴스</u>라는 것을 알아야 합니다.



### 3.4. 속성과 메소드

 Vue 인스턴스는 `data` 객체에 있는 모든 속성을 **프록시** 처리 합니다.

```javascript
// 데이터 객체
var data = { a: 1 }

// Vue인스턴스에 데이터 객체를 추가합니다.
var vm = new Vue({
  data: data
})

// 같은 객체를 참조합니다!
vm.a === data.a // => true

// 속성 설정은 원본 데이터에도 영향을 미칩니다.
vm.a = 2
data.a // => 2

// ... 당연하게도
data.a = 3
vm.a // => 3

```

데이터가 변경되면 화면은 다시 렌더링됩니다. 유념할 점은, `data`에 있는 속성들은 인스턴스가 생성될 때 존재한 것들만 **반응형**이라는 것입니다. 즉, 다음과 같이 새 속성을 추가하면:

```javascript
vm.b = 'hi'
```

`b`가 변경되어도 화면이 갱신되지 않습니다. 어떤 속성이 나중에 필요하다는 것을 알고 있으며, 빈 값이거나 존재하지 않은 상태로 시작한다면 아래와 같이 초기값을 지정할 필요가 있습니다.

```javascript
data: {
  newTodoText: '',
  visitCount: 0,
  hideCompletedTodos: false,
  todos: [],
  error: null
}
```

여기에서 유일한 예외는 `Object.freeze ()`를 사용하는 경우입니다. 이는 기존 속성이 변경되는 것을 막아 반응성 시스템이 추적할 수 없다는 것을 의미합니다.

```javascript
var obj = {
  foo: 'bar'
}

Object.freeze(obj)

new Vue({
  el: '#app',
  data () {
    return {
      obj
    }
  }
})

```

```html
<div id="app">
  <p>{{ obj.foo }}</p>
  <!-- obj.foo는 더이상 변하지 않습니다! -->
  <button @click="obj.foo = 'baz'">Change it</button>
</div>
```

Vue 인스턴스는 데이터 속성 이외에도 유용한 인스턴스 속성 및 메소드를 제공합니다. 다른 사용자 정의 속성과 구분하기 위해 `$` 접두어를 붙였습니다.

예:

```javascript
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch 는 인스턴스 메소드 입니다.
vm.$watch('a', function (newVal, oldVal) {
  // `vm.a`가 변경되면 호출 됩니다.
})
```

나중에 [API reference](https://kr.vuejs.org/v2/api/#Instance-Properties)에서 인스턴스 속성과 메소드 전체 목록을 살펴보세요.

[예제 : data인스턴스](https://kr.vuejs.org/v2/api/#data)



## 4.예제

뷰를 아직 사용 안 해보신 분들을 위해 뷰로 코딩하는 느낌이 어떤 건지 보여드리려고 합니다. 그리고 문법도 간단히 한번 살펴볼께요. 정말 자세한 내용은 다루지 않을 겁니다. 다만, 뷰의 주요 컨셉이 어떤 건지 알아볼게요.

대부분의 자바스크립트처럼 우리도 페이지에 데이터를 표시하는 것으로 시작해보겠습니다. 

```html
<div id="app">
  <h2>
    X are in stock.
  </h2>
</div>

<script>
  var product = "Boots"
</script>
```

![데이터 집어넣기](https://joshua1988.github.io/images/posts/web/translation/why-43percent-devs-wanna-learn-vuejs/4_insert-data-onpage.gif)

위 그림처럼 뷰로 ‘Boots’라는 데이터를 ‘X’에 넣고 싶으면 아래와 같이 구현합니다.

```html
<div id="app">
  <h2>
    {{ product }} is in stock.
  </h2>
</div>
<script src="https://unpkg.com/vue"></script>
<script>
  var app = new Vue({
    el:"#app",
    data:{
      product : "Boots"
    }
  })
</script>
```

 ![vue.js 인스턴스로 표현하기](https://joshua1988.github.io/images/posts/web/translation/why-43percent-devs-wanna-learn-vuejs/5_vue-instance.gif)



위에서 보시는 것처럼 뷰 라이브러리를 불러와서 뷰 인스턴스를 생성하고, ‘app’이라는 화면 요소에 연결하였습니다. 여기서 `el`은 인스턴스가 뿌려질 화면 요소를 의미합니다. 그리고 `data` 안에 표시하고 싶은 값을 정의하여 화면에 }}로 연결했죠.

위 코드를 동작시키면 아래와 같이 나옵니다. ![vue.js 인스턴스 결과화면](https://joshua1988.github.io/images/posts/web/translation/why-43percent-devs-wanna-learn-vuejs/6_first-result.gif)

여기서 특별한 건 없어요. 다만 데이터가 변할 때 뷰의 마법이 시작됩니다. 제가 이제 개발자 도구의 콘솔 창으로 가서 product의 값을 변경해볼게요. 어떤 일이 일어나는지 보세요. 

![콘솔에서 데이터 변경](https://joshua1988.github.io/images/posts/web/translation/why-43percent-devs-wanna-learn-vuejs/7_first-result-with-console.gif)





뷰는 리액티브(Reactive)합니다. 이 말은 웹 페이지 상에 표시된 데이터가 변할 때 뷰에서 다 알아서 그 변경을 처리하는 것을 의미합니다. 이 동작은 문자열뿐만 아니라 모든 유형의 데이터에 모두 적용됩니다. 어디 한번 문자열 대신에 배열을 넣어볼까요? 그리고 HTML은 아이템 목록을 나타낼 수 있게 ul 태그로 바꿨습니다. product의 개수만큼 li 태그를 생성하려면 v-for라는 특별한 속성을 사용합니다. 이렇게 하면 데이터의 개수만큼 li 태그가 찍혀져 나오죠. 

```html
 <div id="app">
  <ul>
    <li v-for="product in products">
    {{ product }}
    </li>
  </ul>
</div>

<script src="https://unpkg.com/vue"></script>
<script>
  var app = new Vue({
    el:'#app',
    data:{
      products:[
      'Boots',
      'Jacket',
      'Hiking Socks'
      ]
    }
  })
</script>
```



![v-for 디렉티브](https://joshua1988.github.io/images/posts/web/translation/why-43percent-devs-wanna-learn-vuejs/8_v-for-directive-usage.png)

이제 브라우저로 가서 코드를 실행하면 아래와 같습니다. ![v-for 디렉티브 결과](https://joshua1988.github.io/images/posts/web/translation/why-43percent-devs-wanna-learn-vuejs/9_v-for-directive-result.gif)

아직 코드가 살짝 부자연스럽네요. 어디 빈 배열로 시작해서 데이터를 불러와 담아볼까요? 불러올 데이터는 데이터베이스에서 가져온다고 가정합시다. 

```html
<script>
  var app = new Vue({
    el:'#app',
    data:{
      products:[]
    },
    created(){
      fetch('https://api.myjson.com/bins/74l63')
      .then(response => response.json())
        .then(json => {
        this.products = json.products
      })
    }
  })
</script>
```

![API로 가져온 데이터 목록 표시하기](https://joshua1988.github.io/images/posts/web/translation/why-43percent-devs-wanna-learn-vuejs/10_v-for-directive-with-apis.gif)

위 코드가 실행된 결과는 아래와 같습니다. ![vue.js 결과 화면](https://joshua1988.github.io/images/posts/web/translation/why-43percent-devs-wanna-learn-vuejs/11_v-for-directive-with-apis-result.gif)

보시는 것처럼 현재 목록의 각 아이템은 데이터를 받아온 객체를 표시합니다. 좀 더 사용자가 보기 편하게 데이터 표현 방식을 아래와 같이 바꿔봅니다. 

```html
<div id="app">
	<ul>
    	<li v-for="product in products">
      	{{product.quantity}} {{product.name}} 	
      </li>  
  	</ul>
</div>
```

![vue.js의 데이터 표현방식](https://joshua1988.github.io/images/posts/web/translation/why-43percent-devs-wanna-learn-vuejs/12_v-for-expression.gif)h

그럼 결과는.. ![vue.js 결과화면](https://joshua1988.github.io/images/posts/web/translation/why-43percent-devs-wanna-learn-vuejs/13_v-for-exp-result.gif)

각 아이템 중에 혹시 개수(quantity)가 0인게 있으면 사용자가 인지할 수 있도록 조금 다르게 표시해볼까요? span 태그로 `item.quantity === 0` 일 때만 OUT OF STOCK 텍스트가 나타나게 하겠습니다. 여기서 이 조건을 위해 v-if 디렉티브를 사용합니다. 

```html
<div id="app">
	<ul>
    	<li v-for="product in products">
      	{{product.quantity}} {{product.name}} 	
          <span v-if="product.quantity === 0">
          	- OUT OF STOCK
          </span>
      </li>  
  	</ul>
</div>
```

![v-if 디렉티브 사용](https://joshua1988.github.io/images/posts/web/translation/why-43percent-devs-wanna-learn-vuejs/14_v-for-another.gif)

아이템 중에 jacket의 재고가 다 떨어졌었네요. 아래와 같이 말이죠. ![v-if 디렉티브 vue.js 결과화면](https://joshua1988.github.io/images/posts/web/translation/why-43percent-devs-wanna-learn-vuejs/15_v-if-directive-result.gif)

만약 모든 상품(product)의 총 재고량을 목록 아래에 표시하려면 어떻게 해야 할까요? totalProducts라는 computed 속성을 활용하면 됩니다. 만약 자바스크립트 reduce() API가 익숙하지 않으시다면 여기서는 그냥 각 상품의 재고의 총합을 구하는 동작이라고 생각하세요. 

```html
<div id="app">
	<ul>
    	<li v-for="product in products">
      	{{product.quantity}} {{product.name}} 	
          <span v-if="product.quantity === 0">
          	- OUT OF STOCK
          </span>
      </li>  
  	</ul>
  <h2>
    TOtal Inventory : {{ totalProducts }}
  </h2>
</div>

<script>
var app = new Vue({
    el:'#app',
    data:{
      products:[]
    },
   computed :{
    totalProducts(){
      return this.products.reduce((sum,product) => {
        return sum + product.quantity
      },0)
    }
  },
  created(){
      fetch('https://api.myjson.com/bins/74l63')
      .then(response => response.json())
        .then(json => {
        this.products = json.products
      })
    }
  })
</script>
```

![vue.js의 computed 속성](https://joshua1988.github.io/images/posts/web/translation/why-43percent-devs-wanna-learn-vuejs/16_computed-prop.gif)

아래에서 볼 수 있듯이 이제 모든 상품의 총 재고량이 표시됩니다. ![vue.js의 computed 속성 결과 화면](https://joshua1988.github.io/images/posts/web/translation/why-43percent-devs-wanna-learn-vuejs/17_computed-prop-result.gif)

자 이제 뷰 개발자 도구(Vue.js Chrome Extension)을 알아보기 좋은 시간이네요. 개발자 도구의 장점 중 하나는 페이지 상에 표시된 데이터를 살펴볼 수 있다는 점입니다. ![vue.js 개발자 도구](https://joshua1988.github.io/images/posts/web/translation/why-43percent-devs-wanna-learn-vuejs/18_vue-devtool.gif)

뷰의 반응성(Reactivity: 데이터가 변함에 따라 뷰에서 반사적으로 화면을 변화시키는 특성)을 다시 살펴볼까요. 아이템 2개를 제거해보겠습니다. 아래 보시는 것처럼 상품 목록뿐만 아니라 남은 상품의 총 재고량도 바뀌네요.

![vue.js 반응성 reactivity](https://joshua1988.github.io/images/posts/web/translation/why-43percent-devs-wanna-learn-vuejs/19_vue-reactivity.gif)

다음으로, 버튼을 이용해서 페이지에 이벤트를 추가해보겠습니다. 각 상품 아이템에 Add라는 버튼을 추가합니다. 그리고 버튼을 클릭했을 때 각 상품의 재고량을 1개씩 늘리겠습니다.

 

```html
<div id="app">
	<ul>
    	<li v-for="product in products">
      	{{product.quantity}} {{product.name}} 	
          <span v-if="product.quantity === 0">
          	- OUT OF STOCK
          </span>
          <button @click="product.quantity += 1">
            Add
          </button>
      </li>  
  	</ul>
  <h2>
    TOtal Inventory : {{ totalProducts }}
  </h2>
</div>

<script>
var app = new Vue({
    el:'#app',
    data:{
      products:[]
    },
   computed :{
    totalProducts(){
      return this.products.reduce((sum,product) => {
        return sum + product.quantity
      },0)
    }
  },
  created(){
      fetch('https://api.myjson.com/bins/74l63')
      .then(response => response.json())
        .then(json => {
        this.products = json.products
      })
    }
  })
</script>
```

![vue.js 버튼 클릭 이벤트 v-on](https://joshua1988.github.io/images/posts/web/translation/why-43percent-devs-wanna-learn-vuejs/20_v-on-click.gif)

아래를 보시면 각 상품의 Add 버튼을 클릭했을 때 각 상품의 재고와 총 재고량 숫자가 올라갑니다. 그리고 Jacket 상품의 Add 버튼을 클릭하면 OUT OF STOCK이라는 글씨도 사라지네요. ![vue.js v-on 디렉티브 결과 화면](https://joshua1988.github.io/images/posts/web/translation/why-43percent-devs-wanna-learn-vuejs/21_v-on-click-result.gif)

허나 여기서 만약 각 상품의 재고량을 그냥 수기로 입력하고 싶으면 어떻게 해야 할까요? 인풋 박스를 하나 만들고 v-model 디렉티브를 연결해봅니다. 그리고 입력되는 값은 항상 숫자라고 지정할게요.

 

```html
<div id="app">
	<ul>
    	<li v-for="product in products">
          <input type="number" v-model.number="product.quantity">
      	{{product.quantity}} {{product.name}} 	
          <span v-if="product.quantity === 0">
          	- OUT OF STOCK
          </span>
          <button @click="product.quantity += 1">
            Add
          </button>
      </li>  
  	</ul>
  <h2>
    TOtal Inventory : {{ totalProducts }}
  </h2>
</div>

<script>
var app = new Vue({
    el:'#app',
    data:{
      products:[]
    },
   computed :{
    totalProducts(){
      return this.products.reduce((sum,product) => {
        return sum + product.quantity
      },0)
    }
  },
  created(){
      fetch('https://api.myjson.com/bins/74l63')
      .then(response => response.json())
        .then(json => {
        this.products = json.products
      })
    }
  })
</script>
```

![vue.js v-model 디렉티브](https://joshua1988.github.io/images/posts/web/translation/why-43percent-devs-wanna-learn-vuejs/22_v-model.gif)

이제 모든 아이템의 재고 숫자를 직접 입력하여 변경할 수 있습니다. 만약 0을 입력하면 자연스럽게 OUT OF STOCK이 함께 표시되네요. 그리고 아까 추가했던 Add 버튼도 정상적으로 동작합니다. ![vue.js v-model 디렉티브 결과 화면](https://joshua1988.github.io/images/posts/web/translation/why-43percent-devs-wanna-learn-vuejs/23_v-model-result.gif)

여기까지 살펴본 코드는 [JSFIDDLE](https://jsfiddle.net/greggpollack/gr1cs2tv/)에서 살펴볼 수 있습니다.

[참고 주소](https://joshua1988.github.io/web-development/translation/why-43percent-devs-wanna-learn-vuejs/)
