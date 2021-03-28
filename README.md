# CAS_DAENG
MIKO-pixel_CAS_DAENG
Projektarbeit "Vertriebsdaten"

Zielsetzung: 
Know your customer zur Risikominimierung und Umsatzmaximierung, Kundenbindung

Problemstellung:
Neuer Markt, Kundendaten werden noch unzureichend ausgewertet

Methoden:
Regression zu Kundenentwicklung
Klassifikation der Kunden

Herkunft der Daten:
DER Spielerplattform des Anbieters. Export per Datagrip in csv

EDA:
Dataset mit 12GB zu gross, Schritt 1 somit Spalten auswählen

EDA 1:
Import erste 100'000 Zeilen des csv in ein Dataframe mittels panda in jupyter lab
<code>
Df.info() zum erkennen der Formate
Export in reduziertes csv
Checks per openrefine, was in den einzelnen Spalten enrhalten ist (v.a. semantisch)
--> allenfalls facets in python?
Entscheiden, welche Spalten entfernt werden sollen bzw. Welche verbleiben
Zu entfernende Spaltenname im xls query editor per M kopiert, in Textfile eingefügt
Zu entfernende Spalten im Texteditor zu Liste mit Komma und Anführungszeichen per suchen und ersetzen erstellt
Zurück ins jupyter lab, um df_drop([Liste]) Spalten zu entfernen
Indem neues dataframe erstwllt wird df_red
Df zu csv exportieren als Zwischensicherung, csv bereit für EDA
