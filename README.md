

# Spotify Playlist CRUD — Postman Mini Projekt

> Jazyk projektu: **angličtina** (v názvech requestů a proměnných).  
> Účel: vytvořit playlist, přidat skladbu, ověřit obsah a odebrat skladbu – včetně testů a automatické obnovy tokenu.

## Proč playlisty a ne alba?
Veřejné Spotify Web API **neumožňuje vytvářet ani upravovat alba**.  
Pro praktickou ukázku CRUD operací se proto používají **playlisty**, které jsou navázány na Váš účet.  
Tento projekt tak realisticky demonstruje typické REST testy pro backend.

## Rychlý start
1. Vytvořte aplikaci v [Spotify Developer Dashboard](https://developer.spotify.com/dashboard).  
   Přidejte Redirect URI, např. `http://localhost/callback`.
2. Získejte hodnoty **client_id** a **client_secret**.
3. Jednorázově vygenerujte **refresh_token** pomocí *Authorization Code Flow* (např. přes OAuth 2.0 helper v Postmanu).
4. V prostředí Postmanu `Spotify SDET Demo — Local` nastavte:
   - `client_id`, `client_secret`, `refresh_token`
   - `basic_auth_b64` = Base64 kód `client_id:client_secret`  
     (např. `ZXhhbXBsZWNsaWVudGlkOmV4YW1wbGVzZWNyZXQ=`)
5. Importujte soubory kolekce a prostředí.
6. Spusťte kolekci v tomto pořadí:
   - `00 ▶ Setup / Refresh Access Token`
   - `01 ▶ Get Current User Profile (whoami)` → uloží `user_id`
   - `02 ▶ Create Playlist` → uloží `playlist_id`
   - `03 ▶ Add Tracks to Playlist`
   - `04 ▶ Verify Playlist Items`
   - `05 ▶ Remove Tracks from Playlist`

## Testovací logika
Každý request obsahuje Postman testovací skripty (`pm.test`), které:
- ověřují stavový kód odpovědi (200, 201 apod.),
- kontrolují přítomnost klíčových polí (`id`, `snapshot_id`),
- ukládají dynamické hodnoty do proměnných prostředí pro navazující kroky.

## Nastavení proměnných
- `playlist_name` – název playlistu, který se vytvoří  
- `track_uris` – seznam Spotify URI skladeb (oddělené čárkami)  
- `track_uri_single` – URI jedné skladby pro test odstranění  

## Poznámky
- Obnova tokenu ukládá `access_token` a počítá hodnotu `token_expires_at` s bezpečnostní rezervou 60 sekund.  
- Kolekci lze spouštět v Postman Runneru nebo pomocí **Newman CLI** v rámci CI pipeline jako rychlý smoke test.  
- Každý krok obsahuje kontrolní testy pro snadnou validaci a ladění.

## Cíl projektu
Cílem je demonstrovat jednoduchou, ale dobře strukturovanou kolekci pro testování REST API s využitím reálného veřejného rozhraní.  

## Použité technologie
- **Postman** – tvorba a spouštění API testů  
- **Spotify Web API** – REST rozhraní pro uživatelská data  
- **OAuth 2.0** – autentizace a správa tokenů  
- **JavaScript (pm.*)** – testovací logika v Postman skriptech
