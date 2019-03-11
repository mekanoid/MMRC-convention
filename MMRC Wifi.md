---
title: MMRC Wifi and MQTT server
---
[Home](README.md) > MMRC Wifi and MQTT server

# MMRC Wifi and MQTT server
Detta är en kortfattad beskrivning av hur man får en Raspberry Pi (RPi) att agera som accesspunkt för ett fristående trådlöst nätverk. Kan vara mycket användbart för man kunna använda sitt eget MMRC-nät på en modulträff.

Det är ganska många moment inblandade i arbetet, men inget av dem är speciellt svårt. Det viktiga är att göra allt i rätt ordning och att vara noga med de texter man skriver in. Att råka skriva stor bokstav istället för liten kan göra att ingenting fungerar. Fråga mig, jag har provat. :-)

## Vad du behöver

- Raspberry Pi 3 eller Zero W (med wifi)
- Bildskärm med HDMI-kabel
- USB tangentbord
- USB-mus
- Strömförsörjning 5V/2A med micro USB-kontakt

Det går att installera och konfigurera en Raspberry Pi utan bildskärm, tangentbord och mus, men hur man gör det ingår inte i denna instruktion.

## Förberedelser

- Ladda hem senaste Raspbian (https://www.raspberrypi.org/downloads/raspbian/) och bränn till ett microSD-kort med exempelvis Etcher (https://etcher.io/).
- Koppla in och starta RPi:en. Tänk på att ha tillräckligt bra strömförsörjning, annars finns risk att Raspbian hänger sig under (första) starten.
- Har du en RPi med grafisk gränssnitt börjar du med att göra de inställningar som föreslås.
- Börja med att uppdatera Raspbian (om du inte redan gjort det grafiskt):

```
     sudo apt-get update
     sudo apt-get upgrade
```

## Installera programvaran
- Vi börjar med att installera den programvara som behövs
```
     sudo apt-get install dnsmasq hostapd
```
- Stoppa programvaran så länge, tills vi har konfigurerat allting korrekt
```
    sudo systemctl stop dnsmasq
    sudo systemctl stop hostapd
```
- Om du inte startade om efter uppdateringen av Raspbian, är det dags att göra det nu:
```
    sudo reboot
```

## Konfigurera statisk IP-adress
Nu ska vi konfigurera en statiskt IP-adress för RPi
- Börja med att starta textredigeraren `nano` för att redigera dhcpcd.conf:
```
    sudo nano /etc/dhcpcd.conf
```

- Skriv in följande rader i slutet av filen:

```
    interface wlan0
        static ip_address=192.168.4.1/24
        nohook wpa_supplicant
```
- Spara sen filen (ctrl-o) och avsluta nano (ctrl-x)


## Konfigurera DHCP-servern
- Nu ska DHCP-servern startas om så den får kännedom om den fasta IP-adressen:
```
    sudo service dhcpcd restart
```

- DHCP-servern ska nu konfigureras, men först kopierar vi undan originalfilen som vi inte har nytta av men vill bevara intakt:
```
    sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf.orig
    sudo nano /etc/dnsmasq.conf
```
- Lägg in följande text i filen:
```
    interface=wlan0      # Wireless interface wlan0
      dhcp-range=192.168.4.2,192.168.4.20,255.255.255.0,24h
```
Vi talar alltså om att för nätverket wlan0 ska vi kunna dela ut IP-adresser mellan 192.168.4.2 och 192.168.4.20, med en "lånetid" på 24 timmar.

## Konfigurera accesspunkten
- Börja med att skapa en ny konfigurationsfil
```
    sudo nano /etc/hostapd/hostapd.conf
```
- Lägg sedan in följande text i den:
```
    interface=wlan0
    driver=nl80211
    ssid=NameOfNetwork
    hw_mode=g
    channel=7
    wmm_enabled=0
    macaddr_acl=0
    auth_algs=1
    ignore_broadcast_ssid=0
    wpa=2
    wpa_passphrase=AardvarkBadgerHedgehog
    wpa_key_mgmt=WPA-PSK
    wpa_pairwise=TKIP
    rsn_pairwise=CCMP
```
Här har vi sagt att vi ska använda nätverkets kanal 7, det ska ha namnet (SSID) "NameOfNetwork" och lösenord "AardvarkBadgerHedgehog". Observera att varken nätverksnamnet (SSID) eller lösenordet ska omges av citationstecken. Lösenordet måste vara mellan 8 och 64 tecken långt.

Raden hw_mode bestämmer vilken nätverksstandard du använder:
```
    a = IEEE 802.11a (5 GHz)
    b = IEEE 802.11b (2.4 GHz)
    g = IEEE 802.11g (2.4 GHz)
    ad = IEEE 802.11ad (60 GHz).
```

- När du sen sparar filen, får du kanske återigen ange dess sökväg och namn (/etc/hostapd/hostapd.conf)

- Till sist ska vi tala om för systemet var denna konfigurationsfil finns:
```
    sudo nano /etc/default/hostapd
```

- Leta reda på raden `#DAEMON_CONF=""` och ändra den till (observera att # ska bort):
```
    DAEMON_CONF="/etc/hostapd/hostapd.conf"
```

## Starta allting
- Nu kan du starta funktionerna med och om detta går bra ska RPi vara igång som accesspunkt:
```
    sudo systemctl start hostapd
    sudo systemctl start dnsmasq
```

## Ta bort grafiska gränssnittet
Om du startade RPi med grafisk gränssnitt, så kanske du hädanefter inte vill att det ska gå igång automatiskt. Du kan ändra den inställningen i RPi eget konfigureringsverktyg:
```
    sudo raspi-config
```
Välj där

- 3 Boot options
- B1 Desktop / CLI
- B3 B1 Console (eller möjligen B2 för automatisk inloggning)


## Felåtgärder

### Hostapd startar inte
När jag skulle starta `hostapd` fick jag följande felmeddelande:

`Failed to start hostapd.service: Unit hostapd.service is masked`

Det åtgärdade jag med kommandot:

`sudo systemctl unmask hostapd.service`

Sen fungerade start-kommandot som det skulle!

### Kontrollera hostapd.conf
Du kan kontrollera att `hostapd.conf` är korrekt genom att köra följande kommando:

`sudo hostapd -d /etc/hostapd/hostapd.conf`

### Accesspunkten startar inte
När jag senare startade om RPi startade inte längre accesspunkten. Jag fick det att fungera genom att i Terminalen skriva:

`sudo systemctl enable hostapd`