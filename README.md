<a id="english"></a>

<div align="center">

# 🔐 CCODE — Secure Mobile Terminal + tmux Manager

**A mobile-friendly web terminal with persistent tmux sessions, password + authenticator (2FA) login, and one-command install.**

**English** | [فارسی](#persian)

</div>

---

## English

CCODE gives you a real shell in your browser — built for phones first — with named **tmux** sessions that keep running when you close the page. It installs from a single self-contained script, runs in the background, and protects the terminal with a generated password, an authenticator app (TOTP), an image CAPTCHA, rate limiting and short-lived terminal tickets.

> ⚠️ A web terminal is a **full shell** running as the Linux user that started it. Use a dedicated, least-privileged account and read the [Security notes](#-security-notes) before exposing it to the internet.

### ✨ Highlights

- 📱 **Mobile-first UI** — dark mode by default, light mode and **English / فارسی** built in
- 🧩 **tmux sessions** — create, switch and delete named sessions (`main`, `main-2`, …); they survive refreshes, closed tabs and server restarts
- 🔑 **Password set for you** — the installer generates a strong random password and prints it together with the URL
- 🛡️ **Two-step login** — password + authenticator app (TOTP), replay-protected, plus an image CAPTCHA, rate limits and a 15-minute lockout after repeated failures
- 🌐 **Two launch modes** — *Local* (this machine only) or *Cloudflare Tunnel* (a temporary public HTTPS URL)
- 🚀 **Runs in the background** — after install your terminal is free again and the URL keeps working
- 🧰 **`CCODE` command** — reset terminals, remove the panel, change the password, or show the URL again
- 📦 **Self-contained** — no external terminal daemon, no CDN; xterm.js is bundled

### 📋 Requirements

| Need | Details |
| --- | --- |
| OS | Linux (apt, dnf, yum, pacman or apk based) |
| User | A **regular user** — the installer and the server refuse to run as `root` |
| Tools | `bash` and `curl` |
| Packages | Node.js 18+, Python 3, `tmux`, `curl`, `tar`, `sha256sum` — missing ones are installed for you with `sudo` (or run the installer as a user who can use `sudo`). If your system Node.js is older than 18, a SHA-256-verified Node.js 22 is installed privately inside the app folder |
| Cloudflare mode | `cloudflared` — installed automatically from Cloudflare's signed repository on Debian/Ubuntu and RPM-based systems; on other distributions install it first from Cloudflare's downloads page |
| Network | Internet access during installation |

### 🚀 Quick Install

```bash
curl -fsSL https://raw.githubusercontent.com/configfars-TM/CCODE/refs/heads/main/terminal-tmux-manager.sh | bash
```

> 💡 **Prefer to review the script first?**
>
> ```bash
> curl -fsSL -o terminal-tmux-manager.sh https://raw.githubusercontent.com/configfars-TM/Web-Terminal/refs/heads/main/terminal-tmux-manager.sh
> less terminal-tmux-manager.sh
> bash terminal-tmux-manager.sh
> ```

The installer asks one question:

```text
Launch mode: [L]ocal or [C]loudflare Tunnel? [L]:
```

and ends with the only two lines you need:

```text
URL: https://example-words.trycloudflare.com
Password: Xk3mP9qRtV2wBnA7sLd4
```

> To skip the question (for scripts or non-interactive shells) set `TERMINAL_MANAGER_MODE=local` or `TERMINAL_MANAGER_MODE=cloudflare`, e.g.
> `curl -fsSL <script-url> | TERMINAL_MANAGER_MODE=cloudflare bash`

### 🧭 First login (one time)

1. Open the **URL** printed by the installer.
2. Enter the **password** from the installer, solve the security check, and continue.
3. The panel shows a **TOTP secret**. In your authenticator app (Google Authenticator, Aegis, 2FAS, …) choose *manual entry*, type the secret, then enter the 6-digit code it generates.
4. You are in. The login cookie lasts up to **30 days** (`HttpOnly`, `SameSite=Strict`, `Secure` over HTTPS). Next time you sign in with the **password + a fresh 6-digit code**.

The password is required *before* the authenticator can be paired, so a stranger who finds your URL cannot take over the account.

### 🧰 The `CCODE` command

The installer adds a `CCODE` command. Type it in any terminal on the machine to open the control menu:

```text
=========== CCODE CONTROL MENU ===========
Panel: running
1) Reset terminal
2) Remove panel
3) Set password again
4) Show password and panel URL again
0) Close
```

| Option | What it does |
| --- | --- |
| `1` Reset terminal | Closes **all** tmux terminals (and what runs in them) and creates a fresh `main`. Asks you to type `YES`. |
| `2` Remove panel | Stops the panel and deletes the installation, saved password and the `CCODE` command. Asks you to type `REMOVE`. |
| `3` Set password again | Generates a **new** password, signs out every browser and requires the authenticator to be paired again (also the way to recover a lost authenticator). Asks you to type `YES`. |
| `4` Show password and panel URL again | Prints the current `URL:` and `Password:`. |
| `0` Close | Leaves the menu. |

Shortcuts:

```bash
CCODE          # open the menu
CCODE show     # print URL and password
CCODE stop     # stop the panel (tmux sessions keep running)
CCODE start    # start it again, e.g. after a reboot
```

> **`CCODE: command not found`?** An already-open shell doesn't see the new command yet. Run `~/.local/bin/CCODE` once, or `source ~/.bashrc`, or open a new terminal.

### 🌐 Launch modes

| | Local | Cloudflare Tunnel |
| --- | --- | --- |
| Reachable from | Only this machine (`http://127.0.0.1:<port>`) | Anywhere, through a temporary `https://….trycloudflare.com` URL |
| Good for | SSH port-forwarding, your own reverse proxy | Quick access from your phone |
| URL changes? | When the random port changes | **Every start** — use `CCODE show` for the current one |

The server always listens on `127.0.0.1` only; the tunnel connects to that loopback port.

### ⚙️ Configuration

Set these environment variables when running the installer (all optional):

| Variable | Default | Meaning |
| --- | --- | --- |
| `TERMINAL_MANAGER_MODE` | asks you | `local` or `cloudflare` |
| `TERMINAL_MANAGER_PORT` | `0` (random free port) | Fixed loopback port, `0`–`65535` |
| `TERMINAL_MANAGER_HOME` | `~/.local/share/terminal-tmux-manager` | Install folder |
| `TERMINAL_MANAGER_TMUX_SOCKET` | `terminal-manager-<uid>` | Name of the isolated tmux socket |

### 📱 Using the terminal

- **Create / switch / delete** sessions from the side menu. Closing the browser does **not** stop them; **deleting** a session ends its processes.
- Language (English / فارسی) and theme (dark / light) can be switched on the login screen and inside the app.
- Terminals use their own tmux socket, so they never touch your ordinary tmux sessions.

### 🗂️ Where things live

```text
~/.local/share/terminal-tmux-manager/
├── state/   auth.json, auth-key, sessions.json, panel-password   (private, 0700)
├── logs/    server.log, cloudflared.log
├── run/     process IDs, mode, current URL
└── runtime/ private Node.js 22 (only if your system Node.js is < 18)
```

Back up `auth.json`, `auth-key` and `sessions.json` **together** if you want to keep your login; never share them.

### 🔄 Update, restart, uninstall

- **Update / restart:** run the install command again. Your account and tmux sessions are kept; the panel restarts (Cloudflare mode gets a new URL).
- **After a reboot:** `CCODE start`.
- **Uninstall:** `CCODE` → `2`.

### 🔒 Security notes

What is built in:

- Server binds to **loopback only**; remote access goes through an HTTPS tunnel or reverse proxy you choose
- Refuses to run as **root**
- Password stored as a salted **scrypt** hash; the TOTP secret is encrypted (AES-256-GCM) with a private key file
- **TOTP replay protection**, image **CAPTCHA** plus a small proof-of-work, per-client **rate limits** and a **15-minute lockout**
- Login cookie is `HttpOnly` + `SameSite=Strict`; only a hash of it is stored on the server
- Terminal WebSockets use a **short-lived, single-use ticket** bound to the session, client IP and origin; request origins are checked; connection counts are capped
- Child shells get a reduced environment; tmux runs on an isolated socket

What you should know:

- The panel is **only as safe as the account it runs under** — use a dedicated low-privilege user, not your main account.
- For *Show password again* to work, the generated password is also kept in plain text in `state/panel-password` (mode `0600`, inside the private `0700` state folder). Choosing `3` replaces it; `2` deletes it.
- A Cloudflare quick tunnel URL is public while it runs. Keep the URL and password private, and stop it (`CCODE stop`) when you don't need it.
- No system can promise "unhackable". Keep your OS, Node.js and tmux updated and review the script before running it.

### 🩺 Troubleshooting

| Problem | Fix |
| --- | --- |
| `CCODE: command not found` | Run `~/.local/bin/CCODE`, then `source ~/.bashrc` (or open a new terminal) |
| `Password: not available here` | An account already existed and its password isn't stored. Run `CCODE` → `3` for a new one |
| Cloudflare URL doesn't open yet | Wait a few seconds and retry; it is checked before being shown, but DNS can lag on some networks |
| Lost your authenticator | `CCODE` → `3` (new password, pair the authenticator again) |
| Panel stopped after reboot | `CCODE start` |
| "Refusing to run as root" | Run the installer as a normal user (with `sudo` available) |
| "Unsupported Linux package manager" | Install Node.js 18+, Python 3, `tmux`, `curl`, `tar`, `sha256sum` manually and run again |

### 👥 Credits

Built by the **CONFIGFARS** team.

### 📄 License

Released under the [MIT License](LICENSE).

---

<a id="persian"></a>

<div dir="rtl" align="right">

## 🔐 CCODE — ترمینال وب امن برای موبایل + مدیریت tmux

[English](#english) | **فارسی**

**CCODE یک ترمینال وب مناسب موبایل با نشست‌های ماندگار tmux، ورود با رمز عبور + برنامهٔ احراز هویت (۲FA) و نصب با یک دستور است.**

CCODE یک شل واقعی را داخل مرورگر در اختیارتان می‌گذارد؛ با اولویت گوشی و نشست‌های نام‌دار **tmux** که با بستن صفحه هم ادامه می‌دهند. نصب از یک اسکریپت مستقل انجام می‌شود، پنل در پس‌زمینه اجرا می‌شود و ترمینال با رمز تصادفی ساخته‌شده، برنامهٔ احراز هویت (TOTP)، CAPTCHA تصویری، محدودیت نرخ و بلیت‌های کوتاه‌مدت ترمینال محافظت می‌شود.

> ⚠️ ترمینال وب یک **شل کامل** است که با دسترسی همان کاربر لینوکسی اجرا می‌شود که آن را راه‌اندازی کرده. از یک حساب جداگانه و کم‌دسترسی استفاده کنید و پیش از قراردادن روی اینترنت، بخش [نکات امنیتی](#-نکات-امنیتی) را بخوانید.

### ✨ ویژگی‌ها

- 📱 **رابط مخصوص موبایل** — حالت تاریک پیش‌فرض، حالت روشن و زبان‌های **English / فارسی** داخلی
- 🧩 **نشست‌های tmux** — ساخت، جابه‌جایی و حذف نشست‌های نام‌دار (`main`، `main-2`، …)؛ با رفرش، بستن تب و ریستارت سرور باقی می‌مانند
- 🔑 **رمز را خود نصب‌کننده می‌سازد** — یک رمز تصادفی قوی تولید می‌کند و همراه URL چاپ می‌کند
- 🛡️ **ورود دومرحله‌ای** — رمز + برنامهٔ احراز هویت (TOTP) با جلوگیری از استفادهٔ مجدد کد، به‌همراه CAPTCHA تصویری، محدودیت نرخ و قفل ۱۵ دقیقه‌ای پس از تلاش‌های ناموفق مکرر
- 🌐 **دو حالت اجرا** — *Local* (فقط همین دستگاه) یا *Cloudflare Tunnel* (آدرس عمومی HTTPS موقت)
- 🚀 **اجرا در پس‌زمینه** — پس از نصب ترمینال آزاد می‌شود و URL همچنان کار می‌کند
- 🧰 **دستور `CCODE`** — ریست ترمینال‌ها، حذف پنل، تغییر رمز یا نمایش دوبارهٔ URL
- 📦 **مستقل** — بدون سرویس ترمینال خارجی و بدون CDN؛ xterm.js همراه پروژه است

### 📋 پیش‌نیازها

| مورد | توضیح |
| --- | --- |
| سیستم‌عامل | لینوکس (مبتنی بر apt، dnf، yum، pacman یا apk) |
| کاربر | یک **کاربر عادی** — نصب‌کننده و سرور با کاربر `root` اجرا نمی‌شوند |
| ابزار | `bash` و `curl` |
| بسته‌ها | Node.js 18+، Python 3، `tmux`، `curl`، `tar`، `sha256sum` — موارد نصب‌نشده با `sudo` خودکار نصب می‌شوند (کاربر باید به `sudo` دسترسی داشته باشد). اگر Node.js سیستم قدیمی‌تر از ۱۸ باشد، Node.js 22 با بررسی SHA-256 به‌صورت خصوصی داخل پوشهٔ برنامه نصب می‌شود |
| حالت Cloudflare | `cloudflared` — در Debian/Ubuntu و توزیع‌های RPM از مخزن امضاشدهٔ Cloudflare خودکار نصب می‌شود؛ در سایر توزیع‌ها ابتدا از صفحهٔ دانلود Cloudflare نصبش کنید |
| شبکه | اینترنت هنگام نصب |

### 🚀 نصب سریع

</div>

```bash
curl -fsSL https://raw.githubusercontent.com/configfars-TM/CCODE/refs/heads/main/terminal-tmux-manager.sh | bash
```

<div dir="rtl" align="right">

> 💡 **می‌خواهید اول اسکریپت را بررسی کنید؟**

</div>

```bash
curl -fsSL -o terminal-tmux-manager.sh https://raw.githubusercontent.com/configfars-TM/Web-Terminal/refs/heads/main/terminal-tmux-manager.sh
less terminal-tmux-manager.sh
bash terminal-tmux-manager.sh
```

<div dir="rtl" align="right">

نصب‌کننده فقط یک سؤال می‌پرسد:

</div>

```text
Launch mode: [L]ocal or [C]loudflare Tunnel? [L]:
```

<div dir="rtl" align="right">

و در پایان فقط همین دو خطی را که لازم دارید چاپ می‌کند:

</div>

```text
URL: https://example-words.trycloudflare.com
Password: Xk3mP9qRtV2wBnA7sLd4
```

<div dir="rtl" align="right">

> برای رد کردن سؤال (اسکریپت‌ها یا شل‌های غیرتعاملی) مقدار `TERMINAL_MANAGER_MODE=local` یا `TERMINAL_MANAGER_MODE=cloudflare` را تنظیم کنید؛ مثلاً:
> `curl -fsSL <script-url> | TERMINAL_MANAGER_MODE=cloudflare bash`

### 🧭 نخستین ورود (فقط یک بار)

1. **URL** چاپ‌شده توسط نصب‌کننده را باز کنید.
2. **رمز** نصب‌کننده را وارد کنید، بررسی امنیتی (CAPTCHA) را حل کنید و ادامه دهید.
3. پنل یک **کلید TOTP** نشان می‌دهد. در برنامهٔ احراز هویت (Google Authenticator، Aegis، 2FAS و…) گزینهٔ *ورود دستی* را بزنید، کلید را وارد کنید و سپس کد ۶ رقمی را در پنل بزنید.
4. وارد شدید. کوکی ورود تا **۳۰ روز** معتبر است (`HttpOnly`، `SameSite=Strict` و روی HTTPS `Secure`). دفعهٔ بعد با **رمز + یک کد ۶ رقمی تازه** وارد می‌شوید.

رمز پیش از اتصال برنامهٔ احراز هویت لازم است؛ پس کسی که URL شما را پیدا کند نمی‌تواند حساب را تصاحب کند.

### 🧰 دستور `CCODE`

نصب‌کننده دستور `CCODE` را اضافه می‌کند. آن را در هر ترمینالی روی همان دستگاه بنویسید تا منوی مدیریت باز شود:

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

| گزینه | کار |
| --- | --- |
| `1` Reset terminal | **همهٔ** ترمینال‌های tmux (و برنامه‌های درونشان) را می‌بندد و یک `main` تازه می‌سازد. تایپ `YES` لازم است. |
| `2` Remove panel | پنل را متوقف می‌کند و نصب، رمز ذخیره‌شده و دستور `CCODE` را پاک می‌کند. تایپ `REMOVE` لازم است. |
| `3` Set password again | یک رمز **جدید** می‌سازد، همهٔ مرورگرها را خارج می‌کند و باید برنامهٔ احراز هویت دوباره وصل شود (راه بازیابی هنگام گم‌شدن authenticator هم همین است). تایپ `YES` لازم است. |
| `4` Show password and panel URL again | `URL:` و `Password:` فعلی را نشان می‌دهد. |
| `0` Close | از منو خارج می‌شود. |

میان‌برها:

</div>

```bash
CCODE          # باز کردن منو
CCODE show     # نمایش URL و رمز
CCODE stop     # توقف پنل (نشست‌های tmux ادامه می‌دهند)
CCODE start    # روشن کردن دوباره، مثلاً پس از ریبوت
```

<div dir="rtl" align="right">

> **خطای `CCODE: command not found`؟** شل بازِ فعلی هنوز دستور جدید را نمی‌شناسد. یک بار `~/.local/bin/CCODE` را اجرا کنید، یا `source ~/.bashrc` بزنید، یا ترمینال جدید باز کنید.

### 🌐 حالت‌های اجرا

| | Local | Cloudflare Tunnel |
| --- | --- | --- |
| دسترسی از | فقط همین دستگاه (`http://127.0.0.1:<port>`) | هرجا، از طریق آدرس موقت `https://….trycloudflare.com` |
| مناسب برای | پورت‌فوروارد SSH، ریورس‌پراکسی خودتان | دسترسی سریع با گوشی |
| تغییر URL؟ | هنگام تغییر پورت تصادفی | **در هر بار اجرا** — آدرس فعلی را با `CCODE show` ببینید |

سرور همیشه فقط روی `127.0.0.1` گوش می‌دهد و تانل به همان پورت loopback وصل می‌شود.

### ⚙️ تنظیمات

این متغیرهای محیطی را هنگام اجرای نصب‌کننده می‌توانید تنظیم کنید (همه اختیاری):

| متغیر | پیش‌فرض | معنی |
| --- | --- | --- |
| `TERMINAL_MANAGER_MODE` | می‌پرسد | `local` یا `cloudflare` |
| `TERMINAL_MANAGER_PORT` | `0` (پورت آزاد تصادفی) | پورت ثابت loopback، از `0` تا `65535` |
| `TERMINAL_MANAGER_HOME` | `~/.local/share/terminal-tmux-manager` | پوشهٔ نصب |
| `TERMINAL_MANAGER_TMUX_SOCKET` | `terminal-manager-<uid>` | نام سوکت جداگانهٔ tmux |

### 📱 استفاده از ترمینال

- از منوی کناری نشست‌ها را **بسازید / جابه‌جا کنید / حذف کنید**. بستن مرورگر نشست‌ها را **متوقف نمی‌کند**؛ اما **حذف** یک نشست برنامه‌های آن را می‌بندد.
- زبان (English / فارسی) و پوسته (تاریک / روشن) در صفحهٔ ورود و داخل برنامه قابل تغییر است.
- ترمینال‌ها سوکت tmux مخصوص خودشان را دارند و به نشست‌های معمولی tmux شما دست نمی‌زنند.

### 🗂️ فایل‌ها کجا هستند

</div>

```text
~/.local/share/terminal-tmux-manager/
├── state/   auth.json, auth-key, sessions.json, panel-password   (private, 0700)
├── logs/    server.log, cloudflared.log
├── run/     process IDs, mode, current URL
└── runtime/ private Node.js 22 (only if your system Node.js is < 18)
```

<div dir="rtl" align="right">

اگر می‌خواهید ورودتان حفظ شود از `auth.json`، `auth-key` و `sessions.json` **با هم** نسخهٔ پشتیبان بگیرید و هرگز آن‌ها را به اشتراک نگذارید.

### 🔄 به‌روزرسانی، ریستارت، حذف

- **به‌روزرسانی / ریستارت:** دستور نصب را دوباره اجرا کنید. حساب و نشست‌های tmux حفظ می‌شوند و پنل دوباره راه می‌افتد (در حالت Cloudflare آدرس جدید می‌گیرد).
- **پس از ریبوت:** `CCODE start`.
- **حذف کامل:** `CCODE` ← `2`.

### 🔒 نکات امنیتی

آنچه داخل پروژه پیاده شده:

- سرور فقط روی **loopback** گوش می‌دهد؛ دسترسی از راه دور از طریق تانل HTTPS یا ریورس‌پراکسی انتخابی شما انجام می‌شود
- اجرا با کاربر **root** را رد می‌کند
- رمز به‌صورت هش **scrypt** همراه salt ذخیره می‌شود؛ کلید TOTP با AES-256-GCM و یک فایل کلید خصوصی رمزنگاری می‌شود
- جلوگیری از **استفادهٔ مجدد کد TOTP**، **CAPTCHA** تصویری به‌همراه یک اثبات کار کوچک، **محدودیت نرخ** برای هر کلاینت و **قفل ۱۵ دقیقه‌ای**
- کوکی ورود `HttpOnly` و `SameSite=Strict` است و فقط هش آن روی سرور ذخیره می‌شود
- WebSocket ترمینال از یک **بلیت کوتاه‌مدت و یک‌بارمصرف** مرتبط با نشست، IP کلاینت و origin استفاده می‌کند؛ origin درخواست‌ها بررسی می‌شود و تعداد اتصال‌ها محدود است
- شل‌های فرزند محیط کاهش‌یافته می‌گیرند؛ tmux روی سوکت جداگانه اجرا می‌شود

آنچه باید بدانید:

- پنل **فقط به اندازهٔ امنیت حسابی که با آن اجرا می‌شود** امن است — از یک کاربر جداگانه و کم‌دسترسی استفاده کنید، نه حساب اصلی‌تان.
- برای اینکه گزینهٔ *Show password again* کار کند، رمز تولیدشده به‌صورت متن ساده در `state/panel-password` هم نگه داشته می‌شود (دسترسی `0600`، داخل پوشهٔ خصوصی `0700`). گزینهٔ `3` آن را جایگزین و گزینهٔ `2` پاک می‌کند.
- آدرس Cloudflare quick tunnel تا زمانی که اجراست عمومی است. URL و رمز را محرمانه نگه دارید و وقتی لازم ندارید با `CCODE stop` خاموشش کنید.
- هیچ سیستمی «هک‌ناپذیر» نیست. سیستم‌عامل، Node.js و tmux را به‌روز نگه دارید و قبل از اجرا اسکریپت را بررسی کنید.

### 🩺 عیب‌یابی

| مشکل | راه‌حل |
| --- | --- |
| `CCODE: command not found` | `~/.local/bin/CCODE` را اجرا کنید، بعد `source ~/.bashrc` بزنید (یا ترمینال جدید باز کنید) |
| `Password: not available here` | حساب از قبل وجود داشته و رمزش ذخیره نشده. با `CCODE` ← `3` رمز جدید بگیرید |
| آدرس Cloudflare هنوز باز نمی‌شود | چند ثانیه صبر کنید و دوباره امتحان کنید؛ آدرس پیش از نمایش بررسی می‌شود، اما DNS در برخی شبکه‌ها دیر می‌رسد |
| authenticator را گم کرده‌اید | `CCODE` ← `3` (رمز جدید و اتصال دوبارهٔ authenticator) |
| پنل پس از ریبوت خاموش است | `CCODE start` |
| «Refusing to run as root» | نصب‌کننده را با یک کاربر عادی (دارای `sudo`) اجرا کنید |
| «Unsupported Linux package manager» | Node.js 18+، Python 3، `tmux`، `curl`، `tar` و `sha256sum` را دستی نصب کنید و دوباره اجرا کنید |

### 👥 سازندگان

ساخته‌شده توسط تیم **CONFIGFARS**.

### 📄 لایسنس

منتشرشده با [لایسنس MIT](LICENSE).

</div>
