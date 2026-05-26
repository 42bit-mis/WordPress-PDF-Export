# WordPress-PDF-Export
Exportiert alle WordPress Beiträge und Seiten (inkl. kennwortgeschützte) direkt aus der MySQL-Datenbank als elegante PDF-Dateien — ohne HTTP, ohne REST API, ohne wkhtmltopdf. Bilder werden als Originale vom Dateisystem eingebettet, nie abgeschnitten, volle Auflösung.


WordPress PDF Export — v3 Zusammenfassung
Stand: 26.05.2026

Was macht das Script?
Exportiert alle WordPress Beiträge und Seiten (inkl. kennwortgeschützte)
direkt aus der MySQL-Datenbank als elegante PDF-Dateien — ohne HTTP,
ohne REST API, ohne wkhtmltopdf. Bilder werden als Originale vom
Dateisystem eingebettet, nie abgeschnitten, volle Auflösung.

Dateien
DateiZweckwordpress_pdf_export.pyExportiert jeden Beitrag als einzelne PDFwordpress_pdf_merge.pyFügt alle Einzel-PDFs zu einer Gesamt-PDF zusammen

Voraussetzungen Server
System-Pakete
bashsudo apt install poppler-utils -y
Python-Module installieren
bashpip install pymysql reportlab Pillow tqdm pdf2image --break-system-packages
Fonts (bereits auf Debian/Ubuntu vorhanden)
/usr/share/fonts/truetype/dejavu/DejaVuSerif.ttf
/usr/share/fonts/truetype/dejavu/DejaVuSerif-Bold.ttf
/usr/share/fonts/truetype/dejavu/DejaVuSans.ttf
/usr/share/fonts/truetype/dejavu/DejaVuSans-Bold.ttf
Falls nicht vorhanden:
bashsudo apt install fonts-dejavu -y

Module Übersicht
ModulZweckInstallationpymysqlMySQL-DatenbankverbindungpipreportlabPDF direkt erstellen (kein HTML)pipPillowBildgröße auslesen & skalierenpiptqdmFortschrittsbalkenpippdf2imageBild-Abschnitt-Erkennungpippoppler-utilsSystemabhängigkeit für pdf2imageapthtmlHTML-Entities dekodierenstdlibhtml.parserWordPress HTML parsenstdlibreRegex für Shortcodes etc.stdlibdatetimeDatum formatierenstdlib

Konfiguration im Script
python# wordpress_pdf_export.py — oben im Script anpassen:
WP_PFAD        = "/var/www/wordpress"         # WordPress-Verzeichnis
AUSGABE_ORDNER = "/home/admin42/wordpress_pdfs"  # Zielordner für PDFs
python# wordpress_pdf_merge.py — oben im Script anpassen:
EINGABE_ORDNER = "/home/admin42/wordpress_pdfs"  # Ordner mit Einzel-PDFs
AUSGABE_DATEI  = "/home/admin42/wordpress_gesamt.pdf"  # Gesamt-PDF
SORTIERUNG     = "id"                             # Chronologisch nach Post-ID

Ausführen
bash# 1. Einzelne PDFs exportieren
cd /var/www/wordpress
python3 /home/admin42/wordpress_pdf_export.py

# 2. Alles zu einer Gesamt-PDF zusammenfügen
python3 /home/admin42/wordpress_pdf_merge.py

Features v3

✅ Direkter MySQL-Zugriff — kein HTTP, kein REST API
✅ wp-config.php wird automatisch ausgelesen
✅ Kennwortgeschützte Beiträge vollständig exportiert
✅ Bilder als Originale eingebettet (kein Vorschaubild)
✅ Altes WP-Format (.thumbnail.jpg) + neues Format (-1024x768.jpg)
✅ Gross-/Kleinschreibung der Dateiendung (.jpg / .JPG)
✅ Bilder nie abgeschnitten — ReportLab KeepTogether
✅ Bildunterschriften (alt-Text) werden übernommen
✅ Shortcodes und WP-Kommentare entfernt
✅ Fettschrift und Kursiv aus dem Original erhalten
✅ Elegantes Layout — DejaVu Serif, Blocksatz
✅ Kopfzeile: Datum links, Titel rechts
✅ Fusszeile: Seitenzahl zentriert
✅ Akzentfarbe Braun — Tagebuch-Feeling
✅ Datum auf Deutsch unter dem Titel
✅ Chronologische Sortierung (ältester Beitrag zuerst)
✅ Fortschrittsbalken mit tqdm
✅ Abschlussbericht mit Fehlerlist
