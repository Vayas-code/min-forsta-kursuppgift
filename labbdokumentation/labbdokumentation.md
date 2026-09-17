# Labbdokumentation: Labbmiljö,Git,CLI,AI
**Namn:** Diana  
**Datum:** 15 September 2026  
**Kurs:** Introduktion till yrkesrollen och grunderna i IT-infrastruktur  



**Introduktion:** Nu går vi igenom virituell labbmiljö, felsökning och sedan dokumenterars i Git med hjälp av Markdown format.

---

## Labbmiljö & Nätverk:
| Hostnamn | Operativsystem | IP-adress | Subnätmask | Standard Gateway |
|:--- | :--- | :--- | :--- | :--- |
| Windows 11 | Windows 11 Pro | 192.168.1.50 | 255.255.255.0 | ingen |  
| Linux | Ubuntu 25.04 | 192.168.1.51 | 24 | inget |  

## **Kommandoradsgenomförande**
***Linux***  
 Skapande av mapp och textfil.  
1. ```sudo mkdir -p " mappnamnet"```  
skrev jag in i terminalen för att skapa mappen. För att sedan skapa filen:    

2. ```cd "mappnamnet" och enter```  
 då kom jag in i mappen. Härifrån skrev jag in    

 3. ``` touch "filnamnet"``` 
 nu fick jag felmeddelande : premission denied - skrev då in:   

 4. ```sudo touch "filnamn"```  
För att kontrollera att det fungerade skrev jag in ```ls - filen``` hittades. **Viktigt!** skriv in ls i mappen du skapade filen i för att inte få upp alla mappar på enheten.  
---
 *Skapa grupp och sätt behörighet:*  
 du kan gå tillbaka till start genom att skriv cd ../../.. (antalet "../" beror på hur många / det finns för att komma till den mappen vi skapade )  
 kontrollera du är på hem : **pdw**  

 1. skapa grupp ```sudo groupadd "gruppnamn"``` 

 2. För att ändra behörigheter ``` sudo chgrp "gruppnamnet vi skapade" "sökvägen till mappen vi gjorde"```     

 3. För att sätta behörigheter  
 ```sudo chmod 750 "mappnamnet"```  
  detta ändrar behögiheten för mappen.    

 4. Ändra behörigheter för filen vi skapade
 ```sudo chmod 650 "mappnamnet + /filnamnet"```  
--- 
 750 och 650 är exempel på behörigheter enbart. Kontrollera vad för behörighet som användaren ska ha och utifrån det ändra siffrorna så dom stämmer överrens med behörighetsnivå.   

 **För att kontrollera behörigheterna skriv in**  
  ```ls -ld "mappnamnet"``` -Fick felmeddelande så skrev: sudo ls ld "mappnamn"  

Kontrollerade även ```sudo ls -l``` för att se behörigeter för filen vi skapat och lagt behörighet på.  

---  
 *Pingade Windows datorn och fick inte respons. Gick in på CMD som administratör i Windows och körde script som tillät att Linux Ip får pinga Windows datorn. Pingade igen från Linux datorn och det fungerade.*    

 ***Windows CMD***  
 Via Powershell skriv kommandot  
  ```mkdir C:"mappnamnet"```  
  För att kontorllera att den skapades skriv  
  ```test-path C:\"mappnamnet"```  
  True = Finns  
  False = Finns inte  
  *Inspektion av behörigheter för mappen vi skapade*  
  Kommandot ```Get-Acl "C:\ mappnamnet" | Format-list```  
  Visar ***| Ägare | grupper | behörighet |***   
  Det finns en ägare : *Administratören*. Det finns inga grupper och *Administratörern* har full behörighet.  
### Nedan ser vi en skärmdump av Windows ping och ipconf ### 
![Deta är skärmdumpen från när den pingade Linux-datorn samt Windows configuration](Ping.png)  

## Git och Versionshantering ## 
>Ändringshistoriken
![Ändringshistoriken](image.png)

[Länk till Git-Repo](https://github.com/Vayas-code/min-forsta-kursuppgift)  

## AI-logg och utvärdering ##  

 *Jag promptade följande*  
 **Hejsan. Jag har en fråga jag gärna vill att du förklarar för mig steg för steg. Hur kommer det sig att efter jag skapat min grupp i linux teminalen, att jag bara behöver skriva chmod 750 sedan min mappväg. Hur kommer det sig att jag inte behöver skriva mitt gruppnamn i skripten?**  

 *Jag fick detta svaret av AI*  

 >Detta beror på två grundläggande funktioner i Linux: processens ägarskap och ägarstrukturen för filer/mappar.

>När du kör ett kommando eller skript i terminalen kopplar Linux automatiskt dina användarrättigheter och dina grupper till det kommandot.

>Processens ägarskap (Vem kör kommandot?)
>När du är inloggad i terminalen körs dina kommandon under din användare och din primära/sekundära grupp.

>Din användare har en primärgrupp: När du skapar en fil eller mapp tilldelas den automatiskt din användare som ägare och din nuvarande primära grupp som gruppägare.

>Kommandon ärver din identitet: När du kör chmod behöver kommandot inte fråga vem du är eller vilken grupp du tillhör – operativsystemet skickar automatiskt med dina användar- och grupp-ID:n till processen.

>Hur chmod 750 fungerar steg för steg
Rättighetstalet 750 bestämmer rättigheter för tre separata nivåer i exakt denna ordning: Ägare (User), Grupp (Group) och Andra (Others).

>Plaintext
  7        5        0
  │        │        │
  ▼        ▼        ▼
 Ägare   Grupp    Andra
(rwx)    (r-x)    (---)
När du kör chmod 750 /din/mappväg händer följande:

>Identifiering: Linux kollar mappen du angav och ser vilken användare och grupp som står som ägare på den.

>Tilldelning av siffervärden:

>7 (Ägare): Sätter rwx (läsa, skriva, exekvera/öppna) för användaren som äger mappen.

>5 (Grupp): Sätter r-x (läsa och exekvera/öppna, men inte skriva/ändra) för gruppen som äger mappen.

>0 (Andra): Sätter --- (inga rättigheter alls) för alla andra användare på systemet.

>Verkställande: chmod ändrar rättighetsbitarna på den specifika mappen utan att behöva ändra vilken grupp som äger den.

>Varför behövs inte gruppnamnet?
Du behöver inte ange gruppnamnet eftersom chmod med tre siffror endast ändrar rättighetsnivåerna på den grupp som redan är tilldelad mappen. Kommandot bryr sig inte om vad gruppen heter, utan bara om rättighetsbitarna för den befintliga gruppägaren.

>Om du vill ändra vilken grupp som äger mappen använder du istället kommandot chgrp eller chown:

>För att ändra gruppägare: chgrp mittgruppnamn /din/mappväg

>För att ändra både ägare och grupp samtidigt: chown minanvändare:mittgruppnamn /din/mappväg

>När rätt grupp väl står som ägare på mappen behöver du alltså bara använda chmod 750 för att styra vad medlemmarna i den gruppen får göra  
--- 
***Kritisk utvärdering av svaret AI gav mig***  
 - Svaret AI gav mig är korrekt ur synpunkten hur chmod och grupper hanteras och skrivs.  
 - Jag upplever inga konstiga hallunicationer eller felaktig svar av AI.  
 - Säkerheten följs bra då jag inte skrev med mappnamnet utan hänvisade till "mappnamn" vilket gör att AI förstår vad jag menar men utan att jag behöver skriva in vad min mapp egentligen heter.  

*AI ger mig ett djupgående svar steg för steg hur chmod och grupper hanteras, hur det är uppbyggt och hur man på ett korrekt sätt skriver skriptet i terminalen. Den ger mig grundlig förklaring på vad varje del i skriptet betyder och varför det är viktigt samtidigt som den ger en tydlig kod där jag själv skriver in mitt gruppnamn/min mappväg. Med detta så ger den mig en bra förståelse på grunden till hanteringen och den får inte tillgång till några namn på mina mappar vilket förstärker säkerhetet för mig.*  
