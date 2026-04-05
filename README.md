> Realizat de studentul: Badia Victor \
> Grupa: I2301
> \
> Verificat de N. Nartea

## Scopul lucrării
Consolidarea celor mai importante practici de securitate în WordPress: gestionarea rolurilor și parolelor, actualizări, hardening de bază (wp-config.php, permisiuni, dezactivarea editorului), backup, monitorizarea activității și configurarea pas cu pas a All In One WP Security & Firewall (AIOS) pentru protecție împotriva atacurilor de tip brute-force, WAF de bază și controlul permisiunilor.

## Condiții
### Pasul 1. Pregătirea mediului
1. În instalarea locală WordPress, accesează panoul de administrare.
2. Asigură-te că ai acces de administrator.
3. Activează modul de depanare în wp-config.php, adăugând:
```bash
define('WP_DEBUG', true);
```

<img width="507" height="51" alt="image" src="https://github.com/user-attachments/assets/b9b849ae-3892-4705-9957-61a8a853a5c3" />

### Pasul 2. Gestionarea rolurilor și parolelor
1. Creează un utilizator de test cu rolul Autor (pentru verificări ulterioare).
<img width="992" height="803" alt="image" src="https://github.com/user-attachments/assets/1cb63c03-d0d6-4f27-a80f-c00ddf52d4d3" />

2. Verifică dacă fiecare administrator are parole complexe activate (minimum 8 caractere, litere/cifre/simboluri).

### Pasul 3. Actualizări ale nucleului, temelor și pluginurilor
1. Verifică existența actualizărilor pentru WordPress, teme și pluginuri.
2. Actualizează toate componentele la ultimele versiuni disponibile.
<img width="1894" height="825" alt="image" src="https://github.com/user-attachments/assets/4a4aef58-419f-40b3-ba46-09101440058f" />
<img width="1181" height="747" alt="image" src="https://github.com/user-attachments/assets/4164e095-47ab-4c4c-87f5-3348216106a9" />

3. Configurează actualizările automate pentru teme și pluginuri.
<img width="426" height="390" alt="image" src="https://github.com/user-attachments/assets/9c97da93-4f0b-4674-ac4a-20818b4a858c" />

4. Asigură-te că toate actualizările s-au aplicat cu succes și că site-ul funcționează corect.
<img width="620" height="169" alt="image" src="https://github.com/user-attachments/assets/ab7ed291-ad61-4836-8b8d-ff8c91535274" />

### Pasul 4. Hardening de bază
1. Dezactivează editarea fișierelor din panoul de administrare, adăugând în wp-config.php:
```bash
define('DISALLOW_FILE_EDIT', true);
```

<img width="517" height="48" alt="image" src="https://github.com/user-attachments/assets/0f6901b2-19e6-4835-bc95-3c9206ec11ce" />

2. Configurează permisiuni corecte pentru fișiere și foldere: Foldere: 755 | Fișiere: 644
3. Protejează fișierul wp-config.php, adăugând în .htaccess:
```bash
<files wp-config.php>
   order allow,deny
   deny from all
</files>
```

<img width="298" height="114" alt="image" src="https://github.com/user-attachments/assets/2d88f2dd-c2a3-4102-9d37-1295b25b1c71" />

### Pasul 5. Instalarea și configurarea inițială a All In One WP Security & Firewall (AIOS)
1. Instalează și activează pluginul All In One WP Security & Firewall.
<img width="558" height="478" alt="image" src="https://github.com/user-attachments/assets/1efbe6d6-6dcf-4373-8278-463f078507ea" />

2. Accesează secțiunea pluginului din panoul de administrare.
<img width="1914" height="851" alt="image" src="https://github.com/user-attachments/assets/3574b8e5-cd14-45c8-beb3-f801810e4405" />

3. Configurează următorii parametri:
User Login: Activează Login Lockdown. Parametri recomandați pentru testare: Max Login Attempts: 5, Login Retry Time Period: 15 min, Lockout Time: 30 min.
Activează Force Logout (de exemplu, 24h) pentru a limita sesiunile „eterne”.
<img width="527" height="286" alt="image" src="https://github.com/user-attachments/assets/28a85a45-f48b-45d0-baf2-9da34c8f18cb" />

User Accounts: Verifică dacă există un utilizator cu loginul admin. Dacă da — redenumește-l prin AIOS cu un nume de utilizator sigur.

### Pasul 6. Testarea protecției brute-force (pe un utilizator de test)
1. Deconectează-te din admin (sau folosește o fereastră privată).
<img width="742" height="520" alt="image" src="https://github.com/user-attachments/assets/b5c7d595-cadc-4b4a-baec-8749b949aa7a" />

2. Accesează noul URL de autentificare, încearcă să introduci o parolă greșită de 5–6 ori.
3. Asigură-te că Lockdown-ul s-a activat (blocare IP/utilizator).
4. Verifică înregistrarea blocării în WP Security → Dashboard / Logs și (dacă este necesar) deblochează IP-ul de test.
<img width="1657" height="68" alt="image" src="https://github.com/user-attachments/assets/51e51f04-4d2a-4f95-82ba-76f09fa641cf" />

### Întrebări de control
1. De ce DISALLOW_FILE_EDIT și permisiunile corecte pe wp-config.php reduc semnificativ riscul post-exploit?
Dacă un atacator obține acces la un cont de administrator, DISALLOW_FILE_EDIT îl împiedică să modifice direct codul temelor/pluginurilor din panou pentru a injecta malware (backdoors). Permisiunile corecte pe wp-config.php (cum este 440 sau 600 pe servere reale) previn citirea fișierului de către alți utilizatori ai serverului, protejând datele de conectare la baza de date.

2. Ce setări ai ales pentru Login Lockdown/Firewall și de ce (explică echilibrul între securitate și experiența utilizatorului)?
Setări: 5 încercări / 15 min perioadă / 30 min blocare.
Motivație: Acest set de reguli oprește atacurile automate de tip brute-force fără a pedepsi sever un utilizator legitim care a uitat parola (30 de minute este un timp de așteptare rezonabil, nu permanent).
WAF (Firewall): Nivelul "Basic" filtrează injecțiile SQL și XSS fără a bloca vizitatorii reali sau a încetini site-ul excesiv.

3. Cu ce se deosebesc măsurile de protecție la nivel WordPress (plugin/WAF) față de cele la nivelul serverului web și al sistemului de operare?
WordPress (Plugin/WAF): Acționează târziu (aplicația PHP trebuie să ruleze deja). Este ușor de gestionat de către utilizatori ne-tehnici, dar consumă resurse (CPU/RAM).
Server/OS (Apache/Nginx/IP Tables): Blochează amenințările înainte ca WordPress să proceseze cererea. Este mult mai rapid și eficient, dar necesită acces la configurarea serverului și cunoștințe tehnice avansate.

4. Ce trebuie inclus neapărat într-un backup „complet” WordPress și cum verifici dacă restaurarea funcționează cu adevărat?
Includem baza de date, fișierele fizice (folderul wp-content pentru teme/pluginuri/poze și wp-config.php/.htaccess).
Singura metodă reală este restaurarea pe un mediu diferit (ex: un subdomeniu sau alt folder local) și verificarea manuală a paginilor, a link-urilor și a prezenței imaginilor în librăria media. Dacă baza de date e acolo, dar imaginile lipsesc de pe disc, backup-ul este incomplet.
