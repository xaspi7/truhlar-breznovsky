# Decap CMS – testovací kopie Březnovského

Tato práce probíhá **pouze ve větvi `cms-sandbox`** repozitáře `xaspi7/truhlar-breznovsky`. Výchozí větev `main` ani nasazení živého webu se nemění.

## Co je připravené

- `public/admin/index.html` – administrační aplikace Decap CMS (v3.17.3).
- `public/admin/config.yml` – české formuláře pro úvod, O mně, služby, realizace a kontakt.
- `src/content/*.json` – texty a fotografie, které nyní používá web.

Úpravy v administraci se ukládají do souborů GitHubu a **mají směřovat pouze do větve `cms-sandbox`**.

## Jak zprovoznit přihlášení přes Google

1. Nasadit tuto větev na **samostatnou testovací adresu**, která nepoužívá doménu `breznovskytruhlar.cz`. Stávající GitHub Actions nasazuje pouze `main`/`master`.
2. Na https://decapbridge.com/ založit účet a přidat testovací site, repo `xaspi7/truhlar-breznovsky`, admin URL testovacího nasazení s `/admin/index.html`, typ autentizace **PKCE** pro Google login.
3. V GitHubu vytvořit **fine-grained Personal Access Token** s přístupem **jen ke konkrétnímu repozitáři** a oprávněním `Contents: Read and write`. Zadávat token **pouze přímo ve službě DecapBridge**, nikdy ne do webu, `config.yml` ani konverzace. Pozor: token na úrovni repozitáře opravňuje k zápisu i do `main`, proto při testování nepřidávat cizí účty a ověřit konfiguraci!
4. V `public/admin/config.yml` nahradit blok `backend:` oficiálním blokem vygenerovaným DecapBridge. V něm nastavit a ověřit `branch: cms-sandbox`; **nikdy `main`**.
5. Přihlásit se přes Google. Před předáním klientovi otestovat změnu pouze testovacího obsahu a potvrdit, že produkční `main` i živý web zůstaly beze změny.

Bez kroku 4 administrace na veřejné URL **zatím nemá funkční přihlášení**.

## Lokální test bez DecapBridge

V repozitáři spusť v jednom terminálu `npm install && npm run dev`, ve druhém `npx decap-server` a otevři `http://localhost:4321/admin/`. `local_backend: true` umožňuje lokální práci s obsahem (viz dokumentace Decap CMS).

## Důležité poznámky

- Zatím jde o správu obsahových **sekcí jedné existující stránky**, nikoli libovolné nové podstránky.
- CMS umožňuje přidat/odebrat položky služeb a fotografie galerie; obrázky se ukládají do `public/images`.
- Změny se na webu projeví až po dalším sestavení příslušné větve; automatické testovací nasazení dosud není zapnuté.
- Před samostatným nasazením je třeba ověřit kompilaci, upload médií a přihlášení.
