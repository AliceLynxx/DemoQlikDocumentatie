# QlikSense Test App Documentatie

Deze repository bevat de documentatie en het script van de QlikSense Test applicatie.

## Overzicht

**App ID:** `2a1e9e8b-80d1-438e-94dc-57fd0d7a720a`  
**App Naam:** Test  
**Doel:** Demo/test applicatie voor dealer sales data analyse

## Inhoud Repository

- `qlik_script.qvs` - Het complete QlikSense load script
- `data_model.md` - Documentatie van het datamodel
- `sheets_overview.md` - Overzicht van de sheets in de app
- `technical_documentation.md` - Technische documentatie

## Quick Start

1. Bekijk het [datamodel](data_model.md) voor een overzicht van de data structuur
2. Raadpleeg de [technische documentatie](technical_documentation.md) voor implementatie details
3. Het originele script staat in `qlik_script.qvs`

## Data Bronnen

De applicatie gebruikt data uit:
- `Dealer Sales.xlsx` - Hoofddata bestand met sales informatie
- `new countrie names.xlsx` - Aanvullende country mapping

Beide bestanden worden geladen vanuit de QlikSense data library.