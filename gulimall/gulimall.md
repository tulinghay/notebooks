### npm install报错

```sh
gyp verb check python checking for Python executable "python" in the PATH
gyp verb `which` succeeded python D:\Program Files\Python38\python.EXE
gyp ERR! configure error
gyp ERR! stack Error: Command failed: D:\Program Files\Python38\python.EXE -c import sys; print "%s.%s.%s" % sys.version_info[:3];
gyp ERR! stack   File "<string>", line 1
gyp ERR! stack     import sys; print "%s.%s.%s" % sys.version_info[:3];
gyp ERR! stack                       ^
gyp ERR! stack SyntaxError: invalid syntax
gyp ERR! stack
gyp ERR! stack     at ChildProcess.exithandler (child_process.js:303:12)
gyp ERR! stack     at ChildProcess.emit (events.js:310:20)
gyp ERR! stack     at maybeClose (internal/child_process.js:1021:16)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (internal/child_process.js:286:5)
gyp ERR! System Windows_NT 10.0.18362
gyp ERR! command "D:\\Program Files\\nodejs\\node.exe" "E:\\JDProject\\renren-fast-vue\\node_modules\\node-gyp\\bin\\node-gyp.js" "rebuild" "--verbose" "--libsass_ext=" "--libsass_cflags=" "--libsass_ldflags=" "--libsass_library="
gyp ERR! cwd E:\JDProject\renren-fast-vue\node_modules\node-sass
gyp ERR! node -v v12.16.3
gyp ERR! node-gyp -v v3.8.0
gyp ERR! not ok
Build failed with error code: 1
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.2.9 (node_modules\fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.9: wanted {"os":"darwin","arch":"any"} (current: {"os":"win32","arch":"x64"})
 
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! node-sass@4.9.0 postinstall: `node scripts/build.js`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the node-sass@4.9.0 postinstall script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.
 
npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\HP\AppData\Roaming\npm-cache\_logs\2020-05-23T06_27_44_952Z-debug.log
```

原因是npm-sass的版本和npm的版本不对应

解决方案：

- 删除项目中的package-lock.json文件，清除缓存
- 修改 package.json 文件  提高 node-sass  版本
- 提高版本后报错加上npm install --force 

```sh
"node-sass": "^6.0.1",
"npm": "^8.0.1",
```



如果上面方法不生效则使用

npm install --legacy-peer-deps

或者cnpm install



### Nacos Config不生效

1.pom文件里导入了config依赖

2.controller类上加了@RefreshScope

3.bootstrap.properties中配置了server-addr

4.nacos服务器上配置了DataId

以上都做了但是不生效，原因是springcloud版本导致bootstrap.properties没有读取到，需要加一个依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-bootstrap</artifactId>
</dependency>
```

