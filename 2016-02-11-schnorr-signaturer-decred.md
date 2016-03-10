---
layout: post
cover: 'assets/images/cover_dig_sig_01.jpg'
title: Schnorr Digitala Signaturer
date:   2016-02-11 14:08:00
tags: blockchain
subclass: 'post tag-fiction'
categories: 'patrick'
navigation: True
logo: 'assets/images/ghost.png'
---
## Från bläck och papper...
Detta Att signera dokument och beslut har människan gjort i hundratals år.
I bilden ovan signerar Amerikanska Senatorer __The Constitution__ vilket
gav staterna en enad lag.

Varje senator signerade alltså med en penna ett pappersdokument (konstitutionen) - handlingen legitimerade och gav dokumentet juridisk betydelse.

### ...till datorer och hemligheter
Datoriseringen av världen sker med accelererande hastighet.
Allt fler beslut fattas via email och telefonsamtal.
Denna värld skapar ett behov av att kunna signera saker digitalt.

Inom, tex kryptovalutor som Bitcoin är det viktigt att veta vem som skickar ett meddelande. Tekniken för att säkerställa detta kallas [Digitala Signaturer](https://en.wikipedia.org/wiki/Digital_signature).
Bitcoin använder idag en variant av digital signatur som kallas Elliptisk Kurvatur Kryptografi. [Jag har tidigare skrivit en post och ett program som beskriver hur denna version av digitala signering går till](www.gp.se).

## Digitala signaturer
Ett mycket vanligt behov på Internet är att kunna bekräfta att den part (person, organisation etc) som skickat ett meddelande, faktiskt är den de utger sig för att vara.

Så, signeringens värde ligger främst hos mottagaren av meddelandet och dennes
behov att kunna bekräfta att meddelandets avsändare är den hon utger sig för att vara.

Denna post är alltså en mindre teknisk post där jag ger mig på att förklara hur digitala signaturer fungerar - och då främst [__Schnorr Digital Signatur__](https://en.wikipedia.org/wiki/Schnorr_signature).
Beskrivningen/definitionen som finns på Wikipediasidan kan nog upplevas
som _helt galet torr!_, men där finns i alla fall allt förklarat, med matematisk stringens.

> I slutet av denna post tänkte jag ge mig på att skapa en påhittad egen minikonstitution, skriva under den och skicka den till två andra parter som gör detsamma.
Lite som en modern version av bilden på signeringen av The Constitution.

### Schnorr Digitala Signaturer
Det finns flera olika varianter av signatur schema som baseras på _[diskreta logaritmer](www.gp.se)_
En huvudsaklig fördel med _Schnorr digital signatur_ är __hastighet__, jämte andra, är att merparten av beräkningen kan göras __innan__ man sänder meddedlandet.
Detta skyndar på processen när man faktiskt vill skicka meddelandet.

Det finns olika versioner av denna algoritm, bland annat den som bygger på elliptiska
kurvor och _finite fields_ varianten.
I denna post väljer jag att använda mig av den allmäna versionen.

När kan detta vara av värde?


> Ja, en jämförelse skulle kunna vara tiden det tar för 10 personer att lägga i ett brev i en brevlåda. Om alla man redan har skrivit adressen och frankerat brevet innan man ställer sig i kön går det väldigt mycket snabbare än om alla skall vänta på att nästa person vid brevlådan skriver adress och slickar på ett frimärke innan de lägger brevet på lådan.

### Implementering i programkod
Teori är super, men personligen har jag svårt att förstå teori om jag inte implementerar
den i något konkret. I denna post lägger jag efter varje sektion in källkod.

Den koden representerar alltså då teorin implementerad.
Det kompletta programmet i sin helhet, återfinns i slutet av posten.

Jag har även lagt programmet på min GitHub sida - men kom ihåg att detta bara är [throw-away-code]()
och inte på något sätt tänkt att användas i något skarpt.

----
## Schnorr Algoritmen
Här nedan går jag igenom hela processen med att signera något med en Schnorr
digital signatur. För den som inte är superförtjust i Matematik - och då framför allt Modulär Aritmetik och Algebraiska Strukturer - [så har jag skrivit några ord som kanske kan motivera lite grann](#cheeryouup)<sup>\*</sup>.




### Att välja Parametrar
Alla användare av signatur [schemat](www.gp.se) kommer överens om en mängd $G$ med generatorn $g$ av primtalsorder $q$ i vilket det diskret logaritmiska problemet är svårt.

> All users of the signature scheme agree on a group G with generator g of prime order q in which the discrete log problem is hard.

Följande parameterar ingår i _Schnorr Digital Signatur_:<br>
$p, q, g, s, v, r, x, y$

Dessa parametrar representerar:

+   $p$: ett stort primtal (ofta representerad med ett 1024 bit tal) som är tillgängligt för alla.
+   $q$: en stor [primtalsfaktor](http://www.mathsisfun.com/definitions/factor.html) av $p-1$  (ofta representerad med ett 160 bit tal) och som är tillgänglig för alla.
+   $g$: ett heltal med $order\;q\;modulo\;p,\;i\;[1, \cdot\cdot\cdot, p-1]$,<br>(i själva verket en [generator](https://crypto.stanford.edu/pbc/notes/numbertheory/gen.html)) som uppfyller $a^q = 1\,mod\,p$

Dessa tre värden, $p,q$ och $g$, kallas tillsammans __publika__. Man kan betrakta dem som en mängd kända värden som användarna av signaturen kommit överens om att använda.

>In modular arithmetic, a branch of number theory, a number g is a primitive root modulo n if every number a coprime to n is congruent to a power of g modulo n. That is, for every integer a coprime to n, there is an integer k such that gk ≡ a (mod n). Such k is called the index or discrete logarithm of a to the base g modulo n.<br>In other words, g is a generator of the multiplicative group of integers modulo n.

#### Källkod: Parametrar
<pre>
package main

import (
	"fmt"
	"math"
)

// Överenskomna parametrar
// p: ett primtal
var p int = 607
// q: en faktor i p-1
//var q int = 101
// g: en generator(funktion)
var g int = 601
</pre>

-----
### Key generation
Nu behöver vi ett [nyckelpar](https://en.wikipedia.org/wiki/Public-key_cryptography):

+	__Private key, x__<br>
$x$: ett heltal där $0 < x < q$, denna parameter representerar vår _privata signeringsnyckel_ och väljs ur _den tillåtna mängden_<br>$$x\in Z_x^q$$

+	__Public key, y__<br>
	$y$: är $a^{-s} \,mod \,q$, detta värde är vår [_publika nyckel_](https://en.wikipedia.org/wiki/Public-key_cryptography).<br>
	Vi behöver räkna fram vår _public verification key_: $$y = g{^x} \; (mod\,p)$$

#### Viktigt att tänka på
Vi får inte dela $x$ med någon annan för då kan de signera i vårt namn.
Den __publika nyckeln__ är vad vi delar med oss på Internet. Den gör det möjgligt för andra att [bekräfta]() vår signatur.

#### Källkod: Key Generation
<pre>
// Private Key
// Välj ett slumptal x, sådant att 0 < x < q
//var x int = rand.Intn(q)
var x int = 3

// Public Key
// Räkna fram en public key sådan att y=g^x (mod p)
var y, gx int
gx = int(math.Pow(float64(g), float64(x)))
y = int(math.Mod(float64(gx), float64(p)))
</pre>

-----

### Signering
För att signera ett meddelande, $M$, så behöver vi gå igenom följande steg:

+   Välja ett slumptal $k$ från den tillåtna mängden, sådant att $$0 < k < q$$
+   Vi använder sedan detta $k$ för att beräkna<br>$$r = g{^k}  \; (mod\,p)$$
+   I nästa steg behöver vi konkatenera (slå ihop) meddelandet som skall signeras, $M$, med vårt beräknade värde, $r$, från steg (4).
<br>Värdet $r$ behöver vara representerat som en [bit sträng](www.gp.se).<br><br>Konkateringsnotationen, $||$, betyder allstå att vi slår ihop två tal.<br>
Resultatet av konkateneringen skall slutligen omvandlas till ett heltal med hjälp av [en envägs kryptografisk hash funktion](https://sv.wikipedia.org/wiki/Hashfunktion) sådan att
$H:\\{ 0,1 \\}^*\rightarrow \mathbb $

<!-- \{0,1\}^*\leftarrow \Bbb_q$ <br> -->

<!-- $$e = H(M\,||\,r)$$ -->

+   Det sista steget i signeringen är att beräkna <br>
    $$s = r\, +\, x \cdot e\, (mod\, q)$$   

##### Signaturen
Vår Schnorr Digitala Signatur är nu paret: $(e,s)$

#### Källkod: Signering
<pre>
// Välja ett slumptal r, sådant att 0 < r < q
var r int = rand.Intn(q)
</pre>

-----

### Skicka
Det vi skickar är två saker:

1.  Meddelandet, $M$
2.  Signaturen, $(e,y)$, i den ordningen

På detta sätt har mottagaren av meddelandet vad de behöver
för att __verifiera__ signaturens riktighet.

> Vi försöker alltså inte att dölja meddelandet. Vi vill bara att
mottagaren skall kunna _verifiera_ att det faktiskt är vi som
skickat meddelandet, inte någon oärlig tredje part.

#### Källkod: Skicka
<pre>
// p: ett primtal, ofta representerad med ett 1024 bit tal
var p int64
</pre>

-----

### Verifiering
Säg nu att jag signerat _ett meddelande_, $M$, och skickat detta tillsammans med _min signatur_, $(e,y)$, till ämnad mottagare.

I nästa steg vill i så fall mottagaren, efter att denne mottagit meddelande, __verifiera__ att meddelandet faktiskt kommer från mig.
Tills sin hjälp i verifieringsprocessen har mottagaren alltså:

+   De mottagna meddelandet, $M$
+   Avsändarens signatur, $e, y$
+   De publika värdena: $p,q,a$
+   Min publika nyckel: $v$

Det första mottagaren behöver beräkna är:
$$x' = a{^y}v{^e}\,mod\,p$$

Vi kan förenkla uttrycket. Först genom att byta ut $v$, vår publika nyckel, mot $a{^-s}$ från parameterlistan.

$$x' = a{^y}v{^e} = a{^y}a{^-se}$$

Med enkla 								algebraiska regler kan vi samla exponenterna...

$$x´ = a{^y-se} = a{^r} = x\, mod\, p$$

Kom ihåg att $x$ var det värde vi slog ihop, eller konkatenerade, med meddelandet
och slutligen Hash:ade, i steg (4).

#### Källkod: Verifiering
<pre>
// p: ett primtal, ofta representerad med ett 1024 bit tal
var p int64
</pre>

-----




Det händer väldigt mycket på den här sidan av kryptografi, mycket på grund av
det stora intresset för kryptovalutor som __Bitcoin__.

I hjärtat av dessa kryptovalutor hittar vi idéen om en digital signatur.
Enkelt uttryckt - för att godkänna att något viktigt genomförs - som att flytta
alla mina Bitcoins från mitt koonto över till ditt konto - så kan jag kräva att
min digitala signatur 'signerar'/godkänner denna process.

Den historiska bilden ovan visar händelsen när man i USA signerarade
__The Constitutionen__  - en mycket viktig händelse i USA's historia.

Just på grund av att detta var så viktigt så kan man förstå att politikernas
signaturer krävdes för att legitimera processen med att göra Constitution
allmängiltlig.

I dagsläget lever vi i en digital Internet-orienterad värld. En signatur måste
klara av en mängd olika typer av bedrägerier för att parterna i en process skall
våga lita på att rätt person givit sitt godkännande för att en process genomförs.

-----

<a name="cheeryouup"></a><p></p><br>

### Några ord om matematik och formalism:
Jag har aldrig varit bra på matematik. Tvärtom - jag är en väldigt visuell person som alltid fått kämpa med skolans, i mitt tycke, torra oinspirerande sätt att lära
ut teoretiska ämnen.Om jag skall förstå något vill jag gärna börja med ett helhetsperspektiv och bilder - något som upprättar en känsla av att _det jag lär mig spelar roll i det mänskliga äventyret_.

Matematikundervisningen kan man tycka vad man vill om - jag tycker den är ofattbart primitiv - men om man haft en oinspirerande och onödigt invecklad introduktion av högre matematik (alltså allt ovan enklare funktioner och grundläggande algebra) så vill jag ge rådet: _Håll ut! Ge inte upp!_

För mig är matematik som en ofattbart vass kniv. Ett instrument som skär rakt
ingenom människans närmast spontana behov av att svänga sig med ogentligheter och extremt ogrundade resonemang och representationer -  allt i ett försök att hantera vår komplexa värld (läs gärna [Nicholas Taleb's utmärka bok Black Swan]() för mer om detta).

Med matematik kan vi ta oss fram och utforska denna extremt komplicerade värld, och faktiskt göra riktiga framsteg, värdefulla betraktelser som nästa generation kan nyttja och jobba vidare på.

Så ge inte upp, lär dig ett matematiskt verktyg i taget - ha alltid ett helikopterperspektiv av det Matematiska landskapet när du lär dig nya saker och
kom ihåg att Matematiken hjälper oss människor att "come to grips" med vår hysteriskt komplicerade värld. Sorry for the rant.
