---
layout: post
cover: 'assets/images/osx_wallpaper2.jpg'
title: Go på OS X
date:   2016-02-10 10:28:00
tags: utveckling
subclass: 'post tag-fiction'
categories: 'patrick'
navigation: True
logo: 'assets/images/ghost.png'
---
Go är väldigt kinking kring hur och var det installeras på datorn.
Denna post är mest en minnesanteckning om hur jag installerat Go på min
Mac. Jag postar den här om någon sitter i samma sits.

> Innehållet i detta dokument är i prakten hämtat från
Golang dokumentet "How to Write Go Code". Läs det för utförligare förklaringar.

## Workspaces
Det är viktigt att förstå att Go projektstrukturer bygger på tanken om publik kod
som delas till exempel på Github.

Go vill att dess källkod ligger inom ett sk workspace.
Ett workspace är en katalogstruktur som har följande tre kataloger i roten:

+	__src__ : här lägger man källkoden för de prjokt man skriver, och de
	 organiserat i sk __packages__ och är ofta .git bevakade.
+	__pkg__ : package objekt
+	__bin__ : exekverbara kommandon

När man skrivit sin kod så bygger go verktyget källkoden till packages
och installerar de resulterande binära filerna i _pkg_ och _bin_.

Workspace katalogen innehåller projekt i form av __repositories__.
Varje repo innehåller i sin tur __commands__ och __libraries__.
<pre>
bin/
	hello #command object
	outyet 								#command object
pkg/
	linux_amd64/
		github.com/pkothbauer/example/
			stringutil.a 				#package object
src/
	github.com/pkothbauer/example
		.git/							#git metadata
		hello/
			hello.go 					#command source
		outyet/
			main.go 					#command source
			main_test.go
		stringutil/
			reversestring.go 			#package source
			reversestring_test.go 		#test source
</pre>

### Ditt Workspace rymmer flera projekt
Det vanliga är att flera olika projekt (repositories) under ett och samma workspace.
De flesta Go programmerar bevarar all sin go källkod och dependencies i ett enda workspace.

## GOPATH
GOPATH variabeln talar helt enkelt om var någonstans ditt workspace finns.
Det är den enda systemvariabeln som behöver sättas för att programmera i Go.

En vanlig plats att lägga ditt workspace är direkt under HOME katalogen,
$ export GOPATH=$HOME

Så här sätter du GOPATH:

<pre>
$ mkdir $HOME/workspace
$ export GOPATH=$HOME/workspace
</pre>

> Notera att ditt workspace __inte__ skall vara lokaliserat på samma path som din Go installation.

Av praktiska skäl (så att du direkt kan exikvera program som ligger här) kan det vara käckt att lägga till din go bin katalog till din PATH.

<pre>$ export PATH=$PATH:$GOPATH/bin</pre>

## Package paths
Din källkod förvarar du som sagt i egna packages (projektpaket).
Det är viktigt att välja en base path som inte riskerar att krocka med Go's egna packages som tex "fmt" osv.

Många väljer sitt Github-konto som base path. Kom ihåg att du inte behöver publicera
den kod du lägger här - men det hjälper att organisera koden på ett bra sätt.

Så, skapa en katalog i ditt workspace i vilket du förvarar dina projekts källkod.
<pre>mkdir -p $GOPATH/src/github.com/patrickbergden</pre>

## Skapa, Kompilera och Kör ditt program
När du vill skapa ett nytt program gör du typiskt så här:

1.	Välj ett namn på ditt package, tex hello
2. 	Skapa katalogen inom ditt workspace  
  	`$ mkdir $GOPATH/src/github.com/patrickbergden/hello`
3.	Skapa nu en hello.go fil som kommer att innehålla din källkod.
<pre>
package main
import "fmt"
func main(){
	fmt.Println("Hello!")
}
</pre>
4.	Nu kan vi bygga och installera programmet med
	hjälp av go verktyget:  
	`$ go install github.com/patrickbergden/hello`
	(Har man definerat GOPATH så listar go verktyget
	ut var koden ligger i vårt workspace.

	Det betyder att vi kan köra koden oavsett var
	vi befinner oss i systemet).
	Detta procducerar en exekverbar binär fil som
	placeras under $GOPATH/workspace/bin under namnet
	hello.
5. 	Vi kan nu sluligen köra den exekverbara filen
	med kommandot $GOPATH/bin/hello  
	($GOPATH är alltså = $HOME/workspace)  
	(eller om vi har lagt till $GOPATH/bin så kan
	vi direkt köra $ hello)

## Source control
I detta läge kan det vara käckt att använda
source control.
<pre>
cd $GOPATH/src/github.com/patrickbergden/hello  
git init
git add hello.go
git commit -m "initial commmit"
</pre>


## Libraries
Kod som vi ofta vill använda i olika projekt läggs
lämligen i egna separata bibliotek (library).

Första steget är att get biblioteket ett namn.  
`$ mkdir $GOPATH/src/github.com/patrickbergden/strutil`

Nu skapar vi en fil som vi döper till reverse.go.

<pre>
package strutil

func Echo(s string) string {
	return s
}
</pre>

Nu kan vi bygga packaget med  
`go build github.com/patrickbergden/strutil`  

Om vi använder install istället så får vi ett package
som läggs under $GOPATH/pkg.

Nu kan vi anropa biblioteket i annan kod.
<pre>
package main
import (
	"fmt"
	"github.com/strutil"
)
func main() {
	fmt.Println("echo " + strutil.Echo("echo"))
}
</pre>

Obs, `go tool` kommer automatiskt att se om vi
importerar bibliotek och även installera dessa.

Kompileras detta får vi en strutil.a fil som läggs under   $GOPATH/pkg/darwin_amd64/github.com/patrickbergden/

### Något om package namn
Konventionen är att ett packages namn är detta sista
namnet i dess sökväg / import väg.
Importeras packaget som "crypto/rot13" så förväntas
packaget heta rot13.

Exekverbara filer måste alltid använda "package main".

Packages behöver inte ha unika namn inom vårt
workspace men path:en måste vara unik.


## Testing
Har vi en fil som heter hello.go så kan vi testa
den genom att skapa en fil vi kallar hello_test.go.
Vi lägger alltså helt enkelt till _test.go.

I hello_test.go använder vi Go's interna testramverk
och dess funktion `func (t *testing.T)`.
Det gör vi genom att: `import "testing"`.

Vi testar funktioner genom att skapa en funktion
och döpa den till TestXXX, där XXX är namnet på
den fil vi vill testa.

Om en sådan funktion anropar t.Error eller t.Fail
så har testet misslyckats.

Nedan lägger vi till ett test till vårt strutil
bibliotek.

<pre>
package strutil

import "testing"

func TestEcho(t *testing.T) {
	cases := []struct {
		in, want string
	}{
		{"Hello", "HelloHello"},
		{"123", "123123"},
		{"",""},
	}
	for _, c := range cases {
		got := Echo(c.in)
		if got != c.want {
			t.Error("Echo(%q) == %q, want %q", c.in, got, c.want)
		}
	}
}
</pre>

Vi testar sedan genom att köra:  
`$ go test github.com/patrickbergden/strutil`

(Kör man testet direkt i package katalogen räcker
det självklart med att skriva go test).

## Remote packages
Det sista är packages som till exempel ligger på
Github. Vi kan hämta packaget genom kommandot  
`go get`

`$ go get github.com/patrickbergden/hello`
