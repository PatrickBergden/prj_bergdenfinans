---
layout: post
cover: 'assets/images/particles10.jpg'
title: Go-Light <br>&mdash; en LightStreamer klient
date:   2016-02-11 14:08:00
tags: utveckling
subclass: 'post tag-fiction'
categories: 'patrick'
navigation: True
logo: 'assets/images/ghost.png'
---

IG erbjuder automatiserade interaktioner med deras servrar genom ett REST-baserat API.
För strömdata använder sig IG en websocket-lösning som kallas Lightstreamer.
Lightstreamer gör det möjligt för en klient att
via HTTP/HTTPS öppna en sk 'ström kanal' till
en Lightstreamer server. Från denna server kan klienten
sedan ta emot realtidsdata, så som i vårt fall kan vara
det senaste priset på FING.

Vi upprättar alltså en web socket server-klient koppling
för att ta emot realtidsdata från IG's server.
Det ger oss möjligheten att bland annat hämta marknadsdata, öppna och stänga positioner, och en hel del annat.

I denna post bekantar jag mig med hur man kan interagera
med IG's server genom programspråket Go.

> Mycket av innehållet i denna post går att läsa sig till
i Lightstreamer dokumentationen. För en mer ingående
beskrivning rekomenderar jag att man läser den.

> Det blir omständigt att hela tiden göra översättningar så
jag använder ofta det engelska namnet på många begrepp, koncept
eller teknologier....exempelvis _stream connection_ istället för
_strömkoppling_.

## Lightstreamer - strömdata över HTTP
För att i realtid kunna ta emot marknadsdata och öppna/stänga
positioner på IGs servrar, så behöver jag öppna en HTTP (över TCP)
eller HTTPS (HTTP över SSL) websocket connection.

Genom denna websocket kan jag sedan authenticera mig så att
jag kommer åt min Depå. När jag gjort detta kan jag sedan
öppna ytterligare en connection - en control connection genom
vilken jag kan sända kommandon till IGs Lightstreamer-servrar.
Med hjälp av dessa kommandon kan jag administrera/styra innehållet
i min stream connection.

Det kommunikationsprotokoll vi använder oss av kallas


### Text Mode
Lightstreamer erbjuder två olika kommunikationsprotokol
när en klient vill kommunicera med en server:
_JavaScript Mode_ och _Text Mode_.

I Text Mode packeteras realtidsuppdateringar genom ett enkelt pipe-separerat ('|'), text-protokoll.
I denna post koncentrerar jag mig just på _Text Mode_.


### HTTP/HTTPS Requests
All kommunikation med IGs Lightstreamer servrar sker alltså över HTTP eller HTTPS.
De två tillgängliga _request_-metoderna för HTTP kommunikation är __POST__ och __GET__.

Jag använder __HTTP__ för alla _control connections_ då dessa requests saknar känsligt
innehåll och främst är till för att administrera strömkopplingarna.

__HTTPS__ bör användas för all data som är känslig och därför bör krypteras.
Det kan rör sig om marknadsdata från IGs servrar till mig, eller
mina användarkontouppgifter från mig till IGs servrar.

In enlighet med HTTP spec:en bör request-formen __POST__ användas när en request
har sido-effekter på serversidan.

__GET__ requests är enklare och kan därför vara smidigare att använda under
testning.

### Servrar bakom en Load Balancer
. p.5















På IGs hemsida finna några exempelklienter man kan ladda hem om man vill bekanta sig med hur interaktion med deras API kan gå till.
IG's klienter är främst skrivna i JavaScript, Java och Excel.

Eftersom att det tyvärr inte fanns någon klient för det programspråk jag helst använder (Go), så skrev jag en egen
Go-klient för att interagera med IG's LightStreamer servrar.


> Jag vill understryka att detta alltså varken är en klient skriven av Lightstreamer teamet eller IG själva. Jag skrev den för eget bruk och lämnar inga garantier för dessa funktionalitet, korrekthet eller prestanda.
Jag hade god hjälp av Lightstreamer klienten för Python när jag skrev min Go-klient.

------

## En rudimentär klient
Denna klient är främst till för att visa på hur man kopplar upp sig till IG's
Lightstreamer server och genomför enklare interaktion.

## Om klienten
Jag skrev den här klienten eftersom att Lightstream själva inte ännu erbjduer
en klient skriven i Go. Det finns klienter Java och Javascript.

## Workflow och Sessionstruktur
Klienten gör i huvudsak följande:

1. Kopplar upp sig mot en angiven Lightstreamer server och skapar en ny [session](https://en.wikipedia.org/wiki/Session_(computer_science)).
2. Upprättar en prenumeration på de objekt man önskar
3. Meddelar när klienten tar emot den real-tids-data som servern skickar
4. Avbryter prenumerationen när sessionen är klar
5. Bryter kopplingen mot servern

Här nedan beskriver jag lite hur klientens kod ser ut.

### <a name="abcd">Session</a>
__LSClient__ är en funktion som implementerar Lightstreamer Text Mode Protocol för
hantering av HTTP request och response för korrekt kommunikation mellan klienten och servern:

1.  func connect  
    Upprätta en kontakt med servern och skapa en ny session.
2.   func disconnect  
    Skicka en begäran till servern om att stänga sessionen.
3.   func subscribe  
    Genomför en begäran (request) om att prenumerera på data från servern.
4.   func unsubscribe  
    Avregistrera klienten (eller den prenumeratrions_nyckel det rör sig om) från servern.

Så, hantering av sessionen (genom connect och disconnect) och hantering av prenumerationen av datan från servern (genom subscribe och unsubscribe).

## Subscriptions och Unsubscriptions
För att kunna ta emot _realtidsuppdateringar_ av t ex marknadsdata från IGs servrar
så måste jag skicka en __subscription__ till servern - alltså tala om för servern:

1. Vilka __items__ data jag vill ha
2. Vilka samlingar av __fält__ jag är intresserad av
3. Eventuellt även vilka __data adapters__ som bidrar med de items jag är intresserad av.

I en klient så är Subscription-funktionen den som sammanställer min begäran om vilken
data jag vill ha realtidsuppdateringar om.

När jag inte längre vill att IGs server skall skicka realtids-uppdateringar kring
en viss item, så talar jag om för servern att jag vill göra en __unsubscribe__ av denna item.

-----


Abstraktionen innehåller prenumerationsdetaljer (_item names_, _field names_
    och _management mode_). Den specifierar även en _Data Adapter_ som bidrar
    med alla _items_

Vidare så underhåller den en intern lista som möjliggör registreringen av
generiska lyssnare (genom funktionen _addlistener_) som vill bli meddelade
när servern skickar ut realtids-uppdateringar.

När en _item_ event skickas ut från servern så kommer Subscription funktionen
att meddela dessa vidare till de lyssnare som registrerats.

De huvudsakliga funktionerna är:

+   _decode  
    Avkodar field value i enlighet med Lightserver Text Protocol spec:en.
+   addlisterner  
+   notifyupdate  
    Invokeras av LSClient varje gång Lightstreamer
    server pushar ett nytt item event.

Funktionen _decode hanterar även avkodning av de text meddelanden som servern
svarar med i enlighet med Lightserver Text Protocol spec.


## Uppkoppling mot IGs Lightstreamer server
När vi vill koppla upp oss mot IGs Lightstreamer server så måste vi först
upprätta en ny _session_.

Detta gör vi genom att _LSClient_ funktionen, som i sin tur tar hjälp av
_connect_ funktionen.

LSClient tar två argument, IGs server adress och ett _Adapter Set_ - vilket är


Här är ett exempel på detta:
<pre>
ig\_lightstreamer\_client = LSClient("http://push.lightstreamer.com:80", "DEMO")
if err != nil {
    fmt.Println("Unable to connect to the IG Lightstreamer Server.")
}
else {
    ig\_lightstreamer\_client.connect()
}
</pre>


## Subscribing
Nästa steg är att upprätta en prenumeration (subscription)
som gör att IG's Lightstream server börjar sända en realtidsström
av data till vår klient.

För att åstadkomma detta behöver vi:

1.  Anropa Subscription funktionen (classen).  
    Denna tar fyra argument.
    +   prenumarations formatet ("MERGE")
    +   listan med artiklar vi vill prenumerera på
    +   listan med fält namn (field names)
    +   Data Adaptern ("QUOTE_ADAPTER")
2.  Vi måste också upprätta en prenumerationslyssnare.  
    Det gör vi med hjälp av funktionen `on_item_update`.
    Denna funktion inkluderar eventuell aktion man vill avfyra
    för varje ny artiel som uppdateras.
    __hade varit bra med ett exempel här!__
3.  `on_item_update` anropas av Subscription funktionen med hjälp
    en annan funktion `addListener`.
4.  En subscribe funktion returnerar en prenumerations nyckel.
    Denna används sedan för att både registrera prenumerationen
    hos LSClient och för senare avregistrering.

## Notifiering
När väl prenumerationen mot IGs Lightstreamer server är aktiverad
så kan vi börja ta emot de realtids event som servern börjar pusha
genom den HTTP koppling vi upprättade under [konnektionsfasen](#abcd),  det första steget när vi startar en ny session.


## Koppla ifrån Servern
Det är viktigt att koppla från servern på ett snyggt sätt.
Det gör vi med hjälp av _disconnect_ metoden
<pre>
#Disconnecting
lightstreamer_client.disconnect()
</pre>


## IG Data Adapter
Okay, så vilken typ artiklar kan vi få från IGs Data Adapter?
Vilka fält(fields) kan vi läsa av och skicka vidare för hantering?



-----


## CLI Interagera med IG Lightstream servern
I många avseende är det just en icke visuell interaktion
som är önskvärd -  gränssnitt

### Öppna en position

### Stäng en position

### Uppdatera en position

### Implementera en Trailing Stop

### Samla Live Data för post Analys

### Styra interaktionen med en Konfigurationsfil

### Alarm och email uppdateringar

### Position Sizing genom Tröskel-Logik
Vi kan stänga en position och sluta automattrada
ifall ett vist tröskelvärde överträffas.
