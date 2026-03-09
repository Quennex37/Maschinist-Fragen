[Maschinist Fragen index.html](https://github.com/user-attachments/files/25847266/Maschinist.Fragen.index.html)
<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Maschinisten Quiz - 1:1 Original</title>
    <style>
        body { font-family: sans-serif; line-height: 1.4; padding: 15px; background: #f0f2f5; }
        .container { max-width: 650px; margin: auto; background: white; padding: 20px; border-radius: 12px; box-shadow: 0 4px 10px rgba(0,0,0,0.1); }
        .progress { font-size: 0.9em; color: #666; margin-bottom: 10px; font-weight: bold; }
        h2 { color: #d32f2f; margin-top: 0; font-size: 1.2em; }
        .question-text { font-weight: bold; margin-bottom: 15px; display: block; font-size: 1.1em; }
        .option { display: block; background: #f8f9fa; margin-bottom: 8px; padding: 10px 10px 10px 40px; border-radius: 6px; cursor: pointer; position: relative; border: 1px solid #ddd; }
        .option input { position: absolute; left: 12px; top: 12px; width: 18px; height: 18px; }
        .selected { border-color: #d32f2f; background: #fff5f5; }
        button { background: #d32f2f; color: white; border: none; padding: 12px; border-radius: 6px; cursor: pointer; width: 100%; font-size: 16px; font-weight: bold; margin-top: 10px; }
        #feedback { margin-top: 15px; padding: 12px; border-radius: 6px; display: none; font-weight: bold; }
        .correct { background: #d4edda; color: #155724; border: 1px solid #c3e6cb; }
        .wrong { background: #f8d7da; color: #721c24; border: 1px solid #f5c6cb; }
        #next-btn, #override-btn { background: #28a745; display: none; }
        #override-btn { background: #6c757d; font-size: 14px; margin-top: 5px; }
        .stats { text-align: center; padding: 20px; }
        .percent-box { font-size: 2.5em; font-weight: bold; color: #d32f2f; margin: 10px 0; }
    </style>
</head>
<body>

<div class="container">
    <div id="progress" class="progress">Frage 1 von 60</div>
    <div id="quiz-box">
        <span class="question-text" id="question-display"></span>
        <div id="options-display"></div>
        <button id="check-btn" onclick="checkAnswer()">Antwort prüfen</button>
        <div id="feedback"></div>
        <button id="override-btn" onclick="overrideCorrect()">Trotzdem als RICHTIG werten</button>
        <button id="next-btn" onclick="nextQuestion()">Nächste Frage</button>
    </div>
</div>

<script>
    const allQuestions = [
        { id: 1, q: "1 Auf was muss für die Betriebssicherheit an einem Feuerwehrfahrzeug geachtet werden?", o: {a: "Wasser", b: "Motorenöl", c: "Beladung vollständig", d: "Reifenluftdruck", e: "Kraftstoff", f: "Funkausrüstung", g: "Elektrische Anlage"}, a: ["a", "b", "d", "e", "g"] },
        { id: 2, q: "2 Wie groß soll der Elektrodenabstand bei der Zündkerze im Motor der Tragkraftspritze mit VW- Industriemotor sein und in welcher Stellung hat der Kraftstoffhahn nach beendetem Einsatz zu stehen?", o: {a: "0,6 bis 0,7 mm", b: "0,4 mm", c: "Unter 0,2 mm", d: "Auf", e: "Zu"}, a: ["b", "d"] },
        { id: 3, q: "3 Auf was muss bei der Überprüfung der Verkehrssicherheit geachtet werden?", o: {a: "Bremsen", b: "Beleuchtung", c: "Bereifung", d: "Beladung", e: "Nebenantrieb", f: "Lenkung", g: "Signal", h: "Spiegel", i: "Scheibenwischer", j: "Kraftstoff"}, a: ["a", "b", "c", "d", "f", "g", "h", "i"] },
        { id: 4, q: "4 Welche der nachfolgenden Feuerwehr-Dienstvorschriften (FwDV) geben Hinweise auf die Aufgaben des Maschinisten?", o: {a: "FwDV 3 – Einheiten im Lösch- und Hilfeleistungseinsatz", b: "FwDV 500 – Einheiten im ABC-Einsatz", c: "FwDV 10 – Die tragbaren Leitern", d: "FwDV 810.3 – Sprechfunkdienst", e: "FwDV 2 – Ausbildung der Freiwilligen Feuerwehren"}, a: ["a", "c", "e"] },
        { id: 5, q: "5 Was hat der Maschinist gemäß den Feuerwehr-Dienstvorschriften und den Richtlinien zur Durch- führung der Leistungsübungen beim Einsatz in der Gruppe bei offener Wasserentnahme zu tun?", o: {a: "Er hilft den Trupps beim Entnehmen der Geräte", b: "Er legt Kupplungsschlüssel (falls erforderlich) Saugkorb, Saugschutzkorb sowie Halte- und Ventilleine bereit", c: "Er kuppelt den Saugkorb an die Saugleitung an", d: "Er kuppelt die Schlauchleitung an die Pumpe an und bedient die Pumpe", e: "Er schlägt die Ventilleine im Bedienbereich der Pumpe und die Halteleine an einem festen Punkt an"}, a: ["a", "b", "d", "e"] },
        { id: 6, q: "6 Welche Wasserentnahmestellen erfordern ein Ansaugen des Wassers?", o: {a: "Schachthydrant", b: "Überflurhydrant", c: "Unterflurhydrant", d: "Löschteich", e: "Unterirdischer Löschwasserbehälter", f: "Löschwasserbrunnen S"}, a: ["d", "e", "f"] },
        { id: 7, q: "7 Welche Einflüsse führen dazu, dass die theoretische Saughöhe von 10,33 Meter nicht erreicht werden kann?", o: {a: "Luftdruck unter dem Normaldruck", b: "Für das Fließen des Wassers im Saugschlauch wird Druck benötigt", c: "Reibungs- und Strömungsverluste", d: "Wasser ist schwerer als Luft", e: "Entlüftungseinrichtungen erzeugen kein 100%iges Vakuum"}, a: ["a", "b", "c", "e"] },
        { id: 8, q: "8 Mit welchem Knoten wird die Halte- und Ventilleine entsprechend der Richtlinie zur Durchfüh- rung der Leistungsübungen an den Festpunkten angebracht?", o: {a: "Doppelschlinge", b: "Mastwurf", c: "Zimmermannsschlag", d: "Schotstich", e: "Ankerstich"}, a: ["b"] },
        { id: 9, q: "9 Welchen Ausgangsdruck soll der Maschinist in der Regel an der Feuerlöschkreiselpumpe ein- halten?", o: {a: "Fünf bis sechs bar bei kurzer Entfernung zur Einsatzstelle", b: "Acht bar bei Wasserförderung über lange Wege", c: "Acht bis zehn bar bei Erzeugung von Schwerschaum"}, a: ["a", "b", "c"] },
        { id: 10, q: "10 Löschwasserbrunnen sind künstlich angelegte Entnahmestellen für Löschwasser aus dem Grundwasser. Für welchen Zeitraum muss die Ergiebigkeit mindestens gewährleistet sein?", o: {a: "Eine Stunde", b: "Drei Stunden", c: "Gesamtdauer des Einsatzes"}, a: ["b"] },
        { id: 11, q: "11 Was bedeutet bei einer Feuerlöschkreiselpumpe die 1. Ziffer in der Bezeichnung „FPN 10/1000“?", o: {a: "Der Nennförderstrom ist 1000 Liter/Minute", b: "Der Nennförderdruck ist 10 bar", c: "Die Förderleistung ist 10 Kilowatt"}, a: ["b"] },
        { id: 12, q: "12 Welche Aussage ist richtig?", o: {a: "Die geodätische Saughöhe kann direkt am Eingangsmanometer der der Feuerlöschkreiselpumpe während der Wasserförderung abgelesen werden", b: "Die geodätische Saughöhe ist der Höhenunterschied in Meter zwischen Pumpenmitte und saugseitigem Was- serspiegel", c: "Das Maß der geodätischen Saughöhe ist für den Förderstrom ohne Einfluss"}, a: ["b"] },
        { id: 13, q: "13 Welche Pumpenwellenabdichtung ist während des Betriebes nachstellbar?", o: {a: "Radialdichtringe", b: "Stopfbuchsenpackung", c: "Gleitringdichtung"}, a: ["b"] },
        { id: 14, q: "14 Welche der nachfolgenden Feuerwehrpumpen sind genormt?", o: {a: "FPN 40/250", b: "FP 8/8", c: "FPN 10/750", d: "FPN 15/2000", e: "FP 24/8", f: "FP 32/8", g: "LP 24/3", h: "TUP 3-1,5", i: "TP 4/1", j: "TTP 8/1/8"}, a: ["a", "c", "d", "h", "i", "j"] },
        { id: 15, q: "15 Welche Bedingungen müssen beim 1. Garantiepunkt einer Feuerlöschkreiselpumpe FPN 15/1000 erfüllt werden?", o: {a: "Der Nennförderdruck muss mindestens 15 bar betragen", b: "Der Nennförderstrom muss mindestens 1000 Liter/Minute sein", c: "Die Nenndrehzahl darf hierbei max. ± 5% abweichen", d: "Die geodätische Saughöhe muss 3,0 Meter betragen", e: "Die geodätische Saughöhe muss 7,5 Meter betragen"}, a: ["a", "b", "c", "d"] },
        { id: 16, q: "16 Was bewirkt Kavitation (Hohlsog) in einer Feuerlöschkreiselpumpe?", o: {a: "In der Feuerlöschkreiselpumpe bilden sich so hohe Drücke, dass das Gehäuse schlagartig auseinander bricht", b: "Es bilden sich Dampfblasen in der Flüssigkeit, der Förderstrom kann nicht mehr erhöht werden", c: "Beim Zerfall der Dampfblasen wird der Werkstoff des Laufrades zerstört"}, a: ["b", "c"] },
        { id: 17, q: "17 Welche Entlüftungseinrichtungen sind Strahlapparate?", o: {a: "Wasserstrahlpumpe", b: "Trockenring-Entlüftungspumpe", c: "Gasstrahler einstufig", d: "Gasstrahler zweistufig"}, a: ["c", "d"] },
        { id: 18, q: "18 Welche Möglichkeiten bestehen, wenn der Eingangsmanometer beim Ansaugen keinen negati- ven Druck anzeigt?", o: {a: "Ausgangsventil der Feuerlöschkreiselpumpe ist offen", b: "Die Feuerlöschkreiselpumpe oder die Saugleitung ist undicht", c: "Das Schutzsieb am Saugkorb ist verstopft", d: "Das Rückschlagorgan sitzt im Saugkorb fest", e: "Der Saugkorb liegt teilweise außerhalb des Wassers"}, a: ["a", "b", "e"] },
        { id: 19, q: "19 Was geschieht in der Feuerlöschkreiselpumpe, wenn der Wasserspaltring beschädigt ist?", o: {a: "In der Feuerlöschkreiselpumpe entsteht ein Wasserkreislauf von der Druckseite zur Saugseite", b: "In der Feuerlöschkreiselpumpe entsteht ein Wasserkreislauf von der Saugseite zur Druckseite", c: "Beim Hydrantenbetrieb wird der Förderstrom größer, weil zusätzlich Wasser zu den Druckausgängen gelangen kann, dafür wird bei Saugbetrieb der Förderstrom geringer", d: "Durch den Wasserkreislauf in der Feuerlöschkreiselpumpe wird der Schließdruck geringer"}, a: ["a", "d"] },
        { id: 20, q: "20 Sie bedienen an der Brandstelle eine Feuerlöschkreiselpumpe FPN 10/1000 und halten einen Ausgangsdruck von fünf bis sechs bar. Es ist zu Nachlöscharbeiten - für Sie nicht sichtbar - noch ein C-Rohr in Stellung. Wasserentnahme: offenes Gewässer, Geodätische Saughöhe sechs Meter. Auf was müssen Sie achten?", o: {a: "Dass der Druck konstant bleibt", b: "Weil die Feuerlöschkreiselpumpe nicht ausgelastet ist, brauchen Sie ihr keine besondere Aufmerksamkeit zu schenken. Sie können deshalb bei der Zurücknahme der nicht mehr benötigten Geräte behilflich sein", c: "Das Pumpengehäuse von Zeit zu Zeit mit der Hand auf Temperatur überprüfen. Notfalls freien Druckausgang etwas öffnen der Verbindungsleitung zum eingebauten Löschwasserbehälter etwas öffnen"}, a: ["a", "c"] },
        { id: 21, q: "21 Was bedeutet „Kavitation“ beim Fördern von Wasser mit einer Feuerlöschkreiselpumpe?", o: {a: "Zerstörung des Laufrades", b: "Zerstörung des Wasserspaltringes", c: "Bildung von Wasserdampfbläschen"}, a: ["a", "c"] },
        { id: 22, q: "22 An welchen äußeren Merkmalen erkennen Sie den Unterschied zwischen einer einstufigen und einer zweistufigen Feuerlöschkreiselpumpe?", o: {a: "An der Zahl der Druckausgänge", b: "Am Typenschild (Nenndrehzahl)", c: "An der Form des Pumpengehäuses", d: "Am angegebenen Schließdruck"}, a: ["b", "c"] },
        { id: 23, q: "23 Die Heckpumpe vom Löschgruppenfahrzeug LF 10 ist an der Brandstelle im Einsatz. Die Was- serversorgung erfolgt über das Sammelstück vom Hydranten. Weil die Entnahme von Lösch- wasser aus der Versorgungsleitung unzureichend ist, wird mittels einer Tragkraftspritze PFPN 10/1000 Wasser aus einem in 200 Meter entfernten Teich entnommen. Frage: Darf die Zubringer- leitung von der PFPN 10/1000 an das Sammelstück der Heckpumpe angeschlossen werden?", o: {a: "Ja, weil Teichwasser wegen der Rückschlagklappe im Sammelstück nicht zum Hydranten gelangen kann", b: "Nein, weil Bakterien ins Rohrnetz der Wasserversorgung gelangen können", c: "Nur wenn der Druck des Rohrleitungsnetzes größer ist als der Druck in der Zubringerleitung von der PFPN 10/1000", d: "Nur wenn der Druck des Rohrleitungsnetzes niedriger ist als der Druck in der Zubringerleitung von der PFPN 10/1000"}, a: ["b"] },
        { id: 24, q: "24 Welche Ursachen sind denkbar, wenn die geodätische Saughöhe ein Meter beträgt, das Ein- gangsmanometer - 0,6 bar anzeigt und trotz Vollgas keine Anzeige am Ausgangsmanometer er- folgt?", o: {a: "Schutzsieb des Saugkorbs verstopft", b: "Saugkorb liegt teilweise außerhalb der Wasseroberfläche", c: "Schutzsieb am Pumpeneingang verstopft", d: "Rückschlagorgan im Saugkorb fehlt", e: "Förderstrom sehr groß"}, a: ["a", "c", "e"] },
        { id: 25, q: "25 Welche wichtigen Bauteile sind an Feuerlöschkreiselpumpen vorhanden?", o: {a: "Kupplung", b: "Gehäuse mit Gehäusedeckel", c: "Kurbelwelle", d: "Kolben und Ventile", e: "Armaturen und Bedienungseinrichtungen", f: "Entlüftungseinrichtung", g: "Laufrad (Laufräder)", h: "Messinstrumente", i: "Schutzhaube"}, a: ["b", "e", "f", "g", "h"] },
        { id: 26, q: "26 Was besagt der Begriff „Kavitation“?", o: {a: "Geräuschbildung in der Feuerlöschkreiselpumpe ab vier Meter Saughöhe", b: "Fremdkörper (Flugsand o.ä.) im Löschwasser", c: "Bildung und Zerstörung von dampfgefühlten Hohlräumen in Flüssigkeiten"}, a: ["c"] },
        { id: 27, q: "27 Welche der aufgeführten Armaturen legt der Maschinist bereit beziehungsweise schließt er an?", o: {a: "Standrohr", b: "Saugkorb", c: "Sammelstück", d: "Druckbegrenzungsventil", e: "Verteiler"}, a: ["b", "c"] },
        { id: 28, q: "28 Worauf muss der Maschinist bei der Löschwasserförderung über lange Strecken achten?", o: {a: "Verkehrsbehinderungen beachten", b: "Verkehrsbehinderungen so gering wie möglich zulassen", c: "Reserveschläuche und Ersatz-Feuerlöschkreiselpumpe bereithalten", d: "Nachrichtenübermittlung sicherstellen", e: "Förderleitung langsam füllen und auf angeordneten Druck gehen", f: "An der Verstärker-Feuerlöschkreiselpumpe freien Druckausgang öffnen, damit Luft aus der Schlauchleitung entweichen kann", g: "Dass der Förderdruck von acht bar eingehalten wird", h: "Bei Temperaturen unter 0º C stets für fließendes Wasser sorgen", i: "Förderleitung beaufsichtigen", j: "Dass Schlauchbrücken verlegt werden"}, a: ["e", "f", "g", "h"] },
        { id: 29, q: "29 Wie viel mm Abstand müssen zwischen Masse und Mittel-Elektrode bei Zündkerzen von Trag- kraftspritzen PFPN 10/1000 vorhanden sein?", o: {a: "Entsprechend der Betriebs- beziehungsweise Bedienungsanleitung", b: "Zehn Millimeter", c: "Acht Millimeter"}, a: ["a"] },
        { id: 30, q: "30 Auf was muss bei der Überprüfung der Verkehrssicherheit an einem Feuerwehrfahrzeug geach- tet werden?", o: {a: "Beladung, Verriegelung der Schubfächer/Geräte", b: "Signal-Warnanlage", c: "Bereifung, Profiltiefe"}, a: ["a", "b", "c"] },
        { id: 31, q: "31 Was muss vor der Prüfung der Leistungswerte (Garantie-Punkte) einer Feuerlöschkreiselpumpe durchgeführt werden?", o: {a: "Geodätische Saughöhe muss überprüft werden", b: "Ölstand überprüfen", c: "Kraftstoffinhalt überprüfen"}, a: ["a"] },
        { id: 32, q: "32 Kreuzen Sie die Entlüftungseinrichtungen an, die verdrängungstechnisch wirken!", o: {a: "Flüssigkeitsring-Entlüftungseinrichtung", b: "Doppel-Freikolben-Entlüftungseinrichtung", c: "Trockenring-Entlüftungseinrichtung", d: "Gasstrahler (einstufig)-Entlüftungseinrichtung", e: "Gasstrahler (zweistufig)-Entlüftungseinrichtung"}, a: ["a", "b", "c"] },
        { id: 33, q: "33 Wann darf die Feuerwehr Sonderrechte im Straßenverkehr in Anspruch nehmen?", o: {a: "Bei allen Einsätzen", b: "Wenn der Einsatzleiter es anordnet", c: "Wenn Menschenleben in Gefahr sind", d: "Zur Erfüllung hoheitlicher Aufgaben, wenn höchste Eile dringend geboten ist"}, a: ["c", "d"] },
        { id: 34, q: "34 Bei einem Löschfahrzeug fällt die Entlüftungseinrichtung aus, weil die Auspuffanlage defekt ist. Welche Entlüftungseinrichtung hat das Fahrzeug?", o: {a: "Flüssigkeitsring-Entlüftungseinrichtung", b: "Trockenring-Entlüftungseinrichtung", c: "Gasstrahler-Entlüftungseinrichtung"}, a: ["c"] },
        { id: 35, q: "35 Zur Überprüfung der Einsatzbereitschaft einer Feuerlöschkreiselpumpe müssen Trockensaug- proben gemacht werden. Wie oft soll dies geschehen?", o: {a: "Mindestens einmal im Jahr", b: "Mindestens einmal im Monat", c: "Nach jedem Einsatz und jeder Übung"}, a: ["b", "c"] },
        { id: 36, q: "36 Was tut man, wenn die grüne Kontrollleuchte einer Tragkraftspritze PFPN 10-1000 mit VW- Industriemotor während des Pumpenbetriebes plötzlich erlischt?", o: {a: "Motor abstellen, Ursache überprüfen und beseitigen, Pumpe in Betrieb nehmen, Einsatzleiter verständigen", b: "Motordrehzahl etwas zurück nehmen, optisch prüfen, ob Keilriemen noch Gebläserad und Lichtmaschine an- treibt, wenn ja, Wasserförderung nicht unterbrechen, da nur eine elektrische Störung vorliegt. Einsatzleiter ver- ständigen"}, a: ["b"] },
        { id: 37, q: "37 Warum müssen während des Betriebes alle Blindkupplungen an den Druckausgängen einer Feuerlöschkreiselpumpe angenommen werden?", o: {a: "Weil der Druck in der Pumpe sonst zu groß würde", b: "Weil sich zwischen dem Absperrorgan und der Blindkupplung ein Druck aufbauen könnte", c: "Weil sonst beim späteren Abnehmen der Blindkupplungen erhöhte Unfallgefahr besteht"}, a: ["b", "c"] },
        { id: 38, q: "38 In welcher Stellung soll sich die Kupplung einer Tragkraftspritze PFPN 10/1000 befinden, wenn sie im Fahrzeug gelagert ist?", o: {a: "Betrieb", b: "Saugen", c: "Kupplung ein", d: "Kupplung aus"}, a: ["a", "c"] },
        { id: 39, q: "39 Wie berechnet man überschlägig den Kraftstoffverbrauch bei einem Löschgruppenfahrzeug während des Einsatzes der Pumpe, wenn die Tankuhr defekt ist?", o: {a: "Eine Betriebsstunde der Feuerlöschkreiselpumpe entspricht etwa 60 km Fahrleistung", b: "Eine Betriebsstunde der Feuerlöschkreiselpumpe entspricht etwa 100 km Fahrleistung"}, a: ["a"] },
        { id: 40, q: "40 Nach einem zweistündigen Einsatz an der Tragkraftspritze PFPN 10/1000 soll der Maschinist abgelöst werden. Worauf hat er zu achten, bevor er von der PFPN 10/1000 weggeht?", o: {a: "Dass die Tragkraftspritze PFPN 10/1000 nur in stillstehendem Zustand übergeben wird", b: "Dass ausreichend Kraftstoff vorhanden ist", c: "Dass er den Ablösenden einweist"}, a: ["b", "c"] },
        { id: 41, q: "41 Während des Einsatzes einer Feuerlöschkreiselpumpe bleibt plötzlich der Antriebsmotor stehen. Welche Ursachen sind denkbar?", o: {a: "Motor und Motorenöl wurden zu heiß", b: "Vergaser beziehungsweise Einspritzpumpe defekt", c: "Kraftstoffmangel"}, a: ["b", "c"] },
        { id: 42, q: "42 Der statische beziehungsweise dynamische Prüfdruck beträgt bei den Feuerlöschkreiselpum- pen FPN 10/1000", o: {a: "15 bar bei stehender, 22,5 bar bei laufender Pumpe", b: "16 bar bei stehender, 24,0 bar bei laufender Pumpe"}, a: ["a"] },
        { id: 43, q: "43 Welche Feuerwehrpumpen dürfen zum Fördern von Heizöl extra leicht verwendet werden?", o: {a: "Handmembranpumpe", b: "Tragkraftspritze", c: "Feuerlöschkreiselpumpe", d: "Umfüllpumpe TUP 3-1,5 ex geschützt", e: "tragbare Tauchpumpe mit Elektromotor TP 4-1", f: "Lenz-Kreiselpumpe"}, a: ["a", "d"] },
        { id: 44, q: "44 Welche Tätigkeiten gehören zu den Aufgaben des Maschinisten nach der Feuerwehr- Dienstvorschrift FwDV 3 „Einheiten im Lösch- und Hilfeleistungseinsatz“?", o: {a: "Bedienung der Feuerlöschkreiselpumpe", b: "Fahren des Löschfahrzeugs", c: "Absichern der Einsatzstelle", d: "Mithilfe bei der Entnahme von Geräten aus dem Löschfahrzeug", e: "Bedienung von Sonderaggregaten", f: "Anschließen der Schlauchleitungen an die Feuerlöschkreiselpumpe", g: "Bereitlegen von Kupplungsschlüsseln (falls erforderlich), Saugkorb, Saugschutzkorb, Halte- und Ventilleine bei offener Wasserentnahme", h: "Standrohr in Stellung bringen bei Wasserentnahme aus dem Rohrnetz"}, a: ["a", "b", "d", "e", "f", "g"] },
        { id: 45, q: "45 Was sagt die Bezeichnung „TLF 2000“?", o: {a: "Es handelt sich um ein Trockenlöschfahrzeug", b: "Es handelt sich um ein Tanklöschfahrzeug", c: "Das Fahrzeug hat eine Feuerlöschkreiselpumpe FPN 15/1000O", d: "Das Fahrzeug hat einen Motor mit 200 KW"}, a: ["b"] },
        { id: 46, q: "46 Welche der nachfolgenden Feuerwehrfahrzeuge sind Löschfahrzeuge?", o: {a: "Löschgruppenfahrzeug LF 20", b: "Tanklöschfahrzeug TLF 3000", c: "Löschgruppenfahrzeug LF 16TS", d: "Schlauchwagen SW 2000", e: "Gerätewagen-Gefahrgut", f: "Tragkraftspritzenfahrzeug TSF"}, a: ["a", "b", "c", "f"] },
        { id: 47, q: "47 Welche der nachfolgenden kraftbetriebenen Geräte können sich nach Norm als zusätzliche Be- ladung auf einem genormten Löschfahrzeug befinden?", o: {a: "Tauchpumpe TP 4-1", b: "Tragbarer Stromerzeuger 5 kVA", c: "Brennschneidgerät", d: "Plasma-Schneidgerät", e: "Presslufthammer", f: "Motorkettensäge", g: "Hydraulisches Rettungsgerät"}, a: ["a", "b", "f", "g"] },
        { id: 48, q: "48 Was beinhaltet die Bezeichnung „HLF 20“?", o: {a: "Löschgruppenfahrzeug", b: "Eingebauter Löschwasserbehälter mit einer nutzbaren Wassermenge von mindestens 1600 Liter vorhanden", c: "Schnellangriff Wasser vorhanden", d: "Feuerlöschkreiselpumpe FPN 10/1000 im Heck eingebaut", e: "Feuerlöschkreiselpumpe FPN 15/1000 als Frontpumpe angebaut"}, a: ["a", "b", "c"] },
        { id: 49, q: "49 Was verstehen Sie unter dem Begriff „Entlüftungszeit“?", o: {a: "Ein negativer Druck von - 0,8 bar muss in 30 Sekunden erreicht sein", b: "Erforderliche Zeit in Sekunden, um eine Pumpe einschließlich der Saugleitung zu entlüften und das Löschwas- ser mit positivem Druck bis zum Austrittsquerschnitt zu fördern.", c: "Er negative Druck darf von - 0,8 bar innerhalb einer Minute nicht mehr als 0,1 bar steigen"}, a: ["b"] },
        { id: 50, q: "50 Warum benötigen Feuerlöschkreiselpumpen eine Entlüftungseinrichtung?", o: {a: "Weil Ein- und Auslassventile nicht vorhanden sind", b: "Weil auch Schmutzwasser gefördert werden kann", c: "Weil die Drehzahl des Laufrades zu niedrig ist", d: "Weil der Luftdruck dem Wasserdruck entgegen wirkt", e: "Weil zwischen Laufrad und Pumpengehäuse kein luftdichter Abschluss ist"}, a: ["e"] },
        { id: 51, q: "51 Welcher Zusammenhang besteht zwischen Saughöhe und Luftdruck?", o: {a: "Hoher Luftdruck - hohe praktische Saughöhe", b: "Niederer Luftdruck – hohe praktische Saughöhe"}, a: ["a"] },
        { id: 52, q: "52 Welche Knoten, Schläge oder Stiche sind zum Aufbau einer Saugleitung nach den Richtlinien erforderlich?", o: {a: "Mastwurf", b: "Pfahlstich", c: "Halbschlag", d: "Schotenstich", e: "Doppelschlinge", f: "Zimmermannschlag"}, a: ["a", "c", "f"] },
        { id: 53, q: "53 Bei welcher Pumpe darf am Druckausgang beziehungsweise in der Druckleitung kein Absperr- organ angebracht werden?", o: {a: "Wasserstrahlpumpe", b: "Turbinentauchpumpe", c: "Tauchpumpe mit elektrischem Antrieb", d: "Exzenterschneckenpumpe", e: "Feuerlöschkreiselpumpe", f: "Fass- und Behälterpumpe"}, a: ["d"] },
        { id: 54, q: "54 Wovon hat sich der Maschinist zu überzeugen, bevor eine Einsatz- oder Unfallstelle verlassen wird?", o: {a: "Brandwache bereitgestellt", b: "Hydranten entwässert", c: "Verkehrssicherungsgerät ins Fahrzeug zurück gebracht", d: "Feuerlöschkreiselpumpe entwässert", e: "Absperreinrichtungen geschlossen und Blindkupplungen aufgesetzt", f: "Löschgruppe vollzählig"}, a: ["c", "d", "e"] },
        { id: 55, q: "55 Welche der folgenden Aussagen sind gemäß Unfallverhütungsvorschriften richtig?", o: {a: "Der Fahrer eines Feuerwehrfahrzeuges hat erst dann anzufahren, wenn der Gruppenführer dazu das Zeichen gibt", b: "Während der Fahrt ist für die Einhaltung der Straßenverkehrsvorschriften allein der Fahrer verantwortlich", c: "Den sonstigen Straßenverkehr auf das abgestellte Feuerwehrfahrzeug aufmerksam machen", d: "Nie mit überladenem Fahrzeug fahren", e: "Keine Schnelligkeit auf Kosten der Sicherheit", f: "Zusätzliche Schutzkleidung beim Betrieb der Motorkettensäge, des Trennschleifers und der hydraulischen Ret- tungsgeräte tragen", g: "Wer Sonderrechte nach § 35 Straßenverkehrsordnung in Anspruch nimmt, ist zu erhöhter Aufmerksamkeit verpflichtet"}, a: ["a", "b", "c", "d", "e", "f", "g"] },
        { id: 56, q: "56 Während des Betriebes einer Tragkraftspritze PFPN 10/1000 mit VW-Industriemotor leuchtet an- fangs die grüne Kontrolllampe auf und erlischt nach kurzer Zeit. Welche Ursachen können vor- liegen?", o: {a: "Der Öldruck hat sich verringert", b: "Die Glühbirne ist durchgebrannt", c: "Der Keilriemen ist abgerissen", d: "Die Stromzuführung zur Glühbirne ist defekt"}, a: ["b", "c", "d"] },
        { id: 57, q: "57 Welches sind Einflüsse, die verhindern, dass die theoretische Saughöhe von 10,33 Meter prak- tisch nicht erreicht werden kann?", o: {a: "Niederer Barometerstand als normal", b: "Entlüftungseinrichtung erzeugt kein vollkommenes Vakuum", c: "Wassertemperatur ist höher als 4° CO", d: "In der Saugleitung oder Feuerlöschkreiselpumpe sind Undichtigkeiten", e: "Bei der saugseitigen Wasserförderung treten hydraulische Verluste (Strömungs- und Reibungsverluste) auf", f: "Die Feuerlöschkreiselpumpe erreicht keine Nenndrehzahl mehr"}, a: ["a", "b", "c", "d", "e"] },
        { id: 58, q: "58 Wie kann sich der Maschinist helfen, wenn die Entlüftungseinrichtung der Feuerlöschkreisel- pumpe ausgefallen ist?", o: {a: "Bei Tanklöschfahrzeugen Feuerlöschkreiselpumpe und Saugleitung aus dem eingebauten Löschwasserbehälter füllen", b: "Gruppenführer benachrichtigen, damit eine Feuerlöschkreiselpumpe nachgefordert wird", c: "Pumpe und Saugleitung „von Hand“ auffüllen"}, a: ["a", "b", "c"] },
        { id: 59, q: "59 Worauf hat der Maschinist im Winter bei einer Flüssigkeitsring-Entlüftungseinrichtung zu ach- ten?", o: {a: "Dass die Feuerlöschkreiselpumpe nur im beheizten Feuerwehrhaus abgestellt wird", b: "Dass die Feuerlöschkreiselpumpe samt Entlüftungseinrichtung nach jedem Einsatz und jeder Übung entleert wird", c: "Dass die Entlüftungseinrichtung mit Frostschutzmittel aufgefüllt wird"}, a: ["b", "c"] },
        { id: 60, q: "60 Wer ist bei einer Einsatzfahrt für das Feuerwehrfahrzeug verantwortlich?", o: {a: "Der Maschinist als Fahrer des Feuerwehrfahrzeuges", b: "Der Gruppenführer", c: "Der Zugführer"}, a: ["a"] }
    ];

    let currentQuestions = [];
    let wrongQuestions = [];
    let currentIdx = 0;
    let score = 0;

    function shuffle(array) {
        for (let i = array.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [array[i], array[j]] = [array[j], array[i]];
        }
    }

    function startQuiz(questionsToUse) {
        currentQuestions = [...questionsToUse];
        shuffle(currentQuestions);
        currentIdx = 0;
        score = 0;
        wrongQuestions = [];
        displayQuestion();
    }

    function displayQuestion() {
        const item = currentQuestions[currentIdx];
        document.getElementById("progress").innerText = `Frage ${currentIdx + 1} von ${currentQuestions.length}`;
        document.getElementById("question-display").innerText = item.q;
        const optionsDiv = document.getElementById("options-display");
        optionsDiv.innerHTML = "";
        
        for (const [key, value] of Object.entries(item.o)) {
            const label = document.createElement("label");
            label.className = "option";
            label.innerHTML = `<input type="checkbox" value="${key}"> ${key}) ${value}`;
            label.onclick = function(e) {
                if(e.target.tagName !== "INPUT") {
                    const cb = this.querySelector("input");
                    cb.checked = !cb.checked;
                }
                this.classList.toggle("selected", this.querySelector("input").checked);
            };
            optionsDiv.appendChild(label);
        }
        
        document.getElementById("feedback").style.display = "none";
        document.getElementById("next-btn").style.display = "none";
        document.getElementById("override-btn").style.display = "none";
        document.getElementById("check-btn").style.display = "block";
    }

    function checkAnswer() {
        const item = currentQuestions[currentIdx];
        const selected = Array.from(document.querySelectorAll("#options-display input:checked")).map(i => i.value);
        const feedback = document.getElementById("feedback");
        
        const isCorrect = JSON.stringify(selected.sort()) === JSON.stringify(item.a.sort());
        
        feedback.style.display = "block";
        if (isCorrect) {
            feedback.className = "correct";
            feedback.innerText = "RICHTIG!";
            score++;
            document.getElementById("override-btn").style.display = "none";
        } else {
            feedback.className = "wrong";
            feedback.innerText = "FALSCH! Richtige Lösung: " + item.a.join(", ").toUpperCase();
            wrongQuestions.push(item);
            document.getElementById("override-btn").style.display = "block";
        }
        
        document.getElementById("check-btn").style.display = "none";
        document.getElementById("next-btn").style.display = "block";
    }

    function overrideCorrect() {
        const item = currentQuestions[currentIdx];
        wrongQuestions = wrongQuestions.filter(q => q.id !== item.id);
        score++;
        const feedback = document.getElementById("feedback");
        feedback.className = "correct";
        feedback.innerText = "Als RICHTIG gewertet!";
        document.getElementById("override-btn").style.display = "none";
    }

    function nextQuestion() {
        currentIdx++;
        if (currentIdx < currentQuestions.length) {
            displayQuestion();
        } else {
            showResults();
        }
    }

    function showResults() {
        const percent = Math.round((score / currentQuestions.length) * 100);
        let resultHTML = `<div class="stats"><h2>Ergebnis</h2>`;
        resultHTML += `<div class="percent-box">${percent}%</div>`;
        resultHTML += `<p>Du bist zu ${percent}% vorbereitet.</p>`;
        resultHTML += `<p>Korrekt: ${score} von ${currentQuestions.length}</p>`;
        
        if (wrongQuestions.length > 0) {
            resultHTML += `<button onclick="retryWrong()">Falsche Fragen wiederholen (${wrongQuestions.length})</button>`;
        }
        resultHTML += `<button style="background:#666;" onclick="location.reload()">Alles neu starten</button></div>`;
        
        document.getElementById("quiz-box").innerHTML = resultHTML;
        document.getElementById("progress").innerText = "Training beendet";
    }

    function retryWrong() {
        document.getElementById("quiz-box").innerHTML = `
            <span class="question-text" id="question-display"></span>
            <div id="options-display"></div>
            <button id="check-btn" onclick="checkAnswer()">Antwort prüfen</button>
            <div id="feedback"></div>
            <button id="override-btn" onclick="overrideCorrect()">Trotzdem als RICHTIG werten</button>
            <button id="next-btn" onclick="nextQuestion()">Nächste Frage</button>
        `;
        startQuiz(wrongQuestions);
    }

    startQuiz(allQuestions);
</script>

</body>
</html>
