# DashLord

Dashboard for technical best practices

**Examples:**
- https://dashlord.incubateur.net  
- https://socialgouv.github.io/dashlord-fabrique  
- https://mtes-mct.github.io/dashlord  
- https://socialgouv.github.io/dnum-dashboard

---

## Usage

### Add a URL to DashLord

Edit the `./dashlord.yml` file and add an entry for your URL.

**Best practice:** omit trailing slashes on URLs.

### Deploying your own instance of DashLord

1. Create a new repository from the DashLord template.
2. Edit the `dashlord.yml` file.
3. Customize `.github/workflows/scans.yml` as needed.
4. Customize `.github/workflows/report.yml` (check the `base-path`, which defines the site’s URL, matching your repository name).
5. In your GitHub repo settings under **Actions**, set **Workflow permissions** to **Read and write** (if unavailable, enable it at the organization level).
6. Under **Pages** settings, choose the `gh-pages` branch as the source (you may create it beforehand or let the first scan generate it).
7. Trigger the **DashLord scans** workflow in the **Actions** tab of your repo.

After scans finish, a report is published in the `gh-pages` branch and is publicly accessible at:

```
https://[organisation].github.io/[repository]
```

---

### Customization

- **`dashlord.yml`** lets you configure URLs and a few dashboard options.
- **`scans.yml`** workflow allows enabling/disabling specific scanners and setting scan frequency (defaults to every Sunday at midnight).
- **`report.yml`** workflow automatically generates the website report using the SocialGouv report action.

You can also manually trigger these workflows from the **Actions** tab.

---

## Tools

Each analysis tool can be toggled in the `dashlord.yml` under the `tools` section. Supported tools include:

- `dashlord-actions` (core actions)
- `nuclei`
- `httpobs` (Mozilla HTTP Observatory)
- `thirdparties` (third-party script scanning)
- `Wappalyzer`
- `dependabotalerts` (GitHub Dependabot alerts)
- `codescanalerts` (GitHub CodeQL alerts)
- `updownio` (uptime & performance monitoring)
- `nmap` (port scans)
- `stats` (checks for `/stats` page presence)
- `detect-404` (404 status detection)
- `trivy` (Docker image vulnerabilities)
- …and screenshot capture, third-party script DB, etc.

---

## Configuration

Some tools require extra setup:

### Dependabot (dependency vulnerability alerts)
- Add a GitHub secret: `DEPENDABOTALERTS_TOKEN`
- Use a personal access token with **Dependabot alerts: read** permission.

### Code scanning
- Add a secret: `CODESCANALERTS_TOKEN`
- Grant your token the **Code scanning alerts: read** scope.

### updown.io (availability monitoring)
- Sign up at updown.io and add your URLs.
- Enable `updownio: true` in `dashlord.yml`.
- Add a **read-only** `UPDOWNIO_API_KEY` secret (GitHub settings → secrets).

With a write-enabled init token, missing URLs are auto-added to your updown.io account. They’ll then be included in the next scan.

**Optional:** `customCss` — you can host a CSS file in your repo, but ensure it’s served with the correct `Content-Type` header. See details in their documentation.

---

## Contributing

Feel free to contribute by:
- Opening high‑quality issues
- Improving documentation
- Adding code

See CONTRIBUTING.md for guidelines.

---

## Developer Mode

DashLord operates in two phases:

1. **Data collection** — each URL is scanned with enabled tools and results are saved as versioned JSON.
2. **Report generation** — the report workflow aggregates JSON data, compresses it, and builds a static web report.

See `SocialGouv/dashlord-actions` for details.

---

## About

Dashboard for technical best practices.  
Hosted at [socialgouv.github.io/dashlord](https://socialgouv.github.io/dashlord)  
**License:** Apache‑2.0
