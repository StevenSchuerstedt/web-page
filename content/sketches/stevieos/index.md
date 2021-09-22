---
title: StevieSnake – on bare metal
author: admin
type: page
date: 2020-11-26T18:12:08+00:00
draft: false
private: true

---
Nach den 8bit Computer ist die spannende Frage wie die einfachen Prinzipien sich auf ein modernes PC System übertragen. Mein Ziel ist es für ein modernen Prozessor ein einfaches Spiel on bare metal, also quasi ohne jegliche Hilfsmittel zu programmieren. Dabei wird schnell klar das ein modernes System viel komplizierter ist als der 8bit Computer, und auch viel komplizierter als es eigentlich hätte sein müssen. Gründe dafür sind vor allem praktischer Natur, zum einen die strenge Rückwärtskompatibilität und die Zurückhaltung von Informationen. Andererseits kommt zusätzliche Komplexität von allen Komponenten die Benutzt werden, wie Display, Keyboard, etc. 

Zuerst steht die Frage für welche Instruction Set Architectur (ISA) das Spiel programmiert werden soll. Es existieren das weitverbreitete x86 IS von Intel (welches auch von AMD benutzt wird), ARM und modernere, offene IS wie RISC-V und ForwardCom. Da ein modernes User System meist immer das x86 IS benützt entschied ich mich für diese Architektur. Das x86 IS ist unglaublich aufgeblasen und schwerfällig, was die ca. 4000 Seiten Dokumentation zeigt. 

On Bare Metal bedeutet jetzt dass das Spiel ohne jegliches Betriebssystem funktionieren muss, es ist quasi sein eigenes Betriebssystem. D.h. nach dem Start des PCs wird auch sofort das Spiel ausgeführt. Ganz ohne Hilfe geht es aber doch (noch) nicht, das BIOS stellt viele wichtige Funktionen bereit. 

PC-Start -> BIOS/UEFI -> Bootloader -> OS

Ziel: simples Spiel für x86 rechner architektur schreiben. Bios lädt über Bootloader 32 bit speicher bereich für mein spiel, danach steuert programm CPU usw. 

Logik für das Spiel in C / C++ schreiben, Assembler nur da benützen wo es wirklich nötig ist => sprich um die Harware zu steuern. Ausgabe am Bildschirm und Eingabe über Tastatur

=> einfache Treiber für Input / Output entwickeln

ToDo:

  * Keyboard Treiber
  * => eigentliches Spiel

### Bootloader

Firmware, BIOS

1 Byte = 8 Bits (short = 16bit, int = 32 bit, long = 64 bit)

Bootloader muss direkt in Binär geschrieben werden, dafür Hex Editor um angenehmer Binär zu schreiben.

Hexadezimal: höchste Zahl F = 15 = 1111, also braucht eine Ziffer in Hexadezimal maximal 4 Bits. Deswegen die Konvention eine Hexadezimalziffer entspricht genau 4 Bits, also ist Hexadezimal 1 = 0001, statt 01 oder 001. Präfix 0x für Hexadezimalzahl (mehr oder weniger willkürliche Konvention) 

Boot record signature 0x55, 0xAA

in 32 bit Modus wechseln

  * Speicher zugriff mit Segmenten

Problem: bei einem 8 Bit oder 16 Bit oder 32 Bit Computer sind nur 2^32 Adressen für Speicher verfügbar, man möchte aber gerne mehr Speicher verwenden. Als Workaround Segmentierung des Speichers in verschiedene Blöcke. 

Daten von Festplatte laden mit Cylinder-Head-Sector Koordinatensystem.

Interrupts um auf BIOS Funktionalität zuzugreifen (um Hardware anzusprechen). Diese BIOS Funktionen sind nur im 16bit Real Mode verfügbar. 

int 0x10 => ein buchstaben auf den Bildschirm schreiben

int 0x13 => etwas von der Hard Disk lesen

  * Wie funktioniert eine Festplatte? Festplatte mit BIOS ansprechen
  * => Ziel: Global Descriptor Table schreiben, Festlegen welche Segmente für Protected/Paging da sein soll
  * => für dieses Projekt einfachst Mögliche GDT => basic flat model

### Von 16Bit Real Mode zu 32Bit protected Mode

Global Descriptor Table. Speicher wird Segmentiert für Sicherheit. Im GDT ist beschrieben welche Segmente es gibt. Dies wir dann gebraucht für Interrupt Descriptor Table (IDT). Wenn eine Taste auf dem Keyboard gedrückt wird muss die CPU wissen wo im Speicher dieser Interrupt behandelt wird. 

Im Prinzip kann dies einfach deklariert werden, mit einer Startadresse, einer Größe und Flags um was für Speicher es sich handelt und was für Zugriffsrechte es gibt, aber wegen Rückwärtskombatibilität ist der GDT unnötig kompliziert. Wir definieren zwei komplett überlappende Segmente, eines für Code und eines für Daten die genau gleich groß sind, also gibt es auch keine Sicherheit. 

=> x86 Instruction Set ist wirklich bloated, ARM besser?

=> alle diese Dinge sind spezifisch für das x86 Instruction Set 

=> andere Prozessortypen sind wohl einfacher, aber es gibt weniger Dokumentation / Tutorials darüber

=> gleiches Spiel für einen anderen Prozessortyp implementieren? rasberry pi mit rust

Treiber

**Grafik mit Aufbau des RAMS**

Das Bios ist fest gecodet in einem EEPROM auf der CPU. Wenn der Computer startet lädt sich das BIOS in den RAM (weil der viel schneller ist als ein EEPROM). Der Bootloader wird dann auch in den RAM geladen, wie auch der Rest vom OS (oder Spiel). => memory layout of ram

### Codeaufbau

bootloader in assembly. BIOS benützen um mehr als 512 bytes Speicher zu laden, danach Wechsel in 32 bit protected mode (BIOS nicht mehr verfügbar). Kernel mit Treibern, eigentlichen Code in C / C++, da offensichtlich angenehmer zu programmieren. C Code wird ja zu nichts anderem als Assembly/Maschinencode kompiliert. Die Bootloader Datei und der kompilierte Kernel Code müssen dann zu einer zusammenhängenden Datei verschmolzen werden. Also quasi zusammenhängenden Speicherbereich haben. Die main Funktion wird dann als Label definiert, so dass vom Assembly Code in den C Code Bereich gesprungen werden kann. 

### Grundlegende Treiber für Input/Output

Größte Komplexität, meiste Arbeit beim Programmieren ist nicht Kontrollieren des Prozessors, sondern der angeschlossenen Hardware. Zum Beispiel des Monitors. Ansteuerung Monitor über VGA Standard. 

Memory Mapped I/O

CPU hat bestimmten Adressbereich den sie kontrollieren kann, 32 bit hat 2^32 Adressen. Der 6502 hat 16 Address pins. Ein Teil dieser Addressen kontrollieren den RAM, ein anderer Teil werden nicht für den RAM benutzt sonder um I/O devices zu kontrollieren (MMIO). 

Vergleich 6502 Prozessor und x86

=> Im Prinzip das gleiche (?), aber der 6502 ist natürlich wesentlich einfacher, greifbarer und nachvollziehbarer als eine x86 Architektur, mit Motherboard, PCI, USB, usw. 

Beim 6502 sind die Adressen X für das LCD Display reserviert. Wieso? Weil sie so von der Hardware zusammengekabelt sind dass das Display über den Interfacer IC nur dann angesprochen wird wenn die entsprechenden Adressen ausgegeben werden. Dann können Daten in das Register des LCD Moduls gespeichert werden, welches diese dann umwandelt um schließlich etwas auszugeben. 

Wie lässt sich diese einfach Verfahren nun auf einen modernen Monitor übertragen? Ein bestimmter Bereich des GB großen Adressbereichs von x86 ist für den Monitor (mit VGA) reserviert. 

Welche Register hat ein VGA Monitor die kontrolliert werden müssen? Wie finde ich heraus welche Adressen den VGA Registern zugeordnet sind? (wird das vom BIOS erledigt?) => viele Fragen die darauf zurückzuführen sind das jmd anders das System konstruiert, es lange gewachsen ist, im Gegensatz zur selbstgebauten Hardware. 

=> VGA Monitor hat keine Register, der Monitor zeigt lediglich die Daten an die durch das VGA Kabel geschickt werden. Die GPU hat die Register auf die geschrieben wird und die dann ein valides Signal für den Monitor erzeugt. Ist das im Prinzip Aufgabe des GPU Treibers? GPU Treiber nicht öfftenlich, also informationen Datasheets für GPUs nicht verfügbar?

ToDO: &#8211; Video von Ben eater zu VGA schauen, jmd darüber fragen? Ist es vllt einfacher das ganze mit einem Rasperry Pi zu implementieren? Hat der eine überschaubarere Architektur?

### 6502 Prozessor

Bild Memory map von 6502 Prozessor (wie selbst erdacht) + Bild von Memory map x86 Prozessor

## OpenHardware, RISC-V

Für moderne, kommerzielle Hardware sind kaum genaue Informationen vorhanden. Datasheets oder genaue Spezifikationen die das programmieren eines Treibers erlauben würden werden von Herstellern nicht herausgegeben. Dies ist für mich eine frustrierende Situation, da es keinen Spass macht, sich durch unvollständige Informationen im Internet zu hangeln um ein äußerst komplexes Gerät zu programmiere. Dazu kommt das kommerzielle Hardware Anforderungen hat wie Backwards-compability was das gesamte System schnell sehr aufgeblasen macht und mir die Arbeit erschwert. 

Heutzutage gibt es aber auch andere open source Ansätze:

  * Was ist open source genau? Vorteile / Nachteile?

RISC-V ist eine offene ISA nach dem RISC Prinzip. Es wurden bereits mehrere Prozessoren für diese Architektur entwickelt. Ich denke das ein open source Modell die Zukunft ist. 

## ForwardCom

Radikaler Ansatz: komplettes redesign einer ISA, ohne irgendwelche Altlasten. Wie würde ein ISA aussehen, wenn wir sie vom heutigen Standpunkt, mit dem heutigen Wissen designen? Sehr anders zu dem was momentan existiert&#8230;(vergleich David Precht redesign vom Bildungssystem/Schule).

Hat das Relevanz für den Markt? Kunden wollen Geräte die mit vorhandener Software etc kompatibel sind (=> großes Problem). x86 ISA unterliegt den freien Markt. Was quasi bedeutet das eher kurzfristige, schnell gewinnbringende Entscheidungen getroffen werden. Das Erfolgreichste ist leider nicht das Beste. (gibt es einen Punkt wo ein Redesign nicht mehr zu umgehen ist? Collapse under own weight)

Was ist mit anderen etablierten Standards VGA, HDMI, PCI, der Prozessor ist nur ein Teil des Puzzels (vermutlich das Größte).