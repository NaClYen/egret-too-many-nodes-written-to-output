# egret-too-many-nodes-written-to-output
test project for "Too many nodes written to output." issue.

## 專案設定
![](https://i.imgur.com/oujgzbh.png)

## 重現流程
- 開啟專案直接在 terminal 下 `egret build`, 可以正常執行看到這樣的訊息  
    ```
    正在使用白鹭编译器 5.3.10 版本
    正在编译项目...
    i ｢wdm｣: wait until bundle finished: /index.html
    i ｢wdm｣: Hash: 559d112c0901dccebb81
    Version: webpack 4.44.1
    Time: 832ms
    Built at: 2021-01-06 10:51:41 AM
          Asset      Size  Chunks                   Chunk Names
     index.html   3.3 KiB          [emitted]
        main.js  24.8 KiB    main  [emitted]        main
    main.js.map  19.2 KiB    main  [emitted] [dev]  main
    Entrypoint main = main.js main.js.map
    Child HtmlWebpackCompiler:
                              Asset      Size               Chunks  Chunk Names
        __child-HtmlWebpackPlugin_0  17.1 KiB  HtmlWebpackPlugin_0  HtmlWebpackPlugin_0
        Entrypoint HtmlWebpackPlugin_0 = __child-HtmlWebpackPlugin_0
    i ｢wdm｣: Compiled successfully.
    ```
- 在 `src` 加入一個 ts 檔案(檔名隨意), 內容如下:
    ```typescript=
    namespace A.B {
        export class TheTest {}
    }
    ```
- 接著在下一次 build, 應該會失敗並看到這樣的訊息:
    ```
    您正在使用白鹭编译器 5.3.10 版本
    正在编译项目...
    i ｢wdm｣: wait until bundle finished: /index.html
    (node:8572) UnhandledPromiseRejectionWarning: Error: Debug Failure. False expression: Too many nodes written to output.
        at extractSingleNode (C:\Users\user_name\AppData\Roaming\EgretLauncher\download\EgretCompiler\@egret\egret-webpack-bundler\node_modules\typescript\lib\typescript.js:75267:18)      
        at visitNode (C:\Users\user_name\AppData\Roaming\EgretLauncher\download\EgretCompiler\@egret\egret-webpack-bundler\node_modules\typescript\lib\typescript.js:74807:54)
        at Object.visitEachChild (C:\Users\user_name\AppData\Roaming\EgretLauncher\download\EgretCompiler\@egret\egret-webpack-bundler\node_modules\typescript\lib\typescript.js:75177:215) 
        at visitNodes (C:\Users\user_name\AppData\Roaming\EgretLauncher\download\EgretCompiler\@egret\egret-webpack-bundler\node_modules\typescript\lib\typescript.js:74849:48)
        at visitLexicalEnvironment (C:\Users\user_name\AppData\Roaming\EgretLauncher\download\EgretCompiler\@egret\egret-webpack-bundler\node_modules\typescript\lib\typescript.js:74882:22)
        at Object.visitEachChild (C:\Users\user_name\AppData\Roaming\EgretLauncher\download\EgretCompiler\@egret\egret-webpack-bundler\node_modules\typescript\lib\typescript.js:75249:54)
        at visitor (C:\Users\user_name\AppData\Roaming\EgretLauncher\download\EgretCompiler\@egret\egret-webpack-bundler\lib\loaders\ts-loader\ts-transformer.js:161:29)
        at Object.visitNode (C:\Users\user_name\AppData\Roaming\EgretLauncher\download\EgretCompiler\@egret\egret-webpack-bundler\node_modules\typescript\lib\typescript.js:74798:23)
        at C:\Users\user_name\AppData\Roaming\EgretLauncher\download\EgretCompiler\@egret\egret-webpack-bundler\lib\loaders\ts-loader\ts-transformer.js:166:42
        at transformSourceFileOrBundle (C:\Users\user_name\AppData\Roaming\EgretLauncher\download\EgretCompiler\@egret\egret-webpack-bundler\node_modules\typescript\lib\typescript.js:76524:57)
        at transformation (C:\Users\user_name\AppData\Roaming\EgretLauncher\download\EgretCompiler\@egret\egret-webpack-bundler\node_modules\typescript\lib\typescript.js:94097:24)
        at transformRoot (C:\Users\user_name\AppData\Roaming\EgretLauncher\download\EgretCompiler\@egret\egret-webpack-bundler\node_modules\typescript\lib\typescript.js:94118:82)
        at Object.map (C:\Users\user_name\AppData\Roaming\EgretLauncher\download\EgretCompiler\@egret\egret-webpack-bundler\node_modules\typescript\lib\typescript.js:551:29)
        at Object.transformNodes (C:\Users\user_name\AppData\Roaming\EgretLauncher\download\EgretCompiler\@egret\egret-webpack-bundler\node_modules\typescript\lib\typescript.js:94104:30)
        at emitJsFileOrBundle (C:\Users\user_name\AppData\Roaming\EgretLauncher\download\EgretCompiler\@egret\egret-webpack-bundler\node_modules\typescript\lib\typescript.js:94692:32)
    (node:8572) UnhandledPromiseRejectionWarning: Unhandled promise rejection. This error originated either by throwing inside of an async function without a catch block, or by rejecting a promise which was not 
    handled with .catch(). (rejection id: 1)
    ```

## 其他發現
在嘗試製作重現專案時, 發現只要讓 webpack 使用 `modern` 模式, 就不會有這個問題.  
其 compile 的內容如下:  
```
您正在使用白鹭编译器 5.3.10 版本
正在编译项目...
Starting type checking service...
Type checking in progress...
i ｢wdm｣: Hash: b829e53fa340d3d425a0
Version: webpack 4.44.1
Time: 271ms
Built at: 2021-01-06 11:06:35 AM
      Asset      Size  Chunks                   Chunk Names
 index.html   3.3 KiB          [emitted]
    main.js  20.5 KiB    main  [emitted]        main
main.js.map  14.7 KiB    main  [emitted] [dev]  main
Entrypoint main = main.js main.js.map
Child HtmlWebpackCompiler:
                          Asset      Size               Chunks  Chunk Names
    __child-HtmlWebpackPlugin_0  17.1 KiB  HtmlWebpackPlugin_0  HtmlWebpackPlugin_0
    Entrypoint HtmlWebpackPlugin_0 = __child-HtmlWebpackPlugin_0
i ｢wdm｣: Compiled successfully.
No type errors found
Version: typescript 3.9.7
Time: 1585 ms
```
推測可能是Module解析問題...吧?

## 應對
1. 採用 ES6 Module.  
    最佳解, 但也最難.
2. 全面避免多層 namespace 的寫法.  
    比較有機會讓老項目延壽的作法.
3. 避免使用 Webpack, 改回使用 CompilePlugin.  
    我懂大家都有那包燙手山芋(?) 或者不得已的地方XDD
