# Calibre web-server

## Project setup

De setup van het project is vrij eenvoudig. Je hebt het main.yml playbook dat elk playbook in de playbooks folder gaat uitvoeren. Dit is gedaan met de bedoeling om de processen overzichtelijker te houden.

## vars.yml

In de vars.yml zijn er 2 variabled die ingevuld moeten worden om de playbook te laten runnen. De user variable is de user die gebruikt wordt op de guest OS waar calibre op zal draaien. Deze variablen is nodig omdat we een aantal files gaan opslagen in de home directory van deze user. Vervolgens is er content_server_subject, deze variablen bestaat uit zichzelf uit 3 delen:

- C = land
- ST = regio
- CN = domain of ip address van de calibre server

Deze variable zal gebruikt worden om een self signed certificaat toe te kennen. De reden waarom er geen let's encrypt gebruikt wordt is dat om let's encrypt te kunnen gebruiken we een domain op het internet nodig hebben. Aangezien ik dit zelf niet heb is er dus gekozen voor een self signed certificaat om toch https te kunnen gebruiken.

## Login

De standaard login is:
- Username = admin
- Passwoord = admin

Kan veranderd worden door users.sqlite aan te passen.

## Belangrijke opmerkingen

De Calibre_Library zal bij de eerste keer dat de playbook gerund word gekopieerd worden naar de home directory van de user. Vervolgens zal dit niet meer gebeuren omdat anders als je een ebook toevoegd via de webinterface deze overschreven zal worden door de library terug te kopieeren. Als u dit niet wilt en dus elke keer als u het script uitvoerd de library folder overschreven wordt door de Calibre_Library folder moet u de "force: no" weghalen in calibre_setup.yml --> Copy files to remote hosts.

De maximale upload grote is gezet op 1GB, indien dit niet gewenst is kan dit aangepast worden onder calibre-proxy.conf --> client_max_body_size.

Er wordt ook gebruik gemaakt van een nginx proxy om te connecteren met de calibre server.

## Launch playbook

Om het playbook te launchen gebruik volgend commando:
```
sudo ansible-playbook main.yml
```
Er wordt vanuit gegaan dat de ansible instellingen host file correct geconfigureerd zijn en er verbinding kan gemaakt worden met de hosts. Dit playbook werkt enkel op linux machines.