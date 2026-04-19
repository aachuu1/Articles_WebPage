# Articles_WebPage
1. De ce Logout este <form method="post"> si nu un link <a href>?

Logout este implementat ca POST deoarece modifica starea aplicatiei, adica utilizatorul este delogat. Actiunile care modifica date nu ar trebui facute prin GET, ci prin POST. Daca logout ar fi un link de tip GET, acesta ar putea fi accesat accidental prin refresh sau prefetch, dar si exploatat prin atacuri de tip CSRF, unde un alt site ar putea forta delogarea utilizatorului fara ca acesta sa isi doreasca. Din acest motiv, folosim un formular POST care este mai sigur.

2. De ce login-ul face doi pasi in loc de unul?

Login-ul face doi pasi deoarece in Identity, email-ul nu este acelasi lucru cu UserName-ul. Mai intai se cauta utilizatorul dupa email cu FindByEmailAsync, iar apoi se foloseste UserName pentru autentificare in PasswordSignInAsync. Nu exista o metoda directa care sa primeasca email si parola pentru ca sistemul de autentificare este construit in jurul UserName-ului, iar email-ul este doar o informatie suplimentara. De aceea este nevoie de acesti doi pasi.

3. De ce nu este suficient sa ascundem butoanele din View?

Chiar daca ascundem butoanele Edit si Delete in View folosind conditii de tipul User.Identity.IsAuthenticated, acest lucru nu este suficient pentru securitate. Un utilizator poate accesa direct URL-ul actiunii din controller si poate incerca sa modifice datele fara sa foloseasca interfata. De aceea, este necesar sa protejam si in backend cu [Authorize] si verificari de tipul IsOwnerOrAdmin(). Daca folosim doar [Authorize] fara sa ascundem butoanele in View, utilizatorul va vedea optiunile dar va primi eroare cand incearca sa le foloseasca, ceea ce este sigur, dar nu ofera o experienta buna.

4. Ce este middleware pipeline-ul in ASP.NET Core?

Middleware pipeline-ul este un lant de componente prin care trece fiecare request HTTP in ASP.NET Core. Fiecare middleware poate procesa request-ul, il poate modifica sau poate decide daca il trimite mai departe. Ordinea este foarte importanta. UseAuthentication() trebuie sa fie inainte de UseAuthorization() deoarece mai intai trebuie sa identificam utilizatorul si abia apoi sa verificam ce are voie sa faca. Daca le inversam, autorizarea nu va sti cine este utilizatorul si accesul poate fi refuzat incorect.

5. Ce am fi trebuit sa implementam manual fara ASP.NET Core Identity?

Daca nu foloseam ASP.NET Core Identity, ar fi trebuit sa implementam manual tot sistemul de autentificare. Asta inseamna inregistrare utilizatori, login, verificarea parolelor cu hash, gestionarea sesiunilor sau a cookie-urilor, resetare parola, confirmare email, precum si sistem de roluri si permisiuni. In plus, ar fi trebuit sa ne ocupam si de securitate, cum ar fi protectia impotriva atacurilor brute force sau CSRF. Practic, Identity ne ofera deja toate aceste lucruri gata implementate.

6. Dezavantajele folosirii ASP.NET Core Identity

Desi Identity este foarte util si rapid de folosit, are si dezavantaje. Este destul de complex si creeaza o schema de baza de date complicata, cu multe tabele generate automat. De asemenea, este mai greu de personalizat daca vrei un sistem foarte diferit de cel standard. Migrarea catre un alt sistem de autentificare poate fi dificila din cauza dependentei de structura sa interna. In plus, pentru aplicatii moderne de tip API sau mobile, uneori nu este cea mai flexibila solutie, deoarece alte solutii precum JWT sunt mai usor de integrat cu frontend-uri separate.

Demo yt: https://youtu.be/poOQyWz_tSE
