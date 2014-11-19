FIS 模块规范说明
====

此规范借鉴了很多 [componentjs](https://github.com/componentjs/component) 的思想。

一个模块可以由 HTML/TPL、JS、CSS、Images、Fonts 或其他前端资源的任意组合组成，它应该具有一定的通用性。每个模块必须包含 [components.json](#componentjson) 文件来描述自己。一个模块可以通过指定依赖，使用其他模块。

模块应该是一个比较独立的整体，模块内部资源引用，应该使用相对路径来引用。模块中的 JS 应该采用 COMMONJS 规范或者 [AMD](https://github.com/amdjs/amdjs-api) 规范来解决依赖和对外暴露的问题。模块对于其依赖中 `main` JS 的引用应该通过模块名来引用，如 `require('jquery')`，`main` js 之外的 js 通过 `name/path` 的方式引用如 `require('jquery-ui/autocomplete')`。

## component.json
每个组件必须包含一个 `component.json` 文件来描述此组件信息。

```json
{
  "name": "dialog",
  "description": "一个简单的 dialog 组件",
  "dependencies": [
    "github://xiangshouding/glob.js@master",
    "gitlab://fis-dev/fis-report-record@master",
    "lights://pc-demo@latest"
  ],
  "roadmap":[
    {
      "reg": "**.css",
      "release": "styles/$&"
    },

    {
      "reg": "*",
      "release": false
    }
  ]
}
```
### name

一个模块必须包含一个 `name` 属性，此属性将会成为其他模块的引用此模块的标识。

模块名只能包含小写英文字母、数字、中划线和下划线，它必须满足该正则 `/^[0-9a-z-_]+$/`。

### description

一个模块应当包含适当的模块信息，帮助他人理解此模块的功能特点。

### version

一个发行的模块必须指定一个版本号，版本形式应当采用 3 组数字组成，如：`1.2.3`。注意：前面没有 `v` 字开头。

如果一个模块托管在 git 仓库，发布此模块的时应当创建版本号同名的 tag。

## keywords

一个模块应当提供几个关键字来协助平台搜索, 建议将组件满足的解决方案名称带上如： `fisp` 或 'jello' 或 'yogurt'。

```json
{
  ...
  "keywords": [
    "fisp",
    "pagination"
  ]
  ...
}
```

## main

当此模块包含 JS 时，应当通过 `main` 来指定主文件，此文件将会通过 `require('模块名')` 的时候被引用。

## dependencies

模块如果需要依赖其他模块，则必须通过此属性指定。

```json
...
"dependencies": [
  "github://xiangshouding/glob.js@0.1.9",
  "gitlab://fis-dev/fis-report-record@latest"
],
...
```

每个依赖必须按照以下形式指定。

```
平台://作者名/模块名@版本
```

* 平台可以是 `github`、`gitlab` 或者 `bitbucket`。
* 版本支持 [semver](https://github.com/npm/node-semver) 格式。

### roadmap

如果仓库中文件存放比较多或希望最终安装后路径与原存放路径不一致，可以通过指定此属性来设定。

```json
{
  ...

  "roadmap": [
    {
      "reg": "/^dist\/(.*)$/",
      "release": "$1"
    },

    {
      "reg": "glob patterns",
      "release": false
    }

  ]

  ...
}
```

* `reg` 用来匹配仓库原文件路径，支持正则与 [glob](https://github.com/isaacs/node-glob) 两种规则。
* `release` 用来指定安装后的存放路径，当 `reg` 为正则表达式时，可以通过 `$数字` 来代替分组捕获。
