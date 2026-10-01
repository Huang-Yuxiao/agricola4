# 农4秋季赛成绩中心

网址：https://huang-yuxiao.github.io/agricola4/

GitHub Pages 提供静态页面，腾讯云服务器运行 Python 计分接口并使用 SQLite 保存对局。
玩家无需登录；提交后同时更新全阶段 ELO 和赛季 ELO。

`config.js` 配置腾讯云 HTTPS API 根地址。接口不可用时页面提示连接失败并禁止提交，不会假装保存成功。

## 维护

`frontend-source.zip` 是前端完整源代码。解压后执行 `npm install`、`npm run build`，将 dist 文件更新到仓库根目录。
`server.tar.gz` 是后台源码和测试，不包含数据库、密钥或账号凭据。
实时比赛数据保存在腾讯云 `/var/lib/agricola4/scores.sqlite3`，不保存在 GitHub。
`config.js` 配置后台 HTTPS 根地址。后台 CORS 允许 `https://huang-yuxiao.github.io`。

## 计分

使用已确认的 128 人 CSV 作为全阶段起点，新玩家从 500 开始；每对手 K 为 40（前10局）、80/3（第11至20局）、40/3（之后）。
赛季从450开始，K=40/3，对手使用赛前全阶段分。Studio计分玩家额外+5赛季分，NPC不计赛季。
达到550或满15局后停止赛季计分。真正并列采用0.5，第二第三并列填1、2、2、4。
