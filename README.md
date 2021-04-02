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

Zuerst Daten cleansen

---> für oc_player-Dataset: allenfalls schon logins aggregieren: Logins pro Spieler. Logins pro Tageszeit (ranges?), Logins pro PLZ, Entwicklung pro PLZ?

Dann explorieren (Quelle: 1_EDA_crime):
verschiedene Explorationstechniken:
# Minimale Anzahl Fälle pro Stunde
min_cons = df_orig['Counts_per_hour'].min()

# Maximale Anzahl Fälle pro Stunde
max_cons = df_orig['Counts_per_hour'].max()

# Durchschnittliche Anzahl Fälle pro Stunde
mean_cons = (df_orig['Counts_per_hour'].mean())

# Median der Anzahl Fälle pro Stunde
median_cons = df_orig['Counts_per_hour'].median()

# Standardabweichung Anzahl Fälle pro Stunde
std_cons = df_orig['Counts_per_hour'].std()

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

# Funktion zur Berechnung der Whiskers:
def iqr_fences(column):
    Q1 = column.quantile(0.25)
    Q3 = column.quantile(0.75)
    IQR = Q3 - Q1
    lowerFence = Q1 - (1.5 * IQR)
    upperFence = Q3 + (1.5 * IQR)
    return lowerFence, upperFence
    
    for label, content in df.items():
        Q1 = content.quantile(0.25)
        Q3 = content.quantile(0.75)
        IQR = Q3 - Q1
        v_col = content[(content <= Q1 - 1.5 * IQR) | (content >= Q3 + 1.5 * IQR)]
        perc = np.shape(v_col)[0] * 100.0 / np.shape(df)[0]
        print("Spalte {} Anteil an Outliers = {}%".format(label, round(perc, 2)))
        
        for i in df.columns:
    result = iqr_fences(df[i])
    lowerFence = result[0]
    upperFence = result[1]
    print("Lower Fence of {}: {}".format(i,lowerFence))
    print("Upper Fence of {}: {}".format(i, upperFence))
    
    Verteilung --> zuerst Variabeln ohne Zielvariabel untersuchen:
    from scipy.stats import norm
features = set(df.columns) - set(['MEDV'])
fig, axs = plt.subplots(ncols=7, nrows=2, figsize=(16, 6))
for i, f in enumerate(features):
    sns.distplot(df[f], fit=norm, ax=axs[int(i/7), i%7])

jetzt Zielvariabel untersuchen:
sns.set(rc={'figure.figsize':(11.7,8.27)})
sns.distplot(df['MV'], bins=30, kde=True)
plt.show()

plt.figure(figsize=(10, 8))
ax = sns.boxplot(y="MV", data=df)
ax = sns.swarmplot(y="MV", data=df, color=".25")

lowerFence, upperFence = iqr_fences(df['MV'])

#Print-Statement für Whiskers
print("Min-Whisker: ${}".format((lowerFence*1000).round(2)))
print("Max-Whisker: ${}".format((upperFence*1000).round(2)))

# Print Anzahl der Datensätze, welche überhalb dem Max-Whisker liegen:
print(len(df[df['MV'] > 36.9625]))
print(len(df[df['MV'] < 5.0625]))

Analyse der extremen in den Zielvariabeln
Q1 = df['MV'].quantile(0.25)
Q3 = df['MV'].quantile(0.75)
IQR = Q3 - Q1
MaxW = Q3 + 1.5*IQR
MinW = Q1 - 1.5*IQR

MIN_MV = df[df['MV']<MinW]
MIN_MV

Korrelationen anzeigen:
corr = df[["AGE", "LSTAT", "TAX", "MV"]]
corr[corr.columns[:]].corr()['MV'][:]

Zur genaueren Betrachtung der einzelnen Verteilungen verwenden wir drei verschiedene Grafiken:

Verteilungen der einzelnen Variablen vor Standardisation
Verteilungen mit MinMaxScaler
Verteilungen durch StandardScaler:
ax.set_title("Ohne Scaler")

fig, ax = plt.subplots(ncols=1, figsize=(10, 8))
sns.kdeplot(corr['AGE'], ax=ax,)
sns.kdeplot(corr['LSTAT'], ax=ax)
sns.kdeplot(corr['TAX'], ax=ax)
sns.kdeplot(corr['MV'], ax=ax)

# Verschiebung der Verteilungen mit dem MinMaxScaler von sklearn
from sklearn import preprocessing

col_names = ["AGE", "LSTAT", "TAX", "MV"]

mm_scaler = preprocessing.MinMaxScaler()
df_mm = mm_scaler.fit_transform(corr)

df_mm = pd.DataFrame(df_mm, columns=col_names)

fig, (ax1) = plt.subplots(ncols=1, figsize=(10, 8))
ax1.set_title('After MinMaxScaler')

sns.kdeplot(df_mm['AGE'], ax=ax1,)
sns.kdeplot(df_mm['LSTAT'], ax=ax1)
sns.kdeplot(df_mm['TAX'], ax=ax1)
sns.kdeplot(df_mm['MV'], ax=ax1)

# Verschiebung der Verteilungen mit dem StandardScaler von sklearn
from sklearn import preprocessing

col_names = ["AGE", "LSTAT", "TAX", "MV"]

st_scaler = preprocessing.StandardScaler()
df_st = st_scaler.fit_transform(corr)

df_st = pd.DataFrame(df_mm, columns=col_names)

fig, (ax1) = plt.subplots(ncols=1, figsize=(10, 8))
ax1.set_title('After StandardScaler')

sns.kdeplot(df_mm['AGE'], ax=ax1,)
sns.kdeplot(df_mm['LSTAT'], ax=ax1)
sns.kdeplot(df_mm['TAX'], ax=ax1)
sns.kdeplot(df_mm['MV'], ax=ax1)



--> boston regression notebook kucken

dann feature selection: (1_EDA_crime)
import seaborn as sns
df_heatmap = df_orig.corr().round(2)
f, ax = plt.subplots(figsize=(15, 10))
sns.heatmap(df_heatmap, annot=True, annot_kws={'size': 12}, linewidths=.5)
