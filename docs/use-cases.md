# Scenarii de Utilizare

[Inapoi la README](../README.md)

---

## Use Case 1 (UC-01): Rezervarea unui Loc la un Workshop

- **Actor:** Participant (Student autentificat)
- **Preconditii:**
  1. Utilizatorul este autentificat cu rolul `PARTICIPANT`.
  2. Sesiunea dorita este in starea `SCHEDULED` si are locuri disponibile.

### Flux Principal
1. Participantul acceseaza pagina `agenda` si filtreaza dupa categoria dorita (ex: `Web`).
2. Consulta detaliile atelierului (titlu, sala, nivel, prerechizite software) si apasa butonul "Rezerva Loc".
3. Serverul verifica tranzactional cele trei conditii de integritate:
   * Numarul de inscrieri confirmate este strict mai mic decat capacitatea salii;
   * Utilizatorul nu are deja o rezervare activa la acest workshop (`UNIQUE(user_id, session_id)`);
   * Intervalul orar al sesiunii nu se suprapune cu nicio alta rezervare confirmata din agenda proprie a utilizatorului.
4. Sistemul insereaza inregistrarea in tabela `registrations` cu starea `REGISTERED` (sau reactiveaza o rezervare anulata anterior).
5. Numarul de locuri libere afisat scade cu 1, iar atelierul este adaugat in sectiunea "Agenda Mea" a studentului.

### Situatii de Exceptie (Erori)
- **3a. Epuizare locuri in timp real:** Ultimul loc a fost ocupat cu o fractiune de secunda inainte de alt utilizator.
  * *Rezultat:* Tranzactia esueaza; serverul returneaza `409 Conflict`, iar interfata afiseaza: "Locurile pentru aceasta sesiune s-au epuizat."
- **3b. Suprapunere in agenda proprie:** Studentul are deja un alt workshop rezervat in acelasi interval orar.
  * *Rezultat:* Serverul returneaza `400 Bad Request` cu mesajul: "Aveti deja o sesiune rezervata in acest interval orar."
- **3c. Inscriere duplicat:** Utilizatorul trimite din greseala doua cereri rapide.
  * *Rezultat:* Constrangerea unica din baza de date respinge a doua cerere cu mesaj de atentionare.

---

## Use Case 2 (UC-02): Anularea unei Rezervari Active

- **Actor:** Participant (Student autentificat)
- **Preconditii:**
  1. Participantul are cel putin o sesiune in starea `REGISTERED` in contul sau.
  2. Sesiunea nu a inceput inca (data curenta este anterioara orei de start).

### Flux Principal
1. Participantul navigheaza in sectiunea "Agenda Mea".
2. Identifica atelierul la care nu mai poate ajunge si apasa butonul "Anuleaza Rezervarea".
3. Sistemul solicita o confirmare rapida ("Sigur doriti sa eliberati locul?").
4. La confirmare, serverul actualizeaza starea rezervarii in `CANCELLED`.
5. Numarul de locuri disponibile pentru acel workshop creste automat cu 1.
6. Sesiunea este eliminata din lista activa a studentului, iar un mesaj de succes confirma eliberarea locului.

### Situatii de Exceptie (Erori)
- **2a. Anulare tardiva:** Participantul incearca sa anuleze rezervarea dupa ce sesiunea a inceput deja.
  * *Rezultat:* Serverul respinge cererea cu codul `400 Bad Request` si mesajul: "Rezervarile nu mai pot fi anulate dupa inceperea sesiunii."

---

## Use Case 3 (UC-03): Propunerea unei Sesiuni Tehnice

- **Actor:** Speaker / Mentor autentificat
- **Preconditii:** Utilizatorul este autentificat cu rolul `SPEAKER`.

### Flux Principal
1. Speakerul acceseaza sectiunea "Propune Sesiune".
2. Completeaza campurile obligatorii din formular:
   * Titlu (ex: "Arhitecturi REST Moderne in Python Flask");
   * Categorie tehnica (selectata din lista configurata de admin: `Web`);
   * Nivel de dificultate (`Incepator`, `Intermediar`, `Avansat`);
   * Durata estimata (ex: 120 minute);
   * Prerechizite tehnice (ex: "Laptop personal cu Python 3.11 preinstalat");
   * Disponibilitate orara preferata (ex: "Vineri dupa ora 14:00");
   * Descriere extinsa a continutului si a obiectivelor practice.
3. Apasa butonul "Trimite Propunerea".
4. Serverul valideaza datele primite; daca sunt complete, salveaza sesiunea cu starea initiala `PENDING` si atribuie cheia straina `speaker_id = current_user.id`.
5. Speakerul este redirectionat catre "Propunerile Mele", unde vizualizeaza tema cu eticheta galbena `In asteptare`.

### Situatii de Exceptie (Erori)
- **3a. Campuri obligatorii lipsa:** Titlul sau descrierea sunt lasate necompletate.
  * *Rezultat:* Validarea pe client si server blocheaza trimiterea, afisand avertismente clare langa campurile lipsa.

---

## Use Case 4 (UC-04): Completarea Formularului de Feedback Anonimizat

- **Actor:** Participant (Student cu prezenta confirmata)
- **Preconditii:**
  1. Participantul este autentificat.
  2. Are inregistrarea pentru atelierul respectiv marcata cu starea `ATTENDED`.
  3. Nu a trimis anterior feedback pentru aceasta sesiune (`feedback_submitted == FALSE`).

### Flux Principal
1. Participantul deschide sectiunea "Agenda Mea" si observa butonul "Evalueaza Sesiunea" activ langa atelierul absolvit.
2. Deschide formularul de evaluare si completeaza:
   * Nota utilitate practica (scala de la 1 la 5);
   * Nota claritate speaker (scala de la 1 la 5);
   * Sugestii / comentarii constructive (optional).
3. Apasa butonul "Trimite Feedback".
4. Serverul valideaza ca valorile sunt numere intregi intre 1 si 5.
5. Se declanseaza decuplarea de date:
   * In tabela de inscrieri se seteaza bifa `feedback_submitted = TRUE` (pentru a preveni trimiterea repetata);
   * Continutul recenziei (notele si textul) este salvat intr-o inregistrare noua in tabela `feedback`, legata doar de `session_id`, fara a stoca vreo referinta la contul studentului.
6. Interfata confirma inregistrarea evaluarii si informeaza utilizatorul ca certificatul de participare este deblocat.

### Situatii de Exceptie (Erori)
- **1a. Lipsa prezentei fizice:** Un utilizator aflat in starea `REGISTERED` (dar neconfirmat ca `ATTENDED`) incearca sa apeleze direct ruta de feedback.
  * *Rezultat:* Serverul returneaza `403 Forbidden` cu mesajul: "Formularul de feedback este disponibil exclusiv participantilor prezenti."
- **1b. Trimitere multipla:** Utilizatorul incearca sa transmita un al doilea formular pentru aceeasi sesiune.
  * *Rezultat:* Cererea este refuzata pe server pe baza indicatorului `feedback_submitted`.
