# Meteor + Blaze
JavaScript platforma pre vývoj webových a mobilných aplikácií<br>
Server - Klient applikácie<br>
Umožňuje vyvýjať v jednom jazyku pre server, webový prehliadač a mobilné zariadenia<br>
Server posiela dáta klientovi (webový prehliadač, android appka, ios appka, ...) a on ich zobrazuje<br>
<br>
verzia 1.8 (začiatok 2012)<br>
<br>
O renderovanie dát sa starajú najmä (jedno z nich)
<ul>
  <li>React</li>
  <li>Blaze (použijeme)</li>
  <li>Angular</li>
</ul>
  
Štruktúra projektu
<ul>
  <li>Server</li>
  <li>Klient</li>
  <ul>
      <li>renderovanie (html, css, js)</li>
      <li>interakcia (ui)</li>
  </ul>
  <li>Spoločné súbory</li>
  <ul>
     <li>api (kolekcie - databáza, práca s nimi)</li>
  </ul>
</ul>

## Inštalácia
https://www.meteor.com/install
### OSX/LINUX

 `curl https://install.meteor.com/ | sh`
 ### WINDOWS
   Treba administrátorský comand prompt<br>
   Nainštalovať Chocolatey
   https://chocolatey.org/install<br>
   Potom Meteor<br>
   `choco install meteor`<br>
## Vývojové prostredie
 prakticky čokoľvek (aj notepad) ale použijeme Visual Code<br>
 potrebujeme cmd<br>
 <br>
 Vytvorenie projektu <br>
 `meteor create menorojektu`<br>
 <br>
 ```
 client/main.js //vstupný bod pre klienta
 client/main.html //definuje view template
 client/main.css // definuje štýl aplikácie
 server/main.js // vstupný bod pre server
 package.json // kontrolný súbor pre npm balíčky
 paskage-lock.json // npm závislosti
 .meteor // interné meteor súbory
 .gitignore // git
 ```
 aplikácia na strane klienta sa skladá z html hlavičky<br>
 `<head>...</head>`<br>
 a tela, ktoré je často v samostatnom súbore<br>
 `<body>...</body>`<br>
 
 ## Blaze
 ďalšia dôležitá časť je Template. Ten sa definuje ako:<br>
 ```
  <template name="menoTemplatu">
   ...
  </template>
 ```
 následne sa volá v html<br>
 ```
 {{> menoTemplatu}}
 ```
 v javascripte je referencovaný ako Template.menoTemplatu<br>
 v js subore sa pre daný template môžu definovať:<br>
 funkcie volané z html 
 ```
 Template.menoTemplate.helpers({f1(){},f2(){},..})
 ```
 eventy (kliknutie myšou, potvrdenie formulára) 
 ```
 Template.menoTemplatu.events({...})
 ```
 helper sa dá v html volať nasledujúcimi spôsobmi aj s použitím logiky:<br>
 Vypíše čokoľvek je jeho návratová hodnota
 ```
 {{f1}}
 ```
 Ak vracia pole objektov
 ```
 {{#each f1}}
   {{> nejakyTemplatePreJedenObjekt}}
 {{/each}}
 ```
 Ak vracia boolean hodnotu<br>
 ```
 {{#if f2}}
   nejaky kod ked plati
 {{else}}
   nejaky kod ked neplati
 {{/if}}
 ```
 kod pre if not<br>
 ```
 {{#unless f3}}
   nejaky kod ked neplati
 {{/unless}}
 ```
 `<body>` má špeciálny template `Template.body`<br>

## Kolekcie - Mongo
 api súbory
 ```
 MenoKolekcie = new Mongo.Collection('menoKolekcie');
 ```
 
 nájdi/dostaň všetky dáta
 ```
 MenoKolekcie.find({})
 ```
 
 nájdi/dostaň špecifické dáta, ktoré obsahujú tieto hodnoty
 ```
 MenoKolekcie.find({label1: value1, ... })
 ```
 
 vráť presne jeden objekt
 ```
 MenoKolekcie.findOne({...})
 ```
 
 vlož dáta
 ```
 MenoKolekcie.insert({label1: vlaue1, label2: value2, ... })
 ```
 
 update dáta
 ```
 MenoKolekcie.update({_id: id, ...}, {$set:{label1: value1,...}})
 ```

 odstráň data
 ```
 MenoKolekcie.remove(id)
 ```

## Spúšťanie
### Server
  Server sa spúšťa pomocou nasledovného príkazu v koreňovom priečinku projektu
  ```
  meteor
  ```
  Defaultne sa spúšťa na porte 3000 a mongo databáza beží na porte 3001
### iOS simulátor (Mac)
príprava
```
  meteor install-sdk ios
  meteor add-platform ios
```
spustenie
```
   meteor run ios
```
### android emulator
príprava
```
  meteor install-sdk android
  meteor add-platform android
```
spustenie
```
   meteor run android
```
### android zariadenie
príprava
```
  meteor install-sdk android
  meteor add-platform android
```
treba pripojiť android cez USB a mať zapnuté debugovanie na androide
potom cez cmd
```
   meteor run android-device
```
### iPhone, iPad (Apple developer account)
príprava
```
  meteor install-sdk ios
  meteor add-platform ios
```
spustenie
```
   meteor run ios-device
```

 ### Nutnosť v client/main.js (pre prístup cez android, ...)
 Treba definovať url servera, na ktorý sa bude appka pripájať.
 ```
 Meteor.absoluteUrl.defaultOptions.rootUrl = theURL;
 process.env.ROOT_URL = theURL;
 process.env.MOBILE_ROOT_URL = theURL;
 process.env.MOBILE_DDP_URL = theURL;
 process.env.DDP_DEFAULT_CONNECTION_URL = theURL;
```

 # Fórum app
 ## Použité balíčky
 reactive-dict        - dočasný stav appky (zobraz/schovaj nejaký element na kliknutie)<br>
 accounts-ui          - prihlasovanie<br>
 accounts-password    - prihlasovanie<br>
 kadira:flow-router   - presmerovanie<br>
 kadrida:blaze-layout - dynamicke renderovanie pre Blaze<br>

 ```
 meteor add reactive-dict
 meteor add accounts-ui accounts-password
 meteor add kadira:flow-router
 meteor add kadira:blaze-layout
 ```
 ## Odstránené balíčky
 insecure           - umožňuje upravoanie databázy z klienta<br>
 autopublish        - posiela všetky dáta z databázy klientovi<br>

```
 meteor remove insecure
 meteor remove autopublish
 ```
  prístup k dátam po odstránení autopublish: Meteor.subscribe Meteor.publish<br>
  vkladanie a upravovanie dát po odstránení insecure: Meteor.call<br>
  
  ## Ako spustiť kód z prednášky
<ul>
  <li>Vytvor prázdny projekt a vojdi do priečinka (cmd)</li>
  <li>Aplikuj príkazy z Použité balíčky (cmd)</li>
  <li>Aplikuj príkazy z Odstránené balíčky (cmd)</li>
  <li>Stiahni a rozbaľ jeden zo zip súborov https://github.com/SlavomirSlovenkai/SPS</li>
  <li>Otvor koreňový priečinok vytvoreného projektu</li>
  <li>Skupíruj doňho všetky súbory aj s priečinkami (client, imports, server, lib) - prepísať existujúce</li>
  <li>Spusti projekt (cmd)</li>
  <li>Prístup cez webový prehliadač</li>
</ul>
## Ako odovzdať kód
 <ul>
    <li>Skupíruj všetky súbory aj s priečinkami (client, imports, server, lib)</li>
</ul>
   
  ## Úloha 1
  Zobraz pri topicu meno autora a dátum vytvorenia.
  ## Úloha 2
  Umožni poslať súkromnú správu hocikomu, kto má účet na fóre (teraz môžeš poslať len, tým ktorý sú online, majú odpoveď vo fóre, alebo majú už s tebou začatú komunikáciu)
  ## Úloha 3 (dokončiť)
  Vytvor pod-fóra a zachovaj funkcionalitu pre topic.<br>
  Každý topic bude súčasťou nejakého pod-fóra. (optional: pod-fórum môže byť časťou iného pod-fóra)<br> 
