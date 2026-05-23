# 📒 Simple Notes Worker

Simple web notebook deployed on cloudflare worker

一个基于 Cloudflare Workers + KV 的轻量级在线笔记应用，支持加密保护、多设备访问、自动保存。

### 主要功能

- 快速创建和编辑笔记（类似 Pastebin / 临时笔记）
- 支持笔记加密（密码保护）
- 目录页自动列出所有笔记（按更新时间排序）
- 自动保存 + 删除笔记
- 响应式设计，支持深色模式
- **新增防爬虫保护**（防止垃圾文件自动生成）

---

### 安全特性（本版本重点优化）

- **Rate Limit**：同一 IP 每分钟最多 8 次操作
- **User-Agent 过滤**：屏蔽常见爬虫和自动化工具
- **可疑路径拦截**：阻止 `wp-`、`admin`、`xmlrpc`、`.env` 等常见垃圾路径
- CSP 安全头 + XSS 防护
- 严格的笔记名称验证

---

### 部署方法

#### 1. 创建 KV Namespace
1. 进入 [Cloudflare Dashboard](https://dash.cloudflare.com)
2. 选择你的账号 → **KV** → **Create Namespace**
3. 命名为 `NOTES_KV`（名称可自定义）

#### 2. 部署 Worker

1. 在 Cloudflare Workers 中新建 Worker
2. 将 `worker.js` 中的代码全部替换为本项目代码
3. 绑定 KV：
   - Variables → KV Namespace Bindings
   - Variable name: `NOTES_KV`
   - KV Namespace: 选择你刚才创建的 `NOTES_KV`

#### 3. 设置环境变量（可选但推荐）

| 变量名           | 说明               | 示例值          |
|------------------|--------------------|-----------------|
| `FIXED_PASSWORD` | 全局访问密码       | `yourpassword123` |

---

### 使用方法

- 访问 `你的域名.com` 查看所有笔记目录
- 直接访问 `你的域名.com/笔记名称` 创建或编辑笔记
- 勾选「密码保护」后保存，之后访问需要输入密码
- 支持中文笔记名称

---

### 项目文件

- `worker.js` —— 主程序代码（Cloudflare Worker）
- `README.md` —— 本说明文件

---

### 注意事项

1. **首次部署后**建议测试以下功能：
   - 普通笔记创建与保存
   - 加密笔记的密码保护是否正常
   - 目录页是否能正常刷新

2. 如遇到加密笔记无法弹出密码框，可尝试清除浏览器缓存或联系开发者。

3. 当前版本已针对爬虫进行防护，大幅减少垃圾笔记生成。

---

### License

MIT License

---

**作者**：基于社区版本优化  
**最后更新**：2026年5月
