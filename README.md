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

 Datenexploration:
 # Minimaler Preis im Datensatz
min_price = df['MV'].min()*1000

# Maximaler Preis im Datensatz
max_price = df['MV'].max()*1000

# Durchschnittlicher Preis im Datensatz
mean_price = (df['MV'].mean()*1000)

# Median-Preis im Datensatz
median_price = df['MV'].median()*1000
# Preisbezogene Standardabweichung
std_price = df['MV'].std()*1000

# Print-Statement der berechneten Variablen
print("Grundlegende Statistiken des Boston-Hauspreise Datensatzes:\n")
print("Minimaler Preis: ${}".format(min_price.round(2)))
print("Maximaler Preis: ${}".format(max_price.round(2)))
print("Durschnittlicher Preis: ${}".format(mean_price.round(2)))
print("Median-Preis: ${}".format(median_price.round(2)))
print("Preisbezogene Standardabweichung: ${}".format(std_price.round(2)))

print("Anzahl Einträge im Datensatzes: {}".format(df.shape[0]))
print("Anzahl Spalten im Datensatzes: {}".format(df.shape[1]))

#nullen detektieren, zwei möglichkeiten
df.isna().sum()

import seaborn as sns
sns.heatmap(df.isnull(), cbar=False)

falls Nullen vorhanden:
Imputationen (imputations)
Entfernungen (removals)

# Printe die Anzahl an einmaligen Werten pro Spalte
df.nunique()

# Print grundlegende Statistiken des gesamten Datensatzes
df.describe()

import seaborn as sns
import matplotlib.pyplot as plt
from scipy import stats

fig, axs = plt.subplots(ncols=7, nrows=2, figsize=(20, 10))
index = 0
axs = axs.flatten()
for label,content in df.items():
    sns.boxplot(y=label, data=df, ax=axs[index])
    index += 1
plt.tight_layout(pad=0.4, w_pad=0.5, h_pad=5.0)

--> boston regression notebook kucken
