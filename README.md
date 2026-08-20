# Netchex MCP Config Builder

A static page that lets teammates check off which MCP servers they want (Salesforce, Gong, Highspot, etc.) and get a ready-to-paste `claude_desktop_config.json` snippet — with placeholder credentials they fill in themselves.

## Deploy to GitHub Pages (one-time setup)

1. Create a new repo (can be private if your GitHub plan supports Pages on private repos, otherwise public is fine — there are no real secrets in this repo, only placeholders).
2. Push these two files (`index.html`, `mcps.json`) to the repo root.
3. In the repo: **Settings → Pages → Source → Deploy from branch → `main` / root**.
4. Your page will be live at `https://<your-username>.github.io/<repo-name>/` within a minute or two.
5. Share that link with teammates instead of the PDF.

## Adding a new MCP later (no code changes needed)

Open `mcps.json` and add a new object to the array, following the existing pattern:

```json
{
  "id": "short-id",
  "name": "Display Name",
  "description": "One line describing what it lets Claude do.",
  "credentialLabel": "ENV_VAR_NAME",
  "credentialHelp": "Where/who to get the credential from.",
  "config": {
    "command": "npx",
    "args": [
      "-y",
      "mcp-remote@latest",
      "https://your-server-url/mcp",
      "--header",
      "x-api-key: ${ENV_VAR_NAME}"
    ],
    "env": {
      "ENV_VAR_NAME": "PASTE_YOUR_KEY_HERE"
    }
  }
}
```

Commit and push — the live page updates automatically (usually within a minute).

## Notes

- **No real credentials ever go in this repo.** Only `PASTE_YOUR_..._HERE` placeholders. Teammates fill in their own personal token after pasting into their local config.
- **`mcps.json` is fetched at page-load**, so GitHub Pages (served over `https://`) works fine. Opening `index.html` directly from your local filesystem (`file://`) will fail to load it due to browser CORS rules — always test via a local server (`python3 -m http.server`) or the live Pages URL.
- Still update the Highspot entry's server URL in `mcps.json` — it currently has a `REPLACE_WITH_HIGHSPOT_MCP_SERVER_URL` placeholder since I didn't have the exact endpoint on hand.
- The Windows Node-path warning is hardcoded copy in `index.html` (the `#win-warning` block) since it's a one-off fix note, not per-MCP data — edit that text directly if the guidance changes.
