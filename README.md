# IR Tabletop Exercise Engine

A self-contained, browser-based incident response tabletop simulator powered by any LLM API. The AI plays a full threat actor — no hand-holding, no narration, no early reveals. Alerts render as realistic tool outputs (CrowdStrike, Splunk, Sentinel, syslog, email) and adapt dynamically to how your team responds.

---

> ⚠️ **API Key Safety**
> This tool sends your API key directly from your browser to your chosen provider — no proxy, no backend. That's by design.
> **Never paste your API key into a hosted instance of this tool that you don't personally control.** If someone has deployed this on Vercel, Netlify, GitHub Pages, or anywhere else, treat it as untrusted. Run it locally or from your own deployment only.

---

## Features

- **20+ threat scenarios** across ransomware, APT, BEC, supply chain, cloud, ICS, and more
- **Multi-provider support** — Claude, ChatGPT, DeepSeek, Qwen, Grok, Perplexity, Mistral, or any OpenAI-compatible endpoint
- **Tech stack mapping** — select your actual tooling (EDR, SIEM, IAM, network, cloud, email, backup); the AI exploits your real gaps
- **Realistic alert rendering** — alerts auto-format as CrowdStrike detections, Splunk searches, Sentinel incidents, syslog entries, terminal output, or email notifications based on context
- **MITRE ATT&CK tagging** — every inject labeled with technique IDs (T####.###)
- **Adaptive adversary** — strong containment slows the attacker; weak response triggers escalation
- **After Action Review (AAR)** — full debrief with score, gaps, recommendations, and tool suggestions tied to your specific stack
- **Configurable exercise length** — 3 injects (~45 min), 4 injects (~60 min), or 5 injects (~90 min)

---

## Quick Start

1. Open `index.html` in any modern browser — no server, no build step, no dependencies
2. Select your AI provider and paste your API key
3. Click **Verify Connection** to confirm
4. Select your tech stack (check everything you actually have; mark gaps honestly)
5. Add environment notes (org size, compliance scope, SOC maturity, network topology)
6. Pick a threat scenario
7. Click **Initiate Exercise**

---

## Configuration

### AI Providers

| Provider | Default Model | Notes |
|---|---|---|
| Claude (Anthropic) | `claude-sonnet-4-20250514` | Recommended; best adversarial role fidelity |
| ChatGPT (OpenAI) | `gpt-4o` | Strong alternative |
| DeepSeek | `deepseek-chat` | Cost-effective |
| Qwen | `qwen-plus` | Via Alibaba DashScope |
| Grok | `grok-3` | Via xAI |
| Perplexity | `llama-3.1-sonar-large-128k-online` | Online-enabled |
| Mistral | `mistral-large-latest` | EU-hosted option |
| Custom | Any | Point at any OpenAI-compatible endpoint |

For **Claude**, the app uses the Anthropic messages API format (`x-api-key`, `anthropic-version`). All other providers use the OpenAI-compatible `Authorization: Bearer` format.

### Tech Stack Chips

Select every tool your environment actually uses. Red-labeled chips (`No EDR`, `No MFA`, `Flat Network`, etc.) represent known gaps — select these if they apply. The system prompt feeds this directly to the AI, which will target your specific weaknesses.

**Categories:**
- Endpoint / EDR
- SIEM / Log Management
- Identity / IAM
- Network / Perimeter
- Cloud
- Email / Collaboration
- Backup / DR
- Vulnerability Management

### Environment Notes

Free-text field for anything not covered by the chips: org size, compliance scope (PCI, HIPAA, FedRAMP), SOC maturity level, network segmentation, air-gapped segments, OT/IT boundaries, etc. The richer this is, the more realistic the scenario.

---

## Scenarios

### Malware & Ransomware
| ID | Name | Description |
|---|---|---|
| `ransomware-ras` | Ransomware (RaaS) | Double extortion, domain compromise |
| `wiper` | Destructive Wiper | State-sponsored, no ransom, max damage |
| `cryptominer` | Cryptominer Infestation | Resource abuse, persistent implant |
| `fileless` | Fileless Malware | Memory-only, LOLBins, evasion-heavy |

### Advanced Persistent Threat
| ID | Name | Description |
|---|---|---|
| `apt-espionage` | APT Espionage | Nation-state, IP theft, long dwell time |
| `apt-ics` | ICS / OT Attack | Industrial systems, PLC targeting |
| `apt-telco` | Telecom Infiltration | SS7 abuse, CDR exfiltration |

### Social Engineering
| ID | Name | Description |
|---|---|---|
| `bec` | Business Email Compromise | CFO fraud, wire transfer, impersonation |
| `vishing` | Vishing / Social Engineering | Help desk bypass, credential reset |
| `deepfake-bec` | AI-Assisted Phishing | Deepfake audio, spear-phish, MFA bypass |

### Identity & Access
| ID | Name | Description |
|---|---|---|
| `insider` | Malicious Insider | Privileged user, data theft, sabotage |
| `credential-stuffing` | Credential Stuffing | Breached passwords, account takeover |
| `cloud-iam` | Cloud IAM Compromise | SSRF, metadata theft, role escalation |

### Supply Chain & Third Party
| ID | Name | Description |
|---|---|---|
| `supply-chain` | Software Supply Chain | Trojanized package, build pipeline |
| `msp-breach` | MSP / Vendor Breach | RMM tool abuse, downstream pivot |
| `open-source` | OSS Dependency Attack | Typosquat, malicious npm/PyPI |

### Cloud & Infrastructure
| ID | Name | Description |
|---|---|---|
| `cloud-breach` | Cloud Misconfiguration | Public S3, exposed API, IMDS abuse |
| `container-escape` | Container / K8s Escape | Privileged pod, cluster takeover |
| `ci-cd` | CI/CD Pipeline Compromise | Build poisoning, secrets theft |

### Web & Application
| ID | Name | Description |
|---|---|---|
| `web-app` | Web Application Attack | SQLi, SSRF, RCE, data breach |
| `api-abuse` | API Abuse / BOLA | Broken object auth, mass data scrape |

### Physical & Hybrid
| ID | Name | Description |
|---|---|---|
| `physical-breach` | Physical Breach + Pivot | Dropped USB, rogue device, office intrusion |
| `telecom-sim` | SIM Swap + Account Takeover | Mobile carrier abuse, MFA bypass |

---

## Exercise Flow

```
┌─────────────────────────────────────────────────────────────┐
│                      EXERCISE FLOW                          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  1. INITIATE          AI delivers first inject cold         │
│     │                 (no preamble, no "exercise begins")   │
│     ▼                                                       │
│  2. INJECT            Realistic alert panel rendered        │
│     │                 MITRE TTP tags labeled                │
│     │                 Focused operational question asked    │
│     ▼                                                       │
│  3. TEAM RESPONSE     Your team types their actions         │
│     │                                                       │
│     ▼                                                       │
│  4. ADAPT             Strong response → attacker pivots     │
│     │                 Weak response  → attacker escalates   │
│     │                 Excellent IR   → early AAR (reward)   │
│     ▼                                                       │
│  5. REPEAT            N injects total (3, 4, or 5)          │
│     ▼                                                       │
│  6. AAR               Score, gaps, recs, key takeaways      │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Inject Format

Each inject includes:
- `[INJECT X/N]` header
- `TTPs: T####.###, ...` — MITRE technique IDs
- One or more realistic alert blocks auto-styled to match your stack
- A specific operational question (not "what do you do?" — e.g., "WS-ENG-05 is still live. What containment steps do you take, in what order? Who do you notify?")

### Alert Rendering

The engine auto-detects which tool surface the alert and renders accordingly:

| Source | Detection Signals | Style |
|---|---|---|
| CrowdStrike Falcon | `crowdstrike`, `falcon` | Red header, glow dot |
| Splunk | `index=`, `EventCode`, `splunk` | Amber header |
| Microsoft Sentinel | `sentinel`, `azure`, `kql` | Blue header |
| Microsoft Defender | `defender`, `mde` | Blue header |
| Email alert | `FROM:`, `SUBJECT:`, `@` | Purple header |
| Syslog / Auth log | Timestamp pattern, `sshd`, `auth.log` | Cyan header |
| Terminal / Shell | `$`, `#`, `powershell`, `bash` | Green terminal |

---

## After Action Review (AAR)

Delivered automatically after the final inject response. Includes:

- **Threat actor identity** — revealed post-exercise (group archetype, motivation)
- **Overall score** — 0–10
- **Overall summary** — 2–3 sentence assessment
- **What went well** — specific actions that helped
- **Gaps and missed steps** — what the team failed to do or detect
- **Recommendations** — numbered, tied to your exact stack
- **Tools to consider** — specific products or processes to fill gaps
- **Key takeaways** — 5 numbered lessons

---

## Architecture

Single HTML file. No framework, no build toolchain, no server.

```
┌─────────────────────────────────────────────────────────┐
│  Browser (index.html)                                   │
│                                                         │
│  ┌──────────────┐   ┌──────────────────────────────┐   │
│  │   Sidebar    │   │        Main Panel             │   │
│  │              │   │                               │   │
│  │  · Provider  │   │  · Chat / inject feed         │   │
│  │  · Env notes │   │  · Alert panel renderer       │   │
│  │  · Stack     │   │  · AAR renderer               │   │
│  │  · Scenario  │   │  · Team input                 │   │
│  │  · Config    │   │                               │   │
│  └──────────────┘   └──────────────────────────────┘   │
│                                                         │
│  State: history[], selectedStack Set, activeScenario    │
│                                                         │
│  API: fetch() → provider endpoint (direct, no proxy)    │
└─────────────────────────────────────────────────────────┘
```

The entire conversation history is maintained in a local `history[]` array and sent with every API call. The system prompt is rebuilt fresh on each call incorporating current stack and scenario selections.

---

## Security & Privacy Notes

- **API keys are never stored** — they exist only in the input field for the session duration
- **No backend, no telemetry** — all API calls go directly from your browser to the provider
- **History is session-only** — closing the tab clears everything
- Use a scoped API key with spend limits for shared/training environments
- The system prompt explicitly instructs the AI not to reveal threat actor identity until the AAR — this is enforced by prompt, not by code; a determined user could extract it via browser devtools

---

## Customization

### Adding Scenarios

Extend the `SCENARIOS` array in the script block:

```javascript
{ cat: 'Your Category', items: [
  { id: 'my-scenario', icon: '🎯', name: 'Scenario Name', sub: 'Short description' },
]}
```

### Adding Providers

Extend the `PROV` object:

```javascript
myProvider: { ep: 'https://api.example.com/v1/chat/completions', model: 'model-name' }
```

OpenAI-compatible endpoints work automatically. For non-compatible formats, extend the `callAPI()` function with a new branch matching `prov.isAnthropic`.

### Modifying the System Prompt

Edit `buildSystemPrompt()`. The inject format instructions, AAR format, and adaptation logic are all in plain English in that function — no parsing magic required.

---

## Intended Use

- SOC team training exercises
- IR plan validation
- New analyst onboarding
- Red team / blue team hybrid drills
- Compliance readiness (SOC 2, FedRAMP, HIPAA IR requirements)
- Security awareness for non-SOC stakeholders (executives, legal, PR)

Run it locally. Run it in a training environment. Don't expose the API key in a shared/public deployment.

---

## License

MIT — use freely, modify freely, don't hold the author liable for your incident response decisions.
