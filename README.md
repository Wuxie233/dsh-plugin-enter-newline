# dsh-client-enter-newline

dsh Web 输入框「回车行为」开关插件：在 **设置 → 通用 → 回车行为** 里选择回车是
发送消息还是换行。

## 行为

| 模式 | 回车 | Shift+回车 | Ctrl/Cmd+回车 |
| --- | --- | --- | --- |
| 回车发送（默认） | 发送 | 换行 | 保持产品行为（插话/加速发送） |
| 回车换行 | 换行 | 发送 | 保持产品行为（插话/加速发送） |

其他守卫：输入法组合中不拦截；斜杠菜单打开时回车仍用于选中菜单项；
设置持久化在 dsh 用户设置的 `ui-enter-newline` 命名空间（`~/.dsh/settings.yaml`）。

## 安装

1. `./install.sh` —— 部署到 `$DSH_HOME/profiles/node_modules/<包名>/`
2. 在 `~/.dsh/profiles/web/cordis.patch.yml` 里确认已有挂载行：

   ```yaml
   - insert:
       - id: ui-enter-newline
         name: '@deepseek-ai/dsh-client-enter-newline'
   ```

3. 重启 dsh web（host 半要注册设置命名空间），刷新页面。

## 维护

- 仓库是唯一真身；运行时目录是可丢弃的副本。每次改完 `./install.sh`，
  然后刷新页面（浏览器半）/ 重启 dsh（host 半）。
- dsh 升级或修复 profile 后如果插件失效，重跑 `./install.sh` 即可。

## 改名清单（如果要换包名/换成自己的 scope）

包名出现在三个地方，必须一起改：

1. `package.json` 的 `name`（决定运行时目录路径）
2. `lib/client.js` 里 `window.__ModuleLoader__.load({ id: ... })` 的 `id`
3. `cordis.patch.yml` 挂载行的 `name`

## 文件

```
package.json   包声明 + dsh.client 清单（client 依赖边，仅供预检展示）
lib/index.js   host 半：注册 ui-enter-newline 设置命名空间 schema
lib/client.js  浏览器半：设置行 + 回车拦截器
install.sh     部署脚本
README.md      本文件
```
