# AWS EC2 CLI-Befehle Referenz

## Instanz-Management

### Instanzen auflisten
```bash
# Alle Instanzen auflisten
aws ec2 describe-instances

# Nur laufende Instanzen
aws ec2 describe-instances --filters Name=instance-state-name,Values=running

# Instanzen mit bestimmtem Tag
aws ec2 describe-instances --filters "Name=tag:Environment,Values=Production"

# Übersichtliche Ausgabe wichtiger Informationen
aws ec2 describe-instances --query 'Reservations[*].Instances[*].[InstanceId,InstanceType,State.Name,PublicIpAddress]' --output table
```

### Instanzen starten und stoppen
```bash
# Instanz starten
aws ec2 start-instances --instance-ids i-1234567890abcdef0

# Instanz stoppen
aws ec2 stop-instances --instance-ids i-1234567890abcdef0

# Instanz neustarten
aws ec2 reboot-instances --instance-ids i-1234567890abcdef0

# Instanz terminieren
aws ec2 terminate-instances --instance-ids i-1234567890abcdef0
```

### Neue Instanz erstellen
```bash
# Basis-Launch einer Instanz
aws ec2 run-instances \
    --image-id ami-12345678 \
    --instance-type t2.micro \
    --key-name mein-keypair \
    --security-group-ids sg-903004f8

# Mit Userdata und Tags
aws ec2 run-instances \
    --image-id ami-12345678 \
    --instance-type t2.micro \
    --user-data file://startup-script.sh \
    --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=TestServer}]'
```

## AMI-Management

### AMIs verwalten
```bash
# AMIs auflisten
aws ec2 describe-images --owners self

# AMI erstellen
aws ec2 create-image \
    --instance-id i-1234567890abcdef0 \
    --name "Meine-AMI" \
    --description "Meine eigene AMI"

# AMI löschen
aws ec2 deregister-image --image-id ami-12345678
```

## Security Groups

### Security Groups verwalten
```bash
# Security Groups auflisten
aws ec2 describe-security-groups

# Neue Security Group erstellen
aws ec2 create-security-group \
    --group-name meine-sg \
    --description "Meine Security Group"

# Regel hinzufügen
aws ec2 authorize-security-group-ingress \
    --group-id sg-903004f8 \
    --protocol tcp \
    --port 22 \
    --cidr 0.0.0.0/0

# Regel entfernen
aws ec2 revoke-security-group-ingress \
    --group-id sg-903004f8 \
    --protocol tcp \
    --port 22 \
    --cidr 0.0.0.0/0
```

## Volumes und Snapshots

### EBS Volumes
```bash
# Volumes auflisten
aws ec2 describe-volumes

# Volume erstellen
aws ec2 create-volume \
    --size 8 \
    --availability-zone eu-central-1a \
    --volume-type gp3

# Volume anhängen
aws ec2 attach-volume \
    --volume-id vol-1234567890abcdef0 \
    --instance-id i-1234567890abcdef0 \
    --device /dev/sdf
```

### Snapshots
```bash
# Snapshot erstellen
aws ec2 create-snapshot \
    --volume-id vol-1234567890abcdef0 \
    --description "Mein Backup"

# Snapshots auflisten
aws ec2 describe-snapshots --owner-ids self

# Snapshot löschen
aws ec2 delete-snapshot --snapshot-id snap-1234567890abcdef0
```

## Key Pairs

```bash
# Key Pairs auflisten
aws ec2 describe-key-pairs

# Neues Key Pair erstellen
aws ec2 create-key-pair --key-name MeinKeyPair --query 'KeyMaterial' --output text > MeinKeyPair.pem

# Key Pair löschen
aws ec2 delete-key-pair --key-name MeinKeyPair
```

## Tags

```bash
# Tags hinzufügen
aws ec2 create-tags \
    --resources i-1234567890abcdef0 \
    --tags Key=Environment,Value=Production

# Tags entfernen
aws ec2 delete-tags \
    --resources i-1234567890abcdef0 \
    --tags Key=Environment
```

## Nützliche Filter und Queries

```bash
# Instanzen nach Name filtern
aws ec2 describe-instances \
    --filters "Name=tag:Name,Values=*webserver*"

# Gestoppte Instanzen finden
aws ec2 describe-instances \
    --filters "Name=instance-state-name,Values=stopped"

# Instanzen nach Typ filtern
aws ec2 describe-instances \
    --filters "Name=instance-type,Values=t2.micro"
```

## Best Practices
- Nutzen Sie `--dry-run`, um Befehle zu testen
- Verwenden Sie Tags für bessere Organisation
- Sichern Sie Key Pairs an einem sicheren Ort
- Dokumentieren Sie Ihre Security Group Änderungen
- Erstellen Sie regelmäßige Snapshots wichtiger Volumes
- Nutzen Sie Filters und Queries für bessere Übersicht
