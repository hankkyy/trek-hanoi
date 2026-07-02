# TREK Hanoi

> 河内 × 下龙湾 旅行规划 — TREK 云端部署版  
> 基于 [TREK](https://github.com/mauriceboe/TREK) 部署于 Oracle Cloud 永久免费实例

## 在线访问

| 服务 | 地址 | 说明 |
|------|------|------|
| TREK 规划台 | http://147.224.9.74:3000 | 私人规划，需登录 |
| hanoi-vercel | https://hanoi-vercel.vercel.app | 公开分享页 |

## 登录

- 邮箱：`hank@trek.local`
- 密码：`123`

## 行程概览

**🇻🇳 河内 × 下龙湾** · 2026年7月23日–26日 · 4天4晚 · 子豪 & 小轰轰

### 数据总览

| 模块 | 数量 | 说明 |
|------|------|------|
| 🗺️ 地点 | 72 | 含GPS坐标、详细描述 |
| 📅 日程 | 38条 | 4天详细安排 |
| 🎫 预订 | 4项 | 往返机票 + 酒店 + 下龙湾游轮 |
| 💰 预算 | 11项 | 总计约¥7,000 |
| 🧳 打包清单 | 13项 | 分类管理 |
| ✅ 待办 | 55项 | 美食/景点/购物打卡 |
| 📝 笔记 | 9篇 | 交通/支付/餐厅/伴手礼攻略 |

## 架构

```
Oracle Cloud Always Free (us-sanjose-1)
└── VM.Standard.E2.1.Micro (1 OCPU, 1GB + 2GB swap)
    └── Ubuntu 22.04 + Docker
        └── TREK v3.1.4 (NestJS + React + SQLite + WebSocket)
```

## ⚠️ 关键配置

必须设置 `COOKIE_SECURE=false`，否则 HTTP 访问时登录 Cookie 会被浏览器丢弃，登录后立即被踢出。

## 项目结构

```
trek-hanoi/
├── AGENTS.md        # AI agent 上下文 + 部署文档
├── README.md
├── fly.toml         # Fly.io 配置（历史）
└── .gitignore
```

## 关联项目

- TREK 上游: https://github.com/mauriceboe/TREK
- hanoi-vercel: https://hanoi-vercel.vercel.app

---

*Powered by TREK + Oracle Cloud Always Free*
