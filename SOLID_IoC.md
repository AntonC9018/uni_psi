# Referat la Sistemele Informatice

Tema: **Principiile SOLID și IoC în dezvoltarea software**

A realizat: **Curmanschii Anton, IA1901**


## Conținutul

- [Referat la Sistemele Informatice](#referat-la-sistemele-informatice)
	- [Conținutul](#conținutul)
	- [Introducere](#introducere)
	- [Rețetă de ou prăjit](#rețetă-de-ou-prăjit)
		- [Dependency Inversion Principle (DIP)](#dependency-inversion-principle-dip)
		- [Inversion of Control (IoC)](#inversion-of-control-ioc)
		- [Single Responsibility (S-ul din SOLID)](#single-responsibility-s-ul-din-solid)
		- [Celelalte principii](#celelalte-principii)
		- [Liskov substituion principle (L-ul)](#liskov-substituion-principle-l-ul)
	- [Concluzii](#concluzii)


## Introducere

Principiile SOLID, IoC (Inversion of Control) și derivatele DI (Dependency Injection) stau la baza dezvoltării software moderne.
În cea mai mare parte principiile acestea se aplică la dezvoltarea software orientată pe obiecte, însă unele idei sunt de fapt utile pentru orice model de programare.

În această lucrare voi încerca să aduc o introducere naturală și intuitivă în conceptele acestea, să le motivez explicând de ce ele sunt atât de utile.


## Rețetă de ou prăjit

Vom încerca să modelăm procesul de pregătire a *oălor prăjite*. 
Prin acest exemplu voi încerca să demonstrez cum principiile de programare menționate în introducere sunt natural aplicate pentru a îmbunătăți implementarea unui algoritm.

Deci realizarea inițială a acestui modelul ar fi de exemplu astfel:
1. Se pune o tigaie pe aragaz.
2. Se aprinde gazul.
3. Se ia un ou de găină din frigider.
4. Se sparge în tigaie.
5. (Opțional) Se adaugă condimentele, sarea.
6. Se așteaptă până momentul când pe laturile oului apare o crustă aurie.  

Sensibil?

Acest model este cel mai obișnuit pentru noi, însă are unele inconsecvețe când utilizat ca o rețetă mai generală.

- *ouăle se iau de la frigider* — poate stăpânul are găini, poate numai ce le-au cumpărat?  
- *ou de găină* este un obiect concret. Poate persoana are ouăle de struț?
- *Tigaia, aragaz* — este posibil să realizăm rețeta pe o placă metalică sub soare în deșert.
- *Se sparge* — putem găti cu ouăle deja sparse.

Concluzia este că rețeta nu trebuie să determine *modelele specifice*, ci *un proces abstract* de pregătire a oulor.
În implementarea noastră naivă *am dat prea multe detalii*, fiind prea specifici referitor la pași, prin urmare am primit un model negeneral.  

> Remarcă: Aceasta nu este necesar rău pentru un algoritm să fie atât de specific. 
> Dacă am descrie o astfel de rețetă pentru oameni din Moldova, ar fi mai simplă, mai ușor de înțeles, și ar fi corectă în 99.99% de cazuri.
> Este important să înțelegeți că ceea ce se descrie mai departe *nu trebuie să fie aplicat în practică cu excepția cazului în care algoritmul de fapt necesită să fie mai general*.
> O capcană comună a programării orientate pe obiecte este că *generalizarea are tendința să fie suprautilizată*, în special de începători.
> Realitatea este că *aceste principii trebuie să fie utilizate numai când se necesită un model mai complex*, deoarece ele mereu sacrifică simplitatea.

> În opinia mea, scopul primal al unui programator bun trebuie să fie tendința spre simplitate.
> Contrar intuiției, complexități în unele părți ale programului pot să aducă simplitate în alte părți ale ei, micșorând complexitatea totală.
> Deci programatorul trebuie să compromită simplitatea în unele părți pentru a atinge mai mare simplitate totală în program.


### Dependency Inversion Principle (DIP)

Ideea este să depindem de abstracții decât de obiecte sau procese specifici.

- "ou de găină" -> "ou" (`ou`);
- "o tigaie pe aragaz, se aprinde gazul" -> "un obiect în care putem găti ouăle" (vom numi `gătitor`).

Acum algoritmul devine mai abstract:
1. Se inițializează `gătitorul`.
2. Se ia un ou de la frigider.
3. Se sparge.
4. Se inserează în `gătitor`.
5. (Opțional) Se adaugă condimentele, sarea.
6. Se așteaptă până momentul când pe laturile oului apare o crustă aurie.

Acum algoritmul permite orice tip de ou (de găină sau de struț), fiind dat ele pot fi sparse, inserate în `gătitor` și manifestă o crustă aurie când gătite până la urmă. 
Acestea sunt restricții impuși asupra obiectului `ou`, sau proprietățile specifice care orice ou posedă.
Am putea fi și mai generali, dar după opinia mea este destul de bună descriere pentru `ou`.

`gătitor` poate fi ori o tigaie și aragaz, ori o placă metalică sub soare în deșert.

> Este ultimul principiu de-a lui SOLID (D-ul).


### Inversion of Control (IoC)

Inversion of control se referă la faptul că trebuie să obținem cumva dependențele când creăm un algoritm specific.
În cazul nostru, într-un algoritm specific avem `gătitor` și `ou` ca dependețele.
Gătitorul poate fi specificat la crearea algoritmului (de ex. prin constructor), selectat printre resurse la dispoziția persoanei, iar `ou` se obține deja direct în specificația algoritmului.

- Se ia un `ou` de la frigider.

Algoritmul poate lucra și dacă `ou` este obținut într-un alt mod, deci detaliul despre frigider nu este necesar pentru algoritm.

Ideea este să definim un `provider` care să ne dea acel `ou` într-un oarecare mod plăcut lui, dependent de specifica implementare a acestui `provider`. El tot ar fi selectat din resurse disponibile persoanei care execută algoritmul.

- Se obține un `ou` folosind `provider`.

Acum algoritmul are și mai puține detalii inutile:
1. Se inițializează `gătitorul`.
2. Se obține un `ou` folosind `provider`.
3. Se sparge.
4. Se inserează în `gătitor`.
5. (Opțional) Se adaugă condimentele, sarea.
6. Se așteaptă până momentul când pe laturile oului apare o crustă aurie.

Deci acum algoritmul este dependent doar de proprietățile caracteristice ale `gătitorului` și ale `oului`.

Am putea schimba specificația și astfel, tot ar fi una validă. Deja depinde de implementarea concretă:

- Primim un `gătitor` inițilizat și un `ou`.
- Urmăm pașii 3-6 din algoritmul inițial. 

De fapt, tehnica concretă utilizată la implementare se numește **DI (Dependency Injection)**. 


### Single Responsibility (S-ul din SOLID)

Acest principiu zice că obiecte trebuie să facă doar un singur lucru.

În cazul nostru algoritmul deja face un singur lucru (definește pașii de pregătire a unui ou), deci principiul este urmărit.

Legat de exemplul nostru cu rețeta, principiul nu ar fi urmărit dacă algoritmul ar include informația cum să serviți masa sau cum să mâncați acel ou. Și pe algoritmul nostru aceste lucruri cu totul nu-l afectează.

De ce să facem acest lucru? Ca să putem gândi despre părțile individuale aparte, ca să le putem modifica și combina în mai multe moduri.

> Iarăși, este necesar să înțelegem că trebuie să facem acest lucru numai dacă ar simplifica programul în întregime.
> O funcție lungă este mai simplă și mai inteigibilă decât mai multe funcții mici, deoarece ea introduce mai puține abstracții.
> Trebuie s-o separăm în mai multe funcții doar pentru a practica reutilizarea codului.
> N-are sens să prierdem timpul nostru la acest lucru dacă nu vom utiliza aceste funcții mai mici mai undeva.


### Celelalte principii

Nu am menționat principiile *Open-closed principle* și *Interface segregation principle*, deoarece ele nu pot fi ilustrate prin acest exemplu simplist.

Dacă IoC a făcut algoritmul mai simplu și mai decuplat de implementări specifice ale oului și ale gătitorului, demostrarea acestor 2 principii doar ar complica algoritmul, deoarece sunt utile numai când avem mai multe așa algoritmi care trebuie să se comunice.

În scurt, urmând *Open-closed principle* am schimba algoritmul nostru la o clasă generică de o rețetă, care poate fi parametrizată de dependențe și care permite specificarea pașilor.

*Interface segregation principle* ar fi util dacă am introduce un algoritm de consumarea unui ou. 
Oul atunci ar putea fi mâncat, iar acest principiu ne spune să nu introducem acestă proprietate în specificația oului legată de pregătire de mai sus,
ci să definim o altă specificație pentru oul care poate fi mâncat și deja să ne depindem de aceasta în algoritm de consumare.  

În scopul unui singur algoritm, aceste lucruri nu sunt utile.


### Liskov substituion principle (L-ul)

Acest principiu descrie proprietățile unei ierarhii de clase.

În programarea modernă, *Liskov substituion principle* nu mai este relevant, deoarece moștenirea nu se mai utilizează, fiind substituită de servicii și interfețe. 
Moștenirea se utilizează în mare parte ca un mixin, util pentru a introduce unele proprietăți, fie un fel de boilerplate, direct într-o clasă, ceea ce nu a fost obiectivul inițial al moștenirii.
Însă chiar în așa cazuri ea este de obicei una simplă (de cel mult un nivel).


## Concluzii

Am demonstrat unele principii SOLID și am ilustrat principiul IoC, am definit DI fără a utiliza codul, pe baza unui exemplu simplu.

Am vrut ca acest articol să fie succint, de aceea nu m-am dat în mai multe exemple pentru a demonstra cum-se-cade toate principiile.
