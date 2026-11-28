<p align="center"><img src="../brand/logo.svg" alt="Teeny CMS" width="220"></p>

# Teeny CMS: installation and user guide

*"Your website, in plain files."*

This guide takes you from an empty web hosting account to a finished website. You don't need to know any code.
If you get stuck, jump to [Troubleshooting](#16-troubleshooting) at the end.

**Contents**

1. [What you need](#1-what-you-need)
2. [Installing on your web host](#2-installing-on-your-web-host)
3. [Trying it on your own computer](#3-trying-it-on-your-own-computer)
4. [Your first look at the Studio](#4-your-first-look-at-the-studio)
5. [Pages](#5-pages)
6. [Editing a page](#6-editing-a-page)
7. [Library (images)](#7-library-images)
8. [Style](#8-style)
9. [Navigation](#9-navigation)
10. [Messages and subscribers](#10-messages-and-subscribers)
11. [Plugins](#11-plugins)
12. [Team](#12-team)
13. [Preferences](#13-preferences)
14. [Profile and security](#14-profile-and-security)
15. [Backups, moving and updating](#15-backups-moving-and-updating)
16. [Troubleshooting](#16-troubleshooting)

---

## 1. What you need

- **A web hosting account with PHP 8.1 or newer.** Almost every shared hosting plan has this (Hostinger, Namecheap, SiteGround, GoDaddy, cPanel hosts and so on). There's no database to set up.
- **A way to upload files.** Your host's **File Manager** in the control panel works, or an FTP program such as FileZilla.
- **The Teeny CMS files.** Download the latest release from GitHub. On the repository page, click **Code → Download ZIP**, or open the **Releases** page.

Optional but recommended:

- **An SSL certificate (https).** Most hosts give one away free. Turn it on in your control panel.
- **PHP zip support.** It's usually on already. It lets you upload themes and plugins from the Studio.

---

## 2. Installing on your web host

### Step 1: Upload the files

**Using your host's File Manager (easiest)**

1. Sign in to your hosting control panel and open **File Manager**.
2. Go to your website's main folder. It's usually called `public_html`, `www` or `htdocs`.
3. Click **Upload** and choose the Teeny CMS `.zip` file.
4. Right-click the uploaded zip and choose **Extract**.
5. If the files ended up inside a folder (for example `public_html/teeny/`), you have two choices:
   - move everything from that folder up into `public_html`, so your site is at `https://your-site.com`, or
   - leave it there, so your site is at `https://your-site.com/teeny/`. That works too.

**Using FTP (FileZilla)**

1. Unzip Teeny CMS on your computer.
2. Connect to your host with the FTP details from your control panel.
3. Open `public_html` on the right side and upload **the contents** of the Teeny folder into it.

> Make sure the hidden `.htaccess` files are uploaded too. In FileZilla turn on **Server → Force showing hidden files**. These files protect your content.

### Step 2: Check folder permissions

Teeny needs to save files in two folders. Most hosts allow this already. If the installer complains, set these folders to permission **755** (or **775** if 755 doesn't work). You can do this by right-clicking the folder → **Permissions** / **Change permissions**:

- `data/`
- `uploads/`

`themes/` and `plugins/` only need to be writable if you want to upload themes and plugins from the Studio.

### Step 3: Run the installer

1. Open your browser and go to `https://your-site.com/install.php` (add the folder name if you installed in a subfolder).
2. Check the **Server check** box at the top. Green ticks are good. A red cross explains what to fix.
3. Fill in the form:
   - **Site name:** you can change it later.
   - **Your owner account:** choose a username and a password of at least 10 characters. Add your email address too, so you can reset the password if you ever forget it.
   - **What are you building?** Pick a starting layout: Landing page, Portfolio, Documentation, Gallery, Link in bio or Blank. It only fills your home page with example sections.
   - **Pick a look:** choose Minimal, Dark or Luxury and a page alignment. You can change both later.
   - **Clean page addresses:** leave this ticked. If your pages later show "Not Found", see [Troubleshooting](#16-troubleshooting).
4. Click **Install Teeny CMS**.
5. **Save your password and rescue code.** A welcome page shows your rescue code. Copy it or download it, and keep it somewhere safe. If you ever forget your password, it lets you choose a new one without email. Tick the box and click **Go to my Studio**.
6. **Set up email** (recommended). Open **Preferences → Email**, fill in your SMTP details and click **Save & send test email**. Without this, **Forgot password?** can't email you a reset link. See [Email](#email).

### Step 4: Tidy up (recommended)

- **Delete `install.php` from your server.** It already locks itself after installing, so this is just extra safety.
- **Turn on https** in your hosting panel if you haven't already.
- **Bookmark your Studio address:** `https://your-site.com/admin/`

### Extra: nginx servers

Most shared hosts use Apache or LiteSpeed, and Teeny's `.htaccess` files take care of everything there. If your server runs **nginx**, ask your host to add these rules:

```nginx
location ~ ^/(core|data|templates|examples|brand|admin/views)/ { deny all; }
location ~ /\.(?!well-known/) { deny all; }
location ~ (^|/)(error_log|config\.php|config\.sample\.php|web\.config)$ { deny all; }
location ~ ^/uploads/.*\.(php\d?|phtml|phar|pl|py|cgi|sh|html?|js|svg)$ { deny all; }
location ~ ^/(themes|plugins)/.*\.(php\d?|phtml|phar|json|html?|svg|txt|md)$ { deny all; }
location ~ \.(json|md|log|bak|old|orig|sql|zip|tar|gz)$ { deny all; }
location / { try_files $uri $uri/ /index.php?$query_string; }
```

Put these rules **above** your PHP `location` block. If Teeny lives in a subfolder (for example `/site`), add the folder to the start of each path: `^/site/(core|data|…)`.

---

## 3. Trying it on your own computer

You can try Teeny without a web host. Pick whichever suits you.

**In your browser (nothing to install).** Open <https://codespaces.new/teenycms/teeny?quickstart=1> and sign in to GitHub (a free account is fine). After about a minute Teeny opens in a new browser tab with the installer. If VS Code asks whether you trust the authors of the files, click **Trust Folder & Continue**. If no tab opens (your browser may block it), open the **Ports** tab at the bottom of VS Code and click the globe icon next to **Teeny CMS**. The test site lives in your own Codespace. GitHub gives free accounts a monthly allowance of Codespaces hours; stop or delete the Codespace when you're done.

**With PHP installed (version 8.1 or newer):**

1. Unzip Teeny CMS.
2. Open a terminal in that folder and run:
   ```
   php -S localhost:8000 router.php
   ```
3. Visit `http://localhost:8000` and follow the installer.

**With Docker** (no PHP needed), in the unzipped folder run:
```
docker run --rm -p 8000:8000 -v "${PWD}:/app" -w /app php:8.3-cli php -S 0.0.0.0:8000 router.php
```
Then visit `http://localhost:8000`.

`router.php` makes PHP's small built-in server behave like a real host: clean page addresses work and private folders stay private. It does nothing on a real web host. The built-in server is for trying Teeny only, not for a live site.

---

## 4. Your first look at the Studio

The **Studio** is where you edit your site. Its address is `https://your-site.com/admin/`.

Across the top you'll find:

| Area | What it's for |
|---|---|
| **Studio** | Overview: a greeting, a preview card of your site, your recent pages, shortcuts and visitor numbers |
| **Pages** | All your pages: create, edit, duplicate, make one the home page, see its history |
| **Library** | Your images |
| **Messages** | Contact form messages and newsletter subscribers |
| **Navigation** | The links at the top of your website |
| **Style** | Theme, colours, fonts, alignment and your own themes |

Click the **round initials button** in the top-right corner for the rest:

- **Profile & security:** your name, password, two-factor sign-in and rescue code
- **Plugins:** add new kinds of sections
- **Team:** owners and editors
- **Preferences:** site name, logo, maintenance mode, email alerts and more
- **Vault:** save or restore a copy of your site
- **Sign out**

**Visit site** (top right) opens your website in a new tab. The bottom of every Studio page shows your Teeny CMS version.

---

## 5. Pages

Open **Pages** to see everything on your site.

- **New page:** click **New page**, type a title and choose what to start from (a layout or blank). The editor opens straight away. Layouts: Landing page, Portfolio, Documentation, Gallery, Link in bio, **Shop**, **Product page** and **Event** (plus **News** while the News add-on is on).
- **Live or Draft:** a draft is only visible to people signed in to the Studio. When you're happy with it, switch it to **Live** in the editor.
- **Home page:** click the ★ button next to a page to make it your home page. The home page is always live.
- **Duplicate:** copies a page as a new draft, which is handy for similar pages.
- **History:** Teeny keeps the last 20 saved versions of every page. Open **History** and click **Restore** to go back. The version you replace is kept too, so nothing is lost.
- **Delete:** owners only. This can't be undone, so download a copy from the **Vault** first if you're unsure.

---

## 6. Editing a page

Click **Edit** on any page. The screen has two parts:

- **On the left, a panel with three tabs:**
  - **Add:** all the sections you can add, grouped by type, plus an **On this page** list where you can drag sections into a new order.
  - **Section:** the settings of the section you've selected.
  - **Page:** title, page address and other page settings.
- **On the right, your page,** looking exactly as it will on your site.

### Changing text

Just **click on the text and type**. Headings, buttons, captions, names and prices all work this way.

In **Text** and **Image + text** sections, select some words and a small toolbar appears:

- **B** and *I*: bold and italic
- **Link:** paste a web address, a page of your site (for example `/about`), or an email address as `mailto:you@example.com`. Leave it empty to remove a link.
- **H2 / H3:** headings inside the text
- **¶:** back to normal paragraph text
- **• List / 1. List:** bullet or numbered lists
- **“ ”:** a quote
- **Clear:** removes formatting

### Adding sections

Open the **Add** tab and click a section. It's added below the section you selected, or at the end of the page.

| Group | Sections |
|---|---|
| Layout | Hero, Profile, Columns, Pricing, Image + text, Steps, Divider |
| Text | Heading, Text, Quote, Code, FAQ, Tabs, Callout, Table, Events, Table of contents |
| Images & video | Image, Gallery, Video (YouTube or Vimeo), Before / after |
| Links | Button, Link list, Social icons, Contact form, Announcement, Newsletter, RSVP (plus Search and Latest news when those add-ons are on) |
| Business | Numbers, Call to action, Logo strip, Team, Products |
| Popups | Popup: a centered box, a bar or a corner card over the page |
| Plugins | Sections added by plugins you've switched on |

A few tips:

- **Table:** write each row on its own line and separate cells with `|`. Type `yes` or `no` in a cell to show ✓ or ✕.
- **Products:** the button can link anywhere, such as a payment link, your online shop or an email address. Or fill in **WhatsApp number for orders** in the section settings: products without their own link then open WhatsApp with "Hi, I'd like to order: …" already typed.
- **Table of contents:** lists the **Heading** sections on the page and links to them. Choose main headings only, or main and sub-headings.
- **RSVP:** guests say whether they can come, how many people, and an optional note. Replies arrive in **Messages → RSVPs**. Set **Reply by** (YYYY-MM-DD) to close replies after that day.
- **Video:** paste a normal YouTube or Vimeo link in the section settings.
- **Contact form** and **Newsletter:** results arrive in **Messages**.
- **Popup:** choose the **Type** (centered box, bar or corner card), **When it appears** (right away, after a few seconds, after scrolling halfway, or when the visitor moves to leave) and **How often** (every time, once per visit, once a day, week or month, or only once). In the editor it shows as a small preview card; visitors see it over the page. "How often" is remembered in each visitor's own browser, with no cookies. Use popups sparingly: one per page is plenty.

### Moving, copying and deleting sections

Hover over a section to see its toolbar: **↑ ↓** move it, **⧉** duplicates it, **+** adds a new section below, and **🗑** deletes it. You can also drag sections in the **On this page** list. Deleted something by mistake? Press **Undo**, or Ctrl+Z.

### Placement: position, width and title size

When you select a section, **Placement** is the first thing in its settings:

- **Position:** Auto, Left, Center or Right
- **Width:** Auto, Narrow, Medium, Wide or Full width
- **Title size:** S, Auto, L or XL (on sections with a title)

Choose **Narrow** or **Medium** first, then use **Position** to move the section. **Auto** follows the page's normal layout and your theme's own title size. Large titles always fit their column, so on a phone or beside a picture, **L** and **XL** grow only as far as there is room.

### Images in sections

Click any image, or any **Choose image** box, to open your library. You can upload a new image from there. List-style sections (gallery, team, products, logos) have a **+ Add** button under them, and each item can be edited, reordered or removed in the section settings.

### Page settings (the Page tab)

- **Page title** and **Page address** (for example `your-site.com/about`)
- **Description:** a short summary shown by search engines
- **Show in navigation** and **Navigation order:** only shown if your navigation is automatic (see [Navigation](#9-navigation))
- **Alignment for this page:** overrides the site-wide alignment
- **Hide the site header:** nice for link-in-bio pages
- **Footer settings:** opens the footer panel (see below)
- **Password protection:** type a password to lock the page. Visitors must enter it to see the page. Click **Remove password** to unlock it again.
- **Version history:** opens the page's saved versions

### The footer

The footer is the small line at the bottom of every page. New sites start with `© 2026 Your Site · Built with Teeny CMS`. Click it in the editor to type in it. The **Footer** panel opens on the left:

- **Footer text:** the same on every page. Shortcuts: `{year}` shows the current year, `{site}` your site name, and `{teeny}` a "Teeny CMS" link. Change or remove any of them.
- **Links:** add up to 6, such as Privacy or Contact. Pick one of your pages or paste any web address.
- **Social icons:** add up to 8 (Instagram, Facebook, X, LinkedIn and more) with your profile address.
- **Order:** use the ↑ and ↓ buttons next to a link or icon to move it.
- **Show first:** links and icons each get their own row under the footer text. Choose whether the links row or the icons row comes first.
- **Position:** Auto follows the page's alignment, or choose Left, Center or Right.
- **Hide the footer on this page:** only this page. In the editor it stays visible but faded, so you can still click it.

Leave the text, links and icons empty to hide the footer on every page. Changes to the footer are saved when you save the page. Only owners can change the text and position; editors can hide it on a page.

### Saving

- Click **Save**, or press **Ctrl+S** (Cmd+S on a Mac). The top of the screen tells you when there are unsaved changes.
- The **Live / Draft** switch decides who can see the page.
- The **monitor / tablet / phone** buttons at the top show how the page looks on different screens.

---

## 7. Library (images)

Open **Library** to manage your pictures.

- **Upload:** drag images onto the page, or click **Upload**. You can use JPG, PNG, GIF or WebP up to 8 MB each. Large photos are resized automatically if your server supports it.
- **Description:** type a short description under each image. Screen readers use it, and so do search engines.
- **Delete:** removes the image from your site. Pages that used it will show an empty space instead.

---

## 8. Style

Open **Style** to change how your whole site looks.

- **Theme:** choose Minimal, Dark, Luxury, or any theme you've added.
- **Page alignment:**
  - **Normal website:** menu across the top, wide content
  - **Centered**, **Left** or **Right:** a narrow column, nice for personal pages
- **Customise:**
  - colours (background, text, accent and more)
  - fonts for headings and body text
  - text size, corner roundness and content width

  The preview on the right updates as you go. Each theme remembers its own changes.
- **Light & dark for visitors:** adds a small switch to your site so visitors can choose a light or dark version.
- **Fonts from Google:** type the name of any font from [Google Fonts](https://fonts.google.com) (for example *Playfair Display*) and click **Add font**. Teeny downloads it once and saves it on your own website, so visitors never connect to Google and you don't need a cookie notice. The font then appears in the **Heading font** and **Body font** menus. You can add up to 8 fonts and remove them any time; text using a removed font goes back to the theme's own font. If your host blocks outgoing connections, Teeny tells you so.
- **Custom CSS:** for people who know CSS and want to fine-tune details.
- Click **Save style** when you're done.

**Your themes** (at the bottom of the Style page):

- **Save my current style as a theme:** turns your colours, fonts and custom CSS into a new theme you can reuse. Fonts from Google stay with your site, so a shared theme uses the closest built-in font instead.
- **Download** a theme as a `.zip` to keep it or share it.
- **Add a theme (.zip):** upload a theme someone made for you. Every file is checked first, and themes can only contain styles, fonts and images.

**Get more themes** (below Your themes): six official themes made for Teeny: **Bio** (link in bio), **Docs** (documentation, with a sidebar menu), **Launch** (landing pages), **Studio** (portfolios), **Paper** (editorial, pairs with News) and **Bistro** (cafés and restaurants). Click **Install**, then pick it under **Theme**. They download from teenycms.github.io and are checked like any upload. You can also download them from the [themes and plugins page](https://teenycms.github.io/extras/) and upload the .zip yourself. Each theme shows its version beside its name. When a newer version of an official theme is out, a notice appears at the top of the Style page: click **Update** (your colours and fonts for that theme are kept).

---

## 9. Navigation

**Navigation** controls the links at the top of your website.

- At first it's **automatic**: pages with **Show in navigation** ticked appear in the order you set.
- To arrange it yourself, add pages or links to other websites (for example your online shop), reorder them with the arrows or by dragging, then click **Save navigation**.
- You can rename a link without renaming the page.
- **Use automatic navigation** switches back.

Draft and deleted pages are never shown to visitors.

---

## 10. Messages and subscribers

**Messages** collects what visitors send you.

- **Messages tab:** everything sent through **Contact form** sections. Click a message to read it, then **Reply by email**, **Mark as read** or **Delete**. New messages show a number in the top bar.
- **Subscribers tab:** email addresses from **Newsletter** sections. Click **Download CSV** to open the list in Excel, Google Sheets or your email tool.
- **RSVPs tab:** replies from **RSVP** sections (for example on an Event page). You see a running total per event ("42 coming"), each guest's answer, number of people and note, and you can **Download CSV**. A guest who replies again with the same email updates their reply. Set a **Reply by** date in the RSVP section to close replies automatically.
- **Email alerts:** in **Preferences → Messages & statistics**, enter your email address to receive each new message. Messages are always saved in the Studio too, even if email fails.

Spam is blocked without puzzles or "I'm not a robot" boxes. Teeny uses a hidden trap field, a timing check, and limits per visitor and per day.

---

## 11. Plugins

Plugins add new kinds of sections to the editor. Three come with Teeny, switched off:

- **Opening hours:** days and times you're open
- **Testimonials:** quotes from clients, with photos
- **Countdown:** a live timer to a launch or event

To use one, open **Plugins** (profile menu), click **Switch on**, then find its section in the editor under **Add → Plugins**.

**Site search (built-in add-on).** At the top of the Plugins page. It's off until you switch it on:

- Click **Switch on**. A **Search** section appears in the editor under **Add → Links**. Put it on any page.
- Tick **Show a search icon in the site header** and click **Save** to add a magnifier at the end of your menu.
- Results show each matching page with a short extract, and the words you searched for highlighted. Only live pages are searched; drafts and password-protected pages never appear.
- Switch it off any time. Search sections are then hidden from your pages, and come back when you switch it on again.

**News (built-in add-on).** Also at the top of the Plugins page, off until you switch it on:

- Click **Switch on**. **News** appears in the top bar. Type a title and click **New post**; the editor opens.
- Posts use the same editor as pages. In the editor's **Page** tab, set the **date**, **category**, **cover image** and a short **summary**.
- Make a post **Live** to publish it. If its date is in the future, it stays hidden until that day and shows as **Scheduled** (dates follow the time zone in Preferences → Advanced).
- Visitors find all posts at **/news**, each post at /news/your-post, and can filter by category. Readers can follow your news with the RSS feed at /news/feed.xml.
- Add a **Latest news** section to any page to show your newest posts, or start a page from the **News** layout. Categories are managed on the News page.
- Switch News off any time. Your posts are kept and come back when you switch it on again.

**Get more plugins:** at the bottom of the Plugins page, Teeny lists the official extra plugins (Menu or price list, Contact buttons, Location card, Timeline and Image slider). Click **Install**: the plugin downloads from teenycms.github.io, is checked like any upload, and arrives switched off. If your host can't download, get the .zip from the [themes and plugins page](https://teenycms.github.io/extras/) instead. When a newer version of an official plugin is out, a notice appears at the top of the Plugins page: click **Update**. A plugin that was on stays on.

**Adding a plugin:** upload its `.zip` under **Add a plugin**. Teeny checks every file and installs it switched off, so you can review what it does before using it.

**Why plugins are safe:**

- They can't run code on your server.
- Their text is cleaned before it's shown.
- Their styles only affect their own section.
- Any interactive part runs in a sealed box that can't see your site, your visitors or your account.

Want to make your own? See [Make your own plugin](PLUGINS.md). For a theme of your own, see [Make your own theme](THEMES.md).

---

## 12. Team

Open **Team** (profile menu) to give other people access.

| Role | Can do |
|---|---|
| **Owner** | Everything |
| **Editor** | Create and edit pages and manage the Library. Editors can't delete pages, change style or preferences, see messages, or manage plugins and the team. |

- **Add a team member:** enter a name, username, password and role, then share the sign-in details with them safely.
- **Edit** (pencil button):
  - change a person's name, username, email address or role
  - set a new password for them
  - turn off their two-factor sign-in if they lost their phone
- **Remove from team:** they can no longer sign in.

There must always be at least one owner.

---

## 13. Preferences

Open **Preferences** (profile menu) for site-wide settings:

- **General:** site name, tagline and which page is the home page. (The footer is edited on the page itself, see [The footer](#the-footer).)
- **Logo & icons:** a logo shown next to your site name, the small icon in the browser tab, and the image shown when your link is shared on social media
- **Search engines:** a site description and your **website address**. Fill in the address so reset emails, your sitemap and social previews work.
- **Maintenance mode:** shows visitors a "Coming soon" page while you work. You still see the real site while signed in. Turn it off when you're ready.
- **Messages & statistics:** an email address for message alerts, and visitor counting on or off
- **Email:** how your site sends email (see below)
- **Advanced:** language code, time zone, clean page addresses, and **Tell me when a new Teeny version is out**. When it's on, owners see "Teeny x.y.z is available" in the Studio footer. Teeny checks teenycms.github.io once a day and sends nothing about your site.

### Email

Your site sends two kinds of email: **password reset links** and **new message alerts**. The **Email** card shows a short checklist:

- your website address is set
- your account has an email address
- how emails are sent

Click **Save & send test email** to send a test to your own address. If something goes wrong, you'll see the reason straight away.

**How to send:**

- **SMTP** (recommended, and the default on new sites) sends through a real email account, so emails actually arrive. You need:
  - the **SMTP server**
  - the **port** and **security**: usually 587 with TLS, or 465 with SSL
  - your mailbox's **username** (usually the full email address) and **password**

  Set **From address** to the same mailbox. Common settings:

  | Email provider | SMTP server | Port / security | Password |
  |---|---|---|---|
  | Google Workspace or Gmail | `smtp.gmail.com` | 587 / TLS | An *app password*: open myaccount.google.com/apppasswords (needs 2-Step Verification) |
  | Zoho Mail | `smtp.zoho.com` | 465 / SSL | Your password, or an app-specific password |
  | Your host's mailbox (cPanel) | usually `mail.yourdomain.com` | 465 / SSL | That mailbox's password. See **Email Accounts → Connect Devices** |

- **Your host's built-in mail** works with no setup, but many hosts block it, and Gmail often rejects it.

Until email is set up, the Studio home page shows a reminder with a link to these settings.

If the test says "Could not connect", your host may block outgoing mail connections. Try the other port (465 with SSL, or 587 with TLS), or ask your host to allow outgoing SMTP.

The SMTP password is never shown again after you save it. Leave the box empty to keep it. It is also left out of Vault downloads, so enter it again after moving to a new host.

**Recent emails** lists the last few emails. Each one is marked sent, failed (with the reason), or not sent (for example "this account has no email address").

---

## 14. Profile and security

Open **Profile & security** from the profile menu.

- **Change your display name.**
- **Change your username.** This is the name you sign in with. Enter your current password to confirm. Your pages, history and two-factor sign-in stay as they are.
- **Add or change your email address.** It's used for **Forgot password?** emails. Changing it also needs your current password.
- **Rescue code.** Lets you choose a new password from the sign-in page if you forget it, even without email. Confirm your password and click **Create my rescue code** (or **Make a new rescue code**). See [Forgot your password?](#forgot-your-password).
- **Change your password.** This signs you out on every other device.
- **Two-factor sign-in (recommended):**
  1. Install an authenticator app on your phone, for example Google Authenticator, Microsoft Authenticator or Authy.
  2. Click **Set up two-factor sign-in** and scan the QR code with the app. If you can't scan it, type the key shown under it instead.
  3. Type the 6-digit code from the app and click **Turn on**.
  4. **Save your recovery codes.** Each one works once if you lose your phone.

From then on, signing in asks for your password and a code from your phone.

### Forgot your password?

There are three ways back in.

**1. A rescue code (works without email).** Right after installing, Teeny shows you a rescue code and asks you to save it. To use it:

1. On the sign-in page, click **Forgot password?**, then **Use your rescue code**.
2. Enter your username or email, the rescue code and a new password.
3. You get a **new rescue code** straight away, because the old one stops working. Save the new one.

Don't have a code yet, or lost it? In **Profile & security → Rescue code**, confirm your password and click **Create my rescue code** (or **Make a new rescue code**). Keep it somewhere safe, like a password manager or a printed note. Treat it like a password.

**2. A reset email.**

1. On the sign-in page, click **Forgot password?**
2. Enter your username or email address and click **Send reset link**.
3. Open the email and click the link within 1 hour, then choose a new password.

For this to work, your account needs an email address and your site must be able to send email (see [Email](#email)).

**3. Ask another owner.** An owner can set a new password for you under **Team**.

For safety:

- **Reset links and rescue codes work once.**
- **Using one signs you out on all devices.** Two-factor sign-in stays on.
- **The pages never say whether an account exists**, and tries are limited to 5 per hour.
### Keeping your site safe: a short checklist

- Use https. If your site is behind Cloudflare, turn on **SSL/TLS → Edge Certificates → Always Use HTTPS** so visitors are always sent to the secure address.
- Use a long, unique password and turn on two-factor sign-in.
- Delete `install.php` after installing.
- Only give the **Owner** role to people who really need it.
- Download a copy from the **Vault** now and then.
- Update Teeny when a new version comes out (see below).

Teeny also locks an account for 15 minutes after 5 wrong passwords. It signs you out after 2 hours without activity, and every session ends after 12 hours.

---

## 15. Backups, moving and updating

### Making a backup

Open the **Vault** (profile menu):

- **Download site copy (.json):** all pages, preferences and style choices. Passwords and team members are never included.
- **Download library images (.zip):** your images. If your server can't make zip files, download the `uploads/` folder by FTP instead.

You can also simply copy the `data/` and `uploads/` folders by FTP. Together they are your whole site. You can zip them, or keep them in a Git repository to track every change. Only use a **private** repository: `data/` holds password hashes, two-factor secrets, messages and subscriber emails.

### What's in the Teeny folder

```
index.php        public website
install.php      one-time installer (locks itself)
admin/           Teeny Studio (editing area), editor and API
core/            CMS engine (storage, security, rendering)
themes/          minimal, dark, luxury + your own themes
plugins/         section plugins (opening-hours, testimonials, countdown included)
templates/       starter page layouts
assets/          shared site CSS/JS
part.php         sandboxed frame for interactive plugins
router.php       lets PHP's built-in server run Teeny on your computer (does nothing on a web host)
data/            your content (JSON), keep it safe
uploads/         your images
```

The GitHub download also has `brand/`, `docs/` and `examples/`. They aren't needed on your server.

### Moving to a new host

1. Install Teeny on the new host (see section 2).
2. Upload your `uploads/` folder.
3. Open the **Vault** and use **Restore from copy** with your site copy file.

### Updating to a new version

1. Make a backup (above).
2. Read [CHANGELOG.md](../CHANGELOG.md). It lists what changed and **which files to upload**.
3. Upload the new files over the old ones and choose **Overwrite** when asked, but **never overwrite**:
   - `data/` (your content)
   - `uploads/` (your images)
   - `config.php` (if you created one)

   Don't delete folders first. Just upload over them. Themes and plugins you added yourself stay.
4. Reload your site and the Studio. Check the version number at the bottom of any Studio page.

There's no database, so there's nothing else to run.

---

## 16. Troubleshooting

**My pages show "Not Found" (but the home page works).**
Your host doesn't support clean page addresses. In **Preferences → Advanced**, untick **Clean page addresses** and save. Your pages will use addresses like `your-site.com/?page=about` instead.

**The installer says a folder isn't writable.**
Set `data/` and `uploads/` to permission 755, or 775 if 755 doesn't work, using File Manager or FTP. Then refresh the installer.

**I see "Security check failed" or "Your session expired".**
The page was open for a long time. Go back, refresh the page and try again.

**I'm locked out after too many wrong passwords.**
Wait 15 minutes. If you can't wait, delete the file `data/throttle.json.php` by FTP.

**I lost my phone (two-factor sign-in).**
Sign in with one of your recovery codes. Or ask another owner to open **Team → Edit → Turn off two-factor** for you.

**I forgot my password.**
Click **Forgot password?** on the sign-in page, then use your rescue code or a reset email (see [Forgot your password?](#forgot-your-password)).

**I forgot the only owner password, lost my rescue code, and reset emails don't arrive.**
By FTP, delete `data/users.json.php` and upload `install.php` again. The installer will let you create a new owner account. Your pages and settings are kept.

**Reset emails or message alerts don't arrive.**
An owner can open **Preferences → Email**. The checklist and **Recent emails** show what happened, for example an empty website address, an account with no email, or the server refusing to send. Click **Save & send test email**. Check your spam folder too. If your host's built-in mail doesn't deliver, switch to **SMTP** (see [Preferences](#13-preferences)). Messages are always saved in **Messages** either way.

**I can't upload themes or plugins.**
Your server is missing PHP zip support, or `themes/` and `plugins/` aren't writable. You can always unzip the package on your computer and copy its folder into `themes/` or `plugins/` by FTP.

**My changes don't show on the site.**
Make sure you clicked **Save** and the page is **Live**. Then reload the page. Press Ctrl+F5 if your browser shows an old copy.

**Something else?**
Open an issue on the project's GitHub page and describe what you did and what you saw. Please don't post passwords or private information.

---

*"Just files. Just websites."*
