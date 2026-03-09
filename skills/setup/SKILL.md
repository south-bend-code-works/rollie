---
name: setup
description: Set up your Rollie API key so you can query workforce data. Run this first if Rollie tools aren't working.
user-invocable: true
---

# Rollie Setup

Help the user connect to Rollie by setting up their API key.

## Steps

1. **Check if already configured**: Try calling `whoami`. If it works, tell the user they're already connected and show their org info. Done!

2. **If not connected**, walk them through it:

   Tell the user:

   > To connect Rollie, you need an API key from your Rollie account.
   >
   > **Get your key:** [Open your Rollie profile](https://app.rolliejobs.com) and go to **My Profile** > **AI Integrations** > **New Connection**.
   >
   > Copy the key — you'll only see it once.

3. **Once they have the key**, help them set it:

   For **Claude Code** users, add it to their shell profile:

   ```bash
   echo 'export ROLLIE_API_KEY=rlk_...' >> ~/.zshrc
   source ~/.zshrc
   ```

   Or for bash users: `~/.bashrc` instead.

   For **Cowork** users, explain that the environment variable needs to be set before launching the app. They can add `export ROLLIE_API_KEY=rlk_...` to their shell profile and restart Cowork.

   Alternatively, they can add the MCP server directly to their project or global config:

   ```json
   {
     "mcpServers": {
       "rollie": {
         "url": "https://rollie-db-mcp-4qqvujfleq-uc.a.run.app/sse",
         "headers": {
           "Authorization": "Bearer rlk_PASTE_YOUR_KEY_HERE"
         }
       }
     }
   }
   ```

4. **Verify**: After setup, call `whoami` to confirm the connection works. Show the user their organizations and permission level.

## Tone

Be friendly and helpful — Rollie is a cheerful roly-poly who loves helping people explore their workforce data. Keep instructions simple and direct.
