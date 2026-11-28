<a id="top"></a>

<div align="center">

# 🔐 CCODE

### Secure Browser Terminal & tmux Session Manager

**Your Linux terminal. Any screen. One browser tab.**

Mobile-first · Desktop-ready · Persistent tmux sessions · Password + TOTP · One-command installer

[English](#english) · [فارسی](#persian) · [Quick Install](#quick-install) · [Security](#security)

</div>

---

<a id="english"></a>

## English

**CCODE** gives you secure browser-based access to a real Linux shell, with a clean interface designed for phones and comfortable to use on tablets, laptops, and desktop computers.

Create named **tmux** sessions, move between them from the browser, and keep your terminal work running when you refresh the page or close the tab. CCODE installs from a self-contained script, runs in the background, and protects access with a generated password, authenticator-based two-factor authentication (TOTP), an image CAPTCHA, rate limiting, and short-lived terminal tickets.

> **One important distinction:** CCODE's **server and installer run on Linux**. You can connect to its web interface from a phone, tablet, laptop, or desktop computer using a modern browser. Windows and macOS can be client devices; they are not supported as the host operating systems by the installer described here.

<div align="center">

**Linux host** &nbsp;·&nbsp; **Phone + Tablet + Laptop + Desktop** &nbsp;·&nbsp; **Browser-based** &nbsp;·&nbsp; **2FA protected**

</div>

> ⚠️ **Security first:** A web terminal is a full shell running with the permissions of the Linux account that launched it. Use a dedicated, least-privileged account. Review the installer and read [Security](#security) before enabling remote access.

### ✨ Why CCODE?

| Capability | What you get |
| --- | --- |
| 📱 **Mobile-first, desktop-ready** | A responsive browser interface for phones, tablets, laptops, and desktop computers. No separate desktop application is required. |
| 🧩 **Named tmux sessions** | Create, switch, and delete sessions such as `main`, `main-2`, and more. Sessions keep running when the browser tab closes or the panel process restarts. |
| 🔐 **Two-step sign-in** | Generated password + authenticator app (TOTP), with replay protection, image CAPTCHA, rate limiting, and a 15-minute lockout after repeated failures. |
| 🌍 **Choose how to connect** | Run locally on the host or use a Cloudflare Quick Tunnel to get a temporary public HTTPS address. |
| ⚡ **Background service** | The installer starts the panel in the background and returns your terminal prompt. |
| 🧰 **Simple control command** | Use `CCODE` to start or stop the panel, reset terminal sessions, change the password, or show the current URL. |
| 📦 **Self-contained interface** | No external terminal daemon and no CDN dependency; xterm.js is bundled with the project. |

### 💻 Where can you use it?

CCODE separates the **Linux machine that hosts the terminal** from the **device you use to access it**.

| Device / platform | Use CCODE? | Details |
| --- | --- | --- |
| Android phone or tablet | **Yes — browser** | Open the panel URL in a modern mobile browser. |
| iPhone or iPad | **Yes — browser** | Open the panel URL in a modern mobile browser. |
| Windows laptop or desktop | **Yes — browser** | Connect to a CCODE instance running on a Linux host. The installer does not install CCODE as a native Windows service. |
| macOS laptop or desktop | **Yes — browser** | Connect to a CCODE instance running on a Linux host. The installer does not install CCODE as a native macOS service. |
| Linux laptop or desktop | **Yes — browser** | Use it as a client, or host CCODE locally on that Linux machine. |
| Linux server / VPS | **Yes — host** | This is where the CCODE installer and terminal processes run. |

**In short:** You do not need to sit in front of the Linux server. Use your computer or phone as the screen and keyboard, while the terminal itself runs on your Linux machine.

### 📋 Requirements

| Requirement | Details |
| --- | --- |
| Host OS | Linux with an `apt`, `dnf`, `yum`, `pacman`, or `apk`-based package manager |
| Host account | A regular, non-`root` Linux user. The installer and server refuse to run as `root`. |
| Basic tools | `bash` and `curl` |
| Runtime and packages | Node.js 18+, Python 3, `tmux`, `curl`, `tar`, and `sha256sum`. Missing system packages are installed with `sudo` when available. Otherwise, install them manually before running the installer. |
| Older Node.js | If the system Node.js version is below 18, the installer downloads Node.js 22 into the app directory and verifies its SHA-256 checksum. |
| Cloudflare mode | `cloudflared`. It is installed from Cloudflare's signed repository on Debian/Ubuntu and RPM-based systems. On other distributions, install it from [Cloudflare's official downloads](https://developers.cloudflare.com/cloudflare-one/networks/connectors/cloudflare-tunnel/downloads/) first. |
| Network | Internet access is required during installation. |

### 🚀 Quick Install

Run this command as a **regular Linux user** (not `root`):

<a id="quick-install"></a>

```bash
curl -fsSL https://raw.githubusercontent.com/configfars-TM/CCODE/refs/heads/main/terminal-tmux-manager.sh | bash
```

The installer asks you to choose a launch mode:

```text
Launch mode: [L]ocal or [C]loudflare Tunnel? [L]:
```

When setup is complete, it prints your panel address and generated password:

```text
URL: <your-panel-url>
Password: <your-generated-password>
```

Keep both values private. The Cloudflare URL is publicly reachable while the tunnel is running.

<details>
<summary><strong>Prefer to inspect the installer before running it?</strong></summary>

Download the script, review it, and then run it:

```bash
curl -fsSL -o terminal-tmux-manager.sh https://raw.githubusercontent.com/configfars-TM/CCODE/refs/heads/main/terminal-tmux-manager.sh
less terminal-tmux-manager.sh
bash terminal-tmux-manager.sh
```

</details>

<details>
<summary><strong>Skip the launch-mode question</strong></summary>

Set `TERMINAL_MANAGER_MODE` to `local` or `cloudflare` before starting the installer. For example:

```bash
curl -fsSL https://raw.githubusercontent.com/configfars-TM/CCODE/refs/heads/main/terminal-tmux-manager.sh \
  | TERMINAL_MANAGER_MODE=cloudflare bash
```

</details>

### 🔑 First login

1. Open the **URL** printed by the installer on your phone, tablet, laptop, or desktop browser.
2. Enter the generated **password**, complete the image CAPTCHA, and continue.
3. CCODE displays a **TOTP setup secret**. Add it manually to an authenticator app such as Google Authenticator, Aegis, or 2FAS.
4. Enter the current six-digit code from your authenticator to finish pairing.
5. You can now use the terminal. When a login is required again, sign in with your password and a fresh six-digit authenticator code.

The login cookie can remain valid for up to **30 days**. It uses `HttpOnly` and `SameSite=Strict`, and the `Secure` attribute is enabled over HTTPS. The password is required before an authenticator can be paired, helping prevent someone who only discovers the URL from taking over the account.

### 🧰 The `CCODE` control command

The installer adds a `CCODE` command to your Linux user environment. Run it in a terminal on the host machine to open the control menu.

```text
=========== CCODE CONTROL MENU ===========
Panel: running
1) Reset terminal
2) Remove panel
3) Set password again
4) Show password and panel URL again
0) Close
```

| Option | Action |
| --- | --- |
| `1` — Reset terminal | Closes all CCODE tmux sessions and the processes running inside them, then creates a fresh `main` session. Requires typing `YES`. |
| `2` — Remove panel | Stops the panel and removes the installation, saved password, and `CCODE` command. Requires typing `REMOVE`. |
| `3` — Set password again | Generates a new password, signs out browser sessions, and requires the authenticator to be paired again. Also use this to recover when the authenticator is lost. Requires typing `YES`. |
| `4` — Show password and URL | Prints the current `URL:` and `Password:`. |
| `0` — Close | Exits the menu. |

Command shortcuts:

```bash
CCODE          # Open the control menu
CCODE show     # Show the current URL and password
CCODE stop     # Stop the panel; tmux sessions keep running
CCODE start    # Start the panel again
```

> **`CCODE: command not found`?** Your current shell may not have loaded the updated PATH yet. Run `~/.local/bin/CCODE` once, then run `source ~/.bashrc`, or open a new terminal.

### 🌐 Launch modes

| | Local | Cloudflare Tunnel |
| --- | --- | --- |
| Address | `http://127.0.0.1:<port>` | Temporary `https://<name>.trycloudflare.com` address |
| Reachability | Only on the host machine by default | Reachable over the internet while the tunnel is running |
| Typical use | Local access, SSH port forwarding, or your own reverse proxy | Convenient remote access from a phone, tablet, or computer |
| Address changes | When the randomly assigned port changes | On every new tunnel start; use `CCODE show` to see the current URL |

The web server listens on `127.0.0.1` only. In Cloudflare mode, `cloudflared` forwards traffic to that local port; the application server is not directly bound to a public network interface.

### ⚙️ Configuration

The installer accepts these optional environment variables:

| Variable | Default | Description |
| --- | --- | --- |
| `TERMINAL_MANAGER_MODE` | Interactive prompt | `local` or `cloudflare` |
| `TERMINAL_MANAGER_PORT` | `0` | Port on loopback. `0` selects a free port; a value from `1` to `65535` selects a specific port. |
| `TERMINAL_MANAGER_HOME` | `~/.local/share/terminal-tmux-manager` | Installation directory |
| `TERMINAL_MANAGER_TMUX_SOCKET` | `terminal-manager-<uid>` | Name of the isolated tmux socket |

### 🖥️ Using the terminal

- **Create, switch, and delete sessions** from the side menu. Closing the browser or refreshing the page does not stop a tmux session. Deleting a session ends its processes.
- **Use it on a larger screen:** open the same panel URL from your laptop or desktop browser; no separate desktop client is needed.
- **Change language and theme:** switch between English / فارسی and dark / light mode on the login screen or inside the app.
- **Keep tmux separate:** CCODE uses its own tmux socket, so it does not attach to or modify your ordinary tmux sessions.

> **About persistence:** tmux sessions continue while the Linux host is running, even if the browser closes or the CCODE panel is stopped and restarted. A Linux machine reboot stops normal running processes, including tmux sessions; do not treat a panel restart and a host reboot as the same thing.

### 🗂️ Data and file locations

```text
~/.local/share/terminal-tmux-manager/
├── state/     auth.json, auth-key, sessions.json, panel-password
├── logs/      server.log, cloudflared.log
├── run/       process IDs, mode, current URL
└── runtime/   private Node.js 22 (only when system Node.js is older than 18)
```

The `state` directory is private (`0700`). Back up `auth.json`, `auth-key`, and `sessions.json` **together** if you need to preserve the authentication setup and session metadata. Never share these files.

### 🔄 Update, restart, and uninstall

| Task | How |
| --- | --- |
| Update / reinstall | Run the install command again. The existing account data and tmux sessions are kept while the panel is updated and restarted. |
| Start the panel | `CCODE start` |
| Stop the panel | `CCODE stop` — tmux sessions continue while the host remains running. |
| After a host reboot | Run `CCODE start` to start the panel again. Processes from before the reboot should not be assumed to have survived. |
| Uninstall | Run `CCODE`, choose option `2`, and confirm with `REMOVE`. |

### 🔒 Security

#### Built-in protections

- **Loopback binding:** the application server listens on `127.0.0.1`; remote access is provided through a tunnel or a reverse proxy you configure.
- **No root execution:** the installer and server refuse to run as `root`.
- **Password protection:** passwords are stored as salted `scrypt` hashes.
- **Encrypted TOTP secret:** the authenticator secret is encrypted with AES-256-GCM using a private key file.
- **Login defenses:** TOTP replay protection, image CAPTCHA with a small proof-of-work, per-client rate limits, and a 15-minute lockout after repeated failures.
- **Session cookie protection:** the cookie is `HttpOnly` and `SameSite=Strict`; only a hash of the cookie is stored on the server. `Secure` is enabled over HTTPS.
- **Terminal connection tickets:** terminal WebSockets require short-lived, single-use tickets bound to the session, client IP, and origin. Request origins are checked and connection counts are capped.
- **Reduced child environment:** spawned shells receive a reduced environment, and tmux runs through an isolated socket.

#### Important operational notes

- **Use a dedicated Linux account.** The panel can execute shell commands with the permissions of the account running it. Do not run it under an account with more privileges than necessary.
- **A password copy is kept locally.** To support `CCODE show` and the “Show password” menu option, the generated password is also stored in plain text at `state/panel-password` with mode `0600`, inside the private `0700` state directory. Option `3` replaces it; option `2` removes it.
- **Treat the Cloudflare URL as public.** Anyone who obtains the URL can reach the login page while the tunnel is active. Keep the URL and password private, and stop the tunnel with `CCODE stop` when you do not need remote access.
- **Review before execution.** The one-line installer downloads and executes a remote script. Use the manual review steps above if you want to inspect it first.
- **Keep the host maintained.** Update Linux, Node.js, and tmux regularly. No software can be guaranteed to be “unhackable”.

<a id="security"></a>

### 🩺 Troubleshooting

| Problem | What to do |
| --- | --- |
| `CCODE: command not found` | Run `~/.local/bin/CCODE`, then `source ~/.bashrc`, or open a new terminal. |
| `Password: not available here` | The account may have existed before a password copy was saved. Run `CCODE` → `3` to generate a new password. |
| Cloudflare URL does not open yet | Wait briefly and retry. The URL is checked before it is shown, but DNS propagation can take longer on some networks. |
| Authenticator lost | Run `CCODE` → `3`, set a new password, and pair the authenticator again. |
| Panel is not running after reboot | Run `CCODE start`. Remember that starting the panel does not automatically mean processes from before the reboot were restored. |
| `Refusing to run as root` | Run the installer as a regular Linux user. Use `sudo` only when the installer needs to add system packages. |
| `Unsupported Linux package manager` | Install Node.js 18+, Python 3, `tmux`, `curl`, `tar`, `sha256sum`, and the required CA certificates manually, then run the installer again. |

### 👥 Credits

Built by the **CONFIGFARS** team.

### 📄 License

Released under the [MIT License](LICENSE).

<div align="center">

[⬆ Back to top](#top)

</div>

---

<a id="persian"></a>

<div dir="rtl" align="right">

## فارسی

### 🔐 CCODE — ترمینال وب امن و مدیریت نشست‌های tmux

**ترمینال لینوکس شما؛ روی هر صفحه‌نمایشی که در اختیار دارید.**

مناسب موبایل · آماده برای کامپیوتر و لپ‌تاپ · نشست‌های ماندگار tmux · ورود دومرحله‌ای · نصب با یک دستور

**CCODE** دسترسی مرورگری به یک شل واقعی لینوکس را فراهم می‌کند؛ با رابطی که اول برای گوشی طراحی شده، اما روی تبلت، لپ‌تاپ و کامپیوتر رومیزی هم قابل استفاده است.

با CCODE نشست‌های نام‌دار **tmux** بسازید، بین آن‌ها جابه‌جا شوید و پس از رفرش صفحه یا بستن تب، کار ترمینال را ادامه دهید. نصب با یک اسکریپت مستقل انجام می‌شود، پنل در پس‌زمینه اجرا می‌شود و دسترسی با رمز تولیدشده، احراز هویت دومرحله‌ای TOTP، کپچای تصویری، محدودیت نرخ و بلیت‌های کوتاه‌مدت ترمینال محافظت می‌شود.

> **یک تفاوت مهم:** خودِ **سرور CCODE و نصب‌کنندهٔ آن روی لینوکس اجرا می‌شوند**؛ اما برای اتصال به پنل، می‌توانید از مرورگر گوشی، تبلت، لپ‌تاپ یا کامپیوتر استفاده کنید. ویندوز و macOS به‌عنوان دستگاهِ متصل‌شونده قابل استفاده‌اند، ولی طبق پیش‌نیازهای فعلی، سیستم‌عامل میزبانِ نصب‌کننده نیستند.

<div align="center">

**میزبان لینوکسی** &nbsp;·&nbsp; **موبایل + تبلت + لپ‌تاپ + کامپیوتر** &nbsp;·&nbsp; **مبتنی بر مرورگر** &nbsp;·&nbsp; **ورود ۲FA**

</div>

> ⚠️ **امنیت در اولویت است:** ترمینال وب یک شل کامل است و با سطح دسترسی همان کاربر لینوکسی اجرا می‌شود که آن را راه‌اندازی کرده است. از یک حساب کاربری جداگانه و کم‌دسترسی استفاده کنید. قبل از فعال‌کردن دسترسی از راه دور، اسکریپت نصب را بررسی و بخش [امنیت](#persian-security) را مطالعه کنید.

### ✨ چرا CCODE؟

| قابلیت | توضیح |
| --- | --- |
| 📱 **مناسب موبایل، آماده برای دسکتاپ** | رابط مرورگری برای گوشی، تبلت، لپ‌تاپ و کامپیوتر رومیزی؛ بدون نیاز به نصب برنامهٔ جداگانه روی کامپیوتر. |
| 🧩 **نشست‌های نام‌دار tmux** | ساخت، جابه‌جایی و حذف نشست‌هایی مانند `main` و `main-2`. نشست‌ها با بستن تب یا توقف و راه‌اندازی دوبارهٔ پنل ادامه پیدا می‌کنند. |
| 🔐 **ورود دومرحله‌ای** | رمز تولیدشده + برنامهٔ احراز هویت TOTP، همراه با جلوگیری از استفادهٔ مجدد کد، کپچای تصویری، محدودیت نرخ و قفل ۱۵ دقیقه‌ای پس از تلاش‌های ناموفق مکرر. |
| 🌍 **دو روش اتصال** | اجرای محلی روی میزبان یا ساخت آدرس عمومی HTTPS موقت با Cloudflare Quick Tunnel. |
| ⚡ **اجرای پس‌زمینه** | پس از نصب، پنل در پس‌زمینه اجرا می‌شود و خط فرمان آزاد خواهد شد. |
| 🧰 **دستور مدیریتی ساده** | با `CCODE` پنل را روشن یا خاموش کنید، نشست‌ها را ریست کنید، رمز را تغییر دهید یا URL فعلی را ببینید. |
| 📦 **رابط مستقل** | بدون سرویس ترمینال خارجی و بدون وابستگی به CDN؛ فایل‌های xterm.js همراه پروژه هستند. |

### 💻 روی چه دستگاه‌هایی قابل استفاده است؟

در CCODE باید بین **دستگاهی که ترمینال روی آن اجرا می‌شود** و **دستگاهی که با آن به پنل وصل می‌شوید** تفاوت قائل شد.

| دستگاه / سیستم‌عامل | امکان استفاده | توضیح |
| --- | --- | --- |
| گوشی یا تبلت اندرویدی | **بله — از طریق مرورگر** | آدرس پنل را در مرورگر موبایل باز کنید. |
| آیفون یا آیپد | **بله — از طریق مرورگر** | آدرس پنل را در مرورگر باز کنید. |
| لپ‌تاپ یا کامپیوتر ویندوزی | **بله — از طریق مرورگر** | به نمونه‌ای از CCODE که روی یک میزبان لینوکسی اجرا شده متصل شوید؛ نصب‌کننده، سرویس بومی ویندوز نیست. |
| لپ‌تاپ یا کامپیوتر مک | **بله — از طریق مرورگر** | به نمونه‌ای از CCODE که روی یک میزبان لینوکسی اجرا شده متصل شوید؛ نصب‌کننده، سرویس بومی macOS نیست. |
| لپ‌تاپ یا کامپیوتر لینوکسی | **بله — از طریق مرورگر** | هم می‌تواند به‌عنوان کلاینت وصل شود و هم در صورت نیاز میزبان اجرای CCODE باشد. |
| سرور یا VPS لینوکسی | **بله — میزبان اصلی** | نصب‌کننده و پردازش‌های ترمینال روی این دستگاه اجرا می‌شوند. |

**خلاصه:** لازم نیست پشت سرور لینوکسی بنشینید. می‌توانید گوشی یا کامپیوتر خود را به صفحه‌کلید و نمایشگر ترمینال تبدیل کنید؛ درحالی‌که خود ترمینال روی دستگاه لینوکسی اجرا می‌شود.

### 📋 پیش‌نیازها

| مورد | توضیح |
| --- | --- |
| سیستم‌عامل میزبان | لینوکس مبتنی بر `apt`، `dnf`، `yum`، `pacman` یا `apk` |
| حساب کاربری میزبان | کاربر عادی لینوکس؛ نصب‌کننده و سرور با کاربر `root` اجرا نمی‌شوند. |
| ابزارهای پایه | `bash` و `curl` |
| بسته‌ها و محیط اجرا | Node.js 18+، Python 3، `tmux`، `curl`، `tar` و `sha256sum`. بسته‌های سیستمیِ موجودنبوده در صورت دسترسی با `sudo` نصب می‌شوند؛ در غیر این صورت باید آن‌ها را دستی نصب کنید. |
| نسخهٔ قدیمی Node.js | اگر نسخهٔ Node.js سیستم کمتر از ۱۸ باشد، نصب‌کننده Node.js 22 را در پوشهٔ برنامه دانلود و صحت آن را با SHA-256 بررسی می‌کند. |
| حالت Cloudflare | ابزار `cloudflared`. در Debian/Ubuntu و سیستم‌های مبتنی بر RPM از مخزن امضاشدهٔ Cloudflare نصب می‌شود. در سایر توزیع‌ها ابتدا آن را از [صفحهٔ رسمی دانلود Cloudflare](https://developers.cloudflare.com/cloudflare-one/networks/connectors/cloudflare-tunnel/downloads/) نصب کنید. |
| شبکه | هنگام نصب، اتصال اینترنت لازم است. |

### 🚀 نصب سریع

دستور زیر را با یک **کاربر عادی لینوکس** (نه `root`) اجرا کنید:

</div>

```bash
curl -fsSL https://raw.githubusercontent.com/configfars-TM/CCODE/refs/heads/main/terminal-tmux-manager.sh | bash
```

<div dir="rtl" align="right">

نصب‌کننده از شما می‌خواهد روش اجرا را انتخاب کنید:

</div>

```text
Launch mode: [L]ocal or [C]loudflare Tunnel? [L]:
```

<div dir="rtl" align="right">

پس از پایان نصب، آدرس پنل و رمز تولیدشده نمایش داده می‌شوند:

</div>

```text
URL: <your-panel-url>
Password: <your-generated-password>
```

<div dir="rtl" align="right">

این دو مقدار را محرمانه نگه دارید. تا وقتی Cloudflare Tunnel فعال است، آدرس آن از طریق اینترنت قابل دسترسی خواهد بود.

**می‌خواهید قبل از اجرا، اسکریپت را بررسی کنید؟**

ابتدا فایل را دانلود و بررسی کنید، سپس آن را اجرا کنید:

</div>

```bash
curl -fsSL -o terminal-tmux-manager.sh https://raw.githubusercontent.com/configfars-TM/CCODE/refs/heads/main/terminal-tmux-manager.sh
less terminal-tmux-manager.sh
bash terminal-tmux-manager.sh
```

<div dir="rtl" align="right">

**رد کردن سؤال انتخاب حالت اجرا:**

برای اجرای خودکار، مقدار `TERMINAL_MANAGER_MODE` را روی `local` یا `cloudflare` قرار دهید. نمونه:

</div>

```bash
curl -fsSL https://raw.githubusercontent.com/configfars-TM/CCODE/refs/heads/main/terminal-tmux-manager.sh \
  | TERMINAL_MANAGER_MODE=cloudflare bash
```

### 🔑 نخستین ورود

1. آدرس **URL** چاپ‌شده توسط نصب‌کننده را در مرورگر گوشی، تبلت، لپ‌تاپ یا کامپیوتر باز کنید.
2. **رمز عبور** تولیدشده را وارد کنید، کپچای تصویری را تکمیل کنید و ادامه دهید.
3. پنل یک **کلید راه‌اندازی TOTP** نمایش می‌دهد. آن را به‌صورت دستی در برنامه‌ای مانند Google Authenticator، Aegis یا 2FAS وارد کنید.
4. کد شش‌رقمی فعلی برنامهٔ احراز هویت را وارد کنید تا اتصال انجام شود.
5. حالا می‌توانید از ترمینال استفاده کنید. هر زمان ورود دوباره لازم شد، با رمز عبور و یک کد شش‌رقمی جدید وارد شوید.

کوکی ورود می‌تواند تا **۳۰ روز** معتبر بماند. این کوکی از `HttpOnly` و `SameSite=Strict` استفاده می‌کند و در اتصال HTTPS ویژگی `Secure` نیز فعال است. پیش از اتصال برنامهٔ احراز هویت، واردکردن رمز لازم است؛ بنابراین صرفاً پیدا کردن URL برای تصاحب حساب کافی نیست.

### 🧰 دستور مدیریتی `CCODE`

نصب‌کننده دستور `CCODE` را به محیط کاربر لینوکس اضافه می‌کند. آن را در یکی از ترمینال‌های همان دستگاه میزبان اجرا کنید تا منوی مدیریت باز شود.

</div>

```text
=========== CCODE CONTROL MENU ===========
Panel: running
1) Reset terminal
2) Remove panel
3) Set password again
4) Show password and panel URL again
0) Close
```

<div dir="rtl" align="right">

| گزینه | عملکرد |
| --- | --- |
| `1` — Reset terminal | همهٔ نشست‌های tmux مربوط به CCODE و پردازش‌های درون آن‌ها را می‌بندد و یک نشست `main` تازه می‌سازد. تایپ `YES` لازم است. |
| `2` — Remove panel | پنل را متوقف و نصب، رمز ذخیره‌شده و دستور `CCODE` را حذف می‌کند. تایپ `REMOVE` لازم است. |
| `3` — Set password again | رمز جدید می‌سازد، نشست مرورگرها را خارج می‌کند و اتصال دوبارهٔ برنامهٔ احراز هویت را لازم می‌کند. برای بازیابی هنگام گم‌شدن authenticator نیز از همین گزینه استفاده کنید. تایپ `YES` لازم است. |
| `4` — Show password and URL | مقدارهای فعلی `URL:` و `Password:` را چاپ می‌کند. |
| `0` — Close | منو را می‌بندد. |

میان‌برها:

</div>

```bash
CCODE          # باز کردن منوی مدیریت
CCODE show     # نمایش URL و رمز فعلی
CCODE stop     # توقف پنل؛ نشست‌های tmux ادامه می‌دهند
CCODE start    # اجرای دوبارهٔ پنل
```

<div dir="rtl" align="right">

> **خطای `CCODE: command not found`؟** ممکن است شل فعلی هنوز PATH جدید را بارگذاری نکرده باشد. یک بار `~/.local/bin/CCODE` را اجرا کنید، سپس `source ~/.bashrc` بزنید یا یک ترمینال جدید باز کنید.

### 🌐 حالت‌های اجرا

| | Local | Cloudflare Tunnel |
| --- | --- | --- |
| آدرس | `http://127.0.0.1:<port>` | آدرس موقت `https://<name>.trycloudflare.com` |
| دسترسی | به‌صورت پیش‌فرض فقط روی همان دستگاه میزبان | تا وقتی تونل فعال است از طریق اینترنت قابل دسترسی است |
| کاربرد | دسترسی محلی، پورت‌فوروارد SSH یا ریورس‌پراکسی شخصی | دسترسی راحت از راه دور با گوشی، تبلت یا کامپیوتر |
| تغییر آدرس | هنگام تغییر پورت تصادفی | با هر بار راه‌اندازی تونل؛ آدرس فعلی را با `CCODE show` ببینید |

سرور وب فقط روی `127.0.0.1` گوش می‌دهد. در حالت Cloudflare، ابزار `cloudflared` ترافیک را به همان پورت محلی هدایت می‌کند؛ بنابراین خود برنامه مستقیماً روی یک رابط عمومی شبکه قرار نمی‌گیرد.

### ⚙️ تنظیمات

هنگام اجرای نصب‌کننده می‌توانید متغیرهای محیطی اختیاری زیر را تنظیم کنید:

| متغیر | مقدار پیش‌فرض | توضیح |
| --- | --- | --- |
| `TERMINAL_MANAGER_MODE` | پرسش تعاملی | مقدار `local` یا `cloudflare` |
| `TERMINAL_MANAGER_PORT` | `0` | پورت loopback؛ مقدار `0` یک پورت آزاد انتخاب می‌کند و مقدار `1` تا `65535` پورت مشخصی را تعیین می‌کند. |
| `TERMINAL_MANAGER_HOME` | `~/.local/share/terminal-tmux-manager` | مسیر نصب برنامه |
| `TERMINAL_MANAGER_TMUX_SOCKET` | `terminal-manager-<uid>` | نام سوکت اختصاصی tmux |

### 🖥️ استفاده از ترمینال

- **ساخت، جابه‌جایی و حذف نشست‌ها:** از منوی کناری استفاده کنید. بستن مرورگر یا رفرش صفحه، نشست tmux را متوقف نمی‌کند؛ اما حذف نشست، پردازش‌های آن را می‌بندد.
- **استفاده با نمایشگر بزرگ‌تر:** همان URL پنل را در مرورگر لپ‌تاپ یا کامپیوتر باز کنید؛ نیازی به کلاینت جداگانه نیست.
- **تغییر زبان و پوسته:** زبان English / فارسی و حالت تاریک / روشن از صفحهٔ ورود یا داخل برنامه قابل تغییر است.
- **جداسازی tmux:** CCODE از سوکت اختصاصی خودش استفاده می‌کند و به نشست‌های معمولی tmux شما متصل نمی‌شود یا آن‌ها را تغییر نمی‌دهد.

> **نکته دربارهٔ ماندگاری نشست‌ها:** نشست‌های tmux تا وقتی سیستم‌عامل میزبان روشن است، حتی با بسته‌شدن مرورگر یا توقف و راه‌اندازی دوبارهٔ پنل ادامه پیدا می‌کنند. با ریبوت خودِ سرور لینوکسی، پردازش‌های در حال اجرا از جمله نشست‌های عادی tmux متوقف می‌شوند؛ بنابراین ریستارت پنل را با ریبوت سیستم یکی ندانید.

### 🗂️ محل فایل‌ها و داده‌ها

</div>

```text
~/.local/share/terminal-tmux-manager/
├── state/     auth.json, auth-key, sessions.json, panel-password
├── logs/      server.log, cloudflared.log
├── run/       process IDs, mode, current URL
└── runtime/   private Node.js 22 (only when system Node.js is older than 18)
```

<div dir="rtl" align="right">

پوشهٔ `state` خصوصی است (`0700`). اگر لازم است تنظیمات ورود و فرادادهٔ نشست‌ها را نگه دارید، از فایل‌های `auth.json`، `auth-key` و `sessions.json` **با هم** نسخهٔ پشتیبان بگیرید. این فایل‌ها را در اختیار دیگران قرار ندهید.

### 🔄 به‌روزرسانی، راه‌اندازی مجدد و حذف

| کار | روش |
| --- | --- |
| به‌روزرسانی / نصب مجدد | دستور نصب را دوباره اجرا کنید. داده‌های حساب و نشست‌های tmux هنگام به‌روزرسانی پنل حفظ می‌شوند. |
| روشن کردن پنل | `CCODE start` |
| خاموش کردن پنل | `CCODE stop` — تا وقتی میزبان روشن است، نشست‌های tmux ادامه می‌دهند. |
| پس از ریبوت میزبان | برای راه‌اندازی دوبارهٔ پنل `CCODE start` را اجرا کنید. فرض نکنید پردازش‌های قبل از ریبوت همچنان فعال‌اند. |
| حذف کامل | دستور `CCODE` را اجرا کنید، گزینهٔ `2` را انتخاب کنید و با `REMOVE` تأیید کنید. |

<a id="persian-security"></a>

### 🔒 امنیت

#### محافظت‌های داخلی

- **اتصال loopback:** برنامه فقط روی `127.0.0.1` گوش می‌دهد؛ دسترسی از راه دور از طریق تونل یا ریورس‌پراکسی انتخابی شما انجام می‌شود.
- **عدم اجرا با root:** نصب‌کننده و سرور اجرای مستقیم با کاربر `root` را رد می‌کنند.
- **محافظت رمز عبور:** رمزها به‌صورت هش `scrypt` همراه با salt ذخیره می‌شوند.
- **رمزنگاری کلید TOTP:** راز برنامهٔ احراز هویت با AES-256-GCM و یک فایل کلید خصوصی رمزنگاری می‌شود.
- **دفاع در برابر تلاش ورود:** جلوگیری از استفادهٔ مجدد کد TOTP، کپچای تصویری همراه با اثبات کار کوچک، محدودیت نرخ برای هر کلاینت و قفل ۱۵ دقیقه‌ای پس از تلاش‌های ناموفق مکرر.
- **محافظت کوکی نشست:** کوکی `HttpOnly` و `SameSite=Strict` است؛ فقط هش آن روی سرور ذخیره می‌شود. در HTTPS ویژگی `Secure` نیز فعال است.
- **بلیت اتصال ترمینال:** WebSocket ترمینال به بلیت کوتاه‌مدت و یک‌بارمصرفی نیاز دارد که به نشست، IP کلاینت و origin متصل است. origin درخواست‌ها بررسی و تعداد اتصال‌ها محدود می‌شود.
- **محیط محدودتر شل:** شل‌های فرزند محیط کاهش‌یافته دریافت می‌کنند و tmux از سوکت جداگانه استفاده می‌کند.

#### نکات مهم عملیاتی

- **از حساب کاربری جداگانه استفاده کنید.** پنل می‌تواند فرمان‌های شل را با دسترسی‌های همان حساب اجرا کند. آن را با حسابی که دسترسی‌های غیرضروری دارد اجرا نکنید.
- **یک نسخهٔ متنی از رمز روی دستگاه نگه‌داری می‌شود.** برای کارکرد `CCODE show` و گزینهٔ نمایش دوبارهٔ رمز، رمز تولیدشده در فایل `state/panel-password` نیز به‌صورت متن ساده با مجوز `0600` ذخیره می‌شود؛ این فایل داخل پوشهٔ خصوصی `state` با مجوز `0700` قرار دارد. گزینهٔ `3` آن را جایگزین و گزینهٔ `2` حذف می‌کند.
- **آدرس Cloudflare را عمومی فرض کنید.** هر فردی که URL را به دست آورد، تا زمان فعال‌بودن تونل می‌تواند صفحهٔ ورود را ببیند. URL و رمز را محرمانه نگه دارید و وقتی دسترسی از راه دور لازم نیست با `CCODE stop` پنل را متوقف کنید.
- **پیش از اجرا بررسی کنید.** دستور نصب یک‌خطی، اسکریپت راه دور را دانلود و اجرا می‌کند. برای بررسی دستی، از دستورهای بخش نصب سریع استفاده کنید.
- **میزبان را به‌روز نگه دارید.** لینوکس، Node.js و tmux را مرتب به‌روزرسانی کنید. هیچ نرم‌افزاری را نمی‌توان «هک‌ناپذیر» تضمین کرد.

### 🩺 عیب‌یابی

| مشکل | راه‌حل |
| --- | --- |
| `CCODE: command not found` | `~/.local/bin/CCODE` را اجرا کنید، سپس `source ~/.bashrc` بزنید یا ترمینال جدید باز کنید. |
| `Password: not available here` | ممکن است حساب از قبل وجود داشته و نسخهٔ قابل نمایش رمز ذخیره نشده باشد. با `CCODE` ← `3` رمز جدید بسازید. |
| آدرس Cloudflare هنوز باز نمی‌شود | کمی صبر کنید و دوباره امتحان کنید. آدرس پیش از نمایش بررسی می‌شود، اما انتشار DNS در بعضی شبکه‌ها زمان بیشتری می‌برد. |
| برنامهٔ احراز هویت را گم کرده‌اید | `CCODE` ← `3` را اجرا کنید، رمز جدید بسازید و برنامهٔ احراز هویت را دوباره متصل کنید. |
| پنل پس از ریبوت اجرا نیست | `CCODE start` را اجرا کنید. توجه کنید اجرای دوبارهٔ پنل به معنی بازیابی پردازش‌های قبل از ریبوت نیست. |
| `Refusing to run as root` | نصب‌کننده را با یک کاربر عادی لینوکس اجرا کنید. فقط برای نصب بسته‌های سیستمی از `sudo` استفاده می‌شود. |
| `Unsupported Linux package manager` | Node.js 18+، Python 3، `tmux`، `curl`، `tar`، `sha256sum` و گواهی‌های CA لازم را دستی نصب و دوباره اجرا کنید. |

### 👥 سازندگان

ساخته‌شده توسط تیم **CONFIGFARS**.

### 📄 مجوز استفاده

منتشرشده تحت [مجوز MIT](LICENSE).

</div>

<div align="center">

[⬆ بازگشت به بالای صفحه](#top)

</div>
