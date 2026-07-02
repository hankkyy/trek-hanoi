# TREK Hanoi — 河内旅行规划平台

> 基于 [TREK](https://github.com/mauriceboe/TREK) (AGPL v3)，部署于 Oracle Cloud 免费实例

## 在线访问

| 服务 | 地址 | 说明 |
|------|------|------|
| TREK 规划台 | `http://147.224.9.74:3000` | 私人规划，需登录 |
| hanoi-vercel | https://hanoi-vercel.vercel.app | 公开分享页，不动 |

登录：`hank@trek.local` / `123`

## 架构

```
Oracle Cloud (永远免费)
└── VM.Standard.E2.1.Micro (1 OCPU, 1GB RAM, 2GB swap)
    └── Ubuntu 22.04
        └── Docker: mauriceboe/trek:latest
            ├── Port 3000
            ├── Volume: trek-data → /app/data (SQLite)
            └── Volume: trek-uploads → /app/uploads
```

## 服务器信息

| 项目 | 值 |
|------|-----|
| IP | `147.224.9.74` |
| SSH | `ssh -i ~/.ssh/trek-hanoi.key ubuntu@147.224.9.74` |
| 密钥 | `~/.ssh/trek-hanoi.key` |
| 区域 | us-sanjose-1 |
| 实例名 | instance-20260702-1118 |

## 常用命令

```bash
# 查看容器状态
ssh -i ~/.ssh/trek-hanoi.key ubuntu@147.224.9.74 "docker ps"

# 查看日志
ssh -i ~/.ssh/trek-hanoi.key ubuntu@147.224.9.74 "docker logs trek --tail 50"

# 重启
ssh -i ~/.ssh/trek-hanoi.key ubuntu@147.224.9.74 "docker restart trek"

# 直接操作数据库
ssh -i ~/.ssh/trek-hanoi.key ubuntu@147.224.9.74 \
  "docker exec trek node -e \"
    const db = require('better-sqlite3')('/app/data/travel.db');
    // 你的查询
    db.close();
  \""

# 重新导入行程（从 hanoi-vercel API）
ssh -i ~/.ssh/trek-hanoi.key ubuntu@147.224.9.74 \
  "docker exec trek node /app/import-from-api.js"
```

## 安全组规则

Oracle Cloud 控制台：Networking → vcn-20260702-1106 → Security Lists → 已有规则
- TCP 3000 → 0.0.0.0/0

## 数据来源

行程数据从 hanoi-vercel 的 CloudBase API 实时拉取：
- API: `https://hanoi-d4gj8vd2q1e7a3dc0.service.tcloudbase.com/api/itinerary`
- 38 个地点，4 天

## 方案规划

```
当前: 方案一 ✅
  - TREK 原版在 Oracle Cloud 上跑
  - hanoi-vercel 保持不变做公开分享页

未来: 方案三 (Fork 换皮)
  - Fork TREK → 改 CSS 变量换成金色主题
  - 变量位置: /client/src/index.css (:root 部分)
  - 配色参考: hanoi-vercel 的 --gold: #C8974A, --emerald: #1A6B4A
```

## 关联仓库

- TREK 上游: https://github.com/mauriceboe/TREK
- 本项目: https://github.com/hankkyy/trek-hanoi
- hanoi-vercel: /Users/hankzhang/Desktop/hanoi-vercel
