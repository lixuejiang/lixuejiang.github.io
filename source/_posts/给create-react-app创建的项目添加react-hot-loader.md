---
title: 给create-react-app创建的项目添加react hot loader
date: 2017-08-09 11:41:16
tags: [react,工程化]
---
create-react-app创建的项目自带了live reloading，但是每次更新的时候是刷新整个页面，而不是只更新修改过的部分，导致在涉及到状态比较复杂的页面在开发的时候不是很方便。所以参考[http://joshbroton.com/add-react-hot-reloading-create-react-app/](http://joshbroton.com/add-react-hot-reloading-create-react-app/)，添加hot loader，提高开发效率。[官网](http://gaearon.github.io/react-hot-loader/getstarted/)具体步骤如下：

<!--more-->

# 安装

## 1.安装create-react-app

`npm install -g create-react-app`

## 2.创建项目

`create-react-app my-app`

# 配置

## 1.弹出配置文件

`npm run eject`

## 2.安装React Hot Loader

`npm install --save-dev react-hot-loader@next`

## 3.添加babel插件

```

"plugins": [
    "react-hot-loader/babel"
]

```

最终package.json里的babel配置如下：

```

"babel": {
  "presets": [
    "react-app",
    "stage-1"
  ],
  "plugins": [
    "react-hot-loader/babel"
  ]
},


```


## 4. webpack.config.dev.js 添加如下入口

`'react-hot-loader/patch'`

## 5. 修改index.js入口文件

- 引入AppContainer
`import {AppContainer} from 'react-hot-loader';`

- 修改默认render函数

```

const rootEl = document.getElementById('root');
 
ReactDOM.render(
    <AppContainer>
        <App />
    </AppContainer>,
  rootEl
);


```


- 处理变更

```

if (module.hot) {
    module.hot.accept('./App', () => {
        const NextApp = require('./App').default; // eslint-disable-line global-require
        ReactDOM.render(
            <AppContainer>
                <NextApp />
            </AppContainer>,
            rootEl
        );
    });
}


```



# 其他方案

- [create-react-app-hot-loader](https://www.npmjs.com/package/create-react-app-hot-loader)

# 参考项目

- [react-hot-boilerplate](https://github.com/gaearon/react-hot-boilerplate)
