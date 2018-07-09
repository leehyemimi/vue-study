[Vue CLI 공식사이트](https://cli.vuejs.org/guide/)





### 웹팩(webpack)

[공식사이트](https://webpack.js.org/)

#### 웹팩이란? 

파일간의 연관관계를 파악하여 하나의 자바스크립트 파일로 변환해 주는 변환도구

- 애플리케이션 동작과 관련된 여러개의 파일(HTML,CSS,자바스크립트,이미지 등)들을 1개의 자바스크립트 파일 안에  다 넣어 버리고, 해당 자바스크립트만 로딩해도 웹 앱이 돌아가게 하자
- HTTP 요청을 줄여 웹 페이지 로딩이 빨라지고, 더 나은 사용자 경험을 제공하는 결과
- 걸프([Gulp](https://scotch.io/tutorials/automate-your-tasks-easily-with-gulp-js)), 그런트([Grunt](https://www.tutorialspoint.com/grunt/index.htm))와 같은 웹 자동화 도구들보다 존재했으며, 최근에는 이도구들보다 더 많은 기능을 추가로 제공하는 웹팩이 등장함



#### 웹팩의 주요 속성

1. entry : 웹팩으로 빌드(변환)할 대상파일을 지정하는 속성0
2. output : 웹팩으로 빌드한 결과물의 위치와 파일 이름 등 세부 옵션을 설정하는 속성
3. loader : 웹팩으로 빌드 할때 html,css,png(이미지) 파일등을 자바스크립트로 변환 하기 위해 필요한 설정을 정의 하는 속성
4. plugin : 웹팩으로 빌드하고 나온 결과물에 대해 추가 기능을 제공하는 속성
5. resolve : 웹팩으로 빌드할때 해당 파일이 어떻게 해석되는지 정의하는 속성



#### 웹팩 데브 서버

웹팩 설정 파일의 변화를 감지하여 빠르게 웹팩을 빌드할수 있도록 지원하는 유틸리티이자 Node.js 서버

웹팩 설정 파일이 내용이 변경되면 브라우저화면을 자동으로 새로고침하고, 바로 다시 웹팩으로 빌드하는 기능을 갖고 있음

인 메모리(in memory) 기반 : 웹팩 데브 서버는 빌드한 파일을 파일 시스템에 저장하지 않고 컴퓨터 메모리에 저장함 - 파일 시스템에 파일을 쓰고 읽는 시간보다 메모리에 저장하고 읽는 시간이 더 빠르기떄문



#### webpack-simple 프로젝트의 웹팩 설정 파일 분석

프로젝트를 생성하고 나면 프로젝트 최상의 레벨에서 `webpack.config.js`  라는 웹팩 설정 파일이 생성

`webpack.config.js`  파일에 정의된 설정에 따라 .vue 파일을 포함한 기타 파일들이 웹팩으로 빌드가 됨

##### 

```javascript
//파일경로와 웹팩 라이브러리 로딩 : output속성에서 사용할 노드 path 라이브러리와 웹팩 플러그인에서 사용할 node_modules의 웹팩 라이브러리를 node_modules의에서 로딩하여 path, webpack에 각각 저장
var path = require('path')
var webpack = require('webpack')

module.exports = {
  //entry속성 : 웹팩으로 빌드할 파일을 src 폴더 밑의 main.js파일로 지정
  entry: './src/main.js',
  
  //output속성 : 웹팩으로 빌드하고난 결과물 파일의 위치와 이름 지정
  output: {
    path: path.resolve(__dirname, './dist'),
    publicPath: '/dist/',
    filename: 'build.js'
  },
  
  //module속성 : 웹팩으로 애플리케이션 파일들을 빌드 할때 html,css 등이 파일을 자바스크립트로 변환해주는 로더를 지정
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          'vue-style-loader',
          'css-loader'
        ],
      },
      {
        test: /\.scss$/,
        use: [
          'vue-style-loader',
          'css-loader',
          'sass-loader'
        ],
      },
      {
        test: /\.sass$/,
        use: [
          'vue-style-loader',
          'css-loader',
          'sass-loader?indentedSyntax'
        ],
      },
      {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: {
          loaders: {
            // Since sass-loader (weirdly) has SCSS as its default parse mode, we map
            // the "scss" and "sass" values for the lang attribute to the right configs here.
            // other preprocessors should work out of the box, no loader config like this necessary.
            'scss': [
              'vue-style-loader',
              'css-loader',
              'sass-loader'
            ],
            'sass': [
              'vue-style-loader',
              'css-loader',
              'sass-loader?indentedSyntax'
            ]
          }
          // other vue-loader options go here
        }
      },
      {
        test: /\.js$/,
        loader: 'babel-loader',
        exclude: /node_modules/
      },
      {
        test: /\.(png|jpg|gif|svg)$/,
        loader: 'file-loader',
        options: {
          name: '[name].[ext]?[hash]'
        }
      }
    ]
  },
  
  //resolve 속성 : 뷰 라이브러리의 여러 유형중어떤걸 선택할지 지정
  resolve: {
    alias: {
      'vue$': 'vue/dist/vue.esm.js'
    },
    extensions: ['*', '.js', '.vue', '.json']
  },
  
  //devServer속성 : 데브서버 관련속성지정
  devServer: {
    historyApiFallback: true,
    noInfo: true,
    overlay: true
  },
  
  //performance속성:빌드한 파일의 크기가 250kb를 넘으면 경고 메시지를 표시할지 정합니다. 
  performance: {
    hints: false
  },
  
  //devtool속성 : 빌드된 파일로 웹앱을 구동했을때 개발자 도구에서 사용할 디버깅 방식 지정
  devtool: '#eval-source-map'
}

//배포할때 옵션
if (process.env.NODE_ENV === 'production') {
  module.exports.devtool = '#source-map'
  // http://vue-loader.vuejs.org/en/workflow/production.html
  module.exports.plugins = (module.exports.plugins || []).concat([
    new webpack.DefinePlugin({
      'process.env': {
        NODE_ENV: '"production"'
      }
    }),
    new webpack.optimize.UglifyJsPlugin({
      sourceMap: true,
      compress: {
        warnings: false
      }
    }),
    new webpack.LoaderOptionsPlugin({
      minimize: true
    })
  ])
}

```



2. 파일경로와 웹팩 라이브러리 로딩

