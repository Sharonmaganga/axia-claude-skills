# Connect WhatsApp to Claude Desktop (beginner guide)

Based on the instructions in github.com/lharries/whatsapp-mcp. No coding needed: you'll install a few free programs, copy-paste some commands, and edit one settings file.

**How it works in one sentence:** a small program (the "bridge") logs into your WhatsApp like WhatsApp Web does and saves your messages on your computer, and Claude Desktop talks to that bridge.

> Where it says **Mac** or **Windows**, follow only the one for your computer.

---

## Part 1: Install the tools (one time only)

### Step 1. Install Claude Desktop
Download it from **claude.ai/download**, install it, and sign in.

### Step 2. Install Go (runs the WhatsApp bridge)
1. Go to **go.dev/dl**.
2. **Mac:** download the `.pkg` file (pick "Apple" for M1/M2/M3/M4 Macs, "x86-64" for older Intel Macs). **Windows:** download the `.msi` file.
3. Double-click it and click Next/Continue until it's done.

### Step 3. Install uv (runs the Claude side of the connection)
Open a terminal:
- **Mac:** press `Cmd + Space`, type **Terminal**, press Enter.
- **Windows:** click Start, type **PowerShell**, press Enter.

Paste this and press Enter:

- **Mac:**
  ```
  curl -LsSf https://astral.sh/uv/install.sh | sh
  ```
- **Windows:**
  ```
  powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
  ```

Then **close the terminal window and open a new one** so it notices the new programs.

uv downloads Python for you when needed. If you later see a Python error, install Python from **python.org/downloads** (on Windows, tick **"Add Python to PATH"** on the first screen).

### Step 4 (Windows only). Install a C compiler
Windows needs one extra tool for the bridge's database.
1. Download and install **MSYS2** from **msys2.org** (keep the default folder `C:\msys64`).
2. When it finishes, an MSYS2 window opens. Paste this, press Enter, and type `Y` when asked:
   ```
   pacman -S mingw-w64-ucrt-x86_64-gcc
   ```
3. Add it to your PATH so Windows can find it:
   - Click Start, type **environment variables**, open **"Edit the system environment variables"**.
   - Click **Environment Variables…** → under *User variables* select **Path** → **Edit** → **New**.
   - Paste `C:\msys64\ucrt64\bin` → OK, OK, OK.
4. Close and reopen PowerShell.

### Step 5. Check everything installed
In the terminal, type each line and press Enter. Each should print a version number, not an error:
```
go version
uv --version
```
Windows only, also: `gcc --version`

---

## Part 2: Download the project

### Step 6. Download the code
Easiest way (no git needed):
1. Open **github.com/lharries/whatsapp-mcp** in your browser.
2. Click the green **Code** button → **Download ZIP**.
3. Unzip it and move the folder somewhere simple:
   - **Mac:** your home folder (e.g. `/Users/yourname/whatsapp-mcp-main`)
   - **Windows:** directly on the C: drive, i.e. `C:\whatsapp-mcp-main` (this avoids "Access is denied" errors)

The folder will be called **whatsapp-mcp-main**. Avoid putting it inside Downloads or a folder with spaces in the name.

### Step 6b. Swap in the fixed bridge files
The original project uses an old WhatsApp library that WhatsApp now rejects ("Client outdated (405)"). Download `main.go`, `go.mod` and `go.sum` from this skill's [`bridge-fix/`](bridge-fix/) folder and copy them into `whatsapp-mcp-main\whatsapp-bridge`, choosing **Replace** when asked.

---

## Part 3: Connect your WhatsApp

### Step 7. Start the bridge
In the terminal, go into the bridge folder (change the path to wherever you put it):

- **Mac:**
  ```
  cd ~/whatsapp-mcp-main/whatsapp-bridge
  go run main.go
  ```
- **Windows:**
  ```
  cd "C:\whatsapp-mcp-main\whatsapp-bridge"
  go env -w CGO_ENABLED=1
  go run main.go
  ```

The first run takes a minute or two while it downloads what it needs.

### Step 8. Scan the QR code
1. A QR code made of blocks appears in the terminal.
2. On your phone, open WhatsApp → **Settings** (iPhone) or **⋮ menu** (Android) → **Linked Devices** → **Link a Device**.
3. Scan the QR code on your screen.
4. Wait while your chats sync. This can take several minutes if you have lots of chats.

**Important:** leave this terminal window open. Claude can only reach WhatsApp while the bridge is running. Next time, just repeat Step 7 (no QR code needed unless it's been about 20 days).

---

## Part 4: Tell Claude Desktop about WhatsApp

### Step 9. Find two file locations
Open a **new** terminal window (leave the bridge one running).

**A. Where uv is installed:**
- **Mac:** type `which uv` → e.g. `/Users/yourname/.local/bin/uv`
- **Windows:** type `where.exe uv` → e.g. `C:\Users\YourName\.local\bin\uv.exe`

**B. The server folder** is your project folder plus `whatsapp-mcp-server`:
- **Mac:** e.g. `/Users/yourname/whatsapp-mcp-main/whatsapp-mcp-server`
- **Windows:** e.g. `C:\whatsapp-mcp-main\whatsapp-mcp-server`

Write both down.

### Step 10. Open Claude's settings file
1. Open Claude Desktop.
2. Go to **Settings** → **Developer** → **Edit Config**.
3. This opens the file `claude_desktop_config.json` (open it with TextEdit on Mac or Notepad on Windows).
   - Mac location: `~/Library/Application Support/Claude/claude_desktop_config.json`
   - Windows location: `%APPDATA%\Claude\claude_desktop_config.json`

### Step 11. Paste in the WhatsApp settings
Delete whatever is in the file and paste the version for your computer, swapping in **your** two paths from Step 9.

**Mac:**
```json
{
  "mcpServers": {
    "whatsapp": {
      "command": "/Users/yourname/.local/bin/uv",
      "args": [
        "--directory",
        "/Users/yourname/whatsapp-mcp-main/whatsapp-mcp-server",
        "run",
        "main.py"
      ]
    }
  }
}
```

**Windows** (note every `\` is typed twice, `\\`, which this file requires):
```json
{
  "mcpServers": {
    "whatsapp": {
      "command": "C:\\Users\\YourName\\.local\\bin\\uv.exe",
      "args": [
        "--directory",
        "C:\\whatsapp-mcp-main\\whatsapp-mcp-server",
        "run",
        "main.py"
      ]
    }
  }
}
```

Tips:
- If the file already had other servers in it, keep them and add the `"whatsapp": {...}` block inside `"mcpServers"`, with a comma between entries.
- Don't copy the `// comments` from the GitHub page. Comments break this file.
- Use straight quotes `"` not curly ones `“ ”` (TextEdit on Mac: Format → Make Plain Text, and turn off smart quotes in Edit → Substitutions).

Save the file.

### Step 12. Restart Claude Desktop
Fully quit Claude (not just close the window):
- **Mac:** `Cmd + Q`, or right-click the dock icon → Quit.
- **Windows:** right-click the Claude icon near the clock (system tray) → Quit.

Open it again.

### Step 13. Test it
1. In a new chat, click the **tools/connectors icon** (near the message box) and check **whatsapp** is listed.
2. Try: *"List my 5 most recent WhatsApp chats."*
3. Claude will ask permission to use the tool the first time. Click **Allow**.
4. Then try: *"Search my WhatsApp contacts for [a name]"* or *"Send a WhatsApp message to [your own number] saying hello."*

🎉 Done!

---

## Troubleshooting

| Problem | Fix |
|---|---|
| `go`, `uv` or `gcc` "not recognized / command not found" | Close and reopen the terminal. If still failing, reinstall that tool (Windows: re-check the PATH step). |
| Windows: `CGO_ENABLED=0, go-sqlite3 requires cgo` | Do Step 4 fully, then run `go env -w CGO_ENABLED=1` and `go run main.go` again. |
| Windows: `gcc not found` | `C:\msys64\ucrt64\bin` isn't on your PATH, or the `pacman` install was skipped (Step 4). |
| Windows: MSYS2 `pacman` says "Operation too slow" / "failed retrieving file" | A download server timed out. Run the same `pacman -S` command again. If it keeps failing, run `sed -i 's/^Server = .*nluug/#&/' /etc/pacman.d/mirrorlist*` in MSYS2 to skip that server, then retry. |
| QR code looks scrambled | Make the terminal window bigger / zoom out (`Cmd -` or `Ctrl -`) and run Step 7 again. |
| Bridge says "Client outdated (405)" | The project's WhatsApp library is too old. Updating it alone breaks the build (newer versions need small code changes), so replace `main.go`, `go.mod` and `go.sum` in `whatsapp-bridge` with the fixed copies in this skill's [`bridge-fix/`](bridge-fix/) folder, then run `go run main.go` again. |
| Windows: `mkdir store: Access is denied` | The project folder is somewhere your Windows account can't write (often another user's folder). Move it to `C:\whatsapp-mcp-main` and run Step 7 from there. |
| "Device limit reached" | On your phone, WhatsApp → Linked Devices → remove an old device. |
| No messages showing | Wait a few minutes after scanning; history syncs gradually. |
| WhatsApp not showing in Claude | Check the config file for typos (missing comma, wrong path, single `\` on Windows). Fully quit and reopen Claude. Settings → Developer shows an error message if the server failed to start. |
| Claude says it can't reach WhatsApp | The bridge terminal (Step 7) was closed. Start it again. |
| Messages out of sync | Stop the bridge (`Ctrl + C`), delete `messages.db` and `whatsapp.db` inside `whatsapp-bridge/store/`, run Step 7 and scan again. |

---

## Privacy & safety (please read)

- This logs into **your personal WhatsApp**. Your messages are saved on your computer and are only sent to Claude when Claude uses a WhatsApp tool in your chat.
- Claude can **send messages as you**. Read each permission prompt before clicking Allow, and avoid "Allow always" for sending.
- The repo's author warns about *prompt injection*: a message someone sends you could contain hidden instructions trying to trick Claude into leaking your data. Be careful asking Claude to act on messages from people you don't know, and don't combine this with other tools that can send data elsewhere without reviewing.
- This is an unofficial tool, not made by WhatsApp or Anthropic. Automated or bulk messaging can get a number banned, so don't use it for spam-style broadcasts.
- To disconnect at any time: WhatsApp on your phone → Linked Devices → log out the device.
