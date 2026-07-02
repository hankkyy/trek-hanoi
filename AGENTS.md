# TREK Hanoi — 河内旅行规划平台

> 基于 [TREK](https://github.com/mauriceboe/TREK) (AGPL v3)，部署于 Oracle Cloud 免费实例

## 在线访问

| 服务 | 地址 | 说明 |
|------|------|------|
| TREK 规划台 | `http://147.224.9.74:3000` | 私人规划，需登录 |
| hanoi-vercel | https://hanoi-vercel.vercel.app | 公开分享页 |

登录：`hank@trek.local` / `123`

---

## 架构

```
Oracle Cloud Always Free (us-sanjose-1)
└── VM.Standard.E2.1.Micro (1 OCPU, 1GB RAM, 2GB swap)
    └── Ubuntu 22.04
        └── Docker: mauriceboe/trek:latest (v3.1.4)
            ├── Port 3000
            ├── Volume: trek-data → /app/data (SQLite, ~1MB)
            └── Volume: trek-uploads → /app/uploads
```

---

## 服务器信息

| 项目 | 值 |
|------|-----|
| IP | `147.224.9.74` |
| SSH | `ssh -i ~/.ssh/trek-hanoi.key ubuntu@147.224.9.74` |
| 密钥 | `~/.ssh/trek-hanoi.key` |
| 实例名 | instance-20260702-1118 |

---

## ⚠️ 关键修复：COOKIE_SECURE=false

**不加这个环境变量，登录后会立即被踢出。**

原因：`NODE_ENV=production` 让 TREK 给 Cookie 加 `Secure` 标记。HTTP 访问时浏览器拒绝存储该 Cookie → 登录后 API 调用全部 401 → React 弹 "Couldn't reach the server" → 踢回登录页。

### 正确的 docker run 命令

```bash
docker run -d --name trek --restart unless-stopped \
  -p 3000:3000 \
  -e ADMIN_EMAIL=hank@trek.local \
  -e ADMIN_PASSWORD=123 \
  -e NODE_ENV=production \
  -e COOKIE_SECURE=false \
  -e ENCRYPTION_KEY=af8d6ed365a04e9bf46be39d049359d284297c1160fdd6648e3e28b63d29bd87 \
  -v trek-data:/app/data \
  -v trek-uploads:/app/uploads \
  mauriceboe/trek:latest
```

---

## 已导入数据

从 hanoi-vercel 导入的全部数据（2026-07-02）：

| 模块 | 数量 | 说明 |
|------|------|------|
| 🗺️ 地点（带GPS坐标） | 72 | 景点/餐厅/SPA/商店，含经纬度+详细描述 |
| 📅 日程安排 | 38条 | 4天 × 每天8-11个活动 |
| 🎫 预订 | 4个 | ZH107去程 + ZH106回程 + Rey Hotel + 下龙湾游轮 |
| 💰 预算 | 11项 | ¥7,000+，覆盖机票/住宿/签证/活动/餐饮/交通等 |
| 🧳 打包清单 | 13项 | 证件/机票/住宿/活动/通讯/衣物/App |
| ✅ 待办事项 | 55项 | 美食打卡/景点打卡/购物/按摩/咖啡 |
| 📝 Collab 笔记 | 9篇 | 交通防坑/下龙湾Tips/支付现金/免税购物/入境申报/越南语速成/必吃餐厅/按摩推荐/伴手礼 |

### 数据格式化注意事项

- **Collab 笔记**：TREK 使用 markdown 渲染。`- ` 开头是列表，段落之间需空行（`\n\n`）。
- **地点描述**：`\n` 换行在 TREK 中不渲染。用 markdown 双换行（`\n\n`）分段。
- **不要用 `<br>` 标签**：TREK 会转义 HTML。

---

## 常用命令

```bash
# 查看容器状态
ssh -i ~/.ssh/trek-hanoi.key ubuntu@147.224.9.74 "docker ps"

# 查看日志
ssh -i ~/.ssh/trek-hanoi.key ubuntu@147.224.9.74 "docker logs trek --tail 50"

# 重启
ssh -i ~/.ssh/trek-hanoi.key ubuntu@147.224.9.74 "docker restart trek"

# 操作数据库（写脚本→上传→运行）
# 1. 本地写 node 脚本
# 2. scp -i ~/.ssh/trek-hanoi.key script.js ubuntu@147.224.9.74:/tmp/
# 3. ssh ... "docker cp /tmp/script.js trek:/app/ && docker exec trek sh -c 'cd /app && node script.js'"
```

---

## 安全组规则

Oracle Cloud 控制台：Networking → vcn-20260702-1106 → Security Lists
- TCP 3000 → 0.0.0.0/0

---

## 数据来源

行程数据从 hanoi-vercel 的 CloudBase API 获取：
- Itinerary: `https://hanoi-d4gj8vd2q1e7a3dc0.service.tcloudbase.com/api/itinerary`
- Checklist: `https://hanoi-d4gj8vd2q1e7a3dc0.service.tcloudbase.com/api/checklist`
- Bucket List: `https://hanoi-d4gj8vd2q1e7a3dc0.service.tcloudbase.com/api/bucket-list`

---

## 关联仓库

- TREK 上游: https://github.com/mauriceboe/TREK
- 本项目: https://github.com/hankkyy/trek-hanoi
- hanoi-vercel: /Users/hankzhang/Desktop/hanoi-vercel
