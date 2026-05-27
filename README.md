# OdontoVista

Local-first dental clinic management for small Greek dental practices.

The application is intentionally simple: a local FastAPI service stores clinical data in SQLite, serves a React interface on localhost, and keeps the odontogram at the center of the workflow.

## Stack

- Frontend: React, TypeScript, Vite, TailwindCSS
- Backend: FastAPI, SQLAlchemy, Alembic, SQLite
- Runtime: local browser UI served by a local Python process
- Packaging: PyInstaller launcher plus Windows installer scripts

## Repository

```text
apps/
  api/        FastAPI backend and SQLite data layer
  web/        React clinical workspace
  launcher/   Windows local launcher
docs/         Architecture, database, UX, packaging notes
scripts/      Development and packaging scripts
tests/        Cross-app tests
```

## Development

Backend:

```powershell
cd apps/api
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -e ".[dev]"
alembic upgrade head
uvicorn app.main:app --host 127.0.0.1 --port 8765
```

Frontend:

```powershell
cd apps/web
npm install
npm run dev
```

The backend defaults to local-only access and stores data under the OS application data directory unless `ODONTOVISTA_DATA_DIR` is set.

## Windows Packaging

Install Inno Setup 6 on the Windows build VM, then run:

```powershell
.\scripts\package-windows.ps1
```

The script builds the backend launcher, frontend assets, and, when Inno Setup is available, a one-click installer at:

```text
dist\installer\OdontoVista-Setup-0.1.0.exe
```

## Legacy Windows 7 Note

Windows 7 is a best-effort offline-only target. Use the pinned Python 3.8 dependency set in `apps/api/requirements-win7.txt` and the legacy packaging notes in `docs/packaging.md`.

## License

OdontoVista is free software released under the GNU Affero General Public License v3.0.

The goal is to help the dental community while keeping improvements available to the same community. You may use, study, modify, and share the software under the terms of the AGPLv3. See [LICENSE](LICENSE) for the full license text.
