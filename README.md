# TasksApp – Webbapp (React + Vite)

Responsiv webbapp för uppgiftshantering. Appen pratar med TasksApps backend (ASP.NET Core WebAPI), som ligger i ett eget repo.

- **Backend-repo:** `<länk till backend-repot>`
- **Mobilapp-repo:** `<länk till mobilappens repo>`

## 📋 Funktioner

- Inloggning, registrering och utloggning
- Lista, skapa, uppdatera och ta bort uppgifter
- Filuppladdning till uppgifter (max 10 MB, validering av filtyp och storlek)
- Responsiv design (mobile-first, testad i 375 px, 768 px och 1920 px bredd)
- Felhantering med toast-notifieringar
- Inställningar för konto, lösenord, sessioner och tvåfaktorsautentisering

## ✅ Förutsättningar

- **Node.js 20+** (LTS rekommenderas) och **npm**
- **Backend igång** på `http://localhost:5277` – se backend-repots README

## 🚀 Installation

### 1. Klona repot och installera beroenden

```bash
git clone <länk till det här repot>
cd <repo-mappen>

npm install --legacy-peer-deps
```

> **Varför `--legacy-peer-deps`?** Projektet använder ESLint 9, men `eslint-plugin-react-hooks` 4.x anger att den bara stöder ESLint upp till version 8. En vanlig `npm install` avbryts därför med felet `ERESOLVE unable to resolve dependency tree`. Flaggan gör att npm ignorerar konflikten. Den påverkar bara lint-verktyget, inte själva appen.
>
> Om du redan har kört `npm install` och fått felet: kör bara kommandot ovan istället.

### 2. Miljövariabler (`.env`)

Filen `.env` följer med i repot och innehåller:

```env
VITE_APP_NAME = "Tasks App"
VITE_APP_DESCRIPTION = "A simple task management application"
VITE_API_URL = "http://localhost:5277/api"
```

Saknas filen, skapa den i projektets rotmapp med innehållet ovan. `VITE_API_URL` måste peka på backend. Filen innehåller inga hemligheter, bara publika inställningar.

### 3. Starta appen

```bash
npm run dev
```

Öppna **http://localhost:3000**.

> Appen måste köras på port **3000** – backend tillåter bara anrop (CORS) från `http://localhost:3000`.

### 4. Logga in

Använd demokontot som backend skapar automatiskt:

- **E-post:** `demo@example.com`
- **Lösenord:** `DemoPassword123!`

## 📜 Kommandon

| Kommando | Beskrivning |
|---|---|
| `npm install --legacy-peer-deps` | Installera beroenden |
| `npm run dev` | Starta utvecklingsservern på port 3000 |
| `npm run build` | Bygg för produktion (resultat i `dist/`) |
| `npm run preview` | Förhandsgranska produktionsbygget |
| `npm run lint` | Kör ESLint |

## 🛠️ Teknik

| Teknik | Användning |
|---|---|
| **React 19** | UI-komponenter |
| **Vite** | Utvecklingsserver och byggverktyg |
| **React Router v6** | Routing mellan sidor |
| **Zustand** | Global state (inloggningsstatus, `src/store/useAuthStore.js`) |
| **Tailwind CSS v4** | Styling och responsiv design |
| **Sonner** | Toast-notifieringar |
| **Lucide React** | Ikoner |

### Kommunikation med backend

All kommunikation går via `src/lib/api.js`, som:

- skickar med inloggnings-cookies (`HttpOnly`) automatiskt (`credentials: "include"`)
- hämtar en CSRF-token från `/api/auth/csrf-token` och skickar den som `X-CSRF-TOKEN` på alla anrop som ändrar data
- förnyar access-token via `/api/auth/refresh` när ett anrop får svaret 401
- visar backendens felmeddelanden som toast-notifieringar

## 📁 Projektstruktur

```
src/
├── app/            # Sidinnehåll och formulär (auth, uppgifter, inställningar, info)
├── components/     # Återanvändbara komponenter (header, footer, UI, auth, inställningar)
├── hooks/          # Egna hooks, t.ex. useTasks.js för uppgifter
├── layouts/        # RootLayout.jsx – gemensam layout
├── lib/            # api.js – API-klient
├── pages/          # Sidkomponenter som kopplas till routes
├── store/          # Zustand-stores
├── App.jsx         # Routes
├── main.jsx        # Startpunkt
└── index.css       # Globala stilar (Tailwind)
```

## 🧯 Felsökning

| Problem | Lösning |
|---|---|
| `ERESOLVE unable to resolve dependency tree` vid installation | Kör `npm install --legacy-peer-deps` |
| "Failed to fetch" eller CORS-fel i konsolen | Kontrollera att backend körs på port 5277 och att appen körs på port 3000 |
| Port 3000 är upptagen | Stäng programmet som använder porten. Byter du port i `vite.config.js` måste backendens `AppUrls:FrontendBaseUrl` ändras till samma adress |
| Appen kraschar direkt eller visar tom sida | Kontrollera att `.env` finns och att alla `VITE_`-variabler är satta. Starta om `npm run dev` efter ändringar i `.env` |

## 📝 Bakgrund

Appen skrevs först i Next.js och konverterades sedan till React + Vite enligt kursens krav. Se `CONVERSION_SUMMARY.md` för detaljer om konverteringen.
