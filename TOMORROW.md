# TOMORROW.md

## Old machine - before switching

```powershell
cd C:\Users\31333\.openclaw\workspace
git status
git add .
git commit -m "Update before switching machines"
git push
openclaw.cmd gateway stop
```

Then copy:

```text
C:\Users\31333\.openclaw
```

---

## New machine - bring Sable online

Install if needed:
- Node.js
- Git
- OpenClaw

Then:

```powershell
# if workspace repo is not there yet
git clone https://github.com/zyintohoku/openclaw-workspace.git C:\Users\<YOUR_USERNAME>\.openclaw\workspace

# restore/copy the old .openclaw state folder
# then run:
openclaw.cmd doctor
openclaw.cmd gateway start
openclaw.cmd status
openclaw.cmd gateway status
```

Then test Telegram.

---

## Rule

Never run both gateways at the same time with the same Telegram bot.
