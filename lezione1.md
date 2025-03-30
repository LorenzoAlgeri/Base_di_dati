# Lezione Universitaria: Basi di Dati - Introduzione a SQL  
*Data: 19/03/25*  

---

## 1. Introduzione a SQL  
SQL (*Structured Query Language*) è il linguaggio standard per gestire database relazionali. Si divide in:  
- **DDL** (Data Definition Language): crea/modifica strutture (es: `CREATE TABLE`).  
- **DML** (Data Manipulation Language): manipola dati (es: `INSERT`, `UPDATE`).  
- **DQL** (Data Query Language): interroga dati (es: `SELECT`).  

---

## 2. Istruzione SELECT  
Recupera dati da una tabella:  
```sql
SELECT colonna1, colonna2 FROM tabella;
```  

**Esempi**:  
```sql
-- Tutti i clienti  
SELECT * FROM Clienti;  

-- Solo nome e cognome degli impiegati  
SELECT nome, cognome FROM Impiegati;  
```

---

## 3. Clausola WHERE  
Filtra i risultati in base a condizioni:  
```sql
SELECT colonne FROM tabella WHERE condizione;
```  

**Esempi**:  
```sql
-- Prodotti con prezzo > 50  
SELECT * FROM Prodotti WHERE prezzo > 50;  

-- Studenti con voto > 25  
SELECT nome FROM Studenti WHERE voto_esame > 25;  
```

---

## 4. Operatori Booleani  
Combina condizioni con `AND`, `OR`, `NOT`:  
```sql
-- AND: entrambe le condizioni devono essere vere  
SELECT * FROM Ordini WHERE data >= '2023-01-01' AND totale > 1000;  

-- NOT: nega la condizione  
SELECT * FROM Clienti WHERE NOT città = 'Roma';
```

---

## 5. Operatore LIKE  
Cerca pattern con wildcard (`%` = zero o più caratteri, `_` = un carattere):  
```sql
-- Nomi che iniziano con "Mar"  
SELECT * FROM Clienti WHERE nome LIKE 'Mar%';  

-- Cognomi di 5 lettere (es: "Rossi")  
SELECT * FROM Impiegati WHERE cognome LIKE 'Ross_';
```

---

## 6. Funzioni per Testo  
### LOWER() / UPPER()  
Convertono il testo in minuscolo/maiuscolo:  
```sql
SELECT UPPER(nome), LOWER(cognome) FROM Clienti;  
```  

### TRIM()  
Rimuove spazi extra all'inizio/fine:  
```sql
SELECT * FROM Clienti WHERE TRIM(indirizzo) = 'Via Roma 123';  
```

---

## Esempio Completo  
```sql
-- Clienti di Milano/Roma con nome che inizia per M/A  
SELECT UPPER(nome) AS nome_maiuscolo, cognome  
FROM Clienti  
WHERE (città = 'Milano' OR città = 'Roma')  
  AND (nome LIKE 'M%' OR nome LIKE 'A%')  
  AND LENGTH(TRIM(cognome)) > 3;
```
