# Dmx4allMetaTool (DMX4ALL Meta tool)

Zentrale SEO-Meta-Angaben für Shopware 6.7 und 6.8 (serverless App, kein App-Server, kein Node/npm).

## Was die App macht
Sie überschreibt nur die Meta-Blöcke der `layout/meta.html.twig`. Ist etwas nicht konfiguriert,
kommt der Shopware-Standard (bzw. das, was andere Plugins ausgeben) unverändert durch.

**Globale Einstellungen** (Erweiterungen > Meine Erweiterungen > Konfigurieren, je Verkaufskanal):
- Titel-Suffix (z. B. `| DMX4ALL`), optional auch auf der Startseite; wird nicht doppelt angehängt
- Fallback-Meta-Beschreibung für Seiten ohne eigene Beschreibung
- Standard-Robots für Produkte, Kategorien und Landingpages
- noindex,follow für Suchergebnisse, gefilterte/sortierte Listen und optional Listenseite 2+
- copyrightYear (Meta-Tag) als Text setzbar, z. B. `2026` oder `2019-2026`; optional aktuelles Jahr, wenn das Feld leer ist
- revisit-after als Text setzbar; zwei Android-Chrome-Icons (192x192, 512x512, PNG) per Bildauswahl aus der Medienverwaltung

**Felder pro Seite** (Zusatzfelder-Set "DMX4ALL Meta tool (SEO)" an Produkt, Kategorie, Landingpage):
Robots, Canonical-URL, Titel-Suffix aus, Open-Graph-Titel, -Beschreibung und -Bild-URL.

Meta-Titel, Meta-Beschreibung und Keywords pflegst du weiter im SEO-Bereich von Shopware selbst.

## Reihenfolge
- Robots: Feld an der Seite > Suche/Filter/Pagination-Regeln > Standard-Robots > Shopware
- og:image: Feld an der Seite > Shopware-Standard
- Seiten außerhalb von Produkt/Kategorie/Landingpage (Konto, Kasse ...) behalten ihre Robots von Shopware.

## Struktur
- manifest.xml – Name, Version, Kompatibilität (>=6.7.0 <6.9.0), Zusatzfelder
- Resources/config/config.xml – globale Einstellungen
- Resources/views/storefront/layout/meta.html.twig – Twig-Override

Config im Twig: `config('Dmx4allMetaTool.config.<key>')`

## ZIP bauen
    ./pack.sh
Erzeugt `../Dmx4allMetaTool-<Version>.zip` (Version aus manifest.xml). rename.sh/pack.sh landen nicht im ZIP.

## Cache
Alle Konfigurationsfelder sind `cache-relevant`. Nach dem Speichern invalidiert Shopware den Storefront-HTTP-Cache selbst, ein manuelles `bin/console cache:clear` ist dafür nicht nötig.

## Installation
    bin/console app:install --activate Dmx4allMetaTool
    bin/console cache:clear
oder ZIP im Admin unter Erweiterungen > Meine Erweiterungen hochladen.
Nach Änderungen an Zusatzfeldern ggf. `bin/console app:refresh Dmx4allMetaTool` (bzw. im Admin aktualisieren).
