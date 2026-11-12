# Plan de Implementare

[Inapoi la README](../README.md)

## 1. Tehnologii Alese
- **Backend (Python 3.11 + Flask):** Framework web usor, modularizare prin Blueprints, autentificare cu `Flask-Login` si utilizarea modulelor native `csv` si `uuid` pentru procesarea fisierelor si generarea seriilor de certificat.
- **Baza de date (SQLite + SQLAlchemy ORM):** Fisier local (`techpulse.db`), integritate relationala prin chei externe si constrangeri `UNIQUE`, iar toate interogarile sunt parametrizate automat impotriva atacurilor de tip SQL Injection.
- **Frontend (HTML5, Bootstrap 5.3, Jinja2, Vanilla JS):** Sabloane randate pe server, design responsive si apeluri asincrone `Fetch API` pentru rezervarea locurilor, anulare si filtrarea agendei fara reincarcarea paginii.

## 2. Organizare Git
Lucram pe GitHub prin feature branches simple:
- Nu facem commit-uri direct pe `main`.
- Cream branch-uri scurte pe functionalitati (ex: `feature/auth`, `feature/booking`, `feature/admin-csv`).
- Modificarile ajung in `main` prin Pull Request cu verificare intre noi inainte de merge.

## 3. Planificare pe Etape
- **Etapa 3 (Saptamanile 8 – 10):** Prototip functional:
  - Initializarea bazei de date SQLite, definirea modelelor SQLAlchemy si popularea cu date de test.
  - Autentificarea utilizatorilor si separarea drepturilor pe roluri (`PARTICIPANT`, `SPEAKER`, `ADMIN`).
  - Fluxurile de baza: propunere sesiuni, aprobare si programare fara coliziuni, agenda publica filtrabila si rezervarea/anularea locurilor prin `Fetch API`.
  - Validarea datelor pe server si tratarea erorilor pentru rutele implementate.
  - Primele teste automate pentru fluxul principal (ultimul loc, coliziune de sala, acces interzis).
- **Etapa 4 (Saptamanile 10 – 14):** Finalizare si predare:
  - Confirmarea prezentei, importul atomic din CSV, formularul de feedback anonimizat si generarea certificatelor digitale cu serie UUID.
  - Panoul complet de administrare, exportul rapoartelor in format `.csv` si verificarile finale de securitate (CSRF, XSS, validare server-side).
