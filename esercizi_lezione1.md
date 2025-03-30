

## Esercizi Medio-Difficili su SQL

### **Esercizio 1: Filtri Complessi con AND/OR**
**Database**: Tabella `Studenti` con colonne `id`, `nome`, `cognome`, `anno_iscrizione`, `voto_medio`  
**Richiesta**:  
Trova tutti gli studenti iscritti dopo il 2020 con voto medio superiore a 25 **oppure** iscritti prima del 2018 con voto medio esattamente 30.  
```sql
SELECT *
FROM Studenti
WHERE (anno_iscrizione > 2020 AND voto_medio > 25)
   OR (anno_iscrizione < 2018 AND voto_medio = 30);
```

---

### **Esercizio 2: LIKE con Pattern Complessi**
**Database**: Tabella `Libri` con colonne `titolo`, `autore`, `genere`, `anno_pubblicazione`  
**Richiesta**:  
Cerca libri il cui titolo:  
- Inizia con "Il" o "La"  
- Contiene la parola "notte" (in qualsiasi posizione)  
- Non termina con "e"  
```sql
SELECT *
FROM Libri
WHERE (titolo LIKE 'Il %' OR titolo LIKE 'La %')
  AND titolo LIKE '%notte%'
  AND NOT titolo LIKE '%e';
```

---

### **Esercizio 3: Funzioni di Testo e WHERE**
**Database**: Tabella `Dipendenti` con colonne `nome`, `cognome`, `email`, `dipartimento`  
**Richiesta**:  
Trova i dipendenti il cui cognome ha almeno 5 lettere, il nome inizia con 'M' e l'email è nel dominio `@azienda.com` (case-insensitive).  
```sql
SELECT *
FROM Dipendenti
WHERE LENGTH(cognome) >= 5
  AND nome LIKE 'M%'
  AND LOWER(email) LIKE '%@azienda.com';
```

---

### **Esercizio 4: Combinazione di Operatori**
**Database**: Tabella `Ordini` con colonne `id`, `cliente_id`, `data`, `totale`, `stato`  
**Richiesta**:  
Seleziona ordini con:  
- Totale tra 100 e 500 euro  
- Stato "Spedito" **o** "In elaborazione"  
- Data nel 2023, eccetto quelli di gennaio  
```sql
SELECT *
FROM Ordini
WHERE totale BETWEEN 100 AND 500
  AND (stato = 'Spedito' OR stato = 'In elaborazione')
  AND data BETWEEN '2023-02-01' AND '2023-12-31';
```

---

### **Esercizio 5: Pulizia Dati con TRIM e LIKE**
**Database**: Tabella `Clienti` con colonne `nome`, `indirizzo`, `città`  
**Richiesta**:  
Trova clienti il cui indirizzo:  
- Non ha spazi extra all'inizio/fine  
- Contiene "Via" o "Piazza"  
- La città è "Roma" (case-insensitive)  
```sql
SELECT *
FROM Clienti
WHERE TRIM(indirizzo) = indirizzo  -- Verifica assenza di spazi extra
  AND (indirizzo LIKE '%Via%' OR indirizzo LIKE '%Piazza%')
  AND LOWER(città) = 'roma';
```

---

### **Esercizi Avanzati**

#### **Esercizio 6: Query con Condizioni Annidate**
**Database**: Tabella `Prodotti` con colonne `id`, `nome`, `prezzo`, `categoria`, `sconto`  
**Richiesta**:  
Seleziona prodotti con:  
- Prezzo originale > 50 euro **e** sconto del 20% **oppure**  
- Prezzo originale < 20 euro **e** categoria "Elettronica"  
```sql
SELECT *
FROM Prodotti
WHERE (prezzo > 50 AND sconto = 20)
   OR (prezzo < 20 AND categoria = 'Elettronica');
```

---

#### **Esercizio 7: Manipolazione Stringhe Complessa**
**Database**: Tabella `Commenti` con colonne `id`, `testo`, `data`  
**Richiesta**:  
Trova commenti che:  
- Contengono la parola "ottimo" (case-insensitive)  
- Hanno più di 50 caratteri  
- Non contengono la parola "ma"  
```sql
SELECT *
FROM Commenti
WHERE LOWER(testo) LIKE '%ottimo%'
  AND LENGTH(testo) > 50
  AND testo NOT LIKE '% ma %';
```

---

### **Soluzioni Separate**
Le soluzioni sono incluse negli snippet SQL sopra. Per un approccio didattico:  
1. Lascia che lo studente provi a risolvere autonomamente.  
2. Confronta con la soluzione fornita.  
3. Discuti eventuali alternative (es.: uso di `IN` invece di `OR` multipli).
