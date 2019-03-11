---
title: MMRC MQTT broker
---
[Home](README.md) > MMRC MQTT broker

# MMRC MQTT broker
Detta är en kort beskrivning av hur man istallerar en s.k. MQTT broker på en Raspberry Pi. Har du redan en RPi som accesspunkt för MMRC, är det lämpligt att du installerar MQTT-brokern på samma RPi.

I denna beskrivning används den mycket vanliga brokern [Mosquitto](https://mosquitto.org/) som är baserad på öppen källkod.

## Vad du behöver

- Raspberry Pi 3 eller Zero W (med wifi)
- Bildskärm med HDMI-kabel
- USB tangentbord
- USB-mus
- Strömförsörjning 5V/2A med micro USB-kontakt


## Installera programvaran

- Starta Terminalen och skriv

```
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get install mosquitto mosquitto-clients
```
Efter installationen ska allting bara starta automatiskt och du kan använda MQTT direkt.


## Testa installation
Om du _inte_ har installerat MQTT på samma dator som du nu ska testa allting på, så ersätter du `localhost` med den IP-adress som datorn med MQTT brokern har (t.ex. 192.186.4.20).

- Testa MQTT genom att i terminalen skriva (du prenumererar på ämnet "testkanal")
```
    mosquitto_sub -h localhost -v -t testkanal
```

- Starta en ny Termnal och skriv följande kommande för att skicka (publicera) meddelandet "Hello world! till ämnet "testkanal":
```
    mosquitto_pub -h localhost -t testkanal -m "Hello world!"
```

Förhoppningsvis dyker nu meddelande upp på den andra Terminalen och du kan se att allting fungerar som det ska!
