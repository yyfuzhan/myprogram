# 准备工作

1.  请使用 vscode 作为开发此项目的 IDE
2.  请安装 ESLint Prettier 插件
3.  请在 vscode 配置文件中添加：

```json
{
  "editor.tabSize": 2,
  "editor.formatOnSave": true
}
```

# 开发环境启动流程

1. 连接公司 vpn
2. 运行 java 包
3. npm run start

## step1 vpn 连接

参考开发部 vpn 连接指导

## step2 运行 java 包

### 环境配置

本地运行 java 包需要 jdk

- [jdk 下载](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)
- [jdk 安装和环境变量配置](https://www.cnblogs.com/liuhongfeng/p/4177568.html)

### jar 包启动

1. 打开 cmd 命令行
2. 切换至 java 包所在文件夹
3. 运行如下命令

```shell
java -jar operation-platform.jar
```

## step3 npm 启动项目

### npm 切换源

cnpm 服务[已迁移至新地址]
https://registry-cnpm.dayu.work/

> 此地址更新于 20200922,如有变更请依据钉钉群"浩鲸智能 ali-whale 社区"的群公告进行更新

#### 使用 cnpm

cnpm 请按如下设置：

```shell
cnpm set registry https://registry-cnpm.dayu.work/
```

#### 使用 nrm

推荐大家使用 nrm / cgr 管理源
示例：

```shell
nrm add ali-whale https://registry-cnpm.dayu.work/
nrm use ali-whale
```

> - 新的的组件的前缀将全部由@cbd 更改为@ali-whale（@cbd 组件的历史版本保持不变，新的版本和新组件将以@ali-whale 作为前缀）
> - ali-whale[原 CBD]项目代码仓库
>   https://code.aliyun.com/gtssolution/aliyun-gts-whale-front
> - CBD 示例地址
>   http://139.224.134.152:8191/

### 安装依赖

```shell
npm i
```

### 项目启动

```shell
npm run start
```

# 项目相关文档

## 接口文档

1. 运行 java 包
2. 访问如下地址 http://localhost:8133/platform/swagger-ui.html#/

> 具体地址可能会有变动,如果无法访问,请咨询提供 java 包的后端

## UI 图

运营管理 3.0(全流程,指标考核,前置资源): https://iwhale-citybrain.yuque.com/zz7cip/pktn99#

## 原型图

杭州警务操作系统>运营管理平台: .

## 大禹需求管理

运营管理 3.0 : https://dayu.work/project/info?businessId=46556787

# 生产环境打包命令

```shell
npm run build-prod
```

# 项目结构

<pre>
public                  // 公共文件 可以放一些第三方字体 样式库等
src
  |-- mock            // mock文件
  |-- components     // 公共组件目录 当业务需要拆分组件的时候，可以在对应的业务文件夹下单独创建一个components文件夹
  |-- models          // 公共model存放位置
    |-- public.js      // 公共model文件 可以多个
  |-- services         // 公共api存放
  |-- pages             // 容器组件
    |-- demo            // 业务容器 相对路由/demo ***不可以有任何大写字母
      |-- index.js      // 业务入口 入口文件只识别index.js 后缀必须是js
      |-- index.less    // 业务样式
      |-- modules       // 业务model目录
        |-- demo.js     // 业务model文件 可以有多个 自动加载
      |-- service       // 业务api目录
        |-- demo.js     // 业务api文件 可以有多个
  |-- utils             // 工具
  |-- global.less       // 样式变量 方法
.eslintignore          // eslint过滤文件清单
.eslintrc.js            // eslint配置
.gitignore
package.json  
README.md  
</pre>

# 开发规范

1. 页面应该写在`pages`目录下，所有页面必须以`index.js`为入口。页面的路径不可以包含大写字母，如目录包含多个单词，请使用"-"分隔，如`pages/user-manage/index.js`
2. 页面/组件应当包含三要素(`index.js`入口，`ComponetName.js`页面/组件本身，`ComponetName.less`页面/组件的样式)
3. 单个页面/组件文件，代码量请控制在 300 行以内，超过该数目，请适当对业务内容进行拆分。
4. 除极个别情况（需要与前端项目负责人沟通确认），不可以在业务代码中使用`eslint-disable`的方式跳过`eslint`校验规则。
5. 页面中引入(组件/服务/models/工具等)资源时，必须遵循规则：禁止依赖子页面或兄弟页面的相关资源，只可以引入当前目录的相关资源，或祖先目录的相关资源。如果某页面需要引用兄弟页面的资源，请进行重构，将该资源提升至他们的共同祖先目录。
6. 项目中已集成`CBD-UI`运行时插件，默认唤起快捷键为`ctrl`+`alt`+`s`(windows)或`ctrl`+`cmd`+`s`(mac)。推荐使用该插件创建页面/组件。
7. 简单的页面，请尽量少使用`dva-model`。当兄弟页面/父子页面之间存在复杂的状态共享时，我们才考虑使用`model`的`state`。当兄弟页面/父子页面之间存在业务逻辑复用时，我们才考虑使用`model`的`effect`
8. 使用`dva-model`时，请尽量少新建`reducers`，大部分的内容都可通过`update`完成(使用`CBD-UI`创建新的`model`时就可以看到)
9. 接口层`services`，组件的`propTypes`等，请编写足够的可读性强的注释，以便于代码维护。
10. 对于控制台中打印的`Warnning`信息，如`React`出现的`duplicate key`, `update on an unmounted component`, `invalid value for prop xxx` 等问题，请务必解决后再提交代码。
11. 使用`dva-model`时，请保持`namespace`与文件名一直，便于后期维护查找。
