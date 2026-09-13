# PlaneFilter — OpenClaw Skill ✈️

**Development, Issues & Pull Requests:**  
https://github.com/aa22396584/planefilter-openclaw-skill

**Mirrors:**  
[GitLab](https://gitlab.com/aa22396584/planefilter-openclaw-skill) ·
[Codeberg](https://codeberg.org/ImL1s/planefilter-openclaw-skill)


> **Why this GitHub home?** Public development moved here from [`ImL1s/planefilter-openclaw-skill`](https://github.com/ImL1s/planefilter-openclaw-skill) because that GitHub account is currently restricted (anonymous visitors get 404 on the profile and many assets). This is the same project. Please open Issues and Pull Requests here.

> Flight aircraft type lookup with multi-source confidence scoring.
> 多資料來源航班機型查詢，附信心評分。

**Query any flight number** → get aircraft type, equipment changes, and confidence scoring from multiple aviation data sources — all within your OpenClaw agent.

**輸入任何航班號** → 從多個航空資料來源取得機型、換機資訊、信心評分 — 全在 OpenClaw agent 裡完成。

## Install / 安裝

### Option 1: ClawHub（推薦）

```bash
npx clawhub install planefilter
```

### Option 2: Git clone

```bash
git clone https://github.com/aa22396584/planefilter-openclaw-skill.git ~/.openclaw/workspace/skills/planefilter
```

### Option 3: Let the agent install it / 讓 agent 幫你裝

Tell your OpenClaw agent / 告訴你的 OpenClaw agent：

> "Install the planefilter skill from https://github.com/aa22396584/planefilter-openclaw-skill"

The agent will clone and place it in the correct directory.
Agent 會自動 clone 並放到正確目錄。

### After install / 安裝後

```bash
# Verify it loaded / 確認已載入
openclaw skills list | grep planefilter

# Set up your API key / 設定 API key
openclaw skills onboard
```

## Setup API Keys / 設定 API Key

| Key | Required / 必要 | Free Tier / 免費額度 | Get One / 取得方式 |
|-----|----------|-----------|---------|
| `RAPIDAPI_KEY` | ✅ Yes | 150 req/month | [AeroDataBox on RapidAPI](https://rapidapi.com/aedbx-aedbx/api/aerodatabox) |
| `AIRLABS_KEY` | Optional / 選用 | 150 req/month | [AirLabs](https://airlabs.co/signup) |

Set via `openclaw skills onboard` (auto-prompted) or manually in `~/.openclaw/openclaw.json`.
透過 `openclaw skills onboard`（自動引導）或手動編輯 `~/.openclaw/openclaw.json` 設定。

## Usage / 用法

Just ask your OpenClaw agent naturally / 用自然語言問你的 agent：

- *"What aircraft is CI101 using today?"*
- *"查一下 BR108 的機型"*
- *"Check if EVA Air flight 12 has an equipment change"*
- *"長榮 12 有沒有換機型？"*

The agent will call `search_flight.js` and interpret the results.
Agent 會自動呼叫 `search_flight.js` 並解讀結果。

## What It Does / 功能

1. **Parallel query / 平行查詢** — OpenSky (free) + AeroDataBox + AirLabs
2. **Confidence scoring / 信心評分** — Weighted votes, agreement detection / 加權投票、一致性偵測
3. **Equipment change / 換機偵測** — Detects scheduled vs actual aircraft swap (upgrade/downgrade/lateral) / 偵測排班與實際機型差異
4. **ICAO normalization / 代碼正規化** — Converts model names (e.g. "Airbus A330-300") to ICAO codes ("A333")
5. **Proactive monitoring / 主動監控** — Cron jobs push alerts to Telegram/Discord / 排程推送通知

## Example Output / 範例輸出

```json
{
  "flightNumber": "CI101",
  "airline": "China Airlines",
  "origin": "NRT",
  "destination": "TPE",
  "aircraftType": "A333",
  "registration": "B-18311",
  "confidence": 0.6,
  "equipmentChange": null,
  "sources": ["aerodatabox"]
}
```

## Cron Monitoring / 排程監控

```bash
# 每 2 小時查一次 CI101，有變化推送 Telegram
openclaw cron add --name "Watch CI101" \
  --every 2h --session isolated \
  --message "Use planefilter to watch CI101. Report if equipment changed." \
  --announce --channel telegram
```

---

## Support / 支持

If this project saved you some time, you can [buy me a coffee](https://buymeacoffee.com/iml1s).

如果這個專案幫你省了點時間，可以請我喝杯咖啡。

## License

MIT
