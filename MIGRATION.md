# MIGRATION.md - Moving OpenClaw to Another Machine

This guide is for moving the OpenClaw Gateway to another computer **without** losing the workspace setup for Sable.

## Current Setup

- Workspace repo: `https://github.com/zyintohoku/openclaw-workspace.git`
- Current machine OS: **Windows**
- Target machine OS: **macOS**
- Workspace path on current machine: `C:\Users\31333\.openclaw\workspace`
- OpenClaw state dir on current machine: `C:\Users\31333\.openclaw`
- Typical OpenClaw path on macOS: `~/.openclaw`
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
- `TOMORROW.md`
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

- Windows: `C:\Users\31333\.openclaw`
- macOS: `~/.openclaw`

---

## Windows -> macOS Migration Notes

This move should work, but it is **not** a perfect byte-for-byte transplant.

Things that should transfer cleanly:
- workspace repo files
- notes and docs
- identity/personality files
- plain-text memory files

Things that may need adjustment on macOS:
- service installation / background service wiring
- machine-specific paths
- local executable paths
- any Windows-specific runtime assumptions

Because of that, the right approach is:
1. use GitHub to restore the workspace cleanly
2. copy `.openclaw` carefully for continuity
3. run `openclaw doctor`
4. test the gateway and Telegram

---

## Recommended Migration Plan (Windows -> macOS)

### On the old Windows machine

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

   Easiest method: copy that whole folder to an external drive, cloud drive, or directly to the Mac.

---

### On the new macOS machine

1. Install prerequisites if needed:
   - Git
   - Node.js
   - OpenClaw

2. Clone the workspace repo:
   ```bash
   git clone https://github.com/zyintohoku/openclaw-workspace.git ~/.openclaw/workspace
   ```

3. Copy the old machine's OpenClaw state into:
   ```text
   ~/.openclaw
   ```

   The continuity-first approach is to copy the old `.openclaw` directory, but expect some platform-specific cleanup afterward.

4. Run Doctor:
   ```bash
   openclaw doctor
   ```

5. Start the Gateway:
   ```bash
   openclaw gateway start
   ```

6. Verify status:
   ```bash
   openclaw status
   openclaw gateway status
   ```

7. Test Telegram by sending Sable a message.

---

## Safest Practical Method

If the Mac is meant to take over fully, the safest practical method is:

1. Stop old Gateway on Windows
2. Push the latest workspace repo changes
3. Copy the entire `C:\Users\31333\.openclaw` folder somewhere accessible
4. Restore what you need into `~/.openclaw` on the Mac
5. Run `openclaw doctor`
6. Start Gateway on the Mac
7. Test Telegram

This is the best shot at preserving continuity while still allowing macOS to fix platform-specific details.

---

## Day-to-Day Two-Machine Workflow

If switching between Windows and macOS over time:

### Before leaving the Windows machine
```powershell
cd C:\Users\31333\.openclaw\workspace
git status
git add .
git commit -m "Update workspace"
git push
openclaw.cmd gateway stop
```

### After arriving at the Mac
```bash
cd ~/.openclaw/workspace
git pull
openclaw doctor
openclaw gateway start
openclaw status
```

If continuity is missing, also restore the latest `.openclaw` state backup.

---

## Notes

- The repo branch is `main`.
- The Git remote is `origin` -> `https://github.com/zyintohoku/openclaw-workspace.git`
- Telegram sender display name may still appear as `bot1` until changed in BotFather.
- Workspace identity is set to **Sable**.
- On macOS, service behavior differs from Windows Scheduled Task behavior, so `openclaw doctor` matters even more.

---

## Quick Tomorrow Checklist

On the old Windows machine tonight / before move:
- [ ] `git push`
- [ ] `openclaw.cmd gateway stop`
- [ ] copy `C:\Users\31333\.openclaw`

On the new Mac tomorrow:
- [ ] install Git / Node.js / OpenClaw
- [ ] clone the workspace repo to `~/.openclaw/workspace`
- [ ] restore or copy needed `.openclaw` state into `~/.openclaw`
- [ ] `openclaw doctor`
- [ ] `openclaw gateway start`
- [ ] test Telegram
