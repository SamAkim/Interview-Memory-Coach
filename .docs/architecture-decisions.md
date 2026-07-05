# Architecture Decision Records — Interview Memory Coach (UC1)

**Hackathon:** WeMakeDevs × Cognee · Jun 29 – Jul 5 2026
**Team size:** 4 · **Available time:** ~25 hrs total

---

## ADR index

| # | Title | Status |
|---|---|---|
| ADR-001 | Agent orchestration framework | Proposed |
| ADR-002 | Cognee storage backend | Proposed |
| ADR-003 | UI framework | Proposed |
| ADR-004 | Cross-session memory strategy | Proposed |

---

## ADR-001: Agent orchestration framework

**Status:** Proposed
**Date:** 2026-06-28
**Deciders:** Full team (affects everyone’s day-to-day coding)

### Context

Three agents need to be orchestrated — Intake, Interviewer, and Analysis — each calling the Anthropic API, reading/writing Cognee, and passing state between them. The choice here determines how fast the team can iterate, how easy debugging is, and how much boilerplate eats into the available 25 hours.

### Decision

Use the **raw Anthropic Python SDK** with thin wrapper functions, not a framework like LangChain or LlamaIndex.

### Consequences

- Each agent is a Python `async def` function accepting a `messages: list` parameter
- State is passed between agents as plain dicts — agreed schema on Day 1
- A ~20-line retry wrapper for rate limit errors will need to be hand-written in `agents/base.py`
- Streaming to the Streamlit UI uses `anthropic.messages.stream()` + `st.write_stream()`

---

## ADR-002: Cognee storage backend

**Status:** Proposed
**Date:** 2026-06-28

### Decision

Use **NetworkX** (graph) + **LanceDB** (vector) — Cognee’s defaults. No Docker, no credentials.

---

## ADR-003: UI framework

**Status:** Proposed

### Decision

Use **Streamlit** with `st.chat_message`, `st.chat_input`, and `st.write_stream`.

---

## ADR-004: Cross-session memory strategy

**Status:** Proposed

### Decision

Implement both flavours but prioritise Flavour 1 (candidate memory) for the demo. Flavour 2 (role memory via `memify()`) is a stretch goal.
