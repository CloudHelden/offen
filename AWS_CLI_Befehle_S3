# AWS S3 CLI-Befehle Referenz

## Bucket-Operationen

### Bucket erstellen
```bash
# Bucket erstellen
aws s3 mb s3://bucket-name

# Bucket in bestimmter Region erstellen
aws s3 mb s3://bucket-name --region eu-central-1
```

### Bucket auflisten
```bash
# Alle Buckets anzeigen
aws s3 ls

# Inhalt eines Buckets anzeigen
aws s3 ls s3://bucket-name

# Inhalt eines Buckets mit Unterordnern anzeigen
aws s3 ls s3://bucket-name --recursive
```

### Bucket löschen
```bash
# Leeren Bucket löschen
aws s3 rb s3://bucket-name

# Bucket mit Inhalt löschen (Force)
aws s3 rb s3://bucket-name --force
```

## Datei-Operationen

### Dateien hochladen
```bash
# Einzelne Datei hochladen
aws s3 cp datei.txt s3://bucket-name/

# Datei mit spezifischem Namen hochladen
aws s3 cp datei.txt s3://bucket-name/neuer-name.txt

# Ordner hochladen
aws s3 cp ordner/ s3://bucket-name/ordner/ --recursive

# Datei öffentlich hochladen
aws s3 cp datei.txt s3://bucket-name/ --acl public-read
```

### Dateien herunterladen
```bash
# Einzelne Datei herunterladen
aws s3 cp s3://bucket-name/datei.txt ./

# Ganzen Ordner herunterladen
aws s3 cp s3://bucket-name/ordner/ ./lokaler-ordner/ --recursive
```

### Dateien synchronisieren
```bash
# Lokalen Ordner mit S3 synchronisieren
aws s3 sync lokaler-ordner/ s3://bucket-name/ordner/

# S3 mit lokalem Ordner synchronisieren
aws s3 sync s3://bucket-name/ordner/ lokaler-ordner/

# Nur neuere Dateien synchronisieren
aws s3 sync s3://bucket-name/ordner/ lokaler-ordner/ --delete
```

### Dateien verschieben/löschen
```bash
# Datei in S3 verschieben
aws s3 mv s3://bucket-name/alte-position/datei.txt s3://bucket-name/neue-position/

# Datei löschen
aws s3 rm s3://bucket-name/datei.txt

# Ordner löschen
aws s3 rm s3://bucket-name/ordner/ --recursive
```

## Zusätzliche Operationen

### Versionierung
```bash
# Alle Versionen einer Datei anzeigen
aws s3api list-object-versions --bucket bucket-name --prefix datei.txt

# Spezifische Version herunterladen
aws s3api get-object --bucket bucket-name --key datei.txt --version-id VERSION-ID lokale-datei.txt
```

### Metadaten und Tags
```bash
# Objekt-Metadaten anzeigen
aws s3api head-object --bucket bucket-name --key datei.txt

# Tags hinzufügen
aws s3api put-object-tagging --bucket bucket-name --key datei.txt --tagging 'TagSet=[{Key=Projekt,Value=Test}]'
```

### Bucket-Policies
```bash
# Bucket-Policy anzeigen
aws s3api get-bucket-policy --bucket bucket-name

# Bucket-Policy setzen (aus Datei)
aws s3api put-bucket-policy --bucket bucket-name --policy file://policy.json
```

## Nützliche Flags und Optionen

```bash
--dryrun                    # Zeigt an, was passieren würde, ohne es auszuführen
--exclude "*.txt"           # Bestimmte Dateien ausschließen
--include "wichtig.txt"     # Bestimmte Dateien einschließen
--storage-class STANDARD_IA # Speicherklasse festlegen
--sse                      # Server-seitige Verschlüsselung aktivieren
--acl private              # Zugriffsrechte setzen
--recursive                # Rekursive Operation
--delete                   # Löscht Dateien am Ziel, die nicht an der Quelle existieren
```

## Best Practices
- Verwenden Sie `--dryrun` vor großen Operationen
- Nutzen Sie `sync` statt `cp` für große Verzeichnisse
- Setzen Sie sinnvolle `--exclude` und `--include` Patterns
- Überprüfen Sie Berechtigungen vor öffentlichen Operationen
- Nutzen Sie Versioning für wichtige Buckets
