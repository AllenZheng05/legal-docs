---
name: legal-docs
description: 为 SaaS / 移动 App 项目生成隐私政策、用户服务协议、Cookie政策、数据处理协议、免责声明，并导出为 PDF。
---

# 法律文档生成 Skill

适用场景：需要为 SaaS 产品或移动 App 撰写合规法律文档，并导出为可交付的 PDF 文件。

---

## 第一步：向用户收集信息

开始生成前，必须先向用户确认以下信息（不可自行推断或从代码中读取）：

**必问项：**
1. **公司/主体全称**（法律文件落款）
2. **对外联系邮箱**（用户投诉/数据权利请求）
3. **收费标准**（套餐名称、价格、付款周期）
4. **产品名称**（App/系统名称）
5. **数据存储服务商**（例：腾讯云、阿里云、AWS）及节点地区

**选问项（有助于提升文档精准度）：**
- 是否有跨境数据传输？
- 是否使用第三方分析/广告 SDK（如 Firebase、友盟）？
- 公司地址（用于用户服务协议落款）
- 文档生效日期（默认当天）

---

## 第二步：生成 Markdown 文档

在项目根目录下创建 `legal/` 文件夹，依次生成以下5份文档：

### 文档1：隐私政策

核心章节结构（按顺序）：
1. 引言：产品名称、适用平台（H5/iOS/Android）
2. 我们收集的信息：按类别列表（基本信息、证件信息、健康/敏感数据、日志）
3. 如何使用信息：目的与信息类型对应表
4. 敏感个人信息的处理：脱敏展示方式、额外保护措施
5. 信息存储：存储地区、各类数据保留期限表
6. 信息共享：列举合法共享情形（服务商、法律要求、紧急情况、用户同意）
7. 安全保护：技术措施列表
8. 用户权利：PIPL 八项权利 + 响应时限（15个工作日）
9. 未成年人：说明系统仅供成年工作人员
10. 政策更新：重大变更提前30天通知
11. 联系我们：公司名、邮箱、地址

**写作要点：**
- 健康医疗数据、身份证号码必须标注为"敏感个人信息"
- 数据保留期限要具体（在院档案持续保存；离院后≥5年；护理记录≥3年）
- 用户权利章节必须包含撤回同意权、可携带权

### 文档2：用户服务协议

核心章节结构：
1. 引言：甲乙双方定义（服务方 ←→ 机构/用户）
2. 定义：系统、服务期限、授权床位等术语
3. 账号注册与管理：创建、安全、注销
4. 服务内容：功能列表
5. **收费标准与支付**（必须使用用户确认的价格）
6. 用户义务：合法合规使用、数据录入责任、禁止行为
7. 服务方义务：SLA、数据安全、技术支持、数据备份
8. 知识产权：软件归服务方；业务数据归用户
9. 服务中止与终止：到期、违规、终止后数据处理（30天导出窗口+90天删除）
10. 免责声明：不可抗力、数据准确性、间接损失、赔偿上限
11. 争议解决：适用中国大陆法律，服务方住所地法院

**写作要点：**
- 收费表格必须含"月付/年付/多年付"三列，价格来自用户确认
- 退款政策：7天内系统原因可全额退，超7天按日退剩余
- 数据归属一定要明确：业务数据所有权归用户（养老机构）

### 文档3：Cookie 政策

核心章节结构：
1. 什么是 Cookie
2. 使用的 Cookie 类型（分四类：必要性/功能性/分析性/广告）
3. 第三方 Cookie（列明使用的第三方 SDK）
4. 如何管理 Cookie（浏览器操作路径）
5. Cookie 与个人信息（明确 Cookie 中不含哪些敏感数据）
6. 联系方式

**写作要点：**
- 必要性 Cookie 列出实际使用的存储 key 名称（如 `ec_hospital`、`ec_user`）
- 如果没有广告/分析 Cookie，明确写"本系统不使用"
- 注明 Cookie 政策仅适用于 H5 网页端，原生 App 使用 localStorage

### 文档4：数据处理协议（DPA）

适用场景：产品以 B2B 模式运营，代替客户处理其用户的个人信息时需要（PIPL 第21条）。

核心章节结构：
1. 引言：引用 PIPL 第21条，界定控制方/处理方
2. 委托处理的个人信息范围：分类列表，标注是否敏感
3. 处理目的与方式：明确处理方不得自行决定处理目的
4. 数据控制方的权利与义务（合法收集、告知、敏感信息授权）
5. 数据处理方的义务（按指令、保密、安全措施、72h 安全事件通知、转委托限制）
6. 数据跨境说明（如不出境，明确写明）
7. 数据留存与删除（服务终止后：30天导出窗口 + 60天内永久删除 + 出具证明）
8. 审计与检查权
9. 责任承担
10. 协议期限与争议解决

**写作要点：**
- 转委托（如使用腾讯云/阿里云）需在协议中披露具体服务商
- "72小时安全事件通知"是 PIPL 的法定要求，必须写入
- 数据删除需出具"数据删除证明"，这是 B2B 场景的常见要求

### 文档5：免责声明

核心章节结构：
1. 系统定位说明（明确不是医疗系统/医疗器械）
2. 健康数据免责（非医疗建议、数据准确性由录入方负责、紧急情况拨120）
3. 系统可用性免责（不可抗力、数据丢失、兼容性）
4. 用户操作免责（录入责任、账号安全、权限管理）
5. 第三方服务免责（底层云服务商）
6. 法律合规责任（机构自行合规、护理规程合规）
7. 知识产权声明
8. 联系方式

**写作要点：**
- 健康类产品必须明确"不构成医疗诊断或医疗建议"
- 紧急情况处理要有明确指引（先拨120，再在系统中记录）

---

## 第三步：导出 PDF

确认系统安装了 Chrome 和 Python3（`python3 -c "import markdown"` 验证，没有则 `pip3 install markdown`）。

**生成单份（以"用户服务协议"为例）：**

```bash
# 1. 生成 HTML
python3 - <<'EOF'
import markdown, pathlib
CSS = """/* 把下面 CSS 模板粘贴到这里 */"""
md = pathlib.Path("legal/用户服务协议.md").read_text("utf-8")
body = markdown.markdown(md, extensions=["tables","fenced_code","nl2br"])
pathlib.Path("legal/用户服务协议.html").write_text(
    f"<!DOCTYPE html><html><head><meta charset='utf-8'><style>{CSS}</style></head><body>{body}</body></html>", "utf-8")
EOF

# 2. HTML → PDF（macOS Chrome 路径）
CHROME="/Applications/Google Chrome.app/Contents/MacOS/Google Chrome"
"$CHROME" --headless=new --disable-gpu --no-sandbox \
  --print-to-pdf="legal/用户服务协议.pdf" --print-to-pdf-no-header \
  "file://$(pwd)/legal/用户服务协议.html" 2>/dev/null

# 3. 清理
rm legal/用户服务协议.html
```

**CSS 模板（粘贴到上方 CSS 变量中）：**

```css
@page { margin: 22mm 20mm; size: A4; }
body { font-family: "PingFang SC","Hiragino Sans GB","Microsoft YaHei",Arial,sans-serif;
       font-size: 13px; line-height: 1.8; color: #1a1a1a; }
h1 { font-size: 20px; font-weight: 700; color: #0D9488;
     border-bottom: 2px solid #0D9488; padding-bottom: 8px;
     margin-top: 36px; text-align: center; }
h2 { font-size: 15px; font-weight: 700; color: #0369A1;
     margin-top: 28px; border-left: 4px solid #0369A1; padding-left: 10px; }
h3 { font-size: 13.5px; font-weight: 700; color: #333; margin-top: 18px; }
h4 { font-size: 13px; font-weight: 700; color: #555; margin-top: 14px; }
table { border-collapse: collapse; width: 100%; margin: 12px 0; font-size: 12px; }
th { background: #0D9488; color: white; padding: 7px 10px; text-align: left; }
td { padding: 6px 10px; border-bottom: 1px solid #e0e0e0; vertical-align: top; }
tr:nth-child(even) td { background: #f7fafa; }
code { background: #f0f0f0; padding: 2px 5px; border-radius: 3px; font-size: 11px; }
blockquote { border-left: 4px solid #0D9488; margin: 8px 0; padding: 8px 14px;
             background: #f0fdfb; color: #444; border-radius: 0 6px 6px 0; font-size: 12px; }
ul, ol { padding-left: 22px; margin: 6px 0; }
li { margin: 4px 0; }
hr { border: none; border-top: 1px solid #ddd; margin: 20px 0; }
p { margin: 6px 0; }
strong { color: #0369A1; }
```

> **主题色说明：** 当前 CSS 使用青绿色 `#0D9488` 和深蓝 `#0369A1`，与本项目官网主题一致。如需其他配色，全局替换这两个颜色值即可。

---

## 更新已有文档

| 场景 | 需要更新 |
|------|---------|
| 收费标准变更 | `legal/用户服务协议.md` → 重新导出 PDF |
| 公司名称/邮箱确定 | 全部5份 → 全局替换占位符 `【公司名称】`/`【联系邮箱】` |
| 新增数据收集项 | `隐私政策.md` + `数据处理协议.md` |
| 引入新第三方 SDK | `隐私政策.md` + `Cookie政策.md` |

---

## Gotchas

- Chrome `--headless=new` 在 macOS 会输出进程权限相关 stderr 警告，属正常现象，PDF 可正常生成，用 `2>/dev/null` 屏蔽即可。
- Markdown 表格必须启用 `extensions=["tables"]`，否则表格不渲染。
- Linux 环境导出中文 PDF 需先安装中文字体：`apt-get install fonts-noto-cjk`。
- `file://` 路径必须是绝对路径，使用 `$(pwd)` 拼接。
