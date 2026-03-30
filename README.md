# 🧄 china-holiday

**Chinese Official Holidays & Workdays Query — Zero Dependencies, Python 3 Only.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![OpenClaw Skill](https://img.shields.io/badge/OpenClaw-Skill-green.svg)](https://github.com/openclaw/openclaw)
[![Python 3](https://img.shields.io/badge/Python-3.7+-orange.svg)](https://www.python.org)
[![API: timor.tech](https://img.shields.io/badge/API-timor.tech-brightgreen.svg)](https://timor.tech/api/holiday)

Query Chinese official holidays, weekends, shift-workdays (调休/补班), and salary multipliers — covering 2013 to present, updated annually.

## 📌 What It Solves

| Question | Answer |
|----------|--------|
| "Is today a workday?" | `info` — returns type + holiday details |
| "Any holidays coming up?" | `next` — next holiday with shift info |
| "When is the next workday?" | `workday` — skips weekends & holidays |
| "Full holiday list for 2026?" | `year 2026` — all holidays + shifts |
| "清明节放几天？" | `info 2026-04-04` — vacation days + wage multiplier |

## 🚀 Installation

### ⚡ One-Click Install for OpenClaw

Copy and paste this prompt directly into OpenClaw:

```
Install the china-holiday skill for me. The SKILL.md is at:
https://raw.githubusercontent.com/DecemJiang/china-holiday/main/china-holiday/SKILL.md

Save it to ~/.openclaw/skills/china-holiday/SKILL.md
and restart the gateway.
```

### OpenClaw (Recommended)

```bash
# Option 1: clawhub (auto-installed skill)
npx clawhub install china-holiday-decem

# Option 2: Copy SKILL.md to your skills directory
mkdir -p ~/.openclaw/skills/china-holiday
cp china-holiday/SKILL.md ~/.openclaw/skills/china-holiday/

# Restart gateway
openclaw gateway restart
```

### Manual (Any Python 3 Environment)

```bash
# Clone the repo
git clone https://github.com/DecemJiang/china-holiday.git
cd china-holiday/china-holiday

# Run directly — no pip, no dependencies
python3 china-holiday/scripts/china_holiday.py info 2026-10-01
python3 china-holiday/scripts/china_holiday.py next
python3 china-holiday/scripts/china_holiday.py year 2026
```

### Claude Code / Codex

```bash
# Download the script
curl -fsSL https://raw.githubusercontent.com/DecemJiang/china-holiday/main/china-holiday/scripts/china_holiday.py \
  -o /tmp/china_holiday.py

# Use it in your agent
python3 /tmp/china_holiday.py info 2026-10-01
```

### As a Python Module

```python
import subprocess
result = subprocess.run(
    ["python3", "china-holiday/china-holiday/scripts/china_holiday.py", "next"],
    capture_output=True, text=True
)
print(result.stdout)
```

## 📖 Usage

```bash
python3 china-holiday/scripts/china_holiday.py <command> [args]
```

| Command | Description | Example |
|---------|-------------|---------|
| `info [date]` | Holiday info for a date | `info 2026-10-01` or `info` (today) |
| `batch <dates...>` | Batch query | `batch 2026-05-01 2026-06-01` |
| `next [date]` | Next holiday (+ shift info) | `next 2026-04-01` |
| `workday [date]` | Next workday | `workday` |
| `year [yyyy or yyyy-mm]` | Full year/month holidays | `year 2026` or `year 2026-02` |
| `tts` | Today's holiday briefing | — |
| `tts-next` | Next holiday briefing | — |
| `tts-tomorrow` | Tomorrow's briefing | — |

### Output Example

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
$ python3 china-holiday/scripts/china_holiday.py tts

🔊 播报内容:
还有5天就是清明节了，别着急。
```

## 📋 Data Fields

### type (日期类型)

| Value | Meaning |
|-------|---------|
| `0` | 工作日 (Workday) |
| `1` | 周末 (Weekend) |
| `2` | 节日 (Holiday) |
| `3` | 调休/补班 (Shift workday) |

### holiday (节假日详情, null if none)

| Field | Description |
|-------|-------------|
| `holiday` | `true` = holiday, `false` = shift |
| `wage` | Salary multiplier (2 = double pay, 3 = triple) |
| `after` | `true` = after vacation, `false` = before |
| `target` | Which holiday this shift belongs to |

## ⚠️ Notes

- **API rate limit**: 10,000 requests/day/IP — plenty for personal use
- **Date format**: `YYYY-MM-DD` (e.g., `2026-10-01`)
- **Full year query**: `year 2026` auto appends trailing slash → `/year/2026/`
- **Year + month**: `year 2026-02` returns February 2026 only

## 🙏 Credits

Holiday data powered by **[timor.tech](https://timor.tech)** — a free, non-commercial API maintained by an independent developer since 2018, updated every year. Deeply appreciated 🌿

## 📁 Project Structure

```
china-holiday/               # Repository root
├── SKILL.md                  # OpenClaw skill definition
├── README.md                 # English documentation
├── README_zh.md              # 中文自述文件
├── VERSION                  # Current version
├── CHANGELOG.md             # 版本历史
├── LICENSE                  # MIT License
└── china-holiday/           # Skill package
    ├── SKILL.md
    ├── .gitignore
    └── scripts/
        └── china_holiday.py  # Main script (stdlib only)
```

## License

MIT — see [LICENSE](LICENSE)
