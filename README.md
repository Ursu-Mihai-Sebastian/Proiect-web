# TechPulse

## 1. Descrierea proiectului si problema abordata

TechPulse este o platforma web dedicata organizarii conferintelor si atelierelor tehnice studentesti, care centralizeaza propunerile de sesiuni, programarea pe sali, inscrierile participantilor si certificarea acestora.

In prezent, organizarea evenimentelor tehnice se bazeaza adesea pe formulare dispersate si tabele manuale greu de urmarit, ceea ce duce la suprapuneri de orar, depasirea capacitatii salilor si evidenta anevoioasa a prezentei. TechPulse rezolva aceste probleme oferind un flux unic de la propunerea unui workshop pana la emiterea certificatului digital.

## 2. Obiectivele proiectului

- Centralizarea propunerilor de ateliere si programarea acestora pe sali si intervale orare fara coliziuni.
- Oferirea unei agende publice filtrabile si a unui sistem de rezervare cu verificare automata a capacitatii si a suprapunerilor.
- Automatizarea evidentei prezentei prin import CSV atomic, colectarea de feedback anonimizat si generarea certificatelor digitale cu serie unica.

## 3. Utilizatori vizati

- **Vizitatori:** Pot consulta agenda publica a conferintei si pot filtra sesiunile dupa categorie si nivel tehnic, fara a avea cont.
- **Participanti:** Studenti autentificati care isi pot rezerva sau anula locuri la ateliere, isi vizualizeaza programul personal, trimit feedback anonim dupa confirmarea prezentei si isi descarca certificatul.
- **Speakeri / Mentori:** Pot propune ateliere noi (cu detalii tehnice si disponibilitate orara) si pot urmari stadiul evaluarii acestora.
- **Administratori:** Gestioneaza salile, categoriile si rolurile utilizatorilor, aproba si programeaza sesiunile, importa prezentele din CSV si exporta rapoartele evenimentului.

## 4. Echipa si responsabilitati

- **Vasiliu Andrei** – Arhitectura Backend & Securitate: Configurarea aplicatiei Flask si structura pe Blueprints, autentificarea si rolurile (inregistrare, login, RBAC), fluxul de propuneri (trimitere, aprobare, respingere), securitatea server-side (CSRF, XSS, validare) si gestiunea repository-ului GitHub.
- **Ursu Mihai-Sebastian** – Frontend & UI/UX: Structura paginilor in HTML5 si Jinja2, stilizarea responsive cu Bootstrap 5.3, interactiunile dinamice prin Fetch API (rezervare, anulare, filtrare agenda), formularele cu validare pe client, wireframe-urile si accesibilitatea. Coordoneaza documentatia si comunicarea cu cadrul didactic.
- **Flaviu Zubatîi** – Baza de Date & Logica de Business: Schema bazei de date SQLite cu SQLAlchemy ORM, regulile de rezervare si detectia coliziunilor, feedback-ul anonimizat si generarea certificatelor UUID, panoul de administrare (sali, categorii, roluri), importul atomic de prezente si exportul rapoartelor CSV.

## 5. Functionalitati principale

1. **Agenda publica filtrabila:** Afisarea cronologica a atelierelor programate cu locurile ramase disponibile si filtrare dinamica dupa categorie si nivel.
2. **Propuneri si programare fara coliziuni:** Trimiterea propunerilor de catre speakeri si aprobarea lor de catre admin cu verificare automata a disponibilitatii salii si a speakerului.
3. **Rezervare si anulare locuri:** Gestionarea inscrierilor din contul participantului, cu blocarea inscrierilor duble, a depasirii capacitatii si a suprapunerilor in orarul propriu.
4. **Prezenta CSV, feedback anonim si certificate:** Importul atomic al listelor de prezenta, colectarea recenziilor fara legatura cu identitatea studentului si generarea diplomelor cu cod unic UUID.
5. **Panou de administrare:** Gestionarea salilor, categoriilor si rolurilor utilizatorilor, plus exportul rapoartelor de prezenta si feedback in format CSV.

## 6. Tehnologii alese

- **Backend:** Python 3.11, Flask, Flask-Login, Flask-WTF, SQLAlchemy ORM.
- **Baza de date:** SQLite.
- **Frontend:** HTML5, Bootstrap 5.3, Jinja2, Vanilla JavaScript (Fetch API).
- **Unelte:** Git, GitHub.

## 7. Documentatia proiectului

Toate detaliile tehnice se gasesc in directorul `docs/`:

- [Cerinte Functionale si Nefunctionale](docs/requirements.md)
- [Scenarii de Utilizare (Use Cases)](docs/use-cases.md)
- [Wireframe-uri si Navigare](docs/wireframes/)
- [Arhitectura Aplicatiei](docs/architecture.md)
- [Modelul de Date si Specificatia Rutelor](docs/data-model.md)
- [Planul de Implementare](docs/implementation-plan.md)
