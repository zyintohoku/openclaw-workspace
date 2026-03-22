# TOMORROW.md

## Old Windows machine - before switching

```powershell
cd C:\Users\31333\.openclaw\workspace
git status
git add .
git commit -m "Update before switching machines"
git push
openclaw.cmd gateway stop
```

Then copy or back up:

```text
C:\Users\31333\.openclaw
```

---

## New Mac - bring Sable online

Install if needed:
- Git
- Node.js
- OpenClaw

Then:

```bash
# if workspace repo is not there yet
git clone https://github.com/zyintohoku/openclaw-workspace.git ~/.openclaw/workspace

# restore/copy the old .openclaw state folder contents as needed
# then run:
openclaw doctor
openclaw gateway start
openclaw status
openclaw gateway status
```

Then test Telegram.

---

## Rule

Never run both gateways at the same time with the same Telegram bot.

## Expect

Windows -> macOS should work, but some platform-specific cleanup may be needed.
If anything looks odd, run `openclaw doctor` first.
