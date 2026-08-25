# 📤 How to Publish This Repo on GitHub

This project is already structured and ready to push. Follow these steps exactly.

## 1. Create the repository on GitHub

1. Go to [github.com/new](https://github.com/new)
2. **Repository name:** `self-hosted-cloud-security-lab` (or any name you prefer)
3. **Description:** `Self-hosted cloud security lab — Docker, Keycloak SSO/MFA, Nextcloud, Vault, TLS, Fail2ban, and a full CIS/GDPR audit.`
4. Set to **Public** (so it shows on your portfolio/profile)
5. **Do NOT** initialize with a README, .gitignore, or license — this project already has them
6. Click **Create repository**

## 2. Unzip the project locally

Unzip the file you downloaded (`cloud-security-lab-portfolio.zip`) to wherever you keep your projects, then open a terminal in that folder.

## 3. Push it to GitHub

```bash
cd self-hosted-cloud-security-lab   # the unzipped folder

git init
git add .
git commit -m "Initial commit: full 5-phase cloud security lab documentation"
git branch -M main
git remote add origin https://github.com/<your-username>/<your-repo-name>.git
git push -u origin main
```

Replace `<your-username>/<your-repo-name>` with your actual GitHub username and the repo name you chose in step 1.

> If you don't have Git installed or aren't authenticated yet, GitHub Desktop is the easiest path: open GitHub Desktop → **Add Local Repository** → select the unzipped folder → **Publish repository**.

## 4. Polish the repo page (5 minutes, optional but recommended)

- **About section** (gear icon, top-right of the repo page): add the same one-line description as above and topics/tags like `cloud-security`, `docker`, `keycloak`, `iam`, `devsecops`, `vault`, `nextcloud`, `cis-benchmark`, `gdpr`.
- **Social preview image:** Settings → General → Social preview → upload `docs/screenshots/phase5/01-infrastructure-diagram.png` so the architecture diagram shows up when the repo link is shared.
- **Pin it:** on your GitHub profile, go to **Customize your pins** and pin this repo so it's the first thing visitors see.

## 5. Double-check before you share the link

- [ ] The README renders correctly on the repo's main page (GitHub renders `README.md` automatically)
- [ ] The infrastructure diagram image loads at the top of the README
- [ ] Each phase's "Screenshots" link opens the right folder
- [ ] No real secrets, passwords, API keys, or private IP/domain info are in the screenshots or reports (see note below)

## ⚠️ Before making it public: sanitize sensitive info

Screenshots from lab environments often contain things you don't want public — real passwords, tokens, internal IPs, or hostnames. Skim through `docs/screenshots/` once before pushing and blur/crop or replace any image where a real secret, token, or personally identifying detail is visible. Lab/demo credentials (e.g. throwaway VM passwords you've since changed) are generally fine, but it's worth a 5-minute pass either way.

---

Once pushed, your project will live at:
`https://github.com/<your-username>/<your-repo-name>`
