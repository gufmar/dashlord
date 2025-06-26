# DashLord

Dashboard for technical best practices

Examples :

- https://dashlord.incubateur.net  
- https://socialgouv.github.io/dashlord-fabrique  
- https://mtes-mct.github.io/dashlord  
- https://socialgouv.github.io/dnum-dashboard

## Usage

### Add a URL to DashLord

Edit the `./dashlord.yml` file and add an entry for your URL.

💡 Best practice : omit trailing slashes on URLs.

### Deploying your own instance of DashLord

1. Create a new repository from the DashLord template.
2. Edit the `dashlord.yml` file.
3. Customize `.github/workflows/scans.yml` as needed.
4. Customize `.github/workflows/report.yml` (check the `base-path`, which defines the site’s URL, matching your repository name).
5. In your GitHub repo settings under "Actions", set "Workflow permissions" to "Read and write" (if unavailable, enable it at the organization level).
6. Under "Pages" settings, choose the `gh-pages` branch as the source (you may create it beforehand or let the first scan generate it).
7. Trigger the "DashLord scans" workflow in the "Actions" tab of your repo.

After scans finish, a report is published in the `gh-pages` branch and is publicly accessible at `https://[organisation].github.io/[repository]` 

### Customization

- `dashlord.yml` lets you configure URLs and a few dashboard options.
- `scans.yml` workflow allows enabling/disabling specific scanners and setting scan frequency (defaults to every Sunday at midnight).
- `report.yml` workflow automatically generates the website report using the SocialGouv report action.

You can also manually trigger these workflows from the Actions tab.

## Tools

Each analysis tool can be toggled in the `dashlord.yml` under the `tools` section. Supported tools include:

| Repo                                                                                      | desc                                   |
| ----------------------------------------------------------------------------------------- | -------------------------------------- |
| [SocialGouv/dashlord-actions](https://github.com/SocialGouv/dashlord-actions)             | Dashlord specific actions              |
| [SocialGouv/dashlord-nuclei-action](https://github.com/SocialGouv/dashlord-nuclei-action) | Dump nuclei result                     |
| [SocialGouv/httpobs-action](https://github.com/SocialGouv/httpobs-action)                 | Dump Mozilla HTTP Observatory result   |
| [SocialGouv/thirdparties-action](https://github.com/SocialGouv/thirdparties-action)       | Dump third party scripts scan result   |
| [SocialGouv/wappalyzer-action](https://github.com/SocialGouv/wappalyzer-action)           | Dump Wappalyzer scan result            |
| [MTES-MCT/dependabotalerts-action](https://github.com/MTES-MCT/dependabotalerts-action)   | Dump Github dependabot security alerts |
| [MTES-MCT/codescanalerts-action](https://github.com/MTES-MCT/codescanalerts-action)       | Dump Github CodeQL security alerts     |
| [MTES-MCT/updownio-action](https://github.com/MTES-MCT/updownio-action)                   | Dump updown.io stats                   |
| [MTES-MCT/nmap-action](https://github.com/MTES-MCT/nmap-action)                           | Dump nmap port scan stats              |
| [MTES-MCT/stats-action](https://github.com/MTES-MCT/stats-action)                         | Detect /stats page.                    |
| [SocialGouv/thirdparties](https://github.com/SocialGouv/thirdparties)                     | thirdparty scripts database            |
| [swinton/screenshot-website](https://github.com/swinton/screenshot-website)               | grab website screenshot                |
| [SocialGouv/detect-404-action](https://github.com/SocialGouv/detect-404-action)           | detect 404 errors                      |
| [aquasecurity/trivy-action](https://github.com/aquasecurity/trivy-action)                 | Scan docker images vulnerabilities     |

## Configuration

Some tools require extra setup:

### Dependabot (dependency vulnerability alerts)
- Add a GitHub secret: `DEPENDABOTALERTS_TOKEN`
- Use a [personal access token](https://github.com/settings/personal-access-tokens/new) with **Dependabot alerts: read** permission.

### Code scanning
- Add a secret: `CODESCANALERTS_TOKEN`
- Grant your [personal access token](https://github.com/settings/personal-access-tokens/new) the **Code scanning alerts: read** scope.

### updown.io (availability monitoring)
- Sign up at [updown.io](https://updown.io) and add your URLs.
- Enable `updownio: true` in `dashlord.yml`.
- Add a **read-only** `UPDOWNIO_API_KEY` secret (GitHub settings → secrets).

With a write-enabled [`init`](https://github.com/SocialGouv/dashlord/blob/48b9362391dc45cf604ceb9d91ee300a028a3021/.github/workflows/scans.yml#L55) token, missing URLs are auto-added to your updown.io account. They’ll then be included in the next scan.

### customCss :

You can host the css file in your Dashlord repo but the link needs to point to a file with the correct Content-Type Header. See here for [details](https://www.twistblogg.com/2020/06/use-github-for-hosting-files.html)

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
