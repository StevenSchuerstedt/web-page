---
title: What is Computation?
author: admin
type: page
date: 2021-05-24T09:23:31+00:00
draft: false
private: true

---
Gödels imcompletness theorem, true statements that cannot be proven

=> question about number theory, goldbach conjecture etc

turing machine => halting problem, undecidable

halting problem is a well defined problem, all needed information is given, and the answer can be yes / no, but still no algorithm possible.

(compare to things like, what is the meaning of life? does god exist? => ok there is no algorithm for that&#8230;.)

asking wether a TM halts or not seems innocent, but you ask for a prove for every math problem there is. (goldbach conjecture) 

related to alot of other problems, solving polynomial equations, wang tiles etc

=> always problem of self reference

Cantor set theory

Why is this stuff helpful?

It reflects about what an algorithm is, what we do when we add two numbers etc. When we understand what algorithms are and how they work, we can construct machines to execute that algorithm. 

=> first mechanical calculaters, baggage, difference machine

=> computer uses circuits, boolean algebra, idea of universality

### Gödel
Formale Systeme:
Starte mit irgendwas: 1 (Axiome)
Regeln für Weiter: 1 -> 101 0 -> 011 (Schlussregeln)

=> Aus den Axiomen können Dinge abgeleitet werden
1 -> 101 -> 10111 -> ...
Zwei Ebenen: - sytaktische Ebene, Symbole nach strikten Regeln manipulieren
- semantische Ebene, Symbole (willkürliche) Bedeutung geben
Interessant: Ebenen lassen sich verknüpfen! Regeln, Axiome können so gewählt werden das es tatstächlich Sinn ergibt.
=> Prädikatenlogik, Principia Mathematica, TNT (GEB)
Systeme sprechen über Mathematik, natürliche Zahlen. Lassen sich alle "richtige" Aussagen so darstellen, ableiten?
Paeno Axiome für natürliche Zahlen

Formales System um ÜBER natürliche Zahlen zu sprechen? Aber in wahrheit trugschluss, natürliche Zahlen sind ein formales System, im Grunde sind alle Formalen Systeme gleich, will über sich selber sprechen
=> widerspruch, man kann nur bis zu einem bestimmten Grad über sich selber sprechen?

Gödelisierung entscheidende Einsicht?
Jede Formel die auf der semantischen Ebene über die natürlichen Zahlen spricht, lässt sich auch in sie übersetzen.
=> Jedes Element in Zahl codieren
=> jede Regel mit Zahlen manipulation darstellen
=> Algebra auch nur ein "bedeutungsleeres" Formales System
=> Formel ist gleichzeitig das was sie eigentlich sagen will? Kreisschluss?
=> Formales System in ein anderes überführen?

Aussagen über Formel sind so möglich,
Formel 
Ich bin unbeweisbar => Lügner Paradoxon
ist eine vollkommen akzeptierbare Formel, aber schwierig, führt zu Widerspruch im Systeme

http://us.metamath.org/mm_100.html zeigt, formale beweise sind möglich für vieles. (genauso in prinicipa mathematica gezeigt)

vergleich: Sprache
syntax einzelner Buchstaben vs. semantik eines wortes
ein wort ist mehr als seine buchstaben, buchstaben sind willkürlich aneinandergereiht, woher kommt der sinn?
ein satz ist mehr als seine wörter, sinn eines satzes? man kann jedes wort in einem satz verstehen, aber dennoch nicht den satz
jedes wort verstehen vs das verstehen was jemand versucht zu sagen, was die wörter ausdrücken