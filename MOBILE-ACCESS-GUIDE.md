# APEX Orchestrator — Remote Mobile Access Setup

Set up your iPhone to reach your APEX system's dashboard and CLI from anywhere, securely, with zero public internet exposure.

**Total time:** ~15 minutes.

---

## What You're Building

- **Dashboard on your phone's Safari** — full 16-panel web UI with live SSE events
- **SSH terminal on your phone** — every `stevie …` CLI command
- **One-tap iOS Shortcuts** — status, add goal, run research, convene council, push to GitHub
- **Voice control** — "Hey Siri, APEX add goal"
- **All private** — routed over a Tailscale WireGuard mesh. Nothing is exposed to the public internet.

---

## Prerequisites

- APEX Orchestrator installed and running on a PC (Windows 10/11)
- An iPhone running iOS 15+
- An identity you can sign into with both your PC browser and your iPhone (Google, Microsoft, GitHub, or Apple ID)

---

## Step 1 — Install and Authenticate Tailscale on the PC

1. Download Tailscale for Windows: https://tailscale.com/download/windows
2. Run the installer with defaults.
3. Open a terminal (PowerShell or the APEX bash shell) and run:

```
"C:\Program Files\Tailscale\tailscale.exe" login
```

4. A browser opens the Tailscale sign-in page. Pick one identity provider (Google / Microsoft / GitHub / Apple). **Remember which one you picked** — you must use the same on your phone.
5. Click **Connect** on the page.
6. Verify the PC now has a Tailscale IP:

```
"C:\Program Files\Tailscale\tailscale.exe" ip -4
```

7. Expect something like `100.x.x.x`. Write this down — this is your **PC's Tailscale IP**.

---

## Step 2 — Install Tailscale on the iPhone

1. App Store → search **Tailscale** → install.
2. Open the app → tap **Get Started** → **Sign in**.
3. Pick the **same identity provider** you chose on the PC.
4. When iOS asks "Tailscale would like to add VPN configurations" → tap **Allow** and enter your phone's passcode.
5. On the main Tailscale screen, flip the toggle to **ON** (green).
6. You should now see your PC listed in the device list (its hostname).

> If you don't see the PC, re-check the account — both devices must be signed in with the same provider and account. Tailscale shows the active account at the top of the app.

---

## Step 3 — Install OpenSSH Server on the PC (for phone → CLI)

1. Press **Windows key** → type `powershell` → right-click **Windows PowerShell** → **Run as administrator** → click **Yes** on the UAC prompt.

2. Paste this single line and press Enter:

```powershell
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0 ; Start-Service sshd ; Set-Service sshd -StartupType Automatic ; New-NetFirewallRule -Name sshd -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
```

3. Wait 1–3 minutes while it downloads the component. Expected output ends with:

```
Name    : OpenSSH Server (sshd)
Enabled : True
```

4. Verify it's running:

```powershell
Get-Service sshd
```

5. Should show `Status : Running`.

---

## Step 4 — Set a Mobile Bearer Token (for iOS Shortcuts)

Used by the APEX server's `/m/*` mobile endpoints so only your phone (holding this token) can hit them.

1. Generate a random token:

```
python -c "import secrets; print(secrets.token_urlsafe(32))"
```

2. Copy the output — this is your **mobile token**. Never share it.

3. Store it in your user environment (persists across reboots):

```
setx STEVIE_MOBILE_TOKEN "PASTE_YOUR_TOKEN_HERE"
```

You'll enter this token once in each iOS Shortcut you build.

---

## Step 5 — Start the APEX Dashboard on Tailscale

1. Open a fresh terminal (not the elevated one).
2. Start the server:

```
stevie serve --tailscale --port 8080
```

3. Expected startup banner includes:

```
Tailscale: http://100.x.x.x:8080
```

4. Leave this terminal running (or use `pythonw` / a scheduled task to run it in the background — see optional Step 9).

---

## Step 6 — Connect from iPhone Safari

Open Safari on the iPhone and go to:

```
http://<YOUR_PC_TAILSCALE_IP>:8080
```

If your dashboard has a PIN configured, enter it. You should see the full 16-panel dashboard.

**Add it to your home screen as a web app:** Share button → **Add to Home Screen**.

---

## Step 7 — Set Up SSH from iPhone

1. App Store → install **Termius** (free tier is fine).
2. Open Termius → **Hosts** tab → **+** → **New Host**.
3. Fill in:
   - **Label:** anything, e.g. `APEX PC`
   - **Address / Hostname:** `<YOUR_PC_TAILSCALE_IP>`
   - **Port:** `22`
   - **Username:** your Windows username (the one you use to log into Windows)
   - **Password:** your Windows login password
4. If Termius offers an AI Agent / Claude Project wizard → **Skip / Cancel** (not needed).
5. Tap **Save** → tap the host to connect.
6. First connection asks to trust the host key fingerprint → **Accept**.
7. You should land in a terminal with a `PS C:\Users\<you>` or `<you>@<pcname>` prompt.
8. Test:

```
stevie goals --list
```

9. If that works, every `stevie …` CLI command is now available from your phone.

---

## Step 8 — Build iOS Shortcuts (One-Tap APEX Control)

Open the built-in **Shortcuts** app on iPhone.

### 8a — "APEX Status"

Shows daemon health in one tap.

1. New Shortcut → rename to **APEX Status**
2. Add action: **Get Contents of URL**
   - URL: `http://<YOUR_PC_TAILSCALE_IP>:8080/m/status`
   - Method: `GET`
   - Expand Headers → **+** → Name: `Authorization` → Value: `Bearer <YOUR_MOBILE_TOKEN>`
3. Add action: **Get Dictionary from Input** (to parse the JSON)
4. Add action: **Show Result**
5. Tap ▶︎ to test. You should see daemon status, cycle, spend, knowledge count, pending predictions.

Add this shortcut to your home screen: share sheet → **Add to Home Screen**.

### 8b — "APEX Add Goal" (Siri-triggerable)

Voice command: *"Hey Siri, APEX add goal"*.

1. New Shortcut → rename to **APEX Add Goal**
2. Action: **Ask for Input** → Prompt: `What should APEX research?`
3. Action: **Dictionary** → add two keys:
   - `goal` = *(variable)* Provided Input
   - `priority` = `8`
4. Action: **Get Contents of URL**
   - URL: `http://<YOUR_PC_TAILSCALE_IP>:8080/m/goal`
   - Method: `POST`
   - Request Body: JSON → select the Dictionary from step 3
   - Headers: `Authorization: Bearer <YOUR_MOBILE_TOKEN>`
5. Action: **Show Result**
6. Tap the settings icon → turn on **Add to Siri** → record the phrase `APEX add goal`.

### 8c — "APEX Council"

Runs a fresh persona council session and shows the proposals.

1. New Shortcut → **APEX Council**
2. Action: **Get Contents of URL**
   - URL: `http://<YOUR_PC_TAILSCALE_IP>:8080/m/council`
   - Method: `POST`
   - Request Body: JSON with `{ "n": 5 }`
   - Headers: `Authorization: Bearer <YOUR_MOBILE_TOKEN>`
3. Action: **Show Result**

### 8d — "APEX Research" (Siri-triggerable)

Voice command: *"Hey Siri, APEX research [topic]"*.

1. New Shortcut → **APEX Research**
2. Action: **Ask for Input** → Prompt: `Research topic?`
3. Action: **Dictionary** → `topic` = Provided Input
4. Action: **Get Contents of URL**
   - URL: `http://<YOUR_PC_TAILSCALE_IP>:8080/m/research`
   - Method: `POST` → JSON body from the Dictionary
   - Headers: `Authorization: Bearer <YOUR_MOBILE_TOKEN>`
5. Action: **Show Result**

### 8e — "APEX Push GitHub"

Manually trigger a publish to the public GitHub repo.

1. New Shortcut → **APEX Push**
2. Action: **Get Contents of URL**
   - URL: `http://<YOUR_PC_TAILSCALE_IP>:8080/m/push-github`
   - Method: `POST`
   - Headers: `Authorization: Bearer <YOUR_MOBILE_TOKEN>`
3. Action: **Show Result**

---

## Mobile Endpoints Reference

All require header `Authorization: Bearer <YOUR_MOBILE_TOKEN>`.

| Method | Path | Purpose | Body |
|--------|------|---------|------|
| `GET` | `/m/status` | One-screen status summary | — |
| `GET` | `/m/digest` | Latest daily digest | — |
| `POST` | `/m/goal` | Add research goal | `{"goal": "...", "priority": 7}` |
| `POST` | `/m/council` | Convene persona council | `{"n": 5}` |
| `POST` | `/m/research` | One-shot research topic | `{"topic": "..."}` |
| `POST` | `/m/push-github` | Publish + push to GitHub | — |

---

## Step 9 (Optional) — Keep the Dashboard Running in the Background

Right now the dashboard stops when the terminal closes. To keep it running after logout/reboot:

Create `C:\Users\<you>\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\ApexServe.vbs`:

```vb
' APEX dashboard launcher — runs at logon, silent, on Tailscale
Set sh = CreateObject("WScript.Shell")
sh.Run "pythonw -m orchestrator.cli serve --tailscale --port 8080", 0, False
```

Log out and back in — the dashboard will be running on your Tailscale IP automatically.

> If you already use the watchdog/heartbeat pair for the daemon, you can add this as a second Startup entry alongside `ApexBoot.vbs`.

---

## Security Notes

- **Tailscale = private WireGuard mesh.** Only devices you sign into the same tailnet can see each other. Not public.
- **Dashboard PIN** (if set) still applies on Tailscale. The mobile endpoints (`/m/*`) use a separate bearer token, not the PIN.
- **Your mobile token is a secret.** Anyone with it can drive APEX from any Tailscale-connected device. Don't share it, don't commit it. If it leaks, generate a new one with `setx STEVIE_MOBILE_TOKEN "<new>"` and restart the server.
- **SSH uses your Windows account password by default.** Consider adding an SSH key (Termius can generate one and upload its public half to `C:\Users\<you>\.ssh\authorized_keys`) so the password isn't needed after initial setup.

---

## Troubleshooting

| Symptom | Fix |
|---------|-----|
| `tailscale ip -4` prints nothing / "NeedsLogin" | Tailscale not signed in yet. Run `tailscale login` again and complete the browser flow. |
| iPhone Tailscale shows no devices | Phone is on a different identity than the PC. Sign out and sign back in using the same provider as the PC. |
| Termius: "Connection refused" | OpenSSH service isn't running. Admin PowerShell → `Get-Service sshd`. If not Running, `Start-Service sshd`. |
| Termius: "Connection timed out" | Tailscale isn't connected on the phone (toggle off) or firewall is blocking port 22. Re-run the `New-NetFirewallRule` line from Step 3 in an Admin PowerShell. |
| Dashboard URL shows "can't connect" in Safari | `stevie serve --tailscale` isn't running on the PC (Step 5), or the phone isn't on Tailscale. |
| iOS Shortcut returns `{"error":"mobile_token_required"}` | `STEVIE_MOBILE_TOKEN` isn't set on the PC, OR the server was started before you set it. `setx` then restart the server. |
| Shortcut returns 200 but wrong body | Ensure the Authorization header is exactly `Bearer <TOKEN>` (with one space after Bearer, no quotes). |

---

## What This Setup Gives You

A working iPhone → APEX system with zero public internet exposure:

- **Safari tab** = full dashboard
- **Termius tab** = full CLI
- **Shortcuts app** = one-tap and voice-triggered APEX control
- **Siri** = voice commands ("APEX add goal", "APEX research")

Replace any `<PLACEHOLDER>` above with the actual values from your own PC and setup. Never share your Tailscale tailnet, your mobile token, or your SSH credentials.

---

*APEX Orchestrator — Copyright 2026 Steven Charles Fulton. Licensed under BSL-1.1.*
