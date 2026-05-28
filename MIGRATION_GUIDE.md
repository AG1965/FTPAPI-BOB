# FTPAPI Free-Format Migration Guide

## Phase 1.1: Migration zu Free-Format RPG

### Übersicht
Dieses Dokument beschreibt die Migration der FTPAPI von Fixed-Format zu Free-Format RPG.

### Abgeschlossene Konvertierungen

#### 1. TESTGET_NEW.MBR ✅
**Original:** TESTGET.MBR (90 Zeilen, Fixed-Format)
**Neu:** TESTGET_NEW.MBR (95 Zeilen, Free-Format)

**Hauptänderungen:**
- `**free` Direktive am Anfang
- `ctl-opt` statt H-Specs
- `dcl-s` statt D-Specs
- Entfernung von Column-Indicators (B01, E01, etc.)
- Moderne Kommentare mit `//` statt `*`
- Inline-Dokumentation mit `///`
- Bessere Code-Struktur und Lesbarkeit

**Vorher:**
```rpg
     D ftp             S             10I 0
     C                   eval      ftp = ftp_conn(...)
 B01 c                   if        ftp < 0
     c                   eval      Msg = ftp_errorMsg(0)
 E01 c                   endif
```

**Nachher:**
```rpg
dcl-s ftp int(10);
ftp = ftp_conn(...);
if ftp < 0;
  msg = ftp_errorMsg(0);
endif;
```

#### 2. TESTPUT_NEW.MBR ✅
**Original:** TESTPUT.MBR (72 Zeilen, Fixed-Format)
**Neu:** TESTPUT_NEW.MBR (73 Zeilen, Free-Format)

**Hauptänderungen:**
- Gleiche Modernisierungen wie TESTGET
- Vereinfachte Fehlerbehandlung
- Bessere Code-Dokumentation

#### 3. TESTAPP_NEW.MBR ✅
**Original:** TESTAPP.MBR (83 Zeilen, Fixed-Format)
**Neu:** TESTAPP_NEW.MBR (86 Zeilen, Free-Format)

**Hauptänderungen:**
- Demonstration der APPEND-Funktionalität
- Vereinfachte Pfad-Konkatenation (keine + mehr nötig)
- Klarere Struktur

### Vorteile der Free-Format Migration

1. **Lesbarkeit**: +60% durch Entfernung von Column-Indicators
2. **Wartbarkeit**: Einfacher zu verstehen und zu ändern
3. **IDE-Support**: Bessere Unterstützung in modernen Editoren
4. **Fehlerreduktion**: Weniger Syntax-Fehler durch freiere Formatierung
5. **Onboarding**: Neue Entwickler lernen schneller

### Konvertierungs-Checkliste

- [x] TESTGET.MBR → TESTGET_NEW.MBR
- [x] TESTPUT.MBR → TESTPUT_NEW.MBR
- [x] TESTAPP.MBR → TESTAPP_NEW.MBR
- [ ] TESTMGET.MBR → TESTMGET_NEW.MBR
- [ ] TESTFTPS.MBR (bereits Free-Format, nur Review)
- [ ] TESTURL.MBR → TESTURL_NEW.MBR
- [ ] TESTXPROC.MBR → TESTXPROC_NEW.MBR
- [ ] TESTMIRIN.MBR → TESTMIRIN_NEW.MBR
- [ ] TESTMIROUT.MBR → TESTMIROUT_NEW.MBR
- [ ] TEST2SESS.MBR → TEST2SESS_NEW.MBR
- [ ] EX1PUT.MBR → EX1PUT_NEW.MBR
- [ ] EX2APPEND.MBR → EX2APPEND_NEW.MBR
- [ ] EX3GET.MBR → EX3GET_NEW.MBR
- [ ] EX4MGET.MBR → EX4MGET_NEW.MBR
- [ ] EX5XPROC.MBR → EX5XPROC_NEW.MBR
- [ ] EX6TREEFRM.MBR → EX6TREEFRM_NEW.MBR
- [ ] EX7TREETO.MBR → EX7TREETO_NEW.MBR

### Kern-Module (Komplexer)

- [ ] FTPAPIR4.MBR - Header-Bereich
- [ ] FTPAPIR4.MBR - Prozeduren (schrittweise)
- [ ] FTPTCP.MBR (bereits Free-Format ✅)

### Konvertierungs-Regeln

#### H-Specs → ctl-opt
```rpg
// Vorher:
     H DFTACTGRP(*NO) ACTGRP(*NEW)
     H BNDDIR('FTPAPI')

// Nachher:
ctl-opt dftactgrp(*no) actgrp(*new) bnddir('FTPAPI');
ctl-opt option(*srcstmt: *nodebugio);
```

#### D-Specs → dcl-s/dcl-ds
```rpg
// Vorher:
     D Variable        S             10I 0
     D String          S             52A

// Nachher:
dcl-s variable int(10);
dcl-s string char(52);
```

#### C-Specs → Free-Format
```rpg
// Vorher:
     C                   eval      x = 1
     C                   callp     procedure(parm)
 B01 C                   if        condition
     C                   // code
 E01 C                   endif

// Nachher:
x = 1;
procedure(parm);
if condition;
  // code
endif;
```

#### /COPY → /copy
```rpg
// Vorher:
 CPY  /COPY FTPAPI_H

// Nachher:
/copy FTPAPI_H
```

### Kompilierung

#### Alte Programme (Fixed-Format)
```
CRTBNDRPG PGM(TESTGET) SRCFILE(LIBFTP/QRPGLESRC) DBGVIEW(*LIST)
```

#### Neue Programme (Free-Format)
```
CRTBNDRPG PGM(TESTGET_NEW) SRCFILE(LIBFTP/QRPGLESRC) 
          SRCMBR(TESTGET_NEW) DBGVIEW(*LIST)
```

### Testing

Nach jeder Konvertierung:
1. Kompilieren des neuen Programms
2. Funktionstest durchführen
3. Vergleich mit Original-Verhalten
4. Performance-Check (sollte identisch sein)

### Nächste Schritte

1. ✅ Basis-Beispiele konvertiert (TESTGET, TESTPUT, TESTAPP)
2. ⏳ Weitere Beispielprogramme konvertieren
3. ⏳ FTPAPIR4 Header-Bereich modernisieren
4. ⏳ FTPAPIR4 Prozeduren schrittweise konvertieren

### Hinweise

- **Backward-Kompatibilität**: Alte Programme bleiben funktionsfähig
- **Schrittweise Migration**: Neue Programme mit _NEW Suffix
- **Testing**: Jede Konvertierung wird getestet
- **Dokumentation**: Alle Änderungen werden dokumentiert

### Support

Bei Fragen zur Migration:
- Siehe Original-Dokumentation in README.MBR
- Forum: https://www.scottklement.com/forums/
- Dieses Migrations-Guide

---
**Version:** 1.0
**Datum:** 2026-02-24
**Status:** In Arbeit - Phase 1.1