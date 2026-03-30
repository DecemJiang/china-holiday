# 🧄 china-holiday 中国节假日查询

**中国法定节假日查询 — 零依赖，仅 Python 3 标准库。**

[![MIT License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![OpenClaw 技能](https://img.shields.io/badge/OpenClaw-技能-green.svg)](https://github.com/openclaw/openclaw)
[![Python 3](https://img.shields.io/badge/Python-3.7+-orange.svg)](https://www.python.org)
[![数据来源 timor.tech](https://img.shields.io/badge/API-timor.tech-brightgreen.svg)](https://timor.tech/api/holiday)

查询中国法定节假日、调休补班、工作日及薪资倍数，数据覆盖 2013 年至今，每年自动更新。

## 🎯 能解决什么问题

| 你可能想问 | 对应命令 |
|-----------|---------|
| 今天上班吗？ | `info` |
| 最近有什么假期？ | `next` |
| 下一个工作日是哪天？ | `workday` |
| 2026年全部节假日？ | `year 2026` |
| 清明节放几天？ | `info 2026-04-04` |
| 国庆怎么调休？ | `next 2026-10-01` |
| 明天放假吗？ | `tts-tomorrow` |

## 📦 安装方式

### ⚡ OpenClaw 一键安装（复制这段话发给 AI 就行）

```
帮我安装 china-holiday 技能，SKILL.md 在这里：
https://raw.githubusercontent.com/DecemJiang/china-holiday/main/china-holiday/SKILL.md

保存到 ~/.openclaw/skills/china-holiday/SKILL.md，然后重启网关。
```

### OpenClaw（推荐方式）

```bash
# 方式一：从 clawhub 安装（自动获取更新）
npx clawhub install china-holiday-decem

# 方式二：手动复制 SKILL.md
mkdir -p ~/.openclaw/skills/china-holiday
cp china-holiday/SKILL.md ~/.openclaw/skills/china-holiday/

# 重启网关使技能生效
openclaw gateway restart
```

### 手动安装（任意 Python 3 环境）

```bash
# 克隆仓库
git clone https://github.com/DecemJiang/china-holiday.git
cd china-holiday/china-holiday

# 直接运行 — 无需 pip，无需任何依赖
python3 china-holiday/scripts/china_holiday.py info 2026-10-01
python3 china-holiday/scripts/china_holiday.py next
python3 china-holiday/scripts/china_holiday.py year 2026
```

### Claude Code / Codex 使用

```bash
# 下载脚本
curl -fsSL https://raw.githubusercontent.com/DecemJiang/china-holiday/main/china-holiday/scripts/china_holiday.py \
  -o /tmp/china_holiday.py

# 在 agent 中调用
python3 /tmp/china_holiday.py info 2026-10-01
```

### 作为 Python 模块调用

```python
import subprocess
result = subprocess.run(
    ["python3", "china-holiday/china-holiday/scripts/china_holiday.py", "next"],
    capture_output=True, text=True
)
print(result.stdout)
```

## 📖 命令详解

```bash
python3 china-holiday/scripts/china_holiday.py <命令> [参数]
```

| 命令 | 说明 | 示例 |
|------|------|------|
| `info [日期]` | 查询指定日期节假日信息 | `info 2026-10-01` 或 `info`（今天） |
| `batch <日期...>` | 批量查询多个日期 | `batch 2026-05-01 2026-06-01` |
| `next [日期]` | 下一节假日（含调休信息）| `next 2026-04-01` |
| `workday [日期]` | 下一工作日 | `workday` |
| `year [年份或月份]` | 全年/全月节假日列表 | `year 2026` 或 `year 2026-02` |
| `tts` | 今日节假日语音播报 | - |
| `tts-next` | 下一节假日语音播报 | - |
| `tts-tomorrow` | 明日节假日语音播报 | - |

### 输出示例

```bash
$ python3 china-holiday/scripts/china_holiday.py info 2026-10-01

📅 2026年10月1日
   【类型】节日 · 国庆节 · 周四
   🎉 国庆节  薪资×3
```

```bash
$ python3 china-holiday/scripts/china_holiday.py next 2026-04-01

🔍 从 2026年4月1日 起:

🎉 下一个节假日: 清明节 (2026-04-04)
   距离还有 3 天，薪资×2

🔧 调休: 清明节前补班 (2026-04-03) 薪资×1
```

```bash
$ python3 china-holiday/scripts/china_holiday.py year 2026

📆 查询结果 (39 个节假日):

  2026-01-01 🎉 元旦 薪资×3
  2026-01-02 🎉 元旦 薪资×2
  2026-02-15 🎉 春节 薪资×2
  2026-02-16 🎉 除夕 薪资×3
  ...
```

```bash
$ python3 china-holiday/scripts/china_holiday.py tts

🔊 播报内容:
还有5天就是清明节了，别着急。
```

## 📋 返回字段说明

### type（日期类型）

| 值 | 含义 |
|----|------|
| `0` | 工作日 |
| `1` | 周末 |
| `2` | 节日 |
| `3` | 调休/补班 |

### holiday 节假日详情（非节假日时为 `null`）

| 字段 | 说明 |
|------|------|
| `holiday` | `true` = 节日，`false` = 调休 |
| `wage` | 薪资倍数（2=双薪，3=三薪） |
| `after` | `true` = 假后补班，`false` = 假前补班 |
| `target` | 调休对应的节日名称 |

## ⚠️ 注意事项

- **API 限额**：每 IP 每天 10000 次，个人使用完全足够
- **日期格式**：`YYYY-MM-DD`（如 `2026-10-01`）
- **全年查询**：自动在年份后加斜杠，实际请求 `/year/2026/`
- **年月查询**：`year 2026-02` 返回 2026 年 2 月节假日

## 🙏 致谢

节假日数据来源于 **提莫的神秘小站**（[timor.tech](https://timor.tech)），由独立开发者利用业余时间维护，从 2018 年起每年更新节假日数据，服务完全免费、不限速、不登录、无广告。深深感谢作者的无私奉献 🌿

## 📁 项目结构

```
china-holiday/               # 仓库根目录
├── SKILL.md                  # OpenClaw 技能定义
├── README.md                 # 英文说明文档
├── README_zh.md              # 中文自述文件
├── VERSION                   # 当前版本
├── CHANGELOG.md              # 版本变更记录
├── LICENSE                   # MIT 许可证
└── china-holiday/           # 技能包（安装时使用此目录）
    ├── SKILL.md
    ├── .gitignore
    └── scripts/
        └── china_holiday.py  # 主脚本（仅标准库）
```

## 📄 开源协议

MIT License — 详见 [LICENSE](LICENSE)
