# Technische Documentatie

## Script Structuur

Het QlikSense script is opgedeeld in 2 hoofdsecties:

### 1. Main Tab
**Functie:** Systeem configuratie en variabelen

#### Configuratie Settings
- **Locale:** en-US (Engels - Verenigde Staten)
- **Datum formaat:** M/D/YYYY (Amerikaanse notatie)
- **Tijd formaat:** h:mm:ss TT (12-uurs met AM/PM)
- **Valuta:** Dollar notatie ($#,##0.00)
- **Decimaal scheidingsteken:** Punt (.)
- **Duizendtal scheidingsteken:** Komma (,)

#### Week/Maand Settings
- **Eerste dag van de week:** Zaterdag (6)
- **Eerste maand van het jaar:** Januari (1)
- **Reference day:** Zondag (0)
- **Broken weeks:** Ingeschakeld (1)

### 2. Section Tab
**Functie:** Data loading en transformatie

## Data Bronnen

### Primaire Bron
**Bestand:** `Dealer Sales.xlsx`  
**Locatie:** `lib://Data - Prorail (vps-17006-1508_wnieuwenhuizen)/`  
**Format:** Excel (ooxml)

### Secundaire Bron
**Bestand:** `new countrie names.xlsx`  
**Locatie:** `lib://AttachedFiles/`  
**Format:** Excel (ooxml)

## Load Statements

Alle tabellen worden geladen met:
- `embedded labels` - Eerste rij bevat kolomnamen
- `ooxml` format - Excel 2007+ formaat
- Specifieke sheet referenties per tabel

## Performance Overwegingen

### Positieve Aspecten
1. **Star schema design** - Optimaal voor analytics
2. **Separate dimensie tabellen** - Goede normalisatie
3. **ID-based relationships** - Efficiënte joins

### Verbeterpunten
1. **Geen WHERE clausules** - Alle data wordt geladen
2. **Geen data validatie** - Geen checks op data kwaliteit
3. **Geen incremental load** - Volledige reload elke keer
4. **Geen error handling** - Geen afhandeling van load fouten

## Aanbevolen Verbeteringen

### 1. Data Validatie
```qlik
// Voorbeeld: Check voor lege waarden
Revenue:
LOAD *
FROM [...]
WHERE Len(Trim(Dealer_ID)) > 0 
  AND Len(Trim(Revenue)) > 0;
```

### 2. Error Handling
```qlik
// Voorbeeld: Error logging
SET ErrorMode = 0;
// Load statements...
IF ScriptError THEN
    TRACE 'Error occurred during load';
END IF
```

### 3. Incremental Loading
```qlik
// Voorbeeld: Datum-gebaseerde incremental load
LET vLastReload = Date(Today()-1);
Revenue:
LOAD *
FROM [...]
WHERE Date >= '$(vLastReload)';
```

### 4. Data Transformatie
```qlik
// Voorbeeld: Datum parsing
Date:
LOAD
    Date_ID,
    Date(Date#(Date, 'M/D/YYYY')) as Date,
    Year(Date#(Date, 'M/D/YYYY')) as Year,
    Month(Date#(Date, 'M/D/YYYY')) as Month
FROM [...];
```

## Security Overwegingen

- Data library paths zijn zichtbaar in script
- Geen data masking of filtering op gebruiker niveau
- Volledige dataset toegankelijk voor alle gebruikers

## Monitoring

**Aanbevolen metrics:**
- Script execution time
- Data volume per tabel
- Failed reload alerts
- Data freshness indicators