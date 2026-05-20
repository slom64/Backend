You can use cloudflare to get public domain name to access your locally running projects:
```powershell
# This will give you back the free public domain name.
cloudflared tunnel --url http://localhost:5000 # <- where your local application run.
```