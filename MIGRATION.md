# MIGRATION.md - Moving OpenClaw to Another Machine

This guide is for moving the OpenClaw Gateway to another computer **without** losing the workspace setup for Sable.

## Current Setup

- Workspace repo: `https://github.com/zyintohoku/openclaw-workspace.git`
- Workspace path on current machine: `C:\Users\31333\.openclaw\workspace`
- OpenClaw state dir on current machine: `C:\Users\31333\.openclaw`
- Active assistant identity: **Sable**

## Important Rule

**Do not run both gateways at the same time** with the same Telegram bot/account.

Safe switching order:
1. Stop Gateway on old machine
2. Sync workspace changes
3. Move/copy needed OpenClaw state
4. Start Gateway on new machine

---

## What GitHub Stores

The GitHub repo stores the workspace, including things like:
- `AGENTS.md`
- `SOUL.md`
- `USER.md`
- `TOOLS.md`
- `IDENTITY.md`
- `HEARTBEAT.md`
- `MIGRATION.md`
- future memory/docs/scripts you choose to commit

This keeps Sable's personality, notes, and repo-managed files consistent across machines.

## What GitHub Does NOT Fully Store

GitHub does **not** fully preserve live OpenClaw runtime state such as:
- channel auth/session state
- Telegram-related runtime state
- session history
- local service wiring
- secrets/tokens outside the workspace repo

Those live under the OpenClaw state directory:

`C:\Users\31333\.openclaw`

---

## Recommended Migration Plan (Windows -> Windows)

### On the old machine

1. Go to the workspace repo:
   ```powershell
   cd C:\Users\31333\.openclaw\workspace
   ```

2. Save and push any latest workspace changes:
   ```powershell
   git status
   git add .
   git commit -m "Update before migration"
   git push
   ```
   If there are no changes, Git will say so.

3. Stop the Gateway:
   ```powershell
   openclaw.cmd gateway stop
   ```

4. Back up the full OpenClaw state directory:
   ```text
   C:\Users\31333\.openclaw
   ```

   Easiest method: copy that whole folder to an external drive, cloud drive, or the new machine directly.

---

### On the new machine

1. Install prerequisites if needed:
   - Node.js
   - Git
   - OpenClaw

2. Clone the workspace repo:
   ```powershell
   git clone https://github.com/zyintohoku/openclaw-workspace.git C:\Users\<YOUR_USERNAME>\.openclaw\workspace
   ```

3. Copy the old machine's OpenClaw state directory contents as needed into:
   ```text
   C:\Users\<YOUR_USERNAME>\.openclaw
   ```

   The simplest continuity-first approach is to copy the whole `.openclaw` directory from the old machine.

4. Run Doctor:
   ```powershell
   openclaw.cmd doctor
   ```

5. Start the Gateway:
   ```powershell
   openclaw.cmd gateway start
   ```

6. Verify status:
   ```powershell
   openclaw.cmd status
   openclaw.cmd gateway status
   ```

7. Test Telegram by sending Sable a message.

---

## Safest Practical Method

If the new machine is meant to take over fully, the safest practical method is:

1. Stop old Gateway
2. Copy the entire `C:\Users\31333\.openclaw` folder to the new machine's corresponding user directory
3. Make sure the workspace repo is present and up to date
4. Run `openclaw.cmd doctor`
5. Start Gateway on the new machine

This is the best shot at preserving continuity.

---

## Day-to-Day Two-Machine Workflow

If switching between two computers over time:

### Before leaving machine A
```powershell
cd C:\Users\31333\.openclaw\workspace
git status
git add .
git commit -m "Update workspace"
git push
openclaw.cmd gateway stop
```

### After arriving at machine B
```powershell
cd C:\Users\<YOUR_USERNAME>\.openclaw\workspace
git pull
openclaw.cmd doctor
openclaw.cmd gateway start
openclaw.cmd status
```

If continuity is missing, also copy over the latest `.openclaw` state backup.

---

## Notes

- The repo branch is `main`.
- The Git remote is `origin` -> `https://github.com/zyintohoku/openclaw-workspace.git`
- Telegram sender display name may still appear as `bot1` until changed in BotFather.
- Workspace identity is set to **Sable**.

---

## Quick Tomorrow Checklist

On the old machine tonight / before move:
- [ ] `git push`
- [ ] `openclaw.cmd gateway stop`
- [ ] copy `C:\Users\31333\.openclaw`

On the new machine tomorrow:
- [ ] install Node.js / Git / OpenClaw
- [ ] restore or copy `.openclaw`
- [ ] verify workspace repo exists
- [ ] `openclaw.cmd doctor`
- [ ] `openclaw.cmd gateway start`
- [ ] test Telegram
