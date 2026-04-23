# CoLa

# Local AI in Art Education
### Towards Institutional Sovereignty, Curatorial Agency, and Breaking the Filter World

> Research and presentation notes from an ongoing investigation into running local AI systems within art academy contexts — with a focus on training set agency, institutional freedom, and using AI as a counter-tool to algorithmic filter culture.
>
> The project is called CoLa — short for Community Language model. The name is also a quiet joke: the current AI landscape largely asks institutions to choose between big-tech providers the way you'd choose between cola brands. CoLa is the alternative: locally made, institution-specific, and answerable only to the people using it.

---

## Table of Contents

- [Motivation](#motivation)
- [Core Arguments](#core-arguments)
  - [The Case for Local Systems](#the-case-for-local-systems)
  - [The Filter World Problem](#the-filter-world-problem)
  - [Local AI as a Curatorial Counter-Tool](#local-ai-as-a-curatorial-counter-tool)
- [Presentation Structure](#presentation-structure)
- [Audience Exercise — Building the Dataset Together](#audience-exercise--building-the-dataset-together)
- [Technical Research — Running Local LLMs](#technical-research--running-local-llms)
  - [Setting Up a Local LLM Server on Debian](#setting-up-a-local-llm-server-on-debian)
  - [Running Claude Code with Local LLMs (Ollama + LiteLLM)](#running-claude-code-with-local-llms-ollama--litellm)
- [Project Timeline](#project-timeline)
- [Open Questions](#open-questions)
- [References & Further Reading](#references--further-reading)

---

## Motivation

What does it mean for an art institution to run its own AI system?

This project explores that question from two directions simultaneously: the **political and pedagogical** case for why an art academy should want to control its own AI infrastructure, and the **practical technical** work of actually making that happen.

The starting point is a dissatisfaction with two things that are usually treated as separate problems but are in fact the same problem: the commercial dependency of institutions on big-tech AI providers, and the algorithmic filter culture that increasingly shapes how students (and everyone else) encounter art, ideas, and unfamiliar work.

Both problems are about who controls what you see — and why.

---

## Core Arguments

### The Case for Local Systems

Running a local AI system rather than relying on commercial API providers gives an institution three things that are difficult or impossible to obtain otherwise:

**Agency over the training set.** When you run your own model, you decide what enters the dataset. For an art academy, this means the system can be trained on collections, archives, critical texts, and artists that reflect the institution's actual values and curriculum — not the general-purpose internet scrapes that underpin most commercial models. What you include is a curatorial act. What you exclude is equally intentional.

**Independence from big tech.** Commercial AI providers are businesses with their own interests: content policies shaped by liability concerns, outputs optimized for broad consumer acceptability, and terms of service that may change. An institution using a third-party API is always downstream of those decisions. A local system removes that dependency entirely — no external drift, no policy changes that affect your students without your consent.

**Institutional freedom.** A locally hosted model can be configured, fine-tuned, and used in ways that serve the institution's specific pedagogical needs without asking permission. No ads, no engagement optimization, no third-party data processing. The model is a tool the institution owns rather than a service it subscribes to.

---

### The Filter World Problem

Parametric, personalized feeds have become the dominant mode through which people encounter media. Recommendation algorithms on every major platform — social media, streaming, search — are optimized to show you what you are statistically likely to approve of, based on your previous behavior.

The result is a progressive narrowing of exposure. You stop encountering the genuinely unfamiliar. You are rarely confronted with work that requires you to form a new opinion rather than confirm an existing one.

For art education, this is a specific problem. Developing aesthetic judgment requires friction — exposure to work you don't understand yet, confrontation with movements and forms outside your existing references, the experience of not knowing whether you like something and having to sit with that. The algorithmic feed is structurally opposed to all of this. It is optimized for comfort, not growth.

Students who have grown up inside filter culture may not even recognize the condition. The absence of the unfamiliar is invisible when you've never experienced the alternative.

---

### Local AI as a Curatorial Counter-Tool

A locally run AI system, built with the right intentions, can be designed to do the opposite of what a recommendation algorithm does.

Rather than showing users more of what they already know, a system built for art education could:

- Systematically surface **contrasting artists** — pairing a student's known references with formally or historically distant work
- Include **institution-specific databases** — regional archives, underrepresented movements, non-Western art histories, collections that commercial systems have no reason to index
- Build in **productive friction** — responses that introduce a counterpoint, a critic who disagrees, a movement that challenges the one being studied
- Operate without engagement optimization — the system has no interest in keeping you on it longer, no metric it is trying to maximize at your expense

The model becomes a curatorial act rather than a convenience tool. Its design reflects a set of values about what students should encounter.

---

## Presentation Structure

The following is a 45-minute talk outline on this topic, designed for an audience of educators and practitioners in art education.

| Section | Content | Time |
|---|---|---|
| **1. Opening hook** | "When did you last encounter something you didn't choose?" — a question to the room, optionally a personal anecdote about a work that genuinely surprised the speaker | 5 min |
| **2. The case for local systems** | Agency over training data, independence from big tech, institutional freedom — made concrete with examples of where commercial AI has constrained or disappointed users | 10 min |
| **3. The filter world problem** | How parametric feeds work phenomenologically; what it means for art students forming aesthetic judgment; the local system as a counter-tool | 10 min |
| **4. Audience exercise** | Collective dataset-building exercise (see below) | 15 min |
| **5. Closing vision** | What a purpose-built art academy model could look like; the model as ongoing curatorial act; open invitation to continue building | 5 min |

**Facilitation note:** Keep the output from section 4 visible during section 5. Referring back to what the room built together transforms the closing from abstract vision into something already begun.

---

## Audience Exercise — Building the Dataset Together

**Prompt given to the audience:**
> *"What should our training set include that no algorithm would show you?"*

The exercise runs in three rounds:

**Round 1 — Individual** *(3 minutes)*
Each person writes down 2–3 sources, artists, archives, or critical texts they consider essential but unlikely to appear in a commercial AI's training data. Written on paper or a card — not typed, deliberately slowing the process down.

**Round 2 — Pairs** *(5 minutes)*
Compare with one other person. Make a case for your choices. Listen to theirs. Note where you agree and where there is genuine tension.

**Round 3 — Collective** *(7 minutes)*
Entries are gathered onto a shared surface — a whiteboard, a live slide, a physical wall. The facilitator groups entries by theme and draws out tensions and outliers rather than smoothing them over. The disagreements are the interesting part.

**Output:** A rough living list — not a definitive curriculum, but a starting point for what a real, institution-specific dataset might include. The list should stay openly editable after the session.

---

## Technical Research — Running Local LLMs

The sections above make the argument for local AI systems in art education. The sections below document the practical work of actually running one — specifically, research into using Claude Code (an agentic coding interface) with fully local model infrastructure, so that no data leaves the local network.

---

## Setting Up a Local LLM Server on Debian

> Based on the [llm-server-docs guide by varunvasudeva1](https://github.com/varunvasudeva1/llm-server-docs). This documents the in-house Debian server setup running Ollama + Open WebUI as a locally accessible chat interface for the institution.

This is the foundation layer of the CoLa infrastructure: a physical server on the local network running open-source models, accessible through a browser interface — with no data leaving the building.

### Stack

| Component | Tool | Purpose |
|---|---|---|
| OS | Debian (stable) | Stable, well-supported Linux base |
| Inference engine | Ollama | Runs local model files, OpenAI-compatible API |
| Chat interface | Open WebUI | Browser-based UI accessible across the local network |
| Web search | SearXNG | Private, self-hosted metasearch engine for grounding model responses |
| Network access | Tailscale | Secure remote access without exposing the server publicly |
| Container runtime | Docker | Isolates and manages all services |

### Architecture

```
Browser (any device on network)
        │
        ▼
  Open WebUI :3000
        │
        ├──► Ollama :11434  ──►  Local LLM (qwen, llama, mistral, etc.)
        │
        └──► SearXNG :5050  ──►  Web search results (no tracking)
```

### Hardware used

The setup runs on an in-house server. For reference, the guide was built around:

- CPU: Intel Core i5-12600KF
- RAM: 96GB DDR4
- GPU: 2× Nvidia RTX 3090 (24GB each)
- Storage: 1TB NVMe SSD

For a smaller institutional setup (single user or light concurrent use), a single RTX 3090 or equivalent AMD GPU with 24GB VRAM is sufficient for 14–32B models.

### Setup overview

Full step-by-step instructions are in the [upstream guide](https://github.com/varunvasudeva1/llm-server-docs). The steps below summarise what was followed for this deployment.

#### 1. Install Debian and GPU drivers

Fresh Debian install, then Nvidia CUDA drivers:

```bash
sudo apt update && sudo apt upgrade
sudo apt install linux-headers-amd64
sudo apt install nvidia-driver firmware-misc-nonfree
# Reboot, then verify:
nvidia-smi
```

#### 2. Install Docker

```bash
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
# Add your user to the docker group to avoid needing sudo on every command:
sudo usermod -aG docker $USER
```

Install Nvidia Container Toolkit so Docker containers can access the GPU:

```bash
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg
sudo apt install -y nvidia-container-toolkit
sudo systemctl restart docker
```

Create a shared Docker network for all services to communicate:

```bash
docker network create app-net
```

#### 3. Install Ollama

```bash
curl -fsSL https://ollama.com/install.sh | sh
# Expose Ollama to the local network (not just localhost):
systemctl edit ollama.service
# Add under [Service]:
# Environment="OLLAMA_HOST=0.0.0.0"
systemctl daemon-reload && systemctl restart ollama
```

Pull a model to test:

```bash
ollama pull qwen2.5:14b
ollama run qwen2.5:14b
```

#### 4. Install Open WebUI

```bash
# With Nvidia GPU support:
docker run -d -p 3000:8080 \
  --network app-net \
  --gpus all \
  --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data \
  --name open-webui \
  --restart always \
  ghcr.io/open-webui/open-webui:cuda
```

Access at `http://localhost:3000` or `http://<server_ip>:3000` from any device on the same network.

#### 5. Install SearXNG (optional but recommended)

Gives the model the ability to search the web without sending queries to Google, Bing, or similar:

```bash
docker pull searxng/searxng
docker run -d -p 5050:8080 \
  --name searxng \
  --network app-net \
  -v "${PWD}/searxng:/etc/searxng" \
  -e "BASE_URL=http://0.0.0.0:5050/" \
  -e "INSTANCE_NAME=searxng" \
  --restart unless-stopped \
  searxng/searxng
```

Add JSON format support by editing `searxng/settings.yml`:

```yaml
search:
  formats:
    - html
    - json
```

Then connect it in Open WebUI under `Admin Panel > Settings > Web Search`.

#### 6. Set a startup script

Create `init.bash` to run at boot (sets GPU power limit and starts services):

```bash
#!/bin/bash
sudo nvidia-smi -pm 1
sudo nvidia-smi -pl 250   # adjust wattage as needed
```

Schedule it via crontab: `@reboot /path/to/init.bash`

#### 7. Remote access via Tailscale (optional)

Tailscale creates a private VPN tunnel so the server is reachable from outside the institution's network without exposing any ports publicly:

```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up --ssh
```

Once connected, the server is reachable at its Tailscale IP from any enrolled device, even off-campus.

### Security notes

The upstream guide includes detailed Docker hardening steps — running containers under a non-root user, dropping unnecessary capabilities, limiting resource access, and mounting volumes as read-only where possible. For an institutional setup where the server is accessible across a network, these are worth implementing. See the [hardening section](https://github.com/varunvasudeva1/llm-server-docs#harden-docker-containers) of the upstream guide.

Firewall setup with UFW:

```bash
sudo apt install ufw
# Allow SSH and Open WebUI only to your local network range:
sudo ufw allow from <your_network_range> to any port 22 proto tcp
sudo ufw allow from <your_network_range> to any port 3000 proto tcp
sudo ufw enable
```

---

## Running Claude Code with Local LLMs (Ollama + LiteLLM)

> Research into minimizing big-tech API dependency by running AI coding assistants entirely on local infrastructure. Conducted at university level to explore open, sovereign AI tooling for software development.

### Stack Overview

```
Claude Code  ──►  LiteLLM (proxy)  ──►  Ollama  ──►  Local LLM
(agent UI)        (translates API)       (runner)      (qwen2.5-coder / qwen3-coder)
```

Claude Code speaks Anthropic's Messages API format. Ollama speaks OpenAI's format. LiteLLM bridges this gap by translating requests between the two protocols.

---

### Setup Process

#### 1. Install dependencies

```bash
# Install Ollama
curl -fsSL https://ollama.com/install.sh | sh

# Pull a local model
ollama pull qwen2.5-coder:14b
# or
ollama pull qwen3-coder

# Install LiteLLM in a virtual environment
python -m venv venv
source venv/bin/activate
pip install litellm
```

#### 2. Create LiteLLM config

LiteLLM does not create a config file by default. Create one manually:

```bash
mkdir -p ~/.litellm
nano ~/.litellm/config.yaml
```

Paste the following:

```yaml
litellm_settings:
  drop_params: true      # silently drops unsupported params (e.g. context_management)
  modify_params: true    # allows LiteLLM to adapt params for the target API

model_list:
  - model_name: "claude-*"   # wildcard catches any Claude model name Claude Code requests
    litellm_params:
      model: ollama/qwen3-coder:latest
      api_base: http://localhost:11434
      extra_body:
        think: false           # disables thinking mode which can interfere with tool calling
```

The `claude-*` wildcard is important — Claude Code requests models by Claude names (e.g. `claude-sonnet-4-6`). This routes all such requests to your local model instead.

#### 3. Configure environment variables

Set these before launching Claude Code so it skips the Anthropic login screen entirely:

```bash
export ANTHROPIC_BASE_URL=http://localhost:4000   # LiteLLM port
export ANTHROPIC_AUTH_TOKEN=sk-litellm-static-key # any static key set in LiteLLM
export ANTHROPIC_MODEL=claude-sonnet-4-6          # triggers the claude-* wildcard
```

To persist these, add them to your `~/.bashrc` or `~/.zshrc`.

Alternatively, set them in `~/.claude/settings.json`:

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "http://localhost:4000",
    "ANTHROPIC_AUTH_TOKEN": "sk-litellm-static-key",
    "ANTHROPIC_MODEL": "claude-sonnet-4-6"
  }
}
```

#### 4. Startup order

Always start services in this order:

```bash
# 1. Start Ollama (bind to all interfaces if using Docker)
OLLAMA_HOST=0.0.0.0 ollama serve

# 2. Start LiteLLM
litellm --config ~/.litellm/config.yaml

# 3. Launch Claude Code
claude
```

---

### Issues Encountered & Fixes

**Login screen shows Bedrock / Vertex / Foundry options only**

Claude Code's login screen only lists cloud provider options. These are not applicable for a local setup. The correct fix is to bypass the login screen entirely by setting `ANTHROPIC_BASE_URL` and `ANTHROPIC_AUTH_TOKEN` environment variables before launching.

If the prompt persists, cached credentials may be interfering:

```bash
rm -rf ~/.claude   # clears cached login state — back up settings.json first
```

**`UnsupportedParamsError: ollama does not support parameters: ['context_management']`**

Claude Code sends Anthropic-specific parameters that Ollama does not understand.

Fix: Add `drop_params: true` to `litellm_settings` in your config.

**`Invalid model name passed in model=ollama/qwen3-coder:latest`**

LiteLLM was passing the raw Ollama model name to Claude Code instead of matching against a configured route.

Fix: Use `claude-*` as the `model_name` wildcard in your config so LiteLLM intercepts any Claude model name Claude Code sends and routes it locally.

**Model outputs raw JSON instead of executing tools**

Smaller models (tested: `qwen2.5-coder:14b`, `qwen3-coder:latest`) output literal JSON tool-call objects rather than executing the agentic loop. This indicates the model cannot reliably parse and follow Claude Code's system prompt and tool-use format.

Partial mitigations:
- Add `modify_params: true` to LiteLLM config
- Disable thinking mode (`think: false`) for qwen3-coder
- Use a larger model variant (72B+)

---

### Model Compatibility Findings

| Model | Tool Calling | Practical Usability |
|---|---|---|
| `qwen2.5-coder:14b` | ❌ Poor | Outputs raw JSON, not usable for agentic tasks |
| `qwen3-coder:latest` | ⚠️ Partial | Better but still struggles with Claude Code's format |
| `qwen2.5-coder:72b` | ✓ Good | Recommended minimum for reliable tool use |
| `qwen3-coder` (232B) | ✓ Best local | Needs significant VRAM (multi-GPU server) |

**Key finding:** Claude Code's agentic tool-use format requires models of roughly 70B+ parameters to function reliably. Smaller models understand they should use tools but fail to format responses in a way Claude Code can parse and execute.

---

### Recommended Local Infrastructure (University / Institutional)

For a multi-user institutional deployment prioritizing sovereignty and privacy:

| Component | Recommended Option | Notes |
|---|---|---|
| Model runner | **vLLM** | Better throughput than Ollama for concurrent users |
| API proxy | **LiteLLM** | Built-in key management, usage tracking, rate limiting |
| Chat UI | **Open WebUI** | For non-technical users alongside the coding agent |
| Coding agent | **Claude Code** | Or any OpenAI-compatible agent |
| Hardware | 2–4× A100/H100 GPUs | Required for reliable 70B+ model inference |

Why this approach:
- No data leaves the local network
- No dependency on Anthropic, OpenAI, AWS, GCP, or Azure
- Full control over model versioning and updates
- Usage can be audited and monitored internally
- Cost is hardware + electricity only after initial setup

---

### Current Limitations & Honest Assessment

The quality gap between local 70B models and hosted frontier models (Claude Sonnet/Opus) remains noticeable for complex agentic tasks. However:

- The gap is narrowing rapidly with each model generation
- For a university context prioritizing sovereignty over raw capability, the tradeoff is reasonable and defensible
- For simpler or well-scoped coding tasks, local 70B models perform adequately

---

## Project Timeline

```
Phase 1 — Conceptual research
──────────────────────────────────────────────────────
  Identify the problem space: filter culture, institutional
  AI dependency, lack of curatorial agency in commercial tools.

  Key questions formed:
  - What does it mean for an art institution to control its own AI?
  - How does algorithmic personalization affect aesthetic development?
  - What would a counter-tool look like?


Phase 2 — Presentation development
──────────────────────────────────────────────────────
  Develop talk structure for 45-minute format.
  Design audience exercise for collective dataset-building.
  Frame local systems as pedagogical and political stance,
  not just a technical preference.

  Output: presentation outline (5 sections, 45 min)
  Core concepts finalized:
  - Agency over training set
  - Institutional freedom
  - The filter world problem
  - Contrasting/friction-based AI design


Phase 3a — In-house server deployment (Debian)
──────────────────────────────────────────────────────
  Set up a physical in-house server on the institution's local
  network running open-source models via Ollama + Open WebUI.
  Based on the llm-server-docs guide (varunvasudeva1/llm-server-docs).

  Stack deployed:
  - Debian OS on in-house hardware (2× RTX 3090)
  - Docker for service isolation and management
  - Ollama as inference engine (OpenAI-compatible API)
  - Open WebUI as browser-based chat interface
  - SearXNG for private, self-hosted web search
  - Tailscale for secure remote access off-campus

  Key outcomes:
  - Fully local setup: no data leaves the building
  - Accessible from any device on the institution's network
  - Models can be swapped and updated without external dependency
  - Remote access enabled via Tailscale VPN tunnel


Phase 3b — Technical feasibility research
──────────────────────────────────────────────────────
  Investigate whether local model infrastructure can realistically
  replace commercial API dependency for an institutional setup.

  Stack tested: Claude Code + LiteLLM + Ollama

  Findings:
  - Setup is achievable with correct configuration
  - 70B+ parameter models required for reliable tool use
  - Quality gap with frontier models remains, but is narrowing
  - vLLM recommended over Ollama for multi-user deployments

  Issues documented and resolved:
  - Login screen bypass via environment variables
  - UnsupportedParamsError → drop_params: true
  - Model wildcard routing → claude-* in LiteLLM config
  - Raw JSON output → larger models + modify_params


Phase 4 — In progress
──────────────────────────────────────────────────────
  [ ] Deliver presentation and run audience exercise
  [ ] Compile dataset list from exercise output
  [ ] Evaluate institutional hardware options
  [ ] Test vLLM deployment for multi-user access
  [ ] Begin building art-academy-specific training corpus
  [ ] Document curation criteria and inclusion decisions
```

---

## Open Questions

These remain unresolved and are offered as starting points for further research and conversation:

- What are the ethical criteria for deciding what enters an institutional training set? Who makes those decisions, and how are they documented?
- How do you design for productive friction without it becoming arbitrary or paternalistic — who decides what counts as "unfamiliar enough"?
- At what point does the quality gap between local and frontier models become acceptable for educational use? What tasks matter most?
- How should the living dataset list from the audience exercise be maintained and governed over time?
- What does it mean to audit or inspect a locally run model's outputs — and how does that compare to the opacity of commercial systems?

---

## References & Further Reading

**Technical**
- [Claude Code LLM Gateway Docs](https://docs.claude.com/en/docs/claude-code/llm-gateway)
- [LiteLLM Proxy Docs](https://docs.litellm.ai/docs/proxy/quick_start)
- [Ollama Model Library](https://ollama.com/library)
- [vLLM Documentation](https://docs.vllm.ai)
- [Open WebUI](https://github.com/open-webui/open-webui)

**Context & background**
- Eli Pariser — *The Filter Bubble* (2011) — foundational text on algorithmic personalization and its effects on public discourse
- Safiya Umoja Noble — *Algorithms of Oppression* (2018) — on the political dimensions of search and recommendation systems
- Hito Steyerl — writings on the internet, image circulation, and the conditions of contemporary image culture
