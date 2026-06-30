# PrivateBot — Project Details

Summary

PrivateBot is a private, secure AI assistant (Python) with extensible skills for email management and task prioritization. It runs locally or against a private Microsoft Foundry deployment and exposes an HTTP API compatible with the AI Toolkit Agent Inspector.

Key features

- Private & secure design; environment-driven credentials (.env)
- Email skills: check/search/prioritize
- Task skills: add/view/prioritize/complete/get_task_stats
- Extensible skill system (add files under skills/)
- HTTP server mode (recommended) + CLI and VS Code debug support

Quick start

1. Prereqs: Python 3.10+, Azure Foundry project, Azure CLI (az login)
2. Create and activate venv: `python -m venv .venv` + `.venv\Scripts\activate`
3. Install: `pip install -r requirements.txt`
4. Configure: copy/edit `.env` with FOUNDRY_PROJECT_ENDPOINT and FOUNDRY_MODEL_DEPLOYMENT_NAME
5. Run (server): `python main.py --server`  — or `python main.py --cli` for CLI
6. Debug (recommended): Press F5 in VS Code (uses Agent Inspector)

Files & docs

- Main entry: main.py
- Skills: skills/*.py (email_skills.py, task_skills.py)
- Requirements: requirements.txt
- Start scripts: start_server.bat / start_server.ps1
- Docs: docs/ARCHITECTURE.md, README.md, USAGE.md

Security notes

- Keep `.env` out of source control
- Sensitive operations require user approval
- Logs written to `privatebot.log`

Repository (local)

C:\Users\wayne\Code\PrivateBot

[waynethompson/PrivateBot](https://github.com/waynethompson/PrivateBot)

If you'd like a different file name or want this appended into an existing note, say which file to update.