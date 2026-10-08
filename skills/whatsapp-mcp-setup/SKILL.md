---
name: whatsapp-mcp-setup
description: Walk a non-technical user through connecting their WhatsApp to Claude Desktop with the open-source lharries/whatsapp-mcp server, on Windows or Mac, and fix the common errors. Use when the user asks to connect WhatsApp to Claude, set up WhatsApp MCP, or shares an error from the WhatsApp bridge.
---

# WhatsApp MCP Setup

Help someone with no coding experience connect their personal WhatsApp to Claude Desktop using [lharries/whatsapp-mcp](https://github.com/lharries/whatsapp-mcp). The full user-facing guide is in [`guide.md`](guide.md); share it, then coach them through it.

## How to coach
- Ask whether they're on **Windows** or **Mac** first, then only give steps for that system.
- Give one part at a time (3–4 steps), with every command in its own copy-paste block. Ask for a screenshot of the result before moving on.
- Explain where to type things: PowerShell on Windows, Terminal on Mac. On Windows, right-click or Ctrl+V pastes into PowerShell.
- Paths with spaces go in quotes: `cd "C:\whatsapp-mcp-main\whatsapp-bridge"`.
- When they send an error, explain in one plain sentence what it means, then give the fix.

## The order
1. Install Claude Desktop, Go (go.dev/dl) and uv. Check with `go version` and `uv --version`.
2. **Windows only:** install MSYS2, run `pacman -S mingw-w64-ucrt-x86_64-gcc`, add `C:\msys64\ucrt64\bin` to the user Path, check with `gcc --version`.
3. Download the repo as a ZIP. **Windows:** extract it to `C:\whatsapp-mcp-main`. **Mac:** extract it to the home folder.
4. **Replace `main.go`, `go.mod` and `go.sum` in `whatsapp-bridge/` with the files in [`bridge-fix/`](bridge-fix/)** (see below).
5. Run the bridge: `cd` into `whatsapp-bridge`, then (Windows) `go env -w CGO_ENABLED=1`, then `go run main.go`. Scan the QR code from WhatsApp → Linked Devices → Link a Device. Keep the window open.
6. Get the uv path (`where.exe uv` on Windows, `which uv` on Mac) and the `whatsapp-mcp-server` folder path.
7. In Claude Desktop go to Settings → Developer → Edit Config and paste the `mcpServers` block from `guide.md` Step 11. On Windows, double every backslash. Don't include the `//` comments from the upstream README.
8. Fully quit and reopen Claude, then test with "List my 5 most recent WhatsApp chats".

## Known errors and fixes
| Error | Cause | Fix |
|---|---|---|
| `Client outdated (405) connect failure` | The upstream repo pins an old whatsmeow version that WhatsApp now rejects. | Use the files in `bridge-fix/`. Running `go get go.mau.fi/whatsmeow@latest` alone **breaks the build**, because newer whatsmeow adds a `context.Context` first argument to `Download`, `sqlstore.New`, `GetFirstDevice`, `GetGroupInfo` and `Contacts.GetContact`. `bridge-fix/main.go` passes `context.Background()` to each one. |
| `mkdir store: Access is denied` (Windows) | The folder is in another account's profile, e.g. signed in as ADMIN but the folder is in `C:\Users\Sharon`. | Move the project to `C:\whatsapp-mcp-main`. |
| MSYS2 `Operation too slow` / `failed to commit transaction` | A pacman mirror timed out. | Re-run the same command. If it fails again, run `sed -i 's/^Server = .*nluug/#&/' /etc/pacman.d/mirrorlist*` and retry. |
| `CGO_ENABLED=0, go-sqlite3 requires cgo` | No C compiler, or CGO is off. | Finish the MSYS2 step, then run `go env -w CGO_ENABLED=1`. |
| WhatsApp missing in Claude | Bad JSON or a wrong path in the config. | Check commas, quotes and doubled backslashes. Settings → Developer shows the server error. |

More fixes are in the Troubleshooting table in `guide.md`.

## Safety (always mention)
- Claude can send messages as the user. Tell them to read each Allow prompt and avoid "Allow always" for sending.
- Messages from strangers could contain prompt injection. Be careful acting on them.
- This is unofficial. Bulk or automated messaging can get a number banned.
- To disconnect, go to WhatsApp → Linked Devices and log out.
