# WEBSITE BACKUP PROTOCOL - Russell Capital (New Website)
**Date Created:** May 19, 2026  
**Purpose:** Ensure full persistence and rapid recovery for the **Russell Capital new website** as part of mission continuity and risk mitigation. Aligns with building practical persistence tools. This is the specified new site from our session.  
**Owner:** Samuel A. (with Peter/Grok support)  
**Status:** Draft v1.0 - To be customized with specific site details.

## Mission Context
Backing up the new website is non-negotiable for long-term project survival. This protocol compounds our efforts in persistence, similar to how we handle corpus, standing orders, and MASTER_SUMMARY files. It prevents single points of failure in hosting, code, or data. We'll integrate this into daily/weekly workflows and version control it.

## Key Principles
- **Redundancy:** Multiple copies in different locations (local, cloud, git).
- **Automation:** Minimize manual effort; script and schedule where possible.
- **Verification:** Backups are useless if not tested.
- **Documentation:** Keep this protocol updated; log all backup events.
- **Truth-Seeking:** Use reliable, proven methods. Avoid over-reliance on any single provider.

## Prerequisites / What You Need
1. **Credentials & Access:**
   - Hosting provider login (cPanel, dashboard, SSH keys, FTP/SFTP).
   - Database access (phpMyAdmin, MySQL credentials) if the site uses a DB.
   - GitHub/GitLab account and repo access (highly recommended).
   - Cloud storage (Google Drive, Dropbox, AWS S3, or similar) for offsite copies.

2. **Tools (install if needed):**
   - Git (for code versioning).
   - rsync or WinSCP/FileZilla for file transfers.
   - Terminal/SSH client.
   - For dynamic sites: mysqldump or hosting export tools.
   - Optional: Backup plugins (e.g., UpdraftPlus for WordPress), Duplicator, or All-in-One WP Migration.

3. **Site Specifics (Fill these in):**
   - **Tech Stack:** [e.g., Static HTML/JS, WordPress, Next.js/React, Laravel, etc.]
   - **Hosting Provider:** [e.g., Vercel, Netlify, SiteGround, Bluehost, AWS, custom VPS]
   - **Domain:** [yourdomain.com]
   - **Database:** Yes/No. Name if yes.
   - **Repo URL:** https://github.com/samtheinsuranceman-debug/Russell-Capital-Solutions-NEW
   - **Current Size Estimate:** [small/medium/large - affects backup time]

## Step-by-Step: How to Back Up the New Website (Manual First, Then Automate)

### 1. Secure the Code / Source Files (Most Important for Rebuilds)
- **If using Git (Best Practice):**
  1. Navigate to project root locally or via SSH.
  2. `git status` to check changes.
  3. `git add .`
  4. `git commit -m "Backup snapshot: [YYYY-MM-DD] - Pre-change / routine"`
  5. `git push origin main` (or your branch).
- **If no Git yet:** Initialize with `git init`, add remote, commit, push. This is priority #1 for any code-based site.
- Download full file tree as zip/tar via hosting file manager or FTP as secondary.

### 2. Database Backup (If Dynamic Site - e.g. WordPress, custom app)
- Via hosting panel (phpMyAdmin): Export entire DB as .sql file. Choose "Quick" or custom with all tables.
- Via command line (SSH):
  ```
  mysqldump -u [username] -p[password] [database_name] > /path/to/backup_[date].sql
  ```
  (Use strong password handling; consider .my.cnf for automation.)
- Compress: `gzip backup.sql`
- Note: Some hosts have one-click DB backups.

### 3. Full Filesystem / Media Backup
- **Via Hosting Tools:** Use built-in backup feature if available (many have daily/weekly).
- **Manual via FTP/SFTP or File Manager:**
  - Download /public_html or root web directory.
  - Especially critical: wp-content/uploads (or equivalent media/assets folder), themes, plugins.
- **Efficient with rsync (recommended for repeats):**
  ```
  rsync -avz --progress user@server:/path/to/website/ /local/backup/path/
  ```
- Include .htaccess, config files, robots.txt, etc.

### 4. Complete Snapshot (Recommended)
- Create a single archive:
  ```
  tar -czvf website_backup_[date].tar.gz /path/to/website/files /path/to/db_backup.sql
  ```
- Or use hosting's full backup tool and download the archive.

### 5. Offsite & Redundant Storage
- Copy to:
  1. Local external drive or computer.
  2. Cloud storage (upload the tar.gz or individual files).
  3. Secondary Git repo or private GitHub repo if sensitive.
  4. Another hosting account or VPS if possible.
- **Never rely on hosting provider alone** — they can have outages, account issues, or data loss.

### 6. Verification & Testing (Critical - Do This!)
- Download backup and attempt restore on a local environment (e.g., XAMPP, LocalWP for WP sites, or Docker).
- For static sites: Serve locally and check functionality.
- Check for missing files, broken links, DB import success.
- Document any issues found.

## Automation Recommendations
- **Cron Jobs (on server or local):**
  - Daily DB + files backup script.
  - Example simple script (bash): Combine mysqldump + rsync + tar + upload to cloud via rclone or similar.
- **Third-Party Services:**
  - WordPress: UpdraftPlus (free version good; premium for more destinations).
  - General: Jetpack Backup, VaultPress, or hosting add-ons.
  - Static sites on Vercel/Netlify: Often have deploy history; supplement with Git.
- **Schedule:**
  - **Daily:** Automated DB + incremental files.
  - **Weekly:** Full snapshot + offsite push.
  - **Before/After Major Updates:** Manual full backup.
- Tools like rclone for cloud sync, or Duplicati for encrypted backups.

## Logging & Continuity
- Maintain a log: Date, type (full/incremental), location(s), size, verification status.
- Update this protocol after each major change to the site.
- Integrate with MASTER_SUMMARY or standing orders: Reference this file in continuity reports.
- Push this .md (and backups) to a dedicated Git repo for the project.

## Risk Mitigation
- **Single Point of Failure:** Hosting account lockout? Have SSH/FTP + Git as fallback.
- **Data Corruption:** Verify regularly; keep multiple dated versions (retain last 7-30 days).
- **Cost:** Free tiers sufficient for most; budget for premium backup services if site grows.
- **Security:** Encrypt sensitive backups; use strong unique passwords; limit access.
- **Theological/ Philosophical Note (per standing guidance):** Data persistence mirrors deeper questions of continuity and legacy. We build tools that endure.

## Next Actions for Samuel
1. Fill in the [Site Specifics] section above with details about the new website.
2. Perform the first full manual backup this week using steps 1-6.
3. Share the tech stack/hosting details with me for a customized script or refined protocol.
4. Set up Git if not already (priority).
5. Decide on automation tool and implement one recurring backup.
6. Test a restore.

## Updates & Versioning
- This file lives in /artifacts/ and should be committed to Git.
- v1.0: Initial protocol based on standard best practices.
- Future: Add site-specific commands, cron examples, restore procedures, monitoring.

**Peter/Grok Note:** This is built for high-intensity execution and persistence. Once customized, it becomes a core standing tool. Let's compound this — back it up, test it, automate it, and move the mission forward. Ready when you are for the next layer (e.g., custom backup script, integration with other continuity files, or full project audit).

---
*End of Protocol v1.0. Update as site evolves.*
