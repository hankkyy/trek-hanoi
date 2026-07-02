# TREK Hanoi

> 河内 × 下龙湾 旅行规划 — TREK 云端部署版  
> 基于 [TREK](https://github.com/mauriceboe/TREK) 的 Fork，部署于 Fly.io

## 在线访问

**https://trek-hanoi.fly.dev**

## 项目结构

```
trek-hanoi/
├── fly.toml              # Fly.io 部署配置
├── theme.css             # 自定义主题（方案三换皮）
└── README.md
```

## 部署

```bash
flyctl deploy --app trek-hanoi
```

## 数据

- 存储：Fly.io 持久卷 (1GB)
- 自动快照：每 24h
- 行程数据源：hanoi-vercel CloudBase API

## 关联项目

- **公开分享页**: https://hanoi-vercel.vercel.app
- **上游项目**: https://github.com/mauriceboe/TREK

---

*Powered by TREK + Fly.io*
