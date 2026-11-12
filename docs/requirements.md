# Cerinte Tehnice

[Inapoi la README](../README.md)

## 1. Cerinte Functionale

### Obligatorii

- **Inregistrare si autentificare:** Vizitatorii isi pot crea cont (rol implicit *Participant*; adresele de email duplicate sunt respinse).
- **Controlul accesului pe baza de rol:** Sistemul separa utilizatorii in *Participant*, *Speaker* si *Admin*. Incercarea unui cont obisnuit de a accesa pagini administrative este blocata de server.
- **Gestiune sali si categorii:** Administratorul poate administra salile si categoriile tehnice (nume unice).
- **Propunere si monitorizare sesiuni:** Utilizatorii cu rol de Speaker pot trimite propuneri de ateliere (titlu, descriere, nivel, durata, categorie, prerechizite si disponibilitatea orara preferata) cu starea initiala In asteptare. O propunere deja aprobata nu mai poate fi editata.
- **Evaluare si programare fara coliziuni:** Administratorul aproba propunerile tinand cont de disponibilitatea mentionata de speaker si alocandu-le o sala si un interval orar exact (sau le respinge cu motiv obligatoriu). Sistemul blocheaza automat programarea daca sala sau speakerul sunt deja ocupati in acel interval la un alt atelier.
- **Agenda publica filtrabila:** Orice vizitator poate consulta atelierele programate, ordonate cronologic cu afisarea locurilor ramase, si le poate filtra dinamic dupa categorie si nivel tehnic.
- **Rezervare locuri cu prevenirea suprapunerilor:** Un participant autentificat isi poate rezerva un loc daca mai exista locuri libere. Serverul blocheaza inscrierile duble la acelasi atelier, respinge cererile cand capacitatea e atinsa si nu permite inscrierea la doua sesiuni care se suprapun ca interval orar.
- **Anularea rezervarii:** Participantul isi poate vedea propriile rezervari si poate anula participarea inainte de inceperea sesiunii, eliberand imediat locul pentru alti colegi.
- **Confirmare prezenta si feedback anonimizat:** Administratorul marcheaza prezenta fizica a participantilor. Doar participantii prezenti pot completa formularul de feedback (note 1–5 si comentarii, o singura data per sesiune). Recenziile sunt salvate complet anonimizat, fara legatura cu contul studentului.
- **Emitere certificat digital:** Dupa confirmarea prezentei si trimiterea feedback-ului, participantul isi poate descarca diploma de participare (formatata pentru tiparire PDF din browser), prevazuta cu o serie unica de identificare.
- **Panou de administrare si operatiuni CSV:** Administratorul are o vedere centralizata a evenimentului, poate exporta rapoartele de prezenta si feedback in format `.csv` deschis si poate importa lista de prezente dintr-un fisier `.csv` cu validare atomica (daca exista o singura linie invalida, tot importul este anulat si se afiseaza erorile pe linii).
- **Schimbarea rolurilor:** Administratorul poate promova un participant la rolul de *Speaker* sau *Admin*.
  
### Optionale

- **Verificare publica a certificatului:** O pagina publica unde oricine poate introduce seria unui certificat pentru a-i confirma autenticitatea.

---

## 2. Cerinte Nefunctionale

### Obligatorii

- **Protectia parolelor si a configurarii:** Parolele utilizatorilor sunt salvate exclusiv criptat, niciodata in clar. Cheia secreta a aplicatiei (folosita pentru securizarea sesiunilor) este pastrata separat intr-un fisier local .env exclus din Git, nu scrisa direct in codul sursa.
- **Controlul accesului:**  Serverul verifica la fiecare cerere daca utilizatorul are rolul si drepturile necesare; accesul neautorizat este respins cu 403.
- **Validare pe server:** Toate regulile (campuri obligatorii, capacitate, suprapuneri orare) sunt verificate obligatoriu in backend; validarea din browser este doar pentru comoditate.
- **Integritatea datelor::** Rezervarile si programarile se fac in tranzactii, astfel incat doua cereri simultane sa nu poata depasi capacitatea sau crea suprapuneri.
- **Tratarea erorilor:** Utilizatorul primeste mesaje clare, fara detalii tehnice, iar fisierele CSV invalide sunt respinse fara a afecta aplicatia.
- **Confidentialitatea datelor:** Se colecteaza doar datele necesare, adresele de email nu sunt publice, iar feedback-ul ramane anonim.
- **Design responsive:** Interfata functioneaza corect pe telefon, tableta si desktop, fara scroll orizontal.
- **Testare automata:** Fluxurile principale si situatiile de eroare (ultimul loc, coliziuni, acces interzis, import CSV invalid) sunt verificate prin teste automate.
- **Portabilitate si format deschis:** Aplicatia ruleaza local prin `pip install -r requirements.txt` si `flask run`, iar fisierele CSV folosesc `UTF-8`, separator virgula si antet.

### Optionale

- **Teste automate:** Teste unitare si de integrare care verifica automat scenariile critice (rezervare pe ultimul loc, coliziuni de sala, import CSV invalid, emitere certificat).
