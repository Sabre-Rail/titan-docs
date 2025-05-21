# Titan Docs

This repository contains internal documentation for Sabre Rail's Titan Platform.

## 📚 Documents Included

- Test Strategy
- C# Coding Standards

## 🚀 Getting Started

### 🔍 Option 1: Local Preview with Python

Make sure you have Python 3 and pip:

```bash
pip install mkdocs mkdocs-material
mkdocs serve
```

Then visit: [http://127.0.0.1:8000](http://127.0.0.1:8000)

### 🐳 Option 2: Local Preview with Docker

Use Docker Compose to build and serve the site:

```bash
docker-compose up --build
```

Then visit: [http://localhost:8080](http://localhost:8080)

To shut it down:

```bash
docker-compose down
```

### 📦 Deploy to GitHub Pages

To deploy to GitHub Pages manually:

```bash
mkdocs gh-deploy --clean
```

> Or simply push to the `main` branch — GitHub Actions will auto-deploy.

### 🔁 CI/CD

GitHub Actions (`gh-pages.yml`) is configured to deploy the site automatically when changes are pushed to the `main` branch.

---

## 🧰 Optional Improvements (Planned)

These features may be added in future iterations:

- 🔒 **Basic Auth**: Password-protect internal docs with Nginx or reverse proxy.
- 🌐 **Custom Domain / LAN Access**: Expose docs on your network or assign a company domain.
- 🔁 **Auto-rebuild on Markdown change**: Use file watchers to rebuild on save.
- 📤 **Auto-deploy on GitHub Push**: CI/CD with webhook to internal Docker host or VM.

---

**Maintained by:** D. L. Pratt  
**Company:** Sabre Rail Services Ltd
