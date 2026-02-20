---
id: "12733fcc15cf5854"
repo: ".github"
tags: ["database", "sql", "trigger", "replica dati"]
code_path: ""
code_url: ""
created_at: "2026-02-03T10:18:13.346Z"
updated_at: "2026-02-20T15:59:35.539Z"
github_owner: "totalErp"
github_path: "Documentazione/replica-causali-contabili.md"
github_branch: "main"
---

# replica-causali-contabili

## Riepilogo

- Repo: .github

## Contenuto


# replica-causali-contabili

## Riepilogo

- Repo: Giorgetti

## Contenuto

---
title: "Replica causali contabili"
repo: "Giorgetti"
tags: ["database", "sql", "trigger", "replica dati"]
created_at: "2026-01-27T14:58:52.252613Z"
updated_at: "2026-01-27T14:58:52.252613Z"
code_path: "Sql\\Trigger Causali Contabili\\trg_sync_tabcauc_to_MKTSANMARINO"
code_url: "https://github.com/totalErp/Giorgetti/blob/main/Sql\\Trigger Causali Contabili\\trg_sync_tabcauc_to_MKTSANMARINO"
---

# Replica causali contabili

*Repository:* Giorgetti
*Creato il:* 2026-01-27T14:58:52.252613Z
*Aggiornato il:* 2026-01-27T14:58:52.252613Z
*Tag:* database, sql, trigger, replica dati
*Riferimento codice:* [Sql\Trigger Causali Contabili\trg_sync_tabcauc_to_MKTSANMARINO](https://github.com/totalErp/Giorgetti/blob/main/Sql\Trigger Causali Contabili\trg_sync_tabcauc_to_MKTSANMARINO)

---

# Trigger `trg_sync_tabcauc_to_MARKET`

## Scopo
Questo trigger sincronizza automaticamente la tabella **tabcauc** dal database:

- **Sorgente**: `GIORGETTISPA.dbo.tabcauc`
- **Destinazione**: `MARKETdbo.tabcauc`

L’obiettivo è mantenere **allineate le causali contabili** tra ambiente di lavoro e ambiente di sviluppo.

---

## Evento di attivazione
```sql
AFTER INSERT, UPDATE
```

Il trigger viene eseguito ogni volta che una causale viene:
- creata (`INSERT`)
- modificata (`UPDATE`)

nel database `GIORGETTISPA`.

---

## Protezione anti-loop
```sql
IF CONTEXT_INFO() = 0xA1
    RETURN;
```

Serve a evitare:
- ricorsioni
- loop infiniti tra database sorgente e destinazione

Il contesto viene impostato dal trigger prima del `MERGE` e azzerato al termine.

---

## Logica di sincronizzazione
La sincronizzazione avviene tramite un’istruzione:

```sql
MERGE MARKET.dbo.tabcauc
```

### Chiave di confronto
```sql
tb_codcauc
```

È considerata la chiave logica univoca della causale.

---

## Comportamento
- **MATCHED** → `UPDATE` di tutte le colonne
- **NOT MATCHED** → `INSERT` completo della causale

Non viene applicato alcun filtro: **tutte le colonne vengono copiate**.

---

## Benefici
- Coerenza totale tra GIORGETTISPA e MARKET
- Nessun allineamento manuale
- Evita errori di configurazione tra ambienti












