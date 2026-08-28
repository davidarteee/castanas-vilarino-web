# Plantilla de desplegament DakerStudio (Hostinger + GitHub Actions FTP)

Copia aquesta carpeta per a cada client nou. El patró és:
**un client = una carpeta neta = un repo de GitHub = un subdomini**.
Cada `push` a `main` publica sol la web al subdomini per FTP.

---

## Opció ràpida (recomanada): script

Des de `DAKER\clients\`, executa a PowerShell:

```powershell
.\nou-client.ps1 -Slug "nom-client" -Html "C:\ruta\a\la-web-del-client.html"
```

Això crea `clients\nom-client\` amb l'`index.html`, el workflow i el `.gitignore`,
fa `git init` i el primer commit. Després segueix els passos manuals de sota (3 a 6).

## Opció manual

1. **Carpeta** — copia `_TEMPLATE` a `clients\nom-client\` i posa la web del client
   com a `index.html` (substituint el de mostra).
2. **Git local**
   ```bash
   git init -b main
   git add -A
   git commit -m "Web inicial nom-client"
   ```
3. **Repo a GitHub** — crea a https://github.com/new un repo BUIT (sense README/gitignore),
   p. ex. `nom-client-web`. Després:
   ```bash
   git remote add origin https://github.com/davidarteee/nom-client-web.git
   git push -u origin main
   ```
4. **Dades FTP a Hostinger** — hPanel → cerca "FTP" → **Cuentas FTP** del subdomini.
   Apunta el **host/IP** i l'**usuari**, i posa una **contrasenya** ("Cambiar contraseña").
5. **Secrets a GitHub** — repo → Settings → Secrets and variables → Actions → New secret:
   - `FTP_SERVER` = la IP/host (sense `ftp://`)
   - `FTP_USERNAME` = l'usuari FTP
   - `FTP_PASSWORD` = la contrasenya del pas 4
6. **Prova** — fes un canvi petit a `index.html`, `git commit`, `git push`, i mira que
   la pestanya Actions surti verda i el subdomini mostri el canvi.

---

## Notes clau (ja resoltes al workflow)

- `server-dir: ./` — a Hostinger l'usuari FTP del subdomini ja aterra DINS de `public_html`.
  (Si algun dia un compte aterra un nivell més amunt, hauries de posar `./public_html/`.)
- `protocol: ftps`, `port: 21`, `timeout: 60000`. Si veus **"Timeout (control socket)"**,
  torna a executar l'Action (**Re-run all jobs**); si passa sovint, canvia `ftps` → `ftp`.
- Els secrets són per repo: cada client té els seus (el seu propi usuari/contrasenya FTP).
