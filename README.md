# 农4秋季赛成绩中心

网址：https://huang-yuxiao.github.io/agricola4/

GitHub Pages 提供静态页面，腾讯云服务器运行 Python 计分接口并使用 SQLite 保存对局。
玩家无需登录；提交后同时更新全阶段 ELO 和赛季 ELO。
无需填写开桌或结束时间，以服务器上报时间结算；不再核验回合制开桌截止或15天时长。

`config.js` 配置腾讯云 HTTPS API 根地址。接口不可用时页面提示连接失败并禁止提交，不会假装保存成功。

## 维护

`frontend-source.zip` 是前端完整源代码。解压后执行 `npm install`、`npm run build`，将 dist 文件更新到仓库根目录。
`server.tar.gz` 是后台源码和测试，不包含数据库、密钥或账号凭据。
实时比赛数据保存在腾讯云 `/var/lib/agricola4/scores.sqlite3`，不保存在 GitHub。
`config.js` 配置后台 HTTPS 根地址。后台 CORS 允许 `https://huang-yuxiao.github.io`。

管理员修改、调序与删除共用的密码只在服务器的 `/etc/agricola4/admin.env` 中以哈希配置，不写入前端或仓库。
删除入口位于查对局的每条记录中，删除后按剩余比赛重算全部榜单。归档保存在 SQLite 的 `deleted_submissions` 表。
管理员可在服务器使用 `AGRICOLA_DB=/var/lib/agricola4/scores.sqlite3 .venv/bin/python restore_submission.py <回执UUID>` 恢复误删；若同一桌号已重新上报，恢复会拒绝覆盖新记录。

查对局支持修改原记录（保留回执、时间、顺序）以及按目标桌号插入其前面计分。两项操作均验证管理员密码，并重算榜单。修改及调序记录保存在 `submission_audit`，计分顺序保存在 `submission_order`；删除后保留顺序，恢复仍回到原位置。
桌号在所有平台间互斥，不区分英文字母大小写。默认上报 Studio 即时。页面每10分钟刷新并保留已有列表，也支持手动刷新；切回窗口不触发刷新。

## 计分

使用已确认的 128 人 CSV 作为全阶段起点，新玩家从 500 开始；每对手 K 为 40（前10局）、80/3（第11至20局）、40/3（之后）。
赛季从450开始，K=40/3，对手使用赛前全阶段分。Studio计分玩家额外+5赛季分。所有玩家自动进入赛季计分，忽略旧的手动NPC标记。
首次达到550时锁定为550，否则第15局结束后锁分；之后自动NPC，仍记录全阶段成绩。550分先按计分局数少，再按冲线时间早排序。真正并列采用0.5，第二第三并列填1、2、2、4。
