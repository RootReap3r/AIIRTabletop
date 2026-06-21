# IR Tabletop Exercise Engine

A self-contained, browser-based incident response tabletop simulator powered by any LLM API. The AI plays a full threat actor — no hand-holding, no narration, no early reveals. Alerts render as realistic tool outputs (CrowdStrike, Splunk, Sentinel, syslog, email) and adapt dynamically to how your team responds. After the exercise, a separate neutral-evaluator pass grades your team's actual performance and exports a full Word report — timeline, scoring, gaps, and the complete transcript.

---

> ⚠️ **API Key Safety**
> This tool sends your API key directly from your browser to your chosen provider — no proxy, no backend. That's by design.
> **Never paste your API key into a hosted instance of this tool that you don't personally control.** If someone has deployed this on Vercel, Netlify, GitHub Pages, or anywhere else, treat it as untrusted. Run it locally or from your own deployment only.

---

## Features

- **23 threat scenarios** across ransomware, APT, BEC, supply chain, cloud, ICS, and more
- **Multi-provider support** — Claude, ChatGPT, DeepSeek, Qwen, Grok, Perplexity, Mistral, or any OpenAI-compatible endpoint
- **Tech stack mapping** — select your actual tooling (EDR, SIEM, IAM, network, cloud, email, backup); the AI exploits your real gaps
- **Two-tier threat actor selection:**
  - **Composite archetypes** — 6 fictional profiles (state espionage, ransomware affiliate, hacktivist, insider-adjacent, organized crime, destructive/disruptive) each with a distinct motivation and behavior under pressure
  - **Named APT groups** — 9 real, publicly-tracked groups (Lazarus, APT28, APT29, Sandworm, APT41, FIN7, Scattered Spider, Conti-style, Volt Typhoon) tagged with their MITRE ATT&CK Group ID and a short list of real associated techniques that bias the AI's inject selection
- **Randomize buttons** for scenario and actor — one click to pick a different (non-repeating) option from whichever list is active
- **Realistic alert rendering** — alerts auto-format as CrowdStrike detections, Splunk searches, Sentinel incidents, syslog entries, terminal output, or email notifications based on context; every alert panel scrolls internally so long outputs never blow out the page layout
- **MITRE ATT&CK tagging** — every inject labeled with technique IDs (T####.###)
- **Adaptive, state-consistent adversary** — strong containment slows the attacker; weak response triggers escalation; the AI is instructed to track what's actually been contained across the conversation and not contradict its own prior turns
- **Early termination as a reward** — a fast, correct response can end the exercise before the full inject count and still score high; the model is told this is the desired outcome, not a fallback
- **Real response timing capture** — wall-clock timestamps recorded automatically for every inject delivered and every team response, independent of anything the AI says
- **After Action Review (AAR)** — in-character debrief with score, threat actor reveal, gaps, recommendations, and tool suggestions tied to your specific stack
- **Performance Analysis** — a second, separate API call where the model switches out of attacker-voice into a neutral grader, scoring the transcript across six categories (detection/triage, containment speed, containment completeness, communication, root cause/scoping, recovery) with per-inject breakdowns
- **Downloadable Word report (.docx)** — one click exports the AAR, performance analysis, real response timeline, and full transcript as a formatted Word document; the `docx` library is bundled directly into the HTML so this works fully offline and from `file://`
- **Restart control** — abandon and reset a live exercise from the topbar at any time, with a confirmation guard
- **Configurable exercise length** — 3 injects (~45 min), 4 injects (~60 min), or 5 injects (~90 min)

---

## Quick Start

1. Open `index.html` in any modern browser — no server, no build step, no dependencies
2. Select your AI provider and paste your API key
3. Click **Verify Connection** to confirm
4. Select your tech stack (check everything you actually have; mark gaps honestly)
5. Add environment notes (org size, compliance scope, SOC maturity, network topology)
6. Pick a threat scenario (or hit **🎲 Randomize**)
7. Pick a threat actor — toggle between **Composite Profiles** and **Named APT Groups**, then select or randomize
8. Click **Initiate Exercise**
9. After the AAR, click **⬇ Download Report (.docx)** for the full graded write-up

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

The system prompt is built **once**, when you click **Initiate Exercise**, and reused for the rest of that run. Editing the tech stack, scenario, or actor mid-exercise has no effect on the live run — the sidebar locks while an exercise is active specifically to prevent this. Use **Restart** if you need to change configuration mid-run.

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

### Threat Actor — Composite Profiles vs. Named APT Groups

The actor selector has two tabs:

**Composite Profiles** — six fictional, generic archetypes. Each gives the AI a motivation and a behavior-under-pressure pattern (does it retreat when contained, or escalate destructively?) without claiming to represent any specific real-world group:

| Archetype | Behavior pattern |
|---|---|
| State-Sponsored Espionage Unit | Patient, stealthy, goes dormant rather than risk detection |
| Ransomware Affiliate (RaaS) | Fast, loud, profit-driven — disengages if the operation stops being worth it |
| Hacktivist Collective | Public-facing, prone to mistakes, escalates publicly if contained |
| Insider-Adjacent / Coerced Access | Starts from legitimate-looking access, blends in for a long time |
| Organized Cybercrime Syndicate | Methodical, BEC/fraud-leaning, tries parallel monetization if blocked |
| Destructive / Disruption-Focused | Trades stealth for impact, escalates aggressively rather than retreating |

**Named APT Groups** — nine real, publicly-tracked threat actors, each tagged with its MITRE ATT&CK Group ID:

| Group | MITRE ID | Known for |
|---|---|---|
| Lazarus Group | G0032 | DPRK-linked; financial theft + espionage; supply chain |
| APT28 (Fancy Bear) | G0007 | Russian GRU-linked; credential phishing |
| APT29 (Cozy Bear) | G0016 | Russian SVR-linked; extreme stealth; cloud/identity |
| Sandworm Team | G0034 | Russian GRU-linked; destructive; ICS/critical infrastructure |
| APT41 (Double Dragon) | G0096 | China-linked; dual espionage + financial; supply chain |
| FIN7 | G0046 | Financially motivated; POS/payment data; social engineering |
| Scattered Spider | G1015 | Help desk/MFA-bypass social engineering specialists |
| Conti-style Ransomware Operation | G0099 (historical) | Enterprise double-extortion ransomware playbook |
| Volt Typhoon | G1017 | China-linked; living-off-the-land; critical infra pre-positioning |

> **Sourcing note:** the MITRE Group IDs and technique associations for the Named APT tier come from the model's training-data knowledge of public MITRE ATT&CK / Mandiant / CrowdStrike reporting — **not a live, freshly-verified pull**. Cross-check [attack.mitre.org/groups](https://attack.mitre.org/groups/) before treating any specific technique mapping as current. The technique list biases which TTPs the AI reaches for; it does not constrain hostnames, IPs, or narrative details, which stay fictionalized for the exercise. The AAR's threat actor reveal will name the real group if one was selected, with an explicit note that the portrayal is a simulated composite of that group's known style, not a verified account of a real incident.

Use **🎲 Randomize** to pick a different option from whichever tab is currently active. Both the scenario and actor selectors lock once an exercise starts.

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
│     │                 System prompt snapshotted, sidebar     │
│     │                 locked, real-time clock starts         │
│     ▼                                                       │
│  2. INJECT            Realistic alert panel rendered         │
│     │                 (scrollable if long)                   │
│     │                 MITRE TTP tags labeled                 │
│     │                 Focused operational question asked     │
│     │                 Delivery timestamp captured             │
│     ▼                                                       │
│  3. TEAM RESPONSE     Your team types their actions           │
│     │                 Response timestamp captured              │
│     ▼                                                       │
│  4. ADAPT             Strong response → attacker pivots        │
│     │                 Weak response  → attacker escalates       │
│     │                 AI re-reads its own prior turns to        │
│     │                 stay state-consistent                      │
│     │                 Excellent IR   → early AAR (reward,         │
│     │                                  not a fallback)             │
│     ▼                                                       │
│  5. REPEAT            N injects total (3, 4, or 5),                │
│     │                 or early termination                          │
│     ▼                                                       │
│  6. AAR               In-character score, threat actor reveal,       │
│     │                 gaps, recs, key takeaways                       │
│     ▼                                                       │
│  7. DOWNLOAD REPORT   Separate neutral-evaluator pass grades          │
│                       the transcript; real timeline + AAR +            │
│                       analysis exported as .docx                        │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Inject Format

Each inject includes:
- `[INJECT X/N]` header
- `TTPs: T####.###, ...` — MITRE technique IDs
- One or more realistic alert blocks auto-styled to match your stack, each scrollable internally (capped height) so dense outputs don't break the layout
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

Both the standard alert body and the terminal-style body have a capped max-height with internal scrolling, so a long CrowdStrike dump or multi-block Splunk query never pushes the rest of the exercise off-screen.

---

## State Consistency & Early Termination

The system prompt explicitly instructs the AI to reconstruct the current state of the engagement (what's been isolated, disabled, reset) from the full conversation history before writing each new inject, and to never contradict its own prior "Attacker operational log." If the team correctly points out an inconsistency ("we already isolated that host"), the AI is instructed to treat the team as very likely correct, concede, and continue from the corrected state rather than argue.

If the team's response at any point would realistically lock the attacker out entirely, the AI is instructed to end the exercise early — deliver a short, honest "attacker disengages" log, then go straight to the AAR — rather than manufacture an artificial escalation to hit the configured inject count. A short exercise ended by excellent IR is treated as a high-scoring outcome, not an incomplete one. How the actor disengages (quietly vs. destructively) is meant to match the selected actor archetype.

---

## After Action Review (AAR)

Delivered automatically after the final inject response, in-character (the AI is still narrating as the attacker for this part). Includes:

- **Threat actor identity** — revealed post-exercise; names the real group if a Named APT was selected (with a disclaimer that the portrayal is a simulated composite), or a plausible codename if a composite archetype was used
- **Overall score** — 0–10
- **Overall summary** — 2–3 sentence assessment
- **What went well** — specific actions that helped
- **Gaps and missed steps** — what the team failed to do or detect
- **Recommendations** — numbered, tied to your exact stack
- **Tools to consider** — specific products or processes to fill gaps
- **Key takeaways** — 5 numbered lessons

## Performance Analysis & Downloadable Report

Clicking **⬇ Download Report (.docx)** on the AAR card triggers a second, independent API call. The model is instructed to drop the attacker persona entirely and act as a neutral evaluator reviewing the full transcript plus the real captured response-timing data, returning a structured score breakdown:

- **Overall percentage score**
- **Six category scores** (Detection & Triage, Containment Speed, Containment Completeness, Communication & Escalation, Root Cause & Scoping, Recovery & Hardening), each with a specific written justification
- **Per-inject breakdown** — what the team actually did, and an assessment of it
- **Strongest moment** and **biggest gap to address**

This is combined with a **Response Timeline** table built purely from real wall-clock timestamps captured automatically during the exercise (inject delivery time, team response time, elapsed duration per round, total exercise duration) — this data is not generated or estimated by the AI.

The final `.docx` contains, in order: a metadata summary table, the performance analysis, the real timeline, the in-character AAR sections, and a full appendix transcript of every inject and team response. The `docx` generation library is bundled directly into the HTML file (not loaded from a CDN), so report generation works fully offline and when the tool is opened directly from `file://` — Chrome and other browsers block cross-origin script loading on local files, which breaks CDN-based approaches entirely.

If the grading call fails for any reason (model error, malformed response), the report still generates with the timeline and in-character AAR — you'll see a warning instead of a silent failure, and the Performance Analysis section is simply omitted.

---

## Architecture

Single HTML file. No framework, no build toolchain, no server. The `docx` report-generation library is embedded inline in `<head>` as a bundled IIFE script (~740KB) rather than fetched from a CDN, specifically so the tool works when opened directly from disk.

```
┌───────────────────────────────────────────────────────────┐
│  Browser (index.html)                                      │
│                                                              │
│  ┌──────────────┐   ┌──────────────────────────────────┐   │
│  │   Sidebar    │   │        Main Panel                  │   │
│  │              │   │                                    │   │
│  │  · Provider  │   │  · Topbar (status, Restart)         │   │
│  │  · Env notes │   │  · Chat / inject feed (scrollable)   │   │
│  │  · Stack     │   │  · Alert panel renderer (scrollable)  │   │
│  │  · Scenario  │   │    per-panel                           │   │
│  │    (+random) │   │  · AAR renderer + Download button       │   │
│  │  · Actor:    │   │  · Team input                            │   │
│  │    Composite │   │                                           │   │
│  │    / Named   │   │                                           │   │
│  │    APT       │   │                                           │   │
│  │    (+random) │   │                                           │   │
│  │  · Config    │   │                                           │   │
│  └──────────────┘   └──────────────────────────────────────────┘   │
│                                                                       │
│  State: history[], activeSystemPrompt (snapshotted at start),         │
│  selectedStack Set, activeScenario, activeAptTier, activeActor /        │
│  activeAptId, timingLog[], lastAAR (sections + transcript + timing)      │
│                                                                          │
│  API: fetch() → provider endpoint (direct, no proxy)                     │
│       + one extra fetch() at report time for the neutral-grader pass      │
│                                                                              │
│  Reporting: docx.js (bundled inline) → Packer.toBlob() → download           │
└───────────────────────────────────────────────────────────────────────────┘
```

The entire conversation history is maintained in a local `history[]` array and sent with every API call. Unlike a chat assistant, the system prompt for a given run is built **once**, when the exercise starts, and snapshotted into `activeSystemPrompt` — it is intentionally *not* rebuilt on every call, so that editing the stack, scenario, or actor mid-exercise (which the locked sidebar prevents anyway) can't silently change what the AI thinks the scenario is partway through a run.

---

## Security & Privacy Notes

- **API keys are never stored** — they exist only in the input field for the session duration
- **No backend, no telemetry** — all API calls go directly from your browser to the provider (plus one additional call to the same provider for the performance-analysis pass when you download a report)
- **History is session-only** — closing the tab clears everything; use **Restart** to deliberately wipe and reset a run, including timing data and the last AAR
- Use a scoped API key with spend limits for shared/training environments
- The system prompt explicitly instructs the AI not to reveal threat actor identity until the AAR — this is enforced by prompt, not by code; a determined user could extract it via browser devtools
- The bundled `docx` library is a vendored copy of the open-source [docx](https://www.npmjs.com/package/docx) npm package (MIT licensed), included verbatim as a browser IIFE build — it does not make network calls

---

## Customization

### Adding Scenarios

Extend the `SCENARIOS` array in the script block:

```javascript
{ cat: 'Your Category', items: [
  { id: 'my-scenario', icon: '🎯', name: 'Scenario Name', sub: 'Short description' },
]}
```

### Adding Composite Actor Archetypes

Extend the `ACTORS` array:

```javascript
{ id: 'my-archetype', icon: '🎭', name: 'Archetype Name', sub: 'Short description',
  profile: 'Full behavioral description the AI uses to roleplay this actor consistently.' }
```

### Adding Named APT Groups

Extend the `NAMED_APTS` array. Verify the MITRE Group ID and technique list against [attack.mitre.org/groups](https://attack.mitre.org/groups/) before adding:

```javascript
{ id: 'my-group', icon: '🌐', name: 'Group Name', mitreId: 'G####',
  sub: 'Short description',
  techniques: ['T#### Technique Name', '...'],
  profile: 'Behavioral description grounded in real public reporting.' }
```

### Adding Providers

Extend the `PROV` object:

```javascript
myProvider: { ep: 'https://api.example.com/v1/chat/completions', model: 'model-name' }
```

OpenAI-compatible endpoints work automatically. For non-compatible formats, extend the `callAPI()` function with a new branch matching `prov.isAnthropic`.

### Modifying the System Prompt

Edit `buildSystemPrompt()`. The inject format instructions, AAR format, state-consistency rules, early-termination logic, and actor-tier handling are all in plain English in that function — no parsing magic required.

### Modifying the Performance Analysis Grading

Edit the `gradingPrompt` template inside `getPerformanceAnalysis()`. The JSON response shape it requests is consumed directly by `downloadReport()` to build the category-score table and per-inject breakdown — if you change the requested JSON shape, update both places together.

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
