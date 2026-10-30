This walkthrough takes you from an Obsidian vault on your computer to a published website using your Porkbun domain. It assumes you're using a Mac, but the Quartz and web-dashboard steps are nearly identical on Windows.

## How the Pieces Fit Together

The publishing workflow is:

**Edit in Obsidian → Sync to GitHub → Netlify rebuilds → Visit your custom domain.**

- **Obsidian:** Write and organize Markdown notes.
    
- **Quartz 5:** Converts your Markdown into a website.
    
- **GitHub:** Stores your website source and published content.
    
- **Netlify:** Builds and hosts your website automatically.
    
- **Porkbun:** Registers your domain and manages its DNS records.
    

Quartz 5 requires Node.js 22 or later and npm 10.9.2 or later. Its official setup flow uses `npx quartz create`, and its documented Netlify build command is `npx quartz plugin install && npx quartz build`, with `public` as the output directory.

## Before You Begin

You'll need:

- An Obsidian vault with your notes.
    
- A GitHub account: [https://github.com/](https://github.com/)
    
- A Netlify account: [https://www.netlify.com/](https://www.netlify.com/)
    
- A domain registered with Porkbun: [https://porkbun.com/](https://porkbun.com/)
    
- A terminal on your computer.
    
- Node.js, npm, and Git installed.
    

Your domain registration, hosting, and DNS are separate services. You can keep the domain registered at Porkbun and host the website through Netlify without transferring the domain.

---

# Part 1 — Install the Required Tools

## Step 1. Install Node.js

Quartz uses Node.js to generate your website.

1. Visit [https://nodejs.org/](https://nodejs.org/).
    
2. Install Node.js version 22 or later. Use a current supported LTS release that meets Quartz's requirements.
    
3. Finish the installer.
    
4. Open Terminal on your Mac.
    

Check that the installation worked:

```
node -v
npm -v
```

You should see a Node version of at least `v22` and npm version `10.9.2` or later.

If either command says `command not found`, restart Terminal and try again.

## Step 2. Check Git

Run:

```
git --version
```

If Git is not installed, install Apple's command-line tools:

```
xcode-select --install
```

Follow the prompts. Then run `git --version` again.

Git tracks changes to your files and lets Quartz synchronize your project with GitHub.

## Step 3. Choose Where Your Website Project Will Live

I recommend keeping the Quartz project separate from your original Obsidian vault.

For example:

```
Documents/
├── MyObsidianVault/
│   ├── Notes/
│   ├── Projects/
│   └── TTRPGs/
│
└── quartz-site/
    ├── content/
    ├── quartz/
    ├── quartz.config.yaml
    └── package.json
```

This separation is useful because your private vault may contain journal entries, personal information, drafts, attachments, or notes you don't want online.

**Important:** Anything Quartz copies or links into its published content folder may become public. We'll use a copy of your notes first, rather than exposing your entire vault automatically.

---

# Part 2 — Create Your Quartz 5 Website Project

## Step 4. Create a GitHub Repository

The recommended approach is to start from the official Quartz repository template. That gives you the Quartz source code and the correct project structure.

1. Open the official repository: [https://github.com/jackyzha0/quartz](https://github.com/jackyzha0/quartz)
    
2. Select **Use this template → Create a new repository**.
    
3. Name your repository something like `my-website` or `personal-wiki`.
    
4. Choose **Private** if you don't want people browsing your source repository directly. Remember, this does not make the published website private.
    
5. Click **Create repository**.
    

Keep the repository's default branch and settings unless you have a specific reason to change them.

## Step 5. Clone the Repository to Your Mac

Open Terminal. Move to the directory where you want your website project stored:

```
cd ~/Documents
```

Replace `YOUR-GITHUB-USERNAME` and `my-website` below with your actual GitHub username and repository name.

```
git clone https://github.com/YOUR-GITHUB-USERNAME/my-website.git
cd my-website
```

Install Quartz's dependencies:

```
npm install
```

This downloads the packages Quartz needs to build the website.

## Step 6. Initialize Quartz for Your Obsidian Vault

From inside the project directory, run:

```
npx quartz create
```

Quartz 5 launches an interactive setup wizard.

Use these choices:

|Prompt|Recommended choice|
|---|---|
|Template|`obsidian`|
|Content strategy|`copy`|
|Source directory|The path to your existing Obsidian vault|
|Base URL|Your eventual domain, such as `yourdomain.com`|
|Link resolution|Use the Obsidian default, usually `shortest`|

### How to Enter Your Vault's Path

On macOS, the path might look like:

```
/Users/yourusername/Documents/MyObsidianVault
```

You can find it in Finder by opening the vault's folder, then dragging that folder into Terminal to insert its full path.

Choose **Copy**, not **Symlink**, for the initial setup. This puts a separate copy of the notes into Quartz's `content/` folder. Your original vault stays intact.

**If the wizard asks for a base URL now:** enter your planned domain without `https://` or a trailing slash. For example, `notes.example.com`. If you haven't chosen a domain name yet, you can use a temporary hostname and update it later.

The wizard may overwrite `quartz.config.yaml`. If you've already added your custom styling or configuration, back up those changes before running it.

## Step 7. Install Quartz 5 Plugins

After the wizard finishes, run:

```
npx quartz plugin install --from-config
```

This installs the plugins specified in your Quartz configuration.

If you encounter a plugin installation error, stop here and resolve it before deploying. Don't repeatedly reinstall plugins without checking the error.

## Step 8. Preview Your Website Locally

Run:

```
npx quartz build --serve
```

Open this address in your browser:

[http://localhost:8080](http://localhost:8080)

You should see your site generated from the copied Markdown notes.

Check these things:

- The homepage appears.
    
- Your main pages and folders are present.
    
- Obsidian wikilinks work.
    
- Images and attachments load.
    
- Notes you intended to exclude are not published.
    

To stop the local server, return to Terminal and press `Control + C`.

If the website is broken locally, fix it now rather than debugging the same problem through Netlify.

---

# Part 3 — Organize and Protect Your Obsidian Content

## Step 9. Organize the Public Content

In your Quartz project, your published Markdown belongs in:

```
my-website/content/
```

For the homepage you described earlier, you could organize it like this:

```
content/
├── index.md
├── Notes/
│   ├── index.md
│   ├── Interesting Ideas.md
│   └── Books and Resources.md
├── Projects/
│   ├── index.md
│   └── Current Project.md
└── TTRPGs/
    ├── index.md
    ├── Campaigns.md
    └── Worldbuilding.md
```

Quartz turns these Markdown files into website pages.

You can create or edit these files in Obsidian, but if you use a separate Quartz `content/` folder, you need a workflow that keeps the two sets of notes synchronized. The simplest safe workflow is to edit the original vault and periodically copy selected public notes into `content/`.

Alternatively, if you want to write directly into the Quartz content folder, you can open that folder as a second vault in Obsidian.

### Exclude Private Notes

Before you publish, review the content folder and make sure it does not contain:

- Personal journals or private notes.
    
- Passwords, API keys, tokens, or credentials.
    
- Sensitive attachments.
    
- Drafts you don't want publicly accessible.
    
- Private notes pulled in by linked content or transclusions.
    

A private GitHub repository does not prevent Netlify from publishing the website. Anyone who can access the public site can read the pages you deploy.

---

# Part 4 — Push Your Quartz Project to GitHub

## Step 10. Commit and Upload the Initial Website

Open Terminal in your Quartz project directory.

Check the repository status:

```
git status
```

Stage and commit the files:

```
git add .
git commit -m "Set up Quartz 5 website"
git push
```

If Git says there is no upstream branch, use the command it suggests or run:

```
git push -u origin HEAD
```

If GitHub requests authentication, complete the browser sign-in or configure GitHub authentication for your computer.

### Verify the Upload

1. Open your repository on GitHub.
    
2. Refresh the page.
    
3. Confirm you can see `quartz.config.yaml`, `content/`, `quartz/`, and `package.json`.
    

Don't upload generated files, secrets, or private content accidentally. Check `.gitignore` and `git status` before committing.

Quartz also provides its own synchronization command:

```
npx quartz sync
```

Use that for your usual publishing workflow once your initial repository is set up. It can commit and push your changes to GitHub.

## Step 11. Establish Your Regular Writing Workflow

There are two reasonable ways to manage your content.

### Option A — Recommended for a Curated Public Website

Edit your original Obsidian vault, then copy only the notes you want public into Quartz's `content/` directory. Run the local build to preview the changes, then commit and push.

### Option B — Write Directly in Quartz's Content Folder

Open the `content/` folder as a vault in Obsidian. When you edit notes there, Quartz uses those files directly, so you don't need a separate copying step.

For a vault with private and public notes, Option A is safer. If you prefer to manage one public collection directly, Option B is simpler.

Your normal publishing cycle will look like this:

```
Write notes
    ↓
Preview locally
    ↓
Commit and push to GitHub
    ↓
Netlify builds automatically
    ↓
Check the live website
```

---

# Part 5 — Connect GitHub to Netlify

## Step 12. Create a Netlify Site

1. Visit [https://app.netlify.com/](https://app.netlify.com/).
    
2. Sign in or create an account.
    
3. Choose **Add new project** (the wording may vary slightly).
    
4. Select **Import an existing project**.
    
5. Choose **GitHub** as the Git provider.
    
6. Authorize Netlify to access your GitHub account.
    
7. Select your Quartz repository.
    

If your repository is private, authorize access to that repository specifically.

## Step 13. Configure the Build Settings

Use the following settings for Quartz 5.

|Setting|Value|
|---|---|
|Production branch|`main` or the branch containing your Quartz 5 code|
|Base directory|Leave blank if `package.json` is at the repository root|
|Build command|`npx quartz plugin install && npx quartz build`|
|Publish directory|`public`|

Make sure Netlify's build environment uses a compatible Node version. If needed, set the `NODE_VERSION` environment variable to `22` or a later supported version in your site's environment variable settings.

Click **Deploy**.

## Step 14. Verify the First Deployment

Wait for the deployment to finish. Open the Netlify-provided URL, which looks like:

`https://your-site-name.netlify.app`

Check that:

- The homepage loads.
    
- Your notes appear.
    
- Navigation and internal links work.
    
- The page styles and images load.
    
- No unexpected private content appears.
    

If the deployment fails, open its build log. Common causes include a wrong branch, a missing dependency, a Node version mismatch, or an incorrect publish directory.

**Do not continue to DNS setup until the Netlify URL works.**

---

# Part 6 — Connect Your Porkbun Domain

For this walkthrough, I'll use `example.com` as a placeholder. Replace it everywhere with the domain you registered at Porkbun.

You can use either:

- `example.com` — your root domain.
    
- `www.example.com` — a subdomain.
    
- `wiki.example.com` — a subdomain that clearly identifies the site as a wiki.
    

For an Obsidian knowledge base, I would consider `wiki.example.com` if you also want to use the root domain for a different website in the future. If this is your main personal website, `example.com` is a good choice.

## Step 15. Add the Custom Domain in Netlify

Do this before editing Porkbun's DNS records.

1. Open your Netlify project.
    
2. Select **Domain management**.
    
3. Choose **Add a domain**.
    
4. Select **Add a domain you already own**.
    
5. Enter the hostname you want to use.
    
6. Follow the verification prompts.
    

Netlify will associate the domain with your deployed site and show the DNS configuration needed.

If you're using the root domain, Netlify may also add the `www` version as an alternate production domain. Plan to configure both if you want both to work.

## Step 16. Open the DNS Settings in Porkbun

Porkbun's official instructions for managing DNS are here:

[https://kb.porkbun.com/article/231-how-to-add-dns-records-on-porkbun](https://kb.porkbun.com/article/231-how-to-add-dns-records-on-porkbun)

1. Log in to Porkbun: [https://porkbun.com/](https://porkbun.com/)
    
2. Open **Domain Management**.
    
3. Find your domain.
    
4. Click its **DNS** button, or open **Details** and select the DNS records edit option.
    
5. Review the existing records before changing anything.
    

You should now see a table of DNS records and an option to add a record.

## Step 17. Add the Correct DNS Records

The exact records depend on whether you chose the root domain or a subdomain.

### If You Want `example.com` and `www.example.com`

For the root domain, the usual external-DNS fallback is an A record pointing to Netlify's load balancer. For `www`, use a CNAME pointing to your Netlify subdomain.

#### Record 1 — Root Domain

|Field|Value|
|---|---|
|Type|`A`|
|Host|`@` or blank, according to Porkbun's form|
|Answer / Value|`75.2.60.5`|

#### Record 2 — WWW Subdomain

|Field|Value|
|---|---|
|Type|`CNAME`|
|Host|`www`|
|Answer / Value|`your-site-name.netlify.app`|

Replace the sample Netlify hostname with the exact hostname assigned to your site. Do not include `https://`.

Porkbun also supports ALIAS records. Netlify recommends an ALIAS record pointing to `apex-loadbalancer.netlify.com` where the DNS provider supports it; the A record above is the fallback. Use the exact instructions Netlify shows for your domain if they differ.

### If You Want `wiki.example.com`

You only need a CNAME for that subdomain, in addition to the custom domain assignment in Netlify.

|Field|Value|
|---|---|
|Type|`CNAME`|
|Host|`wiki`|
|Answer / Value|`your-site-name.netlify.app`|

Save the record. Leave unrelated email records, including MX records, alone. Also remove conflicting records for the same hostname if they point to a previous host, but don't delete records unless you know what they're used for.

## Step 18. Wait for DNS Verification

Return to Netlify's domain settings.

- Look for a **Pending DNS verification** status.
    
- Follow any customized instructions Netlify provides.
    
- Allow time for DNS changes to propagate.
    

DNS updates often happen quickly, but Netlify notes they can take 24–48 hours, and occasionally longer, to fully propagate.

Don't repeatedly change the DNS records while they're propagating. First verify that the saved records match the values Netlify requested.

---

# Part 7 — Finish Quartz's Domain Configuration

## Step 19. Set `baseUrl` in Quartz 5

Now tell Quartz the hostname where your website will live.

Open `quartz.config.yaml` in your project and find the `baseUrl` field under `configuration`.

For a root domain:

```
configuration:
  baseUrl: example.com
```

For a subdomain:

```
configuration:
  baseUrl: wiki.example.com
```

Use your actual domain, not the sample. Do not include `https://` or a trailing slash.

Keep the rest of your existing YAML configuration unchanged.

**Important:** If you've already deployed the site and then change `baseUrl`, commit and push the change so Netlify rebuilds it.

## Step 20. Commit the Final Change

From your Quartz project directory, run:

```
git add quartz.config.yaml
git commit -m "Configure custom domain"
git push
```

Netlify should detect the new commit and start a deployment.

Wait until it finishes successfully.

## Step 21. Enable HTTPS

After Netlify verifies the DNS records, it should be able to provision a TLS certificate for your domain.

1. Open your Netlify project.
    
2. Go to **Domain management** or the site's HTTPS/TLS settings.
    
3. Check the certificate status.
    
4. Wait for certificate provisioning to finish.
    
5. Open your site using `https://` rather than `http://`.
    

If HTTPS provisioning fails, check for incorrect A records, leftover AAAA records pointing to old hosting, or DNSSEC/CAA conflicts. Netlify documents these as common certificate problems.

---

# Part 8 — Test the Complete Workflow

Use this checklist to verify that the entire system is working.

- The Quartz site builds locally without errors.
    
- The correct public notes appear in the generated website.
    
- The GitHub repository contains the latest Quartz source and content.
    
- Netlify successfully builds the GitHub repository.
    
- The `.netlify.app` preview URL loads.
    
- The custom domain is assigned to the correct Netlify project.
    
- Porkbun DNS records match the configuration requested by Netlify.
    
- The custom domain loads over HTTPS.
    
- Obsidian wikilinks, images, and navigation work on the live site.
    
- A new GitHub commit triggers a fresh Netlify deployment.
    

## How You'll Publish Updates from Now On

Once everything is configured, the routine becomes straightforward:

1. Edit or create notes in Obsidian.
    
2. Copy any selected public notes into the Quartz `content/` folder if you're using the separate-vault workflow.
    
3. Preview the site with `npx quartz build --serve`.
    
4. Commit and push the changes using Git or `npx quartz sync`.
    
5. Wait for Netlify to finish its automatic deployment.
    
6. Visit your custom domain to verify the changes.
    

You don't need to manually upload HTML files to Netlify, and you don't need to move your domain away from Porkbun.

## Troubleshooting

The most useful information to diagnose a problem is the exact step where it fails.

### Quartz Build Error

Check the terminal output from:

```
npx quartz build
```

### GitHub Problem

Check the output from:

```
git push
```

or:

```
npx quartz sync
```

### Netlify Deployment Error

Open the deploy log and look for the first relevant error.

### Custom Domain Problem

Check the DNS records in Porkbun and Netlify's verification status.

## Official References

- Quartz 5 Getting Started: [https://github.com/jackyzha0/quartz/blob/v5/docs/getting-started/index.md](https://github.com/jackyzha0/quartz/blob/v5/docs/getting-started/index.md)
    
- Quartz 5 Hosting: [https://quartz-themes.github.io/primary/hosting](https://quartz-themes.github.io/primary/hosting)
    
- Netlify external DNS: [https://docs.netlify.com/manage/domains/configure-domains/configure-external-dns/](https://docs.netlify.com/manage/domains/configure-domains/configure-external-dns/)
    
- Netlify domain setup: [https://docs.netlify.com/manage/domains/get-started-with-domains/](https://docs.netlify.com/manage/domains/get-started-with-domains/)
    
- Porkbun DNS setup: [https://kb.porkbun.com/article/231-how-to-add-dns-records-on-porkbun](https://kb.porkbun.com/article/231-how-to-add-dns-records-on-porkbun)