# WordPress PDF Export — v3 Zusammenfassung
**Stand: 26.05.2026**

---

## Was macht das Script?

Exportiert alle WordPress Beiträge und Seiten (inkl. kennwortgeschützte)
direkt aus der MySQL-Datenbank als elegante PDF-Dateien — ohne HTTP,
ohne REST API, ohne wkhtmltopdf. Bilder werden als Originale vom
Dateisystem eingebettet, nie abgeschnitten, volle Auflösung.

---

## Dateien

| Datei | Zweck |
|---|---|
| `wordpress_pdf_export.py` | Exportiert jeden Beitrag als einzelne PDF |
| `wordpress_pdf_merge.py` | Fügt alle Einzel-PDFs zu einer Gesamt-PDF zusammen |

---

## Voraussetzungen Server

### System-Pakete
```bash
sudo apt install poppler-utils -y
```

### Python-Module installieren
```bash
pip install pymysql reportlab Pillow tqdm pdf2image --break-system-packages
```

### Fonts (bereits auf Debian/Ubuntu vorhanden)
/usr/share/fonts/truetype/dejavu/DejaVuSerif.ttf
/usr/share/fonts/truetype/dejavu/DejaVuSerif-Bold.ttf
/usr/share/fonts/truetype/dejavu/DejaVuSans.ttf
/usr/share/fonts/truetype/dejavu/DejaVuSans-Bold.ttf
Falls nicht vorhanden:
```bash
sudo apt install fonts-dejavu -y
```

---

## Module Übersicht

| Modul | Zweck | Installation |
|---|---|---|
| `pymysql` | MySQL-Datenbankverbindung | pip |
| `reportlab` | PDF direkt erstellen (kein HTML) | pip |
| `Pillow` | Bildgröße auslesen & skalieren | pip |
| `tqdm` | Fortschrittsbalken | pip |
| `pdf2image` | Bild-Abschnitt-Erkennung | pip |
| `poppler-utils` | Systemabhängigkeit für pdf2image | apt |
| `html` | HTML-Entities dekodieren | stdlib |
| `html.parser` | WordPress HTML parsen | stdlib |
| `re` | Regex für Shortcodes etc. | stdlib |
| `datetime` | Datum formatieren | stdlib |

---

## Konfiguration im Script

```python
# wordpress_pdf_export.py — oben im Script anpassen:
WP_PFAD        = "/var/www/wordpress"            # WordPress-Verzeichnis
AUSGABE_ORDNER = "/home/admin42/wordpress_pdfs"  # Zielordner für PDFs
```

```python
# wordpress_pdf_merge.py — oben im Script anpassen:
EINGABE_ORDNER = "/home/admin42/wordpress_pdfs"       # Ordner mit Einzel-PDFs
AUSGABE_DATEI  = "/home/admin42/wordpress_gesamt.pdf" # Gesamt-PDF
SORTIERUNG     = "id"                                 # Chronologisch nach Post-ID
```

---

## Ausführen

```bash
# 1. Einzelne PDFs exportieren
cd /var/www/wordpress
python3 /home/admin42/wordpress_pdf_export.py

# 2. Alles zu einer Gesamt-PDF zusammenfügen
python3 /home/admin42/wordpress_pdf_merge.py
```

---

## Features v3

- ✅ Direkter MySQL-Zugriff — kein HTTP, kein REST API
- ✅ wp-config.php wird automatisch ausgelesen
- ✅ Kennwortgeschützte Beiträge vollständig exportiert
- ✅ Bilder als Originale eingebettet (kein Vorschaubild)
- ✅ Altes WP-Format (.thumbnail.jpg) + neues Format (-1024x768.jpg)
- ✅ Gross-/Kleinschreibung der Dateiendung (.jpg / .JPG)
- ✅ Bilder nie abgeschnitten — ReportLab KeepTogether
- ✅ Bildunterschriften (alt-Text) werden übernommen
- ✅ Shortcodes und WP-Kommentare entfernt
- ✅ Fettschrift und Kursiv aus dem Original erhalten
- ✅ Elegantes Layout — DejaVu Serif, Blocksatz
- ✅ Kopfzeile: Datum links, Titel rechts
- ✅ Fusszeile: Seitenzahl zentriert
- ✅ Akzentfarbe Braun — Tagebuch-Feeling
- ✅ Datum auf Deutsch unter dem Titel
- ✅ Chronologische Sortierung (ältester Beitrag zuerst)
- ✅ Fortschrittsbalken mit tqdm
- ✅ Abschlussbericht mit Fehlerliste
