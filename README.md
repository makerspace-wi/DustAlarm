# DustAlarm

![IMG_8318](https://user-images.githubusercontent.com/42463588/119325080-d0367e00-bc80-11eb-97cb-f9001c4e0334.jpg)

Die Holzwerkstatt des Makerspace Wiesbaden bietet verschiedene professionelle Holzverarbeitungsmaschinen, die an eine Absauganlage angeschlossen sind.
Dabei passiert es gelegentlich, dass sich die Absaugkanäle der Maschinen verstopfen, der Spänesack der Absaugvorrichtung zu voll oder falsch montiert wurde oder Mitglieder Schleifarbeiten ausführen, ohne den Schleifstaub abzusaugen. In diesen Fällen nimmt in den Werkstatträumen die Menge des Feinstaubs dramatisch zu, gefährdet Mitglieder und verstaubt alle anderen Maschinen.
<br>
Im Rahmen eines internen Projektes hatten wir schon vor Monaten in der Holzwerkstatt einen Feinstaubsensor aufgebaut und montiert, dessen Werte (PM10 - Partikel <10um) auch dazu genutzt werden, unsere Raumfilter (3 Stück) automatisch ein- und auch wieder auszuschalten. Ein Beispieldiagramm dazu im folgenden:

<img width="655" alt="Partikelbelastung" src="https://user-images.githubusercontent.com/42463588/119486719-dbf57380-bd58-11eb-92a3-a0d606eb389d.png">

Die Entscheidung fiel auf einen preiswerten Alarmgeber der Firma Vellemann - der 3 Kanäle hat: LED-Stroboskop, LED-Stroboskop plus Sirene (echt laut) und dauerhaft rote LEDs

<img width="400" alt="abc" src="https://user-images.githubusercontent.com/42463588/119487960-4b1f9780-bd5a-11eb-99df-43c25822c345.JPG">

In dem Gehäuse war zum Glück noch ausreichend Platz, um dort einen WEMOS D1, einen kleinen Step-UP Regler (5Vdc auf 9Vdc) und eine Interface Elektronik - mit 2 Tastern (TEST & RESET) unterzubringen.

<img width="400" alt="def" src="https://user-images.githubusercontent.com/42463588/119487904-380cc780-bd5a-11eb-84ed-d20676cd81d8.jpg"><img width="400" alt="ghi" src="https://user-images.githubusercontent.com/42463588/119489747-4bb92d80-bd5c-11eb-8292-f71e675956f6.jpg">

Die Schaltbilder und den Bestückungsplan findet ihr unter: [System Schaltbilder](doc/Alarm_Absaugung.pdf)

Das WEMOS D1 Modul wurde mit der aktuellen Firmware [Tasmota von Theo Arends](https://tasmota.github.io/docs/) geflasht und ist über WLAN - als MQTT-Client - in unser Gesamtnetzwerk eingebunden.<br>
Über verschiedene MQTT-Topics kann für den Dust Alarm Kanal 1 (LED-Stroboskop), Kanal 2 (LED-Stroboskop & Sirene), Kanal 3 (LED-SDauerlicht) oder Kanal 4 (alles AUS) von unserem Systemrechner angesprochen werden.<br>Der Dust Alarm selbst kann über MQTT-Topics das Betätigen der Taste <b>'Test'</b> und der Taste <b>'RESET'</b> an den Systemrechner melden.<br>
Auf unserem Systemrechner (Raspberry Pi 3+) verwaltet die Software [SYMCON](https://symcon.de) die Türeingangskontrolle, die Machinenzugänge, die Lichtregelung, die Temperatursteuerung und vieles anderes mehr. An dieser Stelle möchten wir unseren Dank an die Geschäftsleitung der Firma [SYMCON](https://symcon.de) richten, die uns nun schon mehrere Jahre mit einer Lizenz unterstützt.
<br>

## Auslösung bei zu hoher PM10 (<10µm) Partikelkonzentration

<b>bei Werten >= 750µg/m³</b> startet das Blinklicht in roter Farbe - dies ist eine Vorwarnstufe und es muss geprüft werden, aus welchem Grund so hohe Staubwerte vorliegen.<p>
  <b>Werte >= 1000µg/m³</b> entsprechen dem oberen Warnwert 
<ul>
rotes Blinklicht und die Sirene werden aktiviert<br>
die Arbeiten sind sofort einzustellen (z.B. auch Schleifen ohne Absaugung)<br>
Dichtigkeit der Absauganlage prüfen (z.B. Staubsack nicht korrekt fixiert)<br>
den kompletten Absaugkanal der entsprechenden Maschine auf Verstopfung prüfen und ggf. leer räumen/reinigen<br>
</ul>
<p>Die Sirene kann an der kleinen RESET-Taste für 15 Minuten abgestellt werden, um in dieser Zeit das Problem zu beheben.<br>Während dieser RESET-Zeit leuchten 2 rote LEDs dauerhaft.

