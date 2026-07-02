# TREK Hanoi

> 河内 × 下龙湾 旅行规划 — TREK 云端部署版  
> 基于 [TREK](https://github.com/mauriceboe/TREK) 部署于 Oracle Cloud 永久免费实例

## 在线访问

| 服务 | 地址 | 说明 |
|------|------|------|
| TREK 规划台 | http://147.224.9.74:3000 | 私人规划，需登录 |
| hanoi-vercel | https://hanoi-vercel.vercel.app | 公开分享页 |

## 架构

```
Oracle Cloud Always Free
└── VM.Standard.E2.1.Micro (1 OCPU, 1GB + 2GB swap)
    └── Ubuntu 22.04 + Docker
        └── TREK (NestJS + React + SQLite + WebSocket)
```

## 项目结构

```
trek-hanoi/
├── AGENTS.md             # AI agent 上下文文档
├── fly.toml              # Fly.io 配置（历史，现已迁移 Oracle）
├── README.md
└── .gitignore
```

## 数据

- 存储：Docker volume `trek-data`（SQLite）
- 数据源：从 hanoi-vercel CloudBase API 实时导入
- 行程：4 天 38 个地点

## 关联项目

- 公开分享页: https://hanoi-vercel.vercel.app
- TREK 上游: https://github.com/mauriceboe/TREK

---

*Powered by TREK + Oracle Cloud Always Free*
