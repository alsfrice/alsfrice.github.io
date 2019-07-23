---
title: Node.js C++插件
tags:
  - JavaScript
  - Node.js
categories:
  - JavaScript
  - Node.js
date: 2019-07-05 15:17:04
---
在看《深入浅出Node.js》一书学习Node.js之时，根据书中的内容写C++扩展。念于此书年代过于久远，书中的部分代码已经过时了。写此文章，谨记学习路上踩过的坑。

用C++编写一个简单的"Hello World"插件，功能很简单，就是返回一个"Hello world!"字符串，相当于以下的JavaScript代码：

```
module.exports.hello = () => 'Hello world!'
```

新建hello目录作为自己的项目文件夹，编写hello.cc并将其存储到src目录下，代码如下：

```
#include <node.h>

namespace demo {

using v8::FunctionCallbackInfo;
using v8::Isolate;
using v8::Local;
using v8::Object;
using v8::String;
using v8::Value;

// 实现预定义的方法
void Method(const FunctionCallbackInfo<Value>& args) {
  Isolate* isolate = args.GetIsolate();
  args.GetReturnValue().Set(String::NewFromUtf8(isolate, "Hello world!"));
}

// 初始化函数
void init(Local<Object> exports) {
  NODE_SET_METHOD(exports, "sayHello", Method);
}

// 调用NODE_MODULE()将注册方法定义到内存中，注：NODE_MODULE后面没有分号，因为它不是一个函数
NODE_MODULE(NODE_GYP_MODULE_NAME, init)

}
```

写好C++扩展模块，还需要进行编译。工具使用node-gyp即可，安装方式可用npm或yarn:

```
npm install -g node-gyp
```
或
```
yarn global add node-gyp
```

安装完毕还需要写好.gyp项目文件，node-gyp约定.gyp文件为binding.gyp。它使用一个类似 JSON 的格式来描述模块的构建配置。 内容如下：

```
{
  'targets': [
    {
      'target_name': 'hello',
      'sources': ['src/hello.cc']
    }
  ]
}
```

然后调用：
```
node-gyp configure
```

为当前平台生成相应的项目构建文件。这会在 build/ 目录下生成一个 Makefile 文件（在 Unix 平台上）或 vcxproj 文件（在 Windows 上）。下一步，调用：
```
node-gyp build
```
生成编译后的 hello.node 的文件。 它会被放进 build/Release/ 目录。

在hello目录下编写hello.js，代码如下：

```
const hello = require('./build/Release/hello.node')

console.log(hello.sayHello())
```

调用：
```
node hello.js

// 打印：Hello world!
```