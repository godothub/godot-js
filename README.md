# godot-js

Godot JavaScript / TypeScript 插件，使用 Gode 的 Lite 构建。

本仓库只维护插件分发文件和构建流水线。C++ 运行时、生成器、GDScript
插件源文件和打包脚本统一在 [godothub/gode](https://github.com/godothub/gode)
维护。此仓库也是 Gode 的 `third/godot-js` 子模块。

插件文件由 Gode 的 `.github/shell/sync-godot-js.py` 生成；修改插件时先改
Gode 的 `example/addons/gode`，再运行该脚本同步。不要在此仓库另建 C++ 代码副本。

## 构建与安装

运行 `Build godot-js Lite`，指定包含对应实现的 `gode_ref` 和
`libnode-lite.zip` URL。流水线递归检出 Gode，设置 `GODE_LITE=ON`，构建
Windows x64、Linux x64、macOS arm64、Android arm64、iOS arm64，生成
`godot-js.zip` 和 `GODE-SOURCE-COMMIT.txt` artifact。

解压插件到项目的 `addons/godot-js`，在 Godot 插件设置中启用 godot-js。
完整 Gode 和 godot-js 使用相同的 Godot 类注册和 JS 绑定，同一项目
只安装其中一个插件。JS/TS 的 `godot` 模块及已有绑定保持兼容。

## Lite 能力

保留 JS/TS、CommonJS / ESM、纯 JS npm 包、现有 Godot 绑定和所需
Node/N-API 兼容层。运行时不提供原生 `.node` 插件、子进程、Worker、
Inspector、OpenSSL/crypto/TLS、SQLite、ICU/Intl 和 V8 WebAssembly API。
依赖这些功能的 npm 包不能使用，仅用 JS 编写并不代表不依赖 Node 系统模块。
当前库采用 V8 Lite 模式，关闭 JIT；TypeScript 编译器随插件打包。

流水线还构建 wasm32 Web 扩展及匹配的 Godot 4.7 导出模板，并用
真实导出项目在 Chromium 中验证纯 JS npm 包和 Godot API。

Web 导出时启用线程和 GDExtension，将自定义 Release 模板指向
`addons/godot-js/binary/editor/web/godot.web.template_release.wasm32.dlink.zip`。
模板与扩展均使用 Emscripten 5.0.7 和 wasm32；服务器需要 COOP/COEP
响应头。V8 的 WebAssembly API 不参与 Web 导出。
