# 뷰 템플릿

## 1. 뷰템플릿이란?

1. ES5에서 뷰 인스턴스의 template 속성을 활용하는 방법

```html
<script> 
  new Vue({
    template:'<p>hello {{ message }}</p>'
  })
</script>
```

2. 싱글파일컴포넌트 체계의 <template>코드를 활용하는 방법

```html
<!-- ES6: 싱글파일 컴포넌트 체계 -->
<template>
	<p>hello {{ message }}</p>
</template>
```



## 2. 뷰의 속성와 문법

### 데이터 바인딩

HTML 화면요소를 뷰 인스턴스의 데이터와 연결하는것을 의미

**{{ }} 문법, v-bind 속성**



#### {{ }} - 콧수영 괄호

데이터를 HTML태그에 연결하는 가장 기본적인 텍스트 삽입방식

* data 속성의 message값이 바뀌면 뷰 반응성에 의해 화면이 자동으로 갱신

```html
<div id="app">
  {{ message }}
</div>

<script>
  new Vue({
    el : '#app',
    data : {
      c
    }
  });
</script>
```

* 값을 바꾸고 싶지 않으면 **v-once속성 사용**

```html
<div id="app" v-once>
  {{ message }}
</div>

<script>
  new Vue({
    el : '#app',
    data : {
      message : 'hello Vue.js!'
    }
  });
</script>
```



#### v-bind

HTML 속성값에 뷰 데이터 값을 연결 할때 사용하는 데이터 연결 방식

[예제](https://jsfiddle.net/leemimi/eywraw8t/115385/)

```html
<div id="app">
  <p v-bind:id="idA">아이디 바인딩</p>
  <p v-bind:class="classA">클래스 바인딩</p>
  <p v-bind:style="styleA">스타일 바인딩</p>
</div>

<script>
  new Vue({
    el : '#app',
    data :{
      idA : 10,   
      classA : 'container',
      styleA : 'color:blue',
  	}
  })
</script>
```



### 자바스크립트 표현식

* 자바스크립트 표현식을 쓸수있음

[예제](https://jsfiddle.net/leemimi/eywraw8t/115404/)



* 자바스크립트 표현식에서 주의할점
  1. 선언문과 분기 구문은 사용할수 없음
  2. 복잡한 연산은 인스턴스 안에서 처리하고 화면에는 간단한 연산 결과만 표시

```html
<div id="app">
  {{ var a = 10 }} 
  {{ if(ture) { return 100} }}
  {{ true ? 100 : 0 }}
  
  {{ message.split('').reverse.join('') }}
  {{ reverseMessage }}
</div>
<script>
  new Vue({
    el : '#app',
    data : {
      messaga : 'hello vue.js!'
    },
    computed:{
      reverseMessage : function(){
        return this.message.split('').reverse.join('');
      }
    }
  })
</script>
```

[예제](https://jsfiddle.net/leemimi/eywraw8t/115471/)



### 디렉티브

HTML태그 안에서 v- 접두사를 가지는 모든 속성

```html
<a v-if="flag">두잇 두잇</a>
```

[예제](https://jsfiddle.net/leemimi/eywraw8t/115538/)

```html
<div id="app">
   <a href="#" v-if="flag">두잇</a>
   <ul>
     <li v-for="system in systems">{{ system }}</li>
   </ul>
   <p v-show="flag">두잇</p>
   <h5 v-bind:id="uid">뷰입문서</h5>
   <button v-on:click="popupAlert">경고창버튼</button>
</div>

<script>
	new Vue({
      el : '#app',
      data : {
        flag:true,
        systems :['a','b','c'],
        uid : 10
      },
      methods :{
        popupAlert : function(){
            return alert("경고창표시");
        }
      }
    })
</script>
```



### 이벤트 처리

이벤트를 처리하기 위해 v-on디렉티브와 methods 속성을 활용

```html
<div id="app">
	<button v-on:click="clickBtn(10)">클릭</button>  
</div>

<script>
  new Vue({
    el: "#app",
    methods:{
      clickBtn : function(nm){
        alert("clicked" + nm + 'times');
      }
    }
  })
</script>
```

[예제](https://jsfiddle.net/eywraw8t/117353/)



* event 인자를 이용해 돔 이벤트에 접근하기

```html
<div id="app">
	<button v-on:click="clickBtn">클릭</button>  
</div>

<script>
  new Vue({
    el: "#app",
    methods:{
      clickBtn : function(evnet){
        console.log(evnet);
      }
    }
  })
</script>
```

[예제](https://jsfiddle.net/eywraw8t/117357/)



### 고급템플릿 기법

#### computed 속성

데이터 연산들을 정의하는 영역

1. data 속성값의 변화에 따라 자동으로 다시 연산함
2. 캐싱

```html
<div id="app">
  {{ reversedMessage }}
</div>
<script>
  new Vue({
    el : "#app",
    data : {
      message : 'hello'
    },
    computed:{
      reversedMessage : function(){
        return this.message.split('').reverse().join('');
      }
    }
  })
</script>
```

[예제](https://jsfiddle.net/eywraw8t/117375/)



#### computed 속성과 methods 속성의 차이점

- methods 속성 : 호출할대만 해당 로직이 수행
- computed 속성 : 대상 데이터의 값이 변경되면 자동적으로 수행

```html
<div id="app">
  <p>{{ message }}</p>
  <button v-on:click="reversedMessage">문자열 역순</button>
</div>
<script>
  new Vue({
    el : "#app",
    data : {
      message : 'hello'
    },
    methods:{
      reversedMessage : function(){
       this.message = this.message.split('').reverse().join('');
       return this.message;
      }
    }
  })
</script>
```

복잡한 연산을 반복 수행해서 화면에 나타내야 한다면 computed 속성을 시용하는것이 methods 속성을 이용하는것보다 성능면에서 효율적



#### watch 속성

데이터 변화를 감지하여 자동으로 특정 로직을 수행

데이터 호출과 같이 시간이 상대적으로 더 많이 소모되는 비동기 처리에 적합

```html
<div id="app">
  <input v-model="message">
</div>
<script>
  new Vue({
    el : "#app",
    data : {
      message : 'hello'
    },
    watch:{
      message : function(data){
        console.log("message 가 바뀝니다. : " + data);
      }
    }
  })
</script>
```



### 예제 해보기

1. {{ }} 를 이용해 데이터 바인딩하기

   app.js 코드 주석 #1에 따라 data속성1개를 추가/ 추가한 data 속성을 {{ }}를 이용해 화면에 표시

2. v-bind 디렉티브를 이용해 데이터 바인딩하기

   app.js 코드 주석 #2에 따라 data속성 uid의 값을 10에서 20으로 변경한후 크롬 개발자 도구의 요소 검사 기능을 이용하여 <p>태그의 id 값이 어떻게 바뀌는지 확인하기

3. v-on 디렉티브를 이용해 클릭 이벤트 처리하기

   app.js 코드 주석 #3에 따라

4. v-if 디렉티브 조건에 따라 화면이 어떻게 바뀌는지 확인



```html
<div id="app">
  <header>
  	<h3>
      {{ message }},
      <!-- #1 새로 추가한 데이터 속성을 아래에 추가 -->
    </h3>
  </header>
  <section>
  	<!-- #2 -->
    <p v-bind:id="uid"></p>
    <button v-on:click="clickBtn">alert</button>
    <!-- 위 코드와 아래 코드는 동일한 역할을 수행 -->
   <!-- <button @click="clickBtn">alert</button>-->
    
    <!-- #3 button 태그 추가 -->
    
     <!-- #4 flag 속성값에 따라 어떻게 바뀌는지 확인 -->
    <ul>
      <li>1</li>
      <li>2</li>
      <li>3</li>
    </ul>
  </section>
  
  <script>
    var  app = new Vue({
      el : '#app',
      data :{
        message : 'hello', //#1
        uid : '10', //#2
        flag : true //#4
      },
      methods :{
        //ES6 문법
        clickBtn(){
          console.log('hi');
        }
        //ES5 문법
        //clickBtn function(){
        //  console.log('hi');
        //}
        
        //#3
      }
    })
  </script>
</div>
```

