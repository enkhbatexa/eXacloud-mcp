# eXa Cloud MCP — апп-аа нэг промптоор ажиллуул

[eXa Cloud](https://exacloud.mn) (Монгол) дээр GitHub репогоо **AI туслахаасаа шууд** ажиллуулна:
Claude, ChatGPT, Gemini, Codex. Сервер тохируулах, Docker, nginx, SSL — юу ч хийхгүй.

```
https://github.com/миний/апп-ыг eXa дээр ажиллуул
```

AI туслах репог уншаад тохирох багц, үнийг хэлнэ → та зөвшөөрнө → QPay-ийн холбоос гарна → банкны
апп-аараа төлнө → eXa өөрөө build хийж асаана → `https://апп.exa-vps.com` хаяг гарна (SSL автоматаар).

**MCP хаяг:** `https://panel.exacloud.mn/mcp` (OAuth — нууц үг, API түлхүүр хуулах хэрэггүй)

## 1. AI туслахаа холбох (нэг удаа)

**Claude апп / Claude.ai / Claude Desktop** (хамгийн энгийн, команд хэрэггүй) —
[Settings → Connectors](https://claude.ai/settings/connectors) → *Add custom connector* →
`https://panel.exacloud.mn/mcp` → Add → Connect. Дараа нь энгийн шинэ чат нээнэ.

**Claude Code** (зөвхөн терминалд, чат руу биш; зөвшөөртөл терминалыг хаахгүй)
```bash
claude mcp add --transport http exa https://panel.exacloud.mn/mcp
claude mcp login exa
```

**ChatGPT** (Plus, Pro, Business) — Settings → Apps & Connectors → Advanced settings → *Developer mode* →
Create → URL `https://panel.exacloud.mn/mcp`, Authentication: **OAuth** (client ID, secret хоосон).

**OpenAI Codex**
```bash
codex mcp add exa --url https://panel.exacloud.mn/mcp
codex mcp login exa
```

**Gemini CLI** — `~/.gemini/settings.json`:
```json
{ "mcpServers": { "exa": { "httpUrl": "https://panel.exacloud.mn/mcp" } } }
```
дараа нь Gemini дотор `/mcp auth exa`.

Хөтөч нээгдэж eXa-гийн **«AI туслах холбох»** хуудас гарна. Бүртгэлгүй бол утас эсвэл Google-ээр
бүртгүүлнэ (Google Authenticator-оор хамгаална). **Зөвшөөрөх** дарна.

> ⚠️ Терминалын командыг (`claude mcp …`) Claude апп-ын чат руу хуулж болохгүй — Claude тэдгээрийг өөрийн
> тусдаа орчинд ажиллуулах гэж оролдоод гацна. Claude апп-д зөвхөн Connectors-оор холбоно.

## 2. Ажиллуулах

AI туслахдаа (холбосон чатад) бичнэ:

> https://github.com/миний/апп-ыг eXa дээр ажиллуул: тохирох багц, үнийг хэлээд QPay-ийн холбоос өг, төлсний дараа
> https хаягийг нь өг. Хэрэв eXa-гийн хэрэгслүүд чамд байхгүй бол юу ч суулгах гэж бүү оролд — надад
> https://panel.exacloud.mn/mcp-г connector болгож нэмэх алхмыг товч зааж өгөөд хүлээ.

Дараа нь: «шинэ кодоо deploy хий», «лог харуул», «миний үйлчилгээ юу байна?».

| Хэрэгсэл | Юу хийнэ |
|---|---|
| `exa_plan_for_repo` | Репог уншаад орчин (Node.js / Python / Docker), порт, багц, сарын үнэ санал болгоно |
| `exa_deploy_app` | Апп захиалж QPay холбоос өгнө; төлбөр ормогц eXa өөрөө build хийж асаана |
| `exa_wait` | Төлбөр → build → `https://нэр.exa-vps.com` хүртэл дагана |
| `exa_app`, `exa_redeploy` | Төлөв, лог; шинэ commit-ийг дахин deploy |
| `exa_account`, `exa_prices`, `exa_orders` | Үйлчилгээ, үнэ, захиалга |

## Аюулгүй байдал

- AI туслах **чадахгүй**: юу ч устгах, зогсоох, тохиргоо өөрчлөх, захиалга цуцлах, нууц үг / SSH key /
  Authenticator / гишүүд өөрчлөх, мөнгө шилжүүлэх. Апп-ын нууц тохиргооны (env) утгыг харахгүй.
- Төлбөрийг **үргэлж та өөрөө** QPay-ээр төлнө (НӨАТ-ын баримт автоматаар).
- Холбоосыг хүссэн үедээ салгана: panel.exacloud.mn → Бүртгэл → Аюулгүй байдал → **AI туслахууд** → Салгах.
- OAuth 2.1 + PKCE; токен 1 цаг, refresh 30 хоног (ээлжлэн солигдоно).

## Үнэ

Апп платформ сард 6,900₮-өөс (НӨАТ-гүй, төлөхөд +10%). Үнэгүй туршилтын талаар hello@exacloud.mn-ээс асуугаарай. Vibe coding-ийн бүрэн танилцуулга: https://exacloud.mn/vibe-coding/ · бүрэн жагсаалт: https://exacloud.mn ·
гарын авлага: https://panel.exacloud.mn/static/guide.html#ai

---

### English

Deploy a GitHub repo to [eXa Cloud](https://exacloud.mn) (Mongolia) from your AI assistant. Add the remote MCP
server `https://panel.exacloud.mn/mcp` (OAuth) to Claude Code, Claude.ai, ChatGPT (developer mode), Codex or
Gemini CLI as shown above (in the Claude app use Settings → Connectors — never paste the terminal commands into a chat), approve it once on the eXa consent page, then ask: *“Deploy https://github.com/me/app
on eXa”*. The assistant suggests a plan and a price, gives you a QPay link, and after you pay, eXa builds the repo and
serves it at `https://<app>.exa-vps.com`. The assistant cannot delete, stop or reconfigure anything, cannot see your
app's secret env values, and never pays for you.

More: https://exacloud.mn/en/vibe-coding/ · Support: hello@exacloud.mn
