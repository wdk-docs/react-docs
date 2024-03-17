---
title: React开发人员工具
---

<Intro>

使用React开发人员工具检查React[组件](/learn/your-first-component), 编辑[props](/learn/passing-props-to-a-component) 和 [state](/learn/state-a-components-memory), 并识别性能问题。

</Intro>

<YouWillLearn>

* 如何安装React开发人员工具

</YouWillLearn>

## 浏览器扩展 {/*browser-extension*/}

调试使用React构建的网站的最简单方法是安装React开发人员工具浏览器扩展。它适用于几种流行的浏览器：

* [为**Chrome**安装](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en)
* [为**Firefox**安装](https://addons.mozilla.org/en-US/firefox/addon/react-devtools/)
* [为**Edge**安装](https://microsoftedge.microsoft.com/addons/detail/react-developer-tools/gpphkfbcpidddadnkolkpfckpihlkkil)

现在，如果您访问使**用React构建**的网站，您将看到 _Components_ 和 _Profiler_ 面板。

![React开发人员工具扩展](/images/docs/react-devtools-extension.png)

### Safari 和其他浏览器 {/*safari-and-other-browsers*/}
对于其他浏览器（例如Safari），请安装[`react devtools`](https://www.npmjs.com/package/react-devtools)npm包:
```bash
# Yarn
yarn global add react-devtools

# Npm
npm install -g react-devtools
```

接下来从终端打开开发人员工具:
```bash
react-devtools
```

然后，通过在网站的`<head>`开头添加以下`<script>`标记来连接您的网站：
```html {3}
<html>
  <head>
    <script src="http://localhost:8097"></script>
```

立即在浏览器中重新加载您的网站，以便在开发人员工具中查看。

![React开发人员工具独立的](/images/docs/react-devtools-standalone.png)

## Mobile (React Native) {/*mobile-react-native*/}

React开发人员工具也可以用于检查使用[React Native](https://reactnative.dev/)构建的应用程序。

使用React开发人员工具最简单的方法是全局安装：

```bash
# Yarn
yarn global add react-devtools

# Npm
npm install -g react-devtools
```

接下来，从终端打开开发人员工具。
```bash
react-devtools
```

它应该连接到任何正在运行的本地React Native应用程序。

> 如果几秒钟后开发人员工具无法连接，请尝试重新加载应用程序。

[了解有关调试React Native的更多信息。](https://reactnative.dev/docs/debugging)
