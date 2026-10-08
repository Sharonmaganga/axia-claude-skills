# Fixed WhatsApp bridge files

These replace `main.go`, `go.mod` and `go.sum` in the `whatsapp-bridge` folder of [lharries/whatsapp-mcp](https://github.com/lharries/whatsapp-mcp) (based on upstream commit `7d6a06d`).

**Why:** the upstream bridge pins an old `go.mau.fi/whatsmeow` version that WhatsApp now rejects with `Client outdated (405)`. These files update whatsmeow to `v0.0.0-20261007111105-c386243a72ba` and pass `context.Background()` to the five calls whose signatures changed (`Download`, `sqlstore.New`, `GetFirstDevice`, `GetGroupInfo`, `Contacts.GetContact`). Nothing else is changed.

Original code © 2025 Luke Harries, MIT licence (see [LICENSE](LICENSE)).
