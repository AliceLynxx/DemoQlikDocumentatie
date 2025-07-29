# Datamodel Documentatie

## Overzicht

Het datamodel bestaat uit 7 hoofdtabellen die samen een star schema vormen voor dealer sales analyse.

## Tabellen

### 1. Revenue (Fact Table)
**Bron:** `Dealer Sales.xlsx` - sheet "Revenue"  
**Functie:** Centrale fact table met sales transacties

| Veld | Type | Beschrijving |
|------|------|-------------|
| Dealer_ID | ID | Foreign key naar Dealer tabel |
| Model_ID | ID | Foreign key naar Product tabel |
| Branch_ID | ID | Foreign key naar Branch tabel |
| Date_ID | ID | Foreign key naar Date tabel |
| Units_Sold | Numeriek | Aantal verkochte eenheden |
| Revenue | Numeriek | Omzet in valuta |

### 2. Date (Dimension)
**Bron:** `Dealer Sales.xlsx` - sheet "Date"  
**Functie:** Tijd dimensie voor temporele analyse

| Veld | Type | Beschrijving |
|------|------|-------------|
| Date_ID | ID | Primary key |
| Date | Datum | Datum waarde |
| year | Numeriek | Jaar |
| Month | Tekst/Numeriek | Maand |
| Quarter | Tekst/Numeriek | Kwartaal |
| Date_ID1 | ID | Alternatieve datum ID |

### 3. Branch (Dimension)
**Bron:** `Dealer Sales.xlsx` - sheet "Branch"  
**Functie:** Branch/vestiging informatie

| Veld | Type | Beschrijving |
|------|------|-------------|
| Branch_ID | ID | Primary key |
| Branch_NM | Tekst | Branch naam |

*Note: Country_Name veld is uitgecommentarieerd*

### 4. Dealer (Dimension)
**Bron:** `Dealer Sales.xlsx` - sheet "Dealer"  
**Functie:** Dealer informatie en locatie

| Veld | Type | Beschrijving |
|------|------|-------------|
| Dealer_ID | ID | Primary key |
| Dealer_NM | Tekst | Dealer naam |
| Location_ID | ID | Locatie identifier |
| Location_NM | Tekst | Locatie naam |
| Country_ID | ID | Foreign key naar Country |

### 5. Product (Dimension)
**Bron:** `Dealer Sales.xlsx` - sheet "Procduct" (let op: typo in sheet naam)  
**Functie:** Product en model informatie

| Veld | Type | Beschrijving |
|------|------|-------------|
| Product_ID | ID | Primary key |
| Product_Name | Tekst | Product naam |
| Model_ID | ID | Model identifier |
| Model_Name | Tekst | Model naam |
| E | Onbekend | Onbekende kolom |
| F | Onbekend | Onbekende kolom |
| G | Onbekend | Onbekende kolom |

### 6. Country (Dimension)
**Bron:** `Dealer Sales.xlsx` - sheet "Country"  
**Functie:** Land informatie

| Veld | Type | Beschrijving |
|------|------|-------------|
| Country_ID | ID | Primary key |
| Country_Name | Tekst | Land naam |

### 7. Country2 (Lookup Table)
**Bron:** `new countrie names.xlsx` - sheet "Sheet1"  
**Functie:** Country name mapping/translation

| Veld | Type | Beschrijving |
|------|------|-------------|
| Country_Name | Tekst | Originele country naam |
| Country_Name2 | Tekst | Alternatieve/nieuwe country naam |

## Relaties

```
Revenue (Fact)
├── Date_ID → Date.Date_ID
├── Branch_ID → Branch.Branch_ID  
├── Dealer_ID → Dealer.Dealer_ID
└── Model_ID → Product.Model_ID

Dealer
└── Country_ID → Country.Country_ID

Country
└── Country_Name → Country2.Country_Name (lookup)
```

## Aandachtspunten

1. **Typo in sheet naam:** "Procduct" in plaats van "Product"
2. **Onbekende kolommen:** E, F, G in Product tabel hebben geen duidelijke betekenis
3. **Country mapping:** Country2 tabel lijkt gebruikt te worden voor naam harmonisatie
4. **Uitgecommentarieerde velden:** Country_Name in Branch tabel is uitgeschakeld