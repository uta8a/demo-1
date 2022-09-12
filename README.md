# demo-1

[alosaur/handlebars](https://github.com/alosaur/handlebars) の修正をおこなっている時に遭遇した謎エラー(？)

## 前提

[alosaur/handlebars](https://github.com/alosaur/handlebars) を使っていたら数日前から以下のようなエラーが出てdependencyのインストールに失敗するようになった。

```
Download https://dev.jspm.io/npm:@jspm/core@1.1.1/nodelibs/fs.js
error: Import 'https://dev.jspm.io/npm:source-map@0.6?dew' failed: 500 Internal Server Error
    at https://dev.jspm.io/npm:handlebars@4.7.6/dist/cjs/handlebars/compiler/code-gen.dew.js:2:41
Error: Process completed with exit code 1.
```

元リポジトリでは `dev.jspm.io` を使っていたが [これは非推奨](https://github.com/jspm/project/issues/196#issuecomment-1161937319) なので、 `jspm.dev` に置き換えてPRを作ろうとした。

[forkしたリポジトリ](https://github.com/uta8a/handlebars/tree/fix-jspm-url) で修正してテストを通した後、(テストが心もとなかったので) 動作するか確認していたら以下のエラー(?)に遭遇した。

## 環境

Apple M1 mac mini(2020)

```
$ deno --version
deno 1.25.2 (release, aarch64-apple-darwin)
v8 10.6.194.5
typescript 4.7.4
```

## 状況説明

`normal.ts` のように普通はimportしてexportするだけのコードは正常終了する。(はず)

```
$ deno run -Ar normal.ts 
Download https://deno.land/std@0.149.0/path/mod.ts
Download https://deno.land/std@0.149.0/_util/os.ts
Download https://deno.land/std@0.149.0/path/_interface.ts
Download https://deno.land/std@0.149.0/path/common.ts
Download https://deno.land/std@0.149.0/path/glob.ts
Download https://deno.land/std@0.149.0/path/posix.ts
Download https://deno.land/std@0.149.0/path/separator.ts
Download https://deno.land/std@0.149.0/path/win32.ts
Download https://deno.land/std@0.149.0/path/_constants.ts
Download https://deno.land/std@0.149.0/path/_util.ts
Download https://deno.land/std@0.149.0/_util/assert.ts
```

`stop.ts` はimportしてexportするだけのコードだが、なぜか動作が固まって終了しない

```
$ deno run -Ar stop.ts  
Download https://raw.githubusercontent.com/uta8a/handlebars/fix-jspm-url/mod.ts
Download https://deno.land/std@0.110.0/fs/mod.ts
Download https://deno.land/std@0.110.0/path/mod.ts
Download https://jspm.dev/handlebars@4.7.6
Download https://deno.land/std@0.110.0/fs/copy.ts
Download https://deno.land/std@0.110.0/fs/empty_dir.ts
Download https://deno.land/std@0.110.0/fs/ensure_dir.ts
Download https://deno.land/std@0.110.0/fs/ensure_file.ts
Download https://deno.land/std@0.110.0/fs/ensure_link.ts
Download https://deno.land/std@0.110.0/fs/ensure_symlink.ts
Download https://deno.land/std@0.110.0/fs/eol.ts
Download https://deno.land/std@0.110.0/fs/exists.ts
Download https://deno.land/std@0.110.0/fs/expand_glob.ts
Download https://deno.land/std@0.110.0/fs/move.ts
Download https://deno.land/std@0.110.0/fs/walk.ts
Download https://deno.land/std@0.110.0/_util/os.ts
Download https://deno.land/std@0.110.0/path/_interface.ts
Download https://deno.land/std@0.110.0/path/common.ts
Download https://deno.land/std@0.110.0/path/glob.ts
Download https://deno.land/std@0.110.0/path/posix.ts
Download https://deno.land/std@0.110.0/path/separator.ts
Download https://deno.land/std@0.110.0/path/win32.ts
Download https://jspm.dev/npm:@jspm/core@2/nodelibs/fs
Download https://jspm.dev/npm:handlebars@4.7.6!cjs
Download https://jspm.dev/npm:handlebars@4.7.6/_/a1417093.js
Download https://jspm.dev/npm:handlebars@4.7.6/_/cac9e115.js
Download https://jspm.dev/npm:handlebars@4.7.6/dist/cjs/handlebars!cjs
Download https://jspm.dev/npm:handlebars@4.7.6/dist/cjs/handlebars/compiler/ast!cjs
Download https://jspm.dev/npm:handlebars@4.7.6/dist/cjs/handlebars/compiler/compiler!cjs
Download https://jspm.dev/npm:handlebars@4.7.6/dist/cjs/handlebars/compiler/javascript-compiler!cjs
Download https://jspm.dev/npm:handlebars@4.7.6/dist/cjs/handlebars/compiler/parser!cjs
Download https://jspm.dev/npm:handlebars@4.7.6/dist/cjs/handlebars/compiler/visitor!cjs
Download https://jspm.dev/npm:handlebars@4.7.6/dist/cjs/handlebars/exception!cjs
Download https://jspm.dev/npm:handlebars@4.7.6/dist/cjs/handlebars/runtime!cjs
Download https://jspm.dev/npm:handlebars@4.7.6/dist/cjs/handlebars/safe-string!cjs
Download https://jspm.dev/npm:handlebars@4.7.6/dist/cjs/handlebars/utils!cjs
Download https://jspm.dev/npm:source-map@0.6!cjs
Download https://jspm.dev/npm:@jspm/core@2.0.0-beta.26/_/233f66cc.js
Download https://jspm.dev/npm:@jspm/core@2.0.0-beta.26/_/257ac839.js
Download https://jspm.dev/npm:@jspm/core@2.0.0-beta.26/_/3765dd86.js
Download https://jspm.dev/npm:@jspm/core@2.0.0-beta.26/_/4386c609.js
Download https://jspm.dev/npm:@jspm/core@2.0.0-beta.26/_/6419df6d.js
Download https://jspm.dev/npm:@jspm/core@2.0.0-beta.26/_/83350e25.js
Download https://jspm.dev/npm:@jspm/core@2.0.0-beta.26/_/86eca8fd.js
Download https://jspm.dev/npm:@jspm/core@2.0.0-beta.26/_/ddba2d13.js
Download https://jspm.dev/npm:@jspm/core@2.0.0-beta.26/_/e3194650.js
Download https://jspm.dev/npm:@jspm/core@2.0.0-beta.26/_/ea5153ea.js
Download https://jspm.dev/npm:@jspm/core@2.0.0-beta.26/_/ec8cc5b2.js
Download https://jspm.dev/npm:@jspm/core@2.0.0-beta.26/nodelibs/assert
Download https://jspm.dev/npm:@jspm/core@2.0.0-beta.26/nodelibs/buffer
Download https://jspm.dev/npm:@jspm/core@2.0.0-beta.26/nodelibs/events
Download https://jspm.dev/npm:@jspm/core@2.0.0-beta.26/nodelibs/fs
Download https://jspm.dev/npm:@jspm/core@2.0.0-beta.26/nodelibs/path
Download https://jspm.dev/npm:@jspm/core@2.0.0-beta.26/nodelibs/stream
Download https://jspm.dev/npm:@jspm/core@2.0.0-beta.26/nodelibs/url
Download https://jspm.dev/npm:@jspm/core@2.0.0-beta.26/nodelibs/util
Download https://deno.land/std@0.110.0/_util/assert.ts
Download https://deno.land/std@0.110.0/fs/_util.ts
Download https://jspm.dev/npm:source-map@0.6.1!cjs
Download https://jspm.dev/npm:source-map@0.6.1/_/b1dbe139.js
Download https://jspm.dev/npm:source-map@0.6.1/lib/source-map-consumer!cjs
Download https://jspm.dev/npm:source-map@0.6.1/lib/source-map-generator!cjs
Download https://jspm.dev/npm:source-map@0.6.1/lib/util!cjs
Download https://deno.land/std@0.110.0/path/_constants.ts
Download https://deno.land/std@0.110.0/path/_util.ts
^C <-- いつまで経っても正常終了しないのでCtrl+cした
```

## なぜ？

stop.ts ではforkしたリポジトリのrawgithubcontentをimportしている。これが問題なのか...？

- dependencyのインストールが完了していない可能性
- dependencyのインストールができていて、実行で止まっている可能性

止まっちゃってしまうとデバッグ困難なので困っている

もしかしたら僕の環境だけで起こるのか...？
