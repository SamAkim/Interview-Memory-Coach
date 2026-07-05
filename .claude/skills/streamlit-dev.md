---
name: streamlit-dev
description: >
  Guide for building and editing the Interview Memory Coach Streamlit UI.
---

# Streamlit UI — Interview Memory Coach

The entire UI lives in `app.py`. Navigation is driven by `st.session_state["page"]`
with three values: `"upload"`, `"interview"`, `"report"`.
