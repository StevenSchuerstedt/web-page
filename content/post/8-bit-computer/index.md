---
title: 8-Bit Computer
author: admin
type: page
date: 2020-04-18T14:52:38+00:00
featured_image: "8bit.jpg"

---
Der Computer ist wohl unbestreitbar eine der einflussreichsten Erfindungen der Menschheit. Zum Zeitpunkt der Erstellung dieses Artikels wird der Computer in allen Lebensbereichen verwendet und bietet eine enorme Hilfe um Tätigkeiten zu automatisieren und zu digitalisieren, bzw. durch den Computer erhalten diese Begriffe überhaupt eine Bedeutung. Bei meiner täglichen Arbeit mit dem Computer stellt sich mir die Frage, wie dieses Gerät genau funktioniert. Ich gehöre zu einer Generation in der die Computertechnologie schon seit meiner Kindheit weit fortgeschritten war und ich den Entwicklungsprozess des Computers, von mechanischen Rechenmaschinen zu winzigen Mikroprozessoren nicht verfolgen konnte. Für mich war der Computer einfach so da und Veränderungen gab es nur in der Schnelligkeit des Rechners und vor allem mit dem Internet. 

Um einen Einblick in die Geschichte des Computers und vor allem in seine Funktionsweise zu erhalten, entschloss ich mich einen eigenen Computer zu bauen. Auf youtube findet sich eine exzellente Videoserie von Ben Eater, die Elektronik-Laien wie ich mich Schritt für Schritt durch den Irrgarten der Kabelverbindungen leitet. Welch bemerkenswertes Zeitalter, in dem mich der Computer mit dem Internet befähigt, ihn selbst zu verstehen. 

In diesem Artikel baue ich einen 8-Bit Computer komplett auf Breadboards, basierend auf dem SAP-1 (simple as possible) Modell aus Digital Computer Electronics von Albert Paul Malvino.

Wichtigstes Bauteil für einen modernen Computer ist der Transistor, der im Prinzip ein stromgesteuerter Schalter ist. Mit diesen einfach Bauteil lassen sich kombinatorische und sequentielle Logik Verknüpfungen bauen. Kombinatorische Logik sind Logik-Verknüpfungen die nur von den aktuellen Input abhängen. So z.B. UND, ODER, NICHT Gatter. Mit dieser Art von Logik lassen sich alle Decision-Making Bauteile des Computers bauen, so vor allem die ALU. Sequentielle Logik bedeutet das der output auch vom vergangen input abhängt. Sequentielle Logik hat somit ein Gedächtnis und damit lassen sich z.B. Flip-Flops bauen, die ein Bit speichern können. Diese Art von Logik lässt sich auch mit einfach Logik Gattern wie UND ODER darstellen, aber wird der Output wieder zum Input zurück verknüpft. Durch diese Schleife ist es dann möglich Information zu speichern.

Interessant ist auch der Gedanke das ein Computer prinzipiell völlig unabhängig von der Elektronik funktioniert. Die Idee des Computers, also die einzelnen Logik-Gatter oder Flip-Flops haben erstmal nichts mit Strom oder ähnlichen zu tun, sondern sind einfach abstrakte Ideen die sich mit allen möglichen Dingen praktisch umsetzten lassen. Die ersten Computer wurden komplett mechanisch konstruiert (siehe Baggage Differenzmaschine) und es ist z.B. einfach möglich aus LEGO Steinen ein ODER Gatter zu konstruieren. Die Frage ist immer wie der Input/Output interpretiert werden soll, also wie 1 und 0 dargestellt wird. In der Elektronik ist es Strom an/Strom aus, in anderen Bereichen können andere Dinge dafür herangezogen werden. Klar ist auch das sich die Elektronik und Strom als praktische Umsetzung für die Idee des Computers durchgesetzt hat, da es enorm viele Vorteile gegenüber anderer Techniken gibt. Vor allem die extreme Verkleinerung der Bauteile mit einer gleichzeitig enormen Geschwindigkeit ermöglicht den Computer seine volle Macht erst zu entfalten. <figure class="wp-block-image size-large">


![8bit](/8bit.jpg)
![BUS](/bus.png)
## Die Clock / Taktsignal

Die Clock gibt allen anderen Komponenten des Computers ein periodische Signal weiter, so das alle Aufgaben entsprechend synchronisiert ablaufen können. Viele ICs haben ein extra Pin für ein Clock Signal, das dann ein bestimmtes Verhalten auslöst. 

## Die Register

Als nächstes geht es um das Register, welches ein Speicher Element im Computer darstellt. Für diesen Computer werden insgesamt drei 8-Bit Register verwendet, das heißt Register die jeweilst 8 mal ein Bit speichern können. 

Alle Speicherbausteine für den Computer basieren auch auf Logischen Gattern, bei denen aber Eingang und Ausgang miteinander verbunden werden.

### Die ALU

ALU bedeuted Arithmetic Logic Control, also arithmetische und logische Operationen. Die ALU stellt die eigentlichen Operationen dar, die der Computer auf den Daten ausüben kann. Dafür werden die Daten zuerst in die Register geladen, die eine Art Zwischenspeicher darstellen. Die eigentliche ALU führt dann eine Operation wie Addieren, logische Und, usw. aus.

Für diesen Computer recht eine einfache ALU die nur Addieren und Subtrahieren kann.
### RAM &#8211; random access memory

Der RAM ist ein weiterer Speicherbaustein im 8-Bit Computer. RAM ist ein flüchtiger Speicher der für den eigentlichen Programmablauf benützt wird. In RAM wird das eigentliche Programm abgespeichert. Im Prinzip ist der RAM genauso wie ein Register aufgebaut, er bietet aber wesentlich mehr Speicher. Man unterscheidet zwischen dynamischen und statischen RAM, dieser Computer benützt statischen RAM.

Der RAM hat eine 4-Bit Adresse und jede Adresszeile hat einen 8-Bit Inhalt, also hat der Computer insgesamt 64-Bit RAM aufgeteilt in 16 mal 8 Speicherzellen.
### Program Counter

Der Program Counter ist ein einfacher Binärzähler der von 0 bis 15 Zählt und so angibt bei welchen Befehl der Computer sich gerade befindet.

### Output Display

Bisher sind alle Zahlen im 8-Bit Computer nur als Binärzahlen repräsentiert. Für das Output Display sollen die Binärzahlen nun zu Dezimalzahlen konvertiert werden um so einfacher lesbar zu sein.

Dafür wird ein 7-segment Display verwendet. 
### Control Logic

Alle benötigten Module sind abgeschlossen und nun stellt sich die Frage wie die einzelnen Module automatisch angesteuert werden um ein Programm auszuführen. Jedes Modul besitzt verschiedene Funktionen, die durch Signale kontrolliert werde können. So z.B. ein HALT Signal für die Clock (die Clock wird angehalten), ein Out Signal für das A Register (der Inhalt des A Registers wird auf den Bus geschrieben), usw. Diese Funktionen jedes Moduls sind die Basisbausteine mit denen die Control Logic operieren kann. Die Signale werden abgekürzt (HLT für HALT, AO für output Register A,..) und auf einem Breadboard zusammengefasst.<figure class="wp-block-image size-large is-resized">



|  HLT | MI | RI | RO | IO | II | AI | AO |
|:------:|:------:|:------:|:-------:|:------:|:------:|:------:|:------:|
|   Halte den Computer an   |   Memory Address Register in   |   RAM in  | RAM out | Instruction Register out | Instruction Register in | A Register in | A Register out

|  Sum | SU | BI | OI | CE | CO | J | FI |
|:------:|:------:|:------:|:-------:|:------:|:------:|:------:|:------:|
|   Sum Register out   |   Subtract   |   B Register in | Output Register in | Counter enable | Counter out | Counter in | Flags Register in


Aus diesen Microinstructions werden dann verschiedene Macroinstructions gebaut, also hier schon eine erste Abstraktionsebene. Die Macroinstructions bekommen dann auch Namen bzw. Abkürzungen, sog. Mnecmonics, die als Gedächtnishilfe zu verstehen sind, damit man sich nicht irgendwelche Binärcodes merken muss. Dies ist dann quasi auch die Programmiersprache von Assembler für diesen Computer.

Wenn der Computer ein Programm ausführt, wird dabei zwischen dem Fetch Cycle und dem Execution Cycle unterschieden. Das eigentliche Programm befindet sich vor Programm Start im RAM. Im Fetch Cycle wird die erste Instruction aus dem RAM in das Instruction Register übergeben. Die Reihenfolge der Instruction wird durch den Program Counter angegeben. Der Program Counter ist dabei quasi ein Pointer, der zur aktuellen Speicheradresse im RAM zeigt. Das heißt der Inhalt des Program Counters muss in das Memory Address Register geladen werden um den richtigen Inhalt des RAMs auszuwählen, und dann der Inhalt des RAMs in das Instruction Register geladen werden.

Die Control Words für den Fetch Cycle sehen dabei so aus:

CO, MI

RO, II, CE

die Instruction befindet sich jetzt im Instruction Register und besteht aus zwei Teilen, dem Instruction Field und dem Address Field.<figure class="wp-block-table is-style-stripes">

|  XXXX | XXXX |
|:------:|:------:|
|  Instruction Field  | Address Field   

Die ersten 4-bit sind die Instruction und die letzten 4-bit die Addresse oder ein Parameter.

Im Instruction Field befindet sich der Op-Code für die eigentliche Instruction die der Computer ausführen soll, im Address Field befindet sich ein Pointer auf einen bestimmten Datensatz im RAM. Das Address Field ist quasi das Argument für die Instruction.

Im Excecution Cycle wird die ins Instruction Register gefetchte Instruction ausgeführt. Da eine Instruction aus mehreren Control Words oder Microinstructions besteht, kann die Instruction nicht als eines verarbeitet werden, sondern muss aufgeteilt werden. Dafür wird ein Ring Counter verwendet, der angibt in welchen Teil der Instruction sich der Computer gerade befindet. 

Das Instruction Field zusammen mit dem Inhalt des Ring Counters dienen als Addresse für das Microcode-EEPROM, das dann das korrekte Control Word ausgibt. So lassen sich dann verschiedene Befehle mithilfe des Ring Counters abarbeiten. Z.b. der LDA Befehl (Load A). Dieser Befehl soll den Wert in der Speicheradresse des Address Fields in das A Register laden. Hierfür wären also folgende Mircoinstructions nötig: 

IO | MI

RO | AI

Diese zwei Zeilen benötigen also zwei Taktdurchgänge. Wie die einzelnen Macroinstructions heißen und was sie genau machen, kann man sich nun selbst überlegen. Im Prinzip sind alle Instructions denkbar solange sie aus einer Kombination von Microinstructions dargestellt werden können.<figure class="wp-block-image size-large is-resized">

<img loading="lazy" src="https://steven.schuerstedt.com/wp-content/uploads/2020/10/controllogic-1024x604.jpg" alt="" class="wp-image-936" width="392" height="231" srcset="https://steven.schuerstedt.com/wp-content/uploads/2020/10/controllogic-1024x604.jpg 1024w, https://steven.schuerstedt.com/wp-content/uploads/2020/10/controllogic-300x177.jpg 300w, https://steven.schuerstedt.com/wp-content/uploads/2020/10/controllogic-768x453.jpg 768w, https://steven.schuerstedt.com/wp-content/uploads/2020/10/controllogic.jpg 1194w" sizes="(max-width: 392px) 100vw, 392px" /> <figcaption>Control Logic</figcaption></figure> 

Auf diese Art und Weise lässt sich nun das komplette Instruction Set des 8-Bit Computers angeben. Die Instructions sind meistens von Ben Eater übernommen, zwei habe ich mir selber ausgedacht. 

![ISA](/ISA.svg)
Das Interessante ist jetzt das der Computer mit diesen Befehlssatz und vor allem mit den conditional jump instructions Turing Vollständig ist. Das heißt mit dem Computer kann eine Turing Maschine simuliert werden und nach der Church-Turing-These ist eine Turing Maschine in der Lage jede intuitiv berechenbare Funktion zu berechnen. Unter intuitiv berechenbare Funktionen versteht man jetzt einfach alle Funktionen oder Algorithmen die auf irgendeine Weise berechnet werden können. Begriffe wie &#8220;berechnen&#8221; lassen sich nicht einfach formalisieren und die These lässt sich so nicht beweisen, deswegen ist es auch nur eine These. Sie wird aber im allgemein als korrekt angesehen und die Menge an Algorithmen die eine Turing-Maschine oder ein Computer (der ja auch eine Turing-Maschine ist) berechnen kann ist offenbar, wie unsere moderne Welt zeigt, sehr groß. 

Das bedeutet mit dem Instruction Set des 8-Bit computers lässe sich nun theoretisch jeder Algorithmus bauen den auch ein moderner Computer ausführen kann, so z.b. Multiplikation, Division, Primzahlen berechnen, Fibonacci-Reihe, etc. Praktisch ist der 8-Bit Computer aber dennoch sehr limitiert, da er nur insgesamt 64-Bit speicher hat. Da eine Instruction 8-Bit lang ist, passen nur insgesamt 16 Befehle in den Speicher, zusätzlich müssen auch noch die Daten mit den gerechnet werden soll in den Speicher passen. Mit dieser geringen Anzahl an möglichen Befehlen lassen sich natürlich keine besonders komplexen Programme zusammenbauen, sondern erst müsste mehr Speicher vorhanden sein. Eine Turing-Maschine ist sogar so definiert das sie unendlich viel Speicher hat, d.h. selbst ein moderner Computer ist genau genommen eigentlich keine Turing-Maschine, er hat zwar mehrere hundert Gigabyte Speicher, aber das ist von unendlich genauso weit entfernt wie die 64-Bit Speicher des 8-Bit Computers. <figure class="wp-block-image size-large">

<img loading="lazy" width="949" height="834" src="https://steven.schuerstedt.com/wp-content/uploads/2020/11/8bit-computer.jpg" alt="" class="wp-image-960" srcset="https://steven.schuerstedt.com/wp-content/uploads/2020/11/8bit-computer.jpg 949w, https://steven.schuerstedt.com/wp-content/uploads/2020/11/8bit-computer-300x264.jpg 300w, https://steven.schuerstedt.com/wp-content/uploads/2020/11/8bit-computer-768x675.jpg 768w" sizes="(max-width: 949px) 100vw, 949px" /> <figcaption>Der finale 8-Bit Computer</figcaption></figure> 

#### Beispiel-Programm in Assembler

Dieses Programm rechnet aus ob eine in Speicheraddresse 14 befindliche Zahl eine Primzahl ist. 

0: LDA 14  
1: SUB 15  
2: JZ 12  
3: JC 5  
4: JMP 1  
5: LDA 15  
6: ADDI 1  
7: STA 15  
8: SUB 14  
9: JZ 11  
10: JMP 0  
11: LDI 1  
12: OUT  
13: HLT  
14: Primzahl?  
15: 2

#### Gedanken

Moderne Mikroprozessoren sind genau wie der 8-Bit Computer instruction set processors (ISPs), d.h alle Funktionalität wird vom instruction set abgebildet. Das wiederum bedeutet jedes Programm das ich auf ein solchen Computer schreibe muss letztendlich in das entsprechende Instruction Set abgebildet werden. Die Programmiersprache stellt damit nur eine Abstraktionsebene da, in der es sich leichter und effizienter Programmieren lässt, sie hat aber nicht mehr Möglichkeiten oder Funktionen wie die einfachen Befehle des Instruction Set. Ich finde das interessant, da in modernen C++, oder in anderen Sprachen eine Vielzahl komplizierter Programmierkonstrukte existieren, die aber prinzipiell überhaupt nicht benötigt werden, da letztendlich eh alles durch das Instruction Set abgebildet wird. Ich hatte oft das Gefühl um wirklich programmieren zu können und auch komplexeste Programme zu schreiben muss ich eine Sprache wie C++ komplett verstehen und alle diversen Erweiterungen, Vererbungen, virtuelle Methoden, etc. verwenden. Wenn ich komplexen Code verwende schreibe ich auch komplexe Programme, dies ist aber falsch, man muss überhaupt keine sonderbaren Dinge lernen, sondern es reichen schon sehr einfache elementare Methoden um jedes Programm zu schreiben das denkbar ist. Die komplexen Strukturen und Funktionen sollen den Code nicht komplizierter machen, sondern einfacher, kürzer und für einen Programmiere verständlicher. 

Die komplexen Befehle sind nur eine Verschachtelung von einfacheren Befehlen, eine Schrittweise Abstraktion der Sprache und damit auch der Denkweise. Neue Befehle werden aus alten kreiert und beim kompilieren werden die Befehle wieder aufgespaltet und vermehren sich so dass der Computer sie am Ende ausführen kann. Aus Binärcodes wird Assembler, dann Programmiersprachen wie C++ und am Ende grafische Benutzeroberflächen. Meiner Meinung nach stellen grafische Programme, also mit GUI die oberste Ebene der Abstraktion dar. Sie haben einen bestimmten Zweck, sind also sehr spezialisiert, aber dafür auch einfach zu bedienen. Jeder Button press löst eine Vielzahl an Code aus. 

**Der Transistor und Quantenmechanik**

Maßgebend für die Entwicklung und den großen Erfolg des Computers war der Transistor. Als elementares Bauteil ist er millionenfach in modernen Computer verbaut und ohne ihn ist die Geschwindigkeit und die Rechenleistung moderner PCs undenkbar. Der Transistor ist wohl die wichtigste Erfindung der Menschheit im 20. Jahrhundert. Erfunden wurde er im Jahr 1947 von Bell Laboratories in den Vereinigten Staate. Davor benutzen Computer Relais oder Elektronenröhren die um einiges langsamer, unbeständiger und auch viel mehr platz benötigen. Der Transistor dagegen lässt sich um ein unglaubliches verkleinern, wodurch Mikroprozessoren mit einer enormen Geschwindigkeit erst möglich werden. Grundlagen für die Entwicklung des Transistors war die Quantenmechanik. In der Quantenmechanik gab es erste Erklärungen für das Verhalten von Leitern/Halbleitern und Metallen, dies war dann der Anstoß für Experimente mit Halbleitern und dotierten Halbleitern um die Leitungseigenschaften zu verändern. Diese Experimente resultieren schließlich in der Entwicklung des Transistors. So steckt also in jeden technischen Gerät ein Stückchen Quantenmechanik.