# Lezione Universitaria: Basi di Dati - Tecniche Avanzate di Querying SQL

---

## 1. Self Join

### Teoria e Applicazioni
Un self join permette di unire una tabella con se stessa, creando una relazione tra record della stessa entità. È essenziale per:
- Analisi gerarchiche (organigrammi).
- Trovare record correlati (es. prodotti simili).
- Confronti tra righe della stessa tabella.

**Esempio con Spiegazione**:
```sql
-- Trova dipendenti che guadagnano più del loro manager
SELECT 
    dip.nome AS "Dipendente",
    dip.stipendio AS "Stipendio Dip",
    man.nome AS "Manager", 
    man.stipendio AS "Stipendio Man"
FROM Impiegati dip
JOIN Impiegati man ON dip.manager_id = man.id
WHERE dip.stipendio > man.stipendio;
```

**Output Esempio**:
```
Dipendente   | Stipendio Dip | Manager   | Stipendio Man
-------------|---------------|-----------|-------------
Mario Rossi  | 45000         | Luca Verdi| 40000
Anna Bianchi | 55000         | Paolo Nero| 50000
```

*Spiegazione*: La query confronta ogni dipendente (alias `dip`) con il rispettivo manager (alias `man`) usando `manager_id` come chiave di relazione. Mostra solo le coppie dove lo stipendio del dipendente supera quello del manager.

---

## 2. Viste in SQL

### Teoria e Caso d'Uso
Le viste sono query salvate come tabelle virtuali. Questo esempio crea una vista per semplificare l'accesso ai clienti premium:

```sql
CREATE VIEW ClientiPremium AS
SELECT id, nome, email, punti_fedelta
FROM Clienti
WHERE punti_fedelta > 1000 
  AND status = 'ATTIVO';
```

**Utilizzo Pratico**:
```sql
SELECT * FROM ClientiPremium 
ORDER BY punti_fedelta DESC;
```

**Output Tipico**:
```
id  | nome          | email               | punti_fedelta
----|---------------|---------------------|--------------
102 | Elena Russo   | elena@example.com   | 3500
205 | Marco Neri    | marco@example.com   | 2100
```

*Spiegazione*: La vista filtra solo clienti attivi con oltre 1000 punti fedeltà. Quando interrogata, si comporta come una tabella normale ma i dati sono aggiornati dinamicamente dalla tabella `Clienti` originale.

---

## 3. CTE (Common Table Expressions)

### Esempio con Analisi Dettagliata
```sql
-- Calcola il totale ordini per categoria con filtro
WITH TotaleCategorie AS (
    SELECT 
        c.nome AS categoria,
        SUM(o.importo) AS totale
    FROM Ordini o
    JOIN Prodotti p ON o.prodotto_id = p.id
    JOIN Categorie c ON p.categoria_id = c.id
    GROUP BY c.nome
)
SELECT * FROM TotaleCategorie
WHERE totale > 10000
ORDER BY totale DESC;
```

**Risultato Atteso**:
```
categoria    | totale
-------------|---------
Elettronica  | 25400
Arredamento  | 18700
```

*Spiegazione*: 
1. La CTE `TotaleCategorie` calcola l'importo totale degli ordini per categoria.
2. La query principale filtra solo categorie con oltre 10.000€ di vendite.
3. L'uso di CTE migliora la leggibilità rispetto a una subquery equivalente.

---

## 4. EXPLAIN con Analisi Prestazionale

### Query Esempio
```sql
EXPLAIN ANALYZE
SELECT p.nome, COUNT(o.id)
FROM Prodotti p
LEFT JOIN Ordini o ON p.id = o.prodotto_id
WHERE p.prezzo > 50
GROUP BY p.id;
```

**Output Interpretato**:
```
HashAggregate (cost=1,250.50..1,253.20 rows=180 width=40)
  -> Hash Left Join (cost=850.00..1,200.75 rows=9,950 width=36)
      Hash Cond: (o.prodotto_id = p.id)
      -> Seq Scan on Ordini o (cost=0.00..210.50 rows=9,950)
      -> Hash (cost=845.00..845.00 rows=200 width=36)
          -> Seq Scan on Prodotti p (cost=0.00..845.00 rows=200)
              Filter: (prezzo > 50)
```

*Analisi*:
- `Seq Scan`: Scansione completa delle tabelle (potrebbe beneficiare di un indice).
- `Hash Join`: Unione tramite tabella hash (efficiente per grandi dataset).
- `Filter`: Applica la condizione `prezzo > 50` durante la scansione.
- *Costo stimato*: Da 1,250.50 a 1,253.20 unità.

---

## 5. Operatori Insiemistici

### Esempio con UNION e INTERSECT
```sql
-- Clienti che hanno ordinato sia elettronica che arredamento
SELECT cliente_id FROM Ordini
WHERE categoria = 'Elettronica'

INTERSECT

SELECT cliente_id FROM Ordini
WHERE categoria = 'Arredamento';
```

**Output**:
```
cliente_id
----------
102
205
307
```

*Spiegazione*: L'operatore `INTERSECT` trova l'intersezione tra i due insiemi di clienti, mostrando solo quelli presenti in entrambe le query.

---

## 6. Subquery Complesse

### Esempio Correlato
```sql
-- Prodotti con prezzo superiore alla media della loro categoria
SELECT p.nome, p.prezzo, c.nome AS categoria
FROM Prodotti p
JOIN Categorie c ON p.categoria_id = c.id
WHERE p.prezzo > (
    SELECT AVG(prezzo) 
    FROM Prodotti 
    WHERE categoria_id = p.categoria_id
);
```

**Risultato**:
```
nome         | prezzo | categoria
-------------|--------|-----------
TV 4K        | 899    | Elettronica
Sofa Design  | 1200   | Arredamento
```

*Meccanismo*: La subquery calcola dinamicamente la media dei prezzi per ogni categoria durante l'elaborazione della query principale.
