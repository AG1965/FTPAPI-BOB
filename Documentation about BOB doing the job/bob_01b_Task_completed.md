# FTPAPI - Analyse der Anwendung

## Übersicht
**FTPAPI** ist eine umfassende FTP-Client-Bibliothek für IBM i (AS/400), entwickelt von Scott C. Klement seit 2001. Die Anwendung ermöglicht programmatischen FTP-Zugriff aus ILE RPG-Programmen.

## Technische Details

### Version & Lizenz
- **Aktuelle Version**: 2.9 (Release-Datum: 2025-10-08)
- **Lizenz**: BSD 2-Clause License (Open Source)
- **Mindestanforderung**: IBM i V7R1M0 oder höher
- **Copyright**: 2001-2025 Scott C. Klement

### Architektur

**Hauptkomponenten:**
1. **FTPAPIR4** - Haupt-Service-Program (RPG-Modul)
2. **FTPTCP** - Netzwerk-Routinen-Modul (seit V2.6)
3. **FTPAPI_H** - Header-Datei mit Prototypen und Konstanten
4. **Binding Directory**: FTPAPI für einfache Integration

**Dateistruktur:**
- `QRPGLESRC.FILE/` - RPG-Quellcode (API, Beispiele, Header)
- `QCLSRC.FILE/` - CL-Programme (Installation, Packaging)
- `QSRVSRC.FILE/` - Service-Program-Exports
- `QSH.FILE/` - Shell-Skripte

### Hauptfunktionalitäten

**Kern-Features:**
- FTP-Verbindungsmanagement (Connect, Login, Quit)
- Dateiübertragung (GET/PUT) in ASCII und Binary-Modi
- Verzeichnisverwaltung (CHDIR, PWD, MKD, RMD)
- Mehrfache gleichzeitige FTP-Sessions
- FTPS-Unterstützung (TLS/SSL) - Implicit, Explicit, Private, Clear
- HTTP-Proxy-Unterstützung (seit V2.7)
- Passive und Enhanced Passive Mode (EPSV/EPRT)
- URL-basierte Downloads (FTP_url_get)
- Datei-Append-Funktionalität
- Verzeichnis-Mirroring (Tree-Copy)
- Transfer-Progress-Callbacks
- Codepage-Konvertierung (EBCDIC ↔ ASCII)
- Timeout-Konfiguration (Standard: 180 Sekunden)

**Unterstützte Dateitypen:**
- IFS Stream-Dateien
- Save-Files (*SAVF)
- Database Physical Files (*FILE)
- Source Physical Files
- Datenströme (*DSTMF)

### Beispielprogramme

Die Anwendung enthält 13 Test-/Beispielprogramme:
- **TESTGET** - Datei herunterladen (Binary-Mode)
- **TESTPUT** - Datei hochladen
- **TESTAPP** - Datei anhängen (APPEND)
- **TESTMGET** - Multiple Dateien herunterladen
- **TESTFTPS** - FTPS (TLS/SSL) Verbindung
- **TESTURL** - URL-basierter Download
- **TESTXPROC** - Transfer-Progress anzeigen
- **TESTMIRIN/TESTMIROUT** - Verzeichnis-Mirroring
- **TEST2SESS** - Mehrfache gleichzeitige Sessions
- **EX1PUT bis EX7TREETO** - Weitere Beispiele

### Installation

```
1. RSTOBJ SAVLIB(LIBFTP) DEV(*SAVF) SAVF(FTPAPI)
2. CRTCLPGM LIBFTP/INSTALL SRCFILE(LIBFTP/QCLSRC)
3. CALL LIBFTP/INSTALL (LIBFTP)
4. ADDLIBLE LIBFTP
```

### Verwendungsbeispiel

```rpg
D ftp S 10I 0
// Verbinden
ftp = ftp_conn('ftp.example.com': 'user': 'password');
// Binary-Mode aktivieren
ftp_binaryMode(ftp: *on);
// Datei herunterladen
ftp_get(ftp: 'remote.txt': '/local/file.txt');
// Verbindung beenden
ftp_quit(ftp);
```

### Wichtige Änderungen (Recent)

**Version 2.9 (aktuell):**
- Fix für optionale Parameter in FTP_setProxy
- Copyright-Update auf 2025

**Version 2.7-2.8:**
- HTTP-Proxy-Unterstützung hinzugefügt
- Pfadnamen-Länge auf 5000 Zeichen erhöht (vorher 256)
- Passive Mode als Standard
- Timeout-Standard auf 180 Sekunden gesetzt

**Version 2.6:**
- Quellcode nach IFS verschoben, GitHub-Integration
- FTPS (TLS/SSL) Unterstützung
- Enhanced Passive Mode (EPSV/EPRT)

### Technische Besonderheiten

- **Multi-Session-Fähig**: Globale Variablen in MODs für konfliktfreie parallele Sessions
- **Große Dateien**: Verwendet lstat64() und open64() für Dateien >2GB
- **Fehlerbehandlung**: Umfassende Fehlercodes und -meldungen
- **Exit-Procedures**: Erweiterbar durch benutzerdefinierte Callbacks (Logging, Status)
- **Codepage-Flexibilität**: Unterstützt Job-CCSID und benutzerdefinierte Übersetzungstabellen

### Community & Support

- **Forum**: https://www.scottklement.com/forums/
- **Entwicklung**: Aktiv seit 2001, regelmäßige Updates
- **Dokumentation**: README, CHANGELOG, umfangreiche Beispiele

## Fazit

FTPAPI ist eine ausgereifte, produktionsreife FTP-Bibliothek für IBM i mit über 20 Jahren Entwicklungsgeschichte. Sie bietet umfassende FTP-Funktionalität, moderne Sicherheitsfeatures (FTPS), und ist gut dokumentiert mit vielen Praxisbeispielen. Die BSD-Lizenz ermöglicht freie Nutzung in kommerziellen und Open-Source-Projekten.
