# Primerjava genskih ekspresijskih profilov pri pljučnem raku
###### Vmesno poročilo (2. 4. 2019)
## OPIS NALOGE
Telo je sestavljeno iz različnih tkiv, ki sestavljajo organe in celoten organizem. Tkiva so sestavljena iz posameznih celic, ki opravljajo svojo funkcijo znotraj tkiva. Vse celice enakega organizma imajo enak dedni zapis v obliki dvojne vijačnice DNA. Na njej so zapisani vsi geni organizma, a se v vsakem tkivu ne izražajo vsi. Izražanje gena pomeni prepis genskega zapisa iz DNA v mRNA, čemur rečemo transkripcija. V procesu transkripcije nastane enojna vijačnica mRNA (transkript), ki nosi zapis za aminokislinsko zaporedje proteina. Na mRNA se veže ribosom, ki izvede sintezo proteina, ki je na transkriptu zapisan. Proteini so komponente celic, ki opravljajo večino funkcij v njej (prenos energije, metabolizem, signalne poti, transport, struktura, kataliza reakcij, podvojevanje, ..). 

Koncentracija posameznih proteinov v celici je pomembna za njeno delovanje, vendar je merjenje le-teh težavna. Koncentracijo lahko izmerimo posredno, preko koncentracije transkriptov v celici, ki je sorazmerna s koncentracijo proteinov. Izražamo jo kot število detektiranih transkriptov na milijon transkriptov (TPM – transcript per million). Ob odvzemu tkiva lahko z analizo določimo ekspresijo transkriptov za vse gene in to imenujemo ekspresijski profil.

Ekspresijski profil tkiva se poruši ob stresnih stanjih za tkivo, kot so na primer bolezni in rakava obolenja. Nekateri geni se značilno izražajo v rakavih tkivih, drugi pa se izražajo kot posledica stanja v celici ali izražanja gena, ki se nahaja blizu na kromosomu. 

Za analizo smo si izbrali rakava obolenja pljučnih tkiv, saj je v zadnjih letih povečana incidenca pljučnega raka zaradi kancerogenost cigaretnega dima. Naš namen je poiskati prekomerno izražene gene v rakavem tkivu ter gene, ki se z njimi izražajo obratno (supresija ekspresije). Z aktivacijo izražanja teh genov, bi lahko zmanjšali izražaje ključnih genov za napredovanje raka in s tem zaustavili napredovanje.


## OPIS PODATKOV
Organizem: Homo sapiens (Človek)

Za analizo normalnih (zdravih) tkiv smo uporabili podatke iz baze podatkov Protein Atlas. Zbrane so tkivne ekspresije 37 človeških tkiv, vrednosti za vsak gen pa so povprečne vrednosti izražanja (enote TPM). Opisanih transkriptov je 19600. To so glavni podatki, ki jih uporabljamo za analizo genov, zato je večina opisanih metod izvedena na tem setu podatkov. [datoteka: rna_tissue.csv]

Za analizo rakavega tkiva smo uporabili primerjavo genskih ekspresijskih profilov normalnega in rakavega tkiva pljuč posameznika po izrezu tkiva. Podatki za vsak gen so bili izraženi kot logaritem z osnovo 2 spremembe izražanja (〖log〗_2  (ekspresija v rakavem tkivu)/(ekspresija v normalnem tkivu)) in p-vrednost prejšnjega podatka. Podani so bili le geni, pri katerih p-vrednost ni presegala 1. [datoteka: Lung_cancer.csv]

Pri izražanju genov je pomembna tudi njihova medsebojna lokacija na kromosomu. Za analizo lokacij smo uporabili podatke o lokacijah genov na DNA. Ob izražanju enega gena, se lahko izraža sosednji gen kot posledica bližine na kromosomu. [datoteka: HumanChromoLocation.csv]
 

## IZVEDENE ANALIZE
Podatke smo uredili po genih in tkivih, nato pa se spoznali s samimi vrednostmi. Nekateri transkripti se izražajo v vseh tkivih, drugi v le redkih, tretji le v enem. Vrednosti izražanja se gibajo med 105947 TPM (albumin v jetrih) do 0.1 TPM. Za izvedbo analize medsebojnega vpliva na izražanje ne bomo potrebovali transkriptov, ki se pojavljajo le v določenih tkivih (tkivno specifični), vendar le tiste, ki se izražajo v vseh tkivih (hišni geni). 

Ker je transkriptov veliko in njihov doprinos majhen, želimo iz seta podatkov za obdelavo izločiti neuporabne transkripte oz. tiste, ki jih nam v analizi ne bodo doprinesli k rezultatom.

  ####  1.	Analiza števila maksimalnih ekspresij v tkivih
Izvedli smo analizo, kjer smo ugotavljali, v katerem tkivu ima največ transkriptov največje izražanje. 

![alt text]( https://github.com/AzzySlo/PR19MSADZU/blob/master/ProjektSlike/slika1.png) 

Slika1: Graf števila maksimalnih ekspresij v tkivu

Ugotovili smo, da je največ transkriptov maksimalno izraženih v modih, s česar lahko sklepamo, da je se tam izraža največ tkivno specifičnih genov. V pljučih je število maksimalnih vrednosti transkriptov pod 500, kar nakazuje na manjši delež tkivno specifičnih genov. S tem smo ugotovili, da lahko tkivno specifične transkripte izpustimo iz analize. 

  ####  2.	Določevanje tkivne specifičnosti
Merilo za tkivno specifičnost je v molekularni biologiji postavljeno grobo in empirično, za vsak gen posebej. Želeli smo ovrednotiti tkivno specifičnost transkriptov glede na njihovo izražanje v vseh tkivih in to vrednost poimenovali vrednost tkivne specifičnosti gena (VTSG). Izkazalo se je, da delež izražanja najbolje opiše normalizacija ekspresijskih vrednosti glede na izražanje v tkivu z maksimalnim izražanjem transkripta. Za izračun srednje vrednosti, ki najbolje opiše realno stanje, smo uporabili studentovo porazdelitev. Za primer sta navedena gena INS (gen za inzulin, tkivno specifičen) in SDHA (gen za sukcinat dehidrogenazo, hišni gen).

![alt text]( https://github.com/AzzySlo/PR19MSADZU/blob/master/ProjektSlike/slika2.png)

Slika2: (Levo) Histogram porazdelitve normaliziranih vrednosti ekspresije za transkripta INS in SDHA; (desno) studentova porazdelitev z uporabo podatkov o normalizirani ekspresiji INS in SDHA.

Za VTSG vrednost transkripta smo določili koren ničle odvoda studentove porazdelitve. Ničlo odvoda smo korenili zato, da smo dobili večje razlike med vrednostmi bližje ničli, kjer se nahaja večina tkivno specifičnih transkriptov. 

Na podlagi VTSG vrednosti smo transkripte razvrstili v 16 razredov (od 0 do 15) in sicer tako, da smo VTSG vrednost delili s faktorjem 0.05 in vrednost zaokrožili navzdol. V kolikor je vrednost presegala 15, smo transkript umestili v razred 15.

![alt text]( https://github.com/AzzySlo/PR19MSADZU/blob/master/ProjektSlike/slika3.png)

Slika3: Moč razredov transkriptov po porazdelitvi glede na VTSG.
Za nadaljnje analize smo uporabljali le razrede od 5 naprej.

  ####  3.	Iskanje povezav med ekspresijami transkriptov

Iskanje povezav med ekspresijami transkriptov smo izvedli tako, da smo za dvojico transkriptov izračunali skupno ekspresijo (skpEKS), ki naj bi predstavljala, kako povezano je izražanje transkriptov. Skupna ekspresija je vsota tkivnih ekspresij dveh genov. Tkivna ekspresija gena se izračuna po formuli:

tkivnaEkspresija= -1^k*((ekspresijaTranskripta1-VTSG^2 )^2+ (ekspresijaTranskripta2-VTSG^2 )^2),
kjer je k lih, če sta predznaka razlik ekspresijskih vrednosti in VTSG^2 različna in sod, če enaka.

Skupna ekspresija je pozitivna, ko se transkripta izražata hkrati (KOEKSPRESIJA) in negativna, ko se transkripta izražata izključujoče (SUPRESIJA IZRAŽANJA). 

Izračunane vrednosti so se izkazale za dobro oceno skupnega izražanja transkriptov, vendar smo opazili pomanjkljivosti. Absolutno višje vrednosti so kazale dvojice, ki so v majhnem številu tkiv kazale koekspresijo oz. supresijo, v ostalih pa vrednosti tkivne ekspresije niso signifikantno odstopale od ničelne vrednosti. Primer dveh genov s skupno ekspresijo -5.95128018.

![alt text]( https://github.com/AzzySlo/PR19MSADZU/blob/master/ProjektSlike/slika4.png)

Slika4: Primerjava tkivne ekspresije glede dvojice transkriptov AASS in ZNF296 za 15 tkiv.
 
  ####  4.	Analiza povezav med položajem na kromosomu in skupno ekspresijo
Podatke o položajih smo pretvorili v dostopne podatke za izvajanje analiz. Pripravili smo funkcije, ki povejo razdalje med geni in medsebojno lokacijo.

Analizo o pomenu položaja na kromosomu bomo izvedli, ko bodo znane povezave med izražanjem kromosomov.


## V NADALJEVANJU RAZISKAVE
V nadaljevanju bomo s pomočjo prileganja nabora funkcij poskušali ovrednotit skupno ekspresijo dvojice transkriptov in tako najti povezave med izražanjem transkriptov. Na podlagi analize o ekspresiji rakavega tkiva bomo zožili nabor transkriptov, po katerem bomo iskali primerne za nadaljnjo obdelavo. 
