# 农4秋季赛成绩中心

网址：https://huang-yuxiao.github.io/agricola4/

GitHub Pages提供前端，腾讯云Python接口和SQLite独立运行，关闭聊天不影响上报与保存。
两个榜单共用道馆，点击道馆即可修改，无需密码；player_profiles保存道馆并记录修改审计，使用expectedGym防止并发覆盖。
ELO右侧显示平均分、最高分；赛季按本届全部实际参赛局统计，包括指定NPC、锁分后NPC与超过15局。得分不含Studio奖励或用于区分名次的0.001。
全阶段历史统计已从eloall.py的174场内嵌对局与legal_matches.txt、legal_matches_2026spring.txt还原，并逐人核对128人起点局数、均分一致；historical-stats.json保存起点原始总分与最高分，后续按全部存续逐局记录重算。旧数据按原程序的读取范围还原，未改变128人ELO或局数。
玩家无需登录，支持2—4人对局。上报页仅选择已登记ID；管理员先在全阶段榜添加新ID，初始500分、0局。历史ID与别名自动保留。registered_players表持久保存新增ID。

## 计分
秋季初始450分，每对手K为二人20、三人15、四人40/3；对手使用赛前全阶段分。Studio本局计分选手额外+5，仅加赛季。
全阶段按经验分档：2/3/4人局前10局每对手K为60/45/40，第11—20局为40/30/(80/3)，之后20/15/(40/3)。
达到550封顶锁分，否则15计分局后锁分，之后自动NPC；550先按计分局数少、再按冲线时间早排序。
管理员可逐局指定NPC，本局不增加其秋季分、计分局数或+5；全阶段、实打局数及活跃奖照常统计。修改旧局按原顺序重算后续榜单。
无需填写时间，以服务器上报时间记账；平局使用竞赛排名，如1、2、2、4。同分不同名次自动加0.001区分。

## 维护
config.js配置HTTPS接口地址。frontend-source.zip是完整前端源代码；npm install及npm run build后发布dist文件。
server.tar.gz是后台源码、服务配置和测试，不包含数据库或凭据。正式数据库/var/lib/agricola4/scores.sqlite3，备份/var/backups/agricola4。
管理员密码只在服务器/etc/agricola4/admin.env以哈希配置，新增ID、修改、调序、指定NPC和删除共用。操作留存submission_audit。
删除可用restore_submission.py恢复；deleted_submissions保留原始记录，submission_order保留计分顺序。桌号全平台互斥。
榜单每10分钟刷新，也可手动刷新；切回页面不自动刷新。
HTTPS通过certbot自动续期，Caddy配置位于/opt/agricola4/Caddyfile。
