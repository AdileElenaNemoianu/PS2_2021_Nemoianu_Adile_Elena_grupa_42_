# PS2_2021_Nemoianu_Adile_Elena_grupa_42_

Se va realiza un sistem de conducere pe bază de micro-controller care va efectua următoarele funcții:


Citirea temperaturii sistemului de la distanță.

Controlul luminilor LED de la distanță. ( 1 LED - control on/off, 1 LED RGB - controlul culorii)

Transmiterea mesajelor prin internet către sistem. 

Detecția și alertarea inundațiilor.


Interfața locală

Sistemul va dispune de un LCD 16x2 pe care va fi afișat meniul, iar în modul blocat se vor afișa ora, temperatura curentă și numărul mesajelor necitite. (ex. “    18:02    “, “16°C - 2 mesaje”)

Navigarea prin meniul echipamentului se va face prin patru butoane: “OK”, “Cancel”, “+”, “-”.

Accesul în meniu se face pe baza unei parole prestabilite (ex. “--++--”, “+++---”)

Meniul va permite citirea și ștergerea mesajelor primite precum și citirea evenimentelor de tip inundație.


Interfața web

Sistemul va dispune de o interfață web ce va afișa temperatura curentă(citită de la un senzor LM35) și va oferi posibilitatea de transmitere a unui mesaj către sistem.

Interfața va dispune de un buton de tip ON/OFF pentru controlul unui LED conectat la micro-controller.

Evenimentele de tip inundație vor fi transmise prin e-mail la o adresa prestabilită.  


Dezvoltarea Software

Se vor stoca în memoria nevolatilă ultimele 10 mesaje primite.

Se vor stoca în memoria nevolatilă datele ultimelor 10 evenimente de tip inundație.

Pentru conectarea la internet a sistemului se poate folosi o comunicație serială cu un PC sau o conexiune Wi-Fi/Ethernet/3G.

Se pot folosi orice librării open-source.

Proiectul va avea un repository GitHub 


Teme opționale

Reglarea temperaturii de la distanță. Regulator format din senzorul de temperatură și o sursă de căldură controlată prin PWM.

Stabilirea unui orar zilnic de funcționare din interfața web pentru luminile LED.


