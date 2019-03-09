---
title: MMRC Overview
---
[Home](README.md) > MMRC Overview


# MMRC Overview
This document is written in swedish only.

## Koncept
Tanken med MMRC är att skapa ett flexibelt och decentraliserat sätt att styra modelljärnvägar. Grunden i konceptet är att man har en central meddelandeserver (s.k. MQTT-broker) och olika typer av klienter som sköter olika funktioner på modelljärnvägen. Klienterna kan sedan kommunicera med varandra via meddelandeservern för att både styra och bli styrda av andra klienter.

Genom att använda trådlöst nätverk och små, billiga kretskortsdatorer blir systemet väldigt flexibelt. Det passar speciellt bra på modelljärnvägsmoduler som kan vara placerade på olika platser i en bana, men ändå ska kunna styras och övervakas centralt. 

## Meddelandeserver
En viktig del i MMRC är den centrala meddelandeservern. Till den bör man använda en lite mer kapabel dator och ett förslag är att använda Raspberry Pi med t.ex. [Mosquitto](http://mosquitto.org/) installerat.

## Klienter
Det finns inga specifika typer av klienter i detta system. Det är upp till den som programmerar klienten att bestämma vad den ska kunna göra. Denna test-klient kan exempelvis bara tända och släcka en lysdiod, men mer troliga funktioner är att styra växlar, signaler och känna av tryckknappar.

Det finns inget som bestämmer hur många uppgifter en klient utför. På en liten modul kanske en klient sköter både växel och signaler, medan en större station kanske har olika klienter för växlar, signaler och ställverket. Man kan också tänka sig en signalmodul som bara har in- och utfartssignal styrd av en egen klient.

## CLEES-kompatibel
Idéerna till MMRC har jag haft länge, men det var först när [CLEES](https://github.com/TomasLan/CLEES/) dök upp som jag fick inspiration nog att ta tag i mina egna idéer på allvar. Skillnaderna mellan MMRC och CLEES är små; i grunden är det samma tankar. Största skillnaden är att CLEES koncentrerar sig till på en enda typ av klient som då ska klara av allt.

Tack vare likheterna finns möjligheten att MMRC kan samexistera med CLEES och styra/styras av CLEES-klienter. Det finns en inställning i detta testprogram som gör att klienten blir CLEES-kompatibel.
