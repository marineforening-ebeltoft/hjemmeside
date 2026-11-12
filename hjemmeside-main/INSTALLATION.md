# Ebeltoft Marineforening v3.0 – installation

**Status: installationsklar kode, ikke en aktiv produktionsinstallation.** Ingen eksisterende Supabase-projekter er ændret.

1. Opret et **nyt, særskilt** Supabase-projekt med navnet `ebeltoft-marineforening`. Brug EU-region. Notér projektets URL og **publishable** API key fra Supabase dashboard. Kopiér aldrig service_role/secret key til browserfiler.
2. Åbn SQL Editor i det nye projekt, gennemlæs `database.sql`, og kør den. Kontroller at alle tabeller har RLS, og at policies er oprettet.
3. Under Authentication: opret/invitér en bestyrelsesbruger via dashboardet. Brugeren skal bekræfte sin e-mail. Find brugerens UUID i Authentication > Users. Kør derefter i SQL Editor:
   `insert into public.marine_staff(user_id,role,active) values ('INDSÆT_UUID','admin',true);`
   Opret kun godkendte bestyrelsesbrugere. Slå offentlig signup fra. Konfigurer Site URL og Redirect URLs til GitHub Pages-adressen.
4. Ret `config.js` med Supabase URL og publishable key. Disse må gerne være offentlige, men adgang sikres af RLS.
5. Upload `index.html`, `admin.html`, `config.js` og eksisterende `assets/img/` til **hovedmappen** i GitHub-repositoriet. Upload ikke SQL-filen som en nødvendig del af websiden.
6. Log ind på `https://marineforening-ebeltoft.github.io/hjemmeside/admin.html`. Kontroller adgang. Indsæt og publicér én testnyhed, og kontroller at den vises på forsiden. Log ud og kontroller at administration ikke kan åbnes uden login.
7. Test med en ikke-godkendt Auth-bruger: læseadgang til offentliggjorte data, men ingen oprettelse, redigering, sletning eller billed-upload. Gennemgå Supabase Security Advisor før lancering.

## Dataformater

Alle områder undtagen `settings` er JSON-lister. Eksempel for `news`:
`[{"id":"nyhed-1","title":"Nyhedens overskrift","date":"2026-11-07","text":"Godkendt nyhedstekst."}]`

Eksempel for `events`:
`[{"id":"arr-1","title":"Godkendt arrangement","date":"2027-01-01","time":"10:00","place":"Marinestuen","description":"Beskrivelse","capacity":30,"price":0}]`

Eksempel for `gallery`:
`[{"id":"foto-1","src":"https://PROJECT.supabase.co/storage/v1/object/public/marine-gallery/FIL.jpg","caption":"Billedtekst","category":"Marinestuen","year":"2024"}]`

`settings` er et JSON-objekt: `{"welcomeTitle":"...","welcomeText":"..."}`. Sæt først `published=true`, når data er godkendt.

## Vigtige begrænsninger

- Demo-tilmeldinger er slået fra, så personoplysninger ikke gemmes i browseren. Ægte tilmeldinger kræver særskilt samtykke, dataminimering, sletning og kapacitetskontrol.
- Indholdsstyringen er et **JSON-baseret første administrationspanel**, ikke en fuldt grafisk redaktør med enkeltfelter.
- Billeder kan uploades sikkert. Når `gallery` publiceres med mindst ét gyldigt billede, erstatter det den eksisterende billedsamling på den offentlige hjemmeside. Ellers vises de eksisterende ni billeder.
- Den gamle statiske historietekst og de eksisterende billeder er bevaret. Historiske billeder fra 2014–2024 er endnu ikke overført.
- En rigtig Supabase-installation og live-sikkerhedstest er ikke gennemført. Brug ikke som officielt administrationssystem, før opsætning og kontrol er fuldført.
- Supabase gratisniveau har begrænsninger og kan ændre sig. Kontrollér projektets aktuelle kvoter og advarsler i dashboardet.
