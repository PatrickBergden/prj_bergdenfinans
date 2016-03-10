---
layout: post
cover: 'assets/images/cover8c.png'
title: Positionsdimensionering & <br> Anti Martingale Strategier
date:   2016-01-01 10:18:00
tags: trading
subclass: 'post tag-fiction'
categories: 'patrick'
navigation: True
logo: 'assets/images/ghost.png'
---

När jag började med trading gav någon mig rådet att istället direkt spola ner
mitt tradingkapital på toaletten - då skulle jag åtminstonne ha tiden
kvar till annat.

Och visst, trading är svårt - det märker man snabbt.
Ibland kan det kännas som att hela marknaden konspirerar för att ta just dina pengar. Det är helt klart en sink-or-swim-miljö, med många traders på havets botten.

En sak jag tidigt kände mig väldigt osäker kring, var hur stora positioner
jag skulle ta. I början sköt jag från höften och valde positionsstorlek på magkänsla. Newsflash: magkänsla och framgångsrik trading är ingen hit!

Med tiden började jag inse att skärpa inom området risk management var avgörande för om jag skulle ha en chans att hamna på rätt sida om den statistik som säger att 80-90% av alla traders misslyckas.

Så för någon vecka sedan bestämde jag mig för att lära mig hur man kan tänka kring positionsstorlekar i trading.
Resultatet är denna post.

Om du som läser detta blir hjälpt av innehållet, så blir jag naturligtvis glad. Tveka inte att skicka en rad om du har frågor eller upptäcker något som är fel eller kan förbättras. Jag är grymt tacksam för all feedback.

Obs 1, för att hålla beräkningarna enkla har jag valt att bortse från spreads, slippage och courtage-kostnader.

Obs 2, innehållet i denna post skall inte tas som rekomendationer.
Var och en får bilda sig sin egen uppfattning om hur de bäst vill förvalta och riskera sitt kapital.

-----

## Positionsdimensionering
Okay så ett vanligare namn för detta område är Position Sizing, men jag väljer
här att använda mig av positionsdimensionering som term för:

1. Att bestämma storleken på det kapital man väljer att riskera när man öppnar en position.
2. Att tänka kring faktorer som står i relation till positionsstorleken och hur vi kan ta hänsyn till dessa för att försöka gynsamt påverka utsikterna för en planerad trade.

Med andra ord, när du läser positionsdimension så tänk - hur mycket kronor
av den totala tradingdepån man väljer att riskera i en viss trade.

-----

## Martingale Strategi
Enkelt uttryckt så går denna strategi ut på att man dubblar sin risk varje gång
man förlorar.

Låt säga att vi spelar Roulette.
Om vi satar 1kr på rött och förlorar, så säger Martingale strategin att vi i nästa snurr skall dubbla vår risk och alltså satsa 2 kr.
Förlorar vi även denna gång så skall vi vid nästa snurr satsa dubbelt igen, alltså 4 kr.

Tanken är att när vi väl vinner så vinner vi tillbaks de tidigare förlusterna.
Problemet är främst att om vi har otur så hinner vi bränna vårt konto innan
kulan hamnar på vår satsade färg.

Precis som i roulette, så är det i trading omöjligt att säga hur många förlorande sviter vi kommer att innan vi får rätt (kursen går i vår önskade riktning).

Det är åtminstonne en anledning till att Martingale strategin kanske inte är supergrym för någon som vill förbättra sin risk management i trading.
Men det finns ett besläktat alternativ.

## Anti Martingale Strategi
Denna strategi föreslår att vi istället ökar vår risk så länge vi vinner och
kursen går i vår önskade riktning.

Observera att vi inte uttryckligen säger att man måste dubble in risk.
Det räcker med att öka risken för att följa strategin.

I Roulette-exempelet ovan så blir alltså skillnaden att vi __ökar__ vår risk
varje gång vi vinner. Exempel, satsar vi 1 krona och vinner så kan vi välja att
satsa 1.5kr vid nästa snurr.

På detta vis minskar risken avseevärt för att en förlorande svit skall äta upp
allt vårt kapital innan vi vinner igen.


## R som i Risk

När jag tar en position innebär det att jag tittar på min tradingdepå och
avgör hur stor del av denna (i procent eller kronor) som jag vill riskera när jag
öppnar positionen.

Om jag handlar 50st aktier som kostar 100 kr styck, så blir min R (risk) 5000kr (100kr*50=5000kr).

## R-multipler
Istället för att tala om profiter och förluster i kronor, så kan vi nu tala om R-multipler.

Om aktien ovan går upp och blir värd 400kr styck så kan jag istället för att tala om en profit på 50*(400-100)=15.000 kr, istället
betrakta det som att jag har en 3R profit (15.000/5.000=3).

Fördelen är att vi nu kan prata i största allmänhet om profit och förlust i form av R-Multipler.


<pre>Exempel
Om R motsvarar 5 investerade kronor och marknaden går upp 30 kr då har du gjort en förtjänst på 6R (30/5=6).
Om R motsvarar 6 investerade kronor och marknaden går ner 60 kr då har du
en gjort en förlust på 10R (60/6=10).
</pre>


Vi kan uttrycka förluster och förtjänster i form av R.
Köper vi 100 st aktier som kostar 10 kr styck så är risken 1000 kr.
Om vi senare säljer dessa aktier och gör en förtjänst på 40 000 kr så
kan vi säga att vi gjort en 40000/1000=40, 40R förtjänst.

Exempel: vi köper 100st FING aktier för 400 kr = 40 000 kr och sätter en stopp loss
på 2% - det vill säga att vi (om vi är long) placerar en stop limit order på 392 kr.
I detta fall är alltså R = 400 * 100 - 392 * 100 = 800 kr.

Du riskar 700 kr (R) och gör senare en profit på 2800 kr, detta motsvarar då en 4R profit (2800/700=4).

Du riskar 5000 kr (R) och gör senare en förlust på 2500 kr, detta motsvarar då en 0.5R (2500/5000=0.5) förlust.

Du riskar 50 kr per aktie och gör en
förlust på 100 kr per aktie, detta motsvarar då en 2R förlust (100/50=2).

Målsättningen är att ha stora R-Multiple profiter och max 1R förluster.



#### Beräkna R-Multipler vid aktieköp

1. Först räknar jag ut hur mycket risk jag lägger.  
Detta blir min 1R.
2. Sedan multiplicerar jag 1R med antal aktier.
3. Nu räknar jag ut total profit (eller förlust) inklusive courtage.
4. Nu delar jag det värdet jag räknade ut i föregående steg med den initiala risken.
5. Till sist får jag R-Multipeln som en vinst eller förlust.

#### Hur många aktier skall jag köpa?
Säg att jag handlar FING vid $23 per aktie, med en stopp loss på 25%.  

Det betyder att stop lossen är placerad vid:  
$23*0.25 = $5.75
$23-5.75 = $17,25 per aktie.  

Om jag nu väljer att riska 1% av mitt totala kapital, vi säger att det är 100 000 kr, så innebär det att jag skall handla $1000/5.75=174 FING-aktier till ett pris på $23, och en total summa i cash på $4 000 kr.  

Om aktien går ner 25% från $23 till
$17,25 så säljer vi.

#### Vad blir R-Multipeln vid en profit?  
Om jag köpte 174 aktier för $23 och senare säljer dessa för $38 gör jag en
profit på $38*174 - $23*174 =  $2610.  
Nu delar jag denna summa med min risk för att få min R-Multipel:
$2610/$1000=2.61R profit.




## Expectancy
Expectancy är ett mått på förväntat resultat i ett trading system.
Formeln är: Summa R-Multiples / Number of trades.

[1R, -2R, 4R, 1.1R, 3R] / 5 = 1.42 R

Det skulle betyda att varje investering som använder vårt tradingsystem bör
ge oss i genomsnitt 1.42 ggr profit på satsat kapital.






-----




<!-- ### Strategier för att avgöra storleken på Global Positioner -->





##Kapitalmodeller
Så, nu är frågan - hur mycket kaptial skall jag satsa när jag bestämt mig för att
ta en position? Här är tre modeller.

### 1. Totalt Kapital

En enkel men antaligen för aggresiv modell är att helt enkelt satsa en fix procent av totalt kapital vid varje trade.

<pre>
<mark>Exempel Total Kapital</mark>
Säg att jag har ett totalt kapital på 100 000 kr.
Låt oss även säga att jag bestämt mig för att alltid
riska 1% i mina trades.

Om jag nu vill ta en position i FING så betyder det
att jag skall köpa FING för 100 000*0.01 = 1 000 kr.

Senare på eftermiddagen ser jag att ERICA rör sig i den
riktning jag hoppats på och öppnar däför ytterligare en
postion.

Återigen riskar jag 1% av mitt totala kapital och öppnar
en position till ett värde av 1 000 kr.

Nu har jag två aktiva positioner med en sammanlagd risk
på 2000 kr och 100 000 - 2000 = 98 000 kr i cash.
</pre>


Med andra ord - jag tar inte hänsyn till hur det går för mina öppna positioner när jag positionsdimensionerar.


Med tanke på att många trades blir förluster är detta
som sagt en ganska aggresiv modell.


### 2. Core Kapital

I denna modell bestämmer vi storleken på positionerna utifrån faktiskt
tillgänglig cash i vårt totala kapital.
Vi tar inte hänsyn till öppna positioner förräns de stängts.

<pre>
<mark>Exempel Core Kapital</mark>

Totalt kapital: 100 000 kr  
Vi öppnar en position på 1% av det totala kapitalet:
100 000 * 0.01 = 1 000 kr  

När vi tagit vår position är tillgänglig cash:  
100 000 - 1 000 kr = 99 000 kr  

Nu öppnar vi ytterligare en position.
Även denna gång riskar vi 1% av totalt kapital.
Men denna gång blir det:
99 000 * 0.01 = 990 kr

När vi tagit vår andra position är tillgänglig cash:  
99 000 - 990 kr = 98 010 kr.
</pre>

Att tänka på - vi betraktar våra öppna positioner som förlorat kapital fram tills dess att vi stängt positionen (till vinst eller förlust).

Det kan vara ett bra sätt att tänka kring placeringar och tillgängligt kapital.
Så vi antar helt enkelt att det kapital som investerats i olika positioenr
är förlorat fram till dess att vi stängt positionen.


### 3. Reducerat Totalt Kapital
Även denna model bygger på tillgänglig cash, men tar hänsyn till de pengar vi låser in när vi flyttar våra stopp lossar.

I likhet med Core modellen räknar vi kapital i öppna
possitioner som förlorade - med till skillnad från Core så blir dessa pengar återigen en del av vårt totala kapital om/när vi flyttar våra stopp lossar så att risken blir starkt minimerad.

<pre><mark>Exempel Reducerat Totalt Kapital</mark>  
Säg att jag börjar med ett totalt kapital på 100 000 kr.
Jag tar en position på 1% av detta totala kapital,
dvs 1 000 kr.  

Säg nu att aktiekursen börjar gå min väg.
Jag sätter i så fall en stopp loss som gör att jag
går ur om jag förlorar 500 kr
(tex 100 st aktier för 1 kr med en stopp loss
på 0.50 kr).  

Positionens storlek: 1000 kr
Stopp lossen är satt vid 500 kr, det vill säga att jag
säljer om positionens värde går ner till 500 kr.  

10 st aktier till priset av 100 kr = 1000 kr
Priset går ner till 50 kr -> 10*50 kr = 500 kr
</pre>

I Core hade vi räknat total kapital till 99 000 kr
I Core blir det istället 99500 kr




-----
## Skala in positioner med Anti Martingale Strategin
Strategin är enkel:

+   När en aktie går upp 25% från det pris vi köpte den för
så öppnar vi ytterligare en position i samma aktie.
+   Vi riskar då ytterligare risk, vars storlek då styrs av vilken kapitalmodell vi föredrar (se ovan).
+   Vi gör detta upp till fyra gånger.

<pre>
<mark>Exempel Skala in Position</mark>  
Säg att jag har ett Total Kapital på 100 000 kr.  

Jag bestämer mig för att riska 1% av detta totala
kapital, alltså 1 000 kr.

Jag väljer att följa Core Kapitalmodellen när jag
beräknar hur mycket jag skall riska i nästföljande positioner.  

Min trailing stopp loss sätter jag på 25% - alltså
en bred stopp.

Jag börjar med att köpa FING när den kostar 460 kr
per aktie.

Det betyder att jag skall köpa:
1000/(460*0.25) = 8 aktier
till ett pris på 8*460 = 3680 kr

Säg nu att aktiens pris ökar med 25% upp till
460 * 1.25 = 587,50 kr.
Jag öppnar (skalar in) ytterligare en 1% position.  

Då jag valt att använda Core Kapitalmodellen så
skall jag köpa:  
100 000 - 1000 = 99 000 kr
99 000 * 0.01 = 990 kr  
990 / (587,50 * 0.25) = 6.74 ≈ 6 aktier,
för en totalsumma på 6 * 587,50 ≈ 3525 kr
</pre>

<a href="https://www.tradingview.com/x/nCIAgnlQ"><img src="/assets/images/fing_03.png" ></img></a>




| Totalt Kapital    | Pris          | Aktier  | Värde  |
| -------------     |:-------------:| -----:|-----:|
| 100 000           | 23            | $1600 | $1600|
| 99 000            | 45            |   $12 | $1600|
| 98 100            | 57            |    $1 | $1600|


<!-- <img src="/assets/images/400.gif"><br> -->
<!-- <img src="/assets/images/testimg1.jpeg"> -->
<br>


-----

## Stop the Loss

### Tight Stop Loss
Det finns fördelar  och nackdelar med en tight stop.

#### Tight Stop uttryckt i Procent
Jag tar 5% som exempel på en tight stop i uttryckt i procent.

Om jag vill riska 5000 kr - säg att jag har ett total kapital på 500 000 kr - för at köpa AZN @ 550kr så skall jag köpa:
<pre>5000 / (550*0.05) = 181,82 ≈ 181 aktier</pre>

Det betyder också att om priset på AZN går ner 5% till 522,50 kr så kommer jag att stoppas ur och har då gjort en förlust på 5 000 kr.

<a href="https://www.tradingview.com/x/zxvm2VHr/"><img src="/assets/images/azn_tightstop_01.png" ></img></a>

#### Tight Stop uttryckt i Points
> <a href="http://www.investopedia.com/ask/answers/04/043004.asp#ixzz3yu4hVqso">För aktier gäller allmänt att</a>: For stocks, one point equals one dollar. So when you hear that a stock has lost or gained X number of "points", this is the same as saying that the stock has lost or gained X number of dollars.

En tight stopp loss uttryckt i points skulle kunna vara 2 points - alltså $2.

<pre>Om jag vill riska $1000 med en $2 stopp så skall jag
alltså köpa $1000/2 = 500 aktier.
</pre>

#### Nackdelar med en tight stopp loss

En tight stopp innebär att jag kommer att stoppas ur rätt ofta. Av den anledningen är det viktigt att vara försiktig när intradag voltilitet är hög.

En annan nackdel är att jag kommer att behöva köpa mer
aktier när jag har en tight stop och på så vis binda upp mer av mitt kapital.

<pre>
En stopp loss på 5% innebär att jag behöver köpa  
1R/0.05 = 20R i aktier.

Med en 25% stopp loss behöver jag bara köpa  
1R/0.25 =  4R i aktier.

I fallet med Astra Zeneca aktierna skulle det innebära att
jag handlar
5000 / (550*0.25) = 36,36 ≈ 36 aktier
till en kostnad på 36*550 = 19 800 kr.

Jämför detta med 181 aktier
till en kostnad på 181*550 = 99 550 kr, eller 20%
av vårt totala kapital på 100 000 kr.
</pre>

#### Fördelar med en tight stopp loss

Det är enkelare att få en hög R-Multipel när vi använder en tight stopp. Får vi en förlust så är även denna relativt liten vid en tight stopp loss.


> <a href="http://www.investopedia.com/terms/s/slippage.asp#ixzz3yuIfAWgh">Slippage</a>: The difference between the expected price of a trade, and the price the trade actually executes at. Slippage often occurs during periods of higher volatility, when market orders are used, and also when large orders are executed when there may not be enough interest at the desired price level to maintain the expected price of trade.

### Bred Stop Loss
En bred stopp skulle kunna vara en 25% trailing stopp
eller till exempel en 100 kr stop på AZN.

#### Fördelar och nackdelar med en bred stopp
+   Jag riskerar inte att stoppas ut lika ofta.
+   Jag behöver heller inte binda upp så mycket kapital
eftersom att jag kan köpa en mindre mängd aktier.
+   Jag kan uppleva det som att kursen oftare rör sig i enlighet med min TA eftersom att jag inte stoppas ut på grund av intradagsvolatiliteten i aktien.

Nackdelen är att jag får en mindre R-Multipel - kursen
måste rör sig oerhört mycket för att jag skall kunna få en hög R-Multipel.

-----

### Volatilitetsbaserad Positionsdimensionering
Hur kan vi ta vara på fördelar från både en tight stop och en bred stop?
En möjlighet är att välja storleken på stop lossen utifrån aktiens volatilitet.

Om vi helt enkelt tar reda på de senaste dagarnas volatilitet i aktien, så kan
vi använda denna för vår positionsdimensionering.

Det kan dessutom vara smart att multiplicera denna volatilitet med en faktor så att vi håller oss utanför rangen och sänker risken för att stoppas ut.

Om TLSN de senaste 5 dagarna i genomsnitt haft en range på 10 kr, så kan jag
till exempel välja att sätta min stop loss 2 ggr detta värde under det pris jag väljer att ta en position.

<pre>
Exempel

Säg att den genomsnitliga rangen de senaste tre dagarna
i FING är 20 kr.  

Istället för att som tidigare köpa 1000/2 = 75 aktier
så köper vi nu istället:
1 000 / 50 = 20 aktier  

Jag sätter stopp lossen på 20 kr * 2 = 40 kr
För ett aktuellt pris på 550 kr så blir det 550 - 40 = 510 kr.
</pre>

##### Här är ett liknande exempel med Telia Sonera (priset i Euro)

I första grafen ser vi att priset ser ut att börja gå sidleds mellan 0.5 och
full retrace på fibben. Jag söker därför att korta aktien nästa gång den är på
väg upp för att testa resistance nivån (.5 fib nivån).

<a href="https://www.tradingview.com/x/CRFbZojd/"><img src="/assets/images/tlsn_01.png" ></img></a>

Aktien studsar som väntat mot resistance nivån (fib 0.5).
Molnet som omger priset representerar de fem senaste dagarnas genomsnittliga range, multiplicerat med den stop loss risk jag vill ha - i detta fall 2 ggr genomsnittlig rangen.

<pre>
Exempel
Om priset på en aktie i varierat med 10 kr per dag </pre>

Som man kan se når inte priset ända ner till support, vilket påminner om vikten
av att ta profit på vägen mot target.

<a href="https://www.tradingview.com/x/7ACyqsAl/"><img src="/assets/images/tlsn_02.png" ></img></a>

#### Hur lönsam blir denna trade?

Genom att använda R-Multipel-konceptet kan vi enkelt generalisera diskussioner
kring hur profitabla våra trades är.

<pre>
Vad blir det för R-Multipel?

Köppris: €48.79  
Stopp loss vid: €50.85 (€2.06 eller 4.2% ovanför 0.5 fibben)  
1R är allstå: €2.06  
Target: €40.69  
R-Multipel: (48.79 - 40.69) / 2.06 ≈ 3,9R  
</pre>

Det betyder alltså att denna trade ger ca 3.9 gånger satsat kapital, förutsat
att volymerna är tillräckligt stora för att jag skall få mina ordrar fyllda.

Volatilitetsbaserad positionsdimensionering känns spontant ganska användbart,
det finns helt klart många olika intressant strategier att koda fram och testa.

-----






### Procentrisk Positionsdimensionering
En av de vanligaste Anti Martingale Strategierna.

### Optimal Risk


## Högre R/R
