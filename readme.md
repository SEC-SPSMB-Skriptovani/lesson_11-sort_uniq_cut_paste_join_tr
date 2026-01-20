# Lekce 11 - sort uniq cut paste join tr

## sort – třídění řádků
Seřadí řádky vstupu (abecedně nebo číselně).
Nejpoužívanější přepínače:
``` bash
-n … číselné třídění

-r … obrácené pořadí

-u … unikátní řádky (jako sort | uniq)

-t … oddělovač sloupců

-k … klíč (sloupec) pro třídění
```

Příklad:

`sort -t';' -k2`

## uniq – práce s duplicitami

Odstraňuje nebo počítá sousední duplicitní řádky.

Nejpoužívanější přepínače:
``` bash 

-c … spočítá výskyty

-d … vypíše jen duplicitní řádky

-u … vypíše jen unikátní řádky
```

⚠️ Funguje správně jen na seřazených datech.

Příklad:

`sort | uniq -c`

## cut – výběr sloupců

Vybere části řádků (sloupce nebo znaky).

Nejpoužívanější přepínače:

``` bash
-d … oddělovač (delimiter)

-f … pole (sloupce)

-c … znaky (pozice)
```

Příklad:

`cut -d';' -f1,3`

## paste – Spojení souborů 

Spojí soubory **po řádcích** vedle sebe.

Nejpoužívanější přepínače:

``` bash
-d … vlastní oddělovač

-s … slepí řádky jednoho souboru za sebe
```

Příklad:

`paste -d' ' jmena.txt kouzla.txt`

## join – spojení souborů podle klíče

Spojí dva soubory podle společného sloupce (jako SQL JOIN).

📌 Soubory musí být seřazené podle klíče.

Nejpoužívanější přepínače:
``` bash 
-t … oddělovač

-1 … klíčový sloupec v 1. souboru

-2 … klíčový sloupec ve 2. souboru
```
Příklad:

`join -t';' -1 1 -2 1 a.txt b.txt`

## tr – překlad znaků

Nahrazuje nebo maže jednotlivé znaky.

Nejpoužívanější přepínače:
``` bash
[:lower:] / [:upper:] … třídy znaků

-d … maže znaky

-s … sloučí opakující se znaky
```

Příklad:

`tr 'a-z' 'A-Z'`
`tr -d '_'`

----
# Cvičení

## 1) Seřaďte `postavy.txt` podle abecedy
```bash
sort postavy.txt
```
➡️ Seřadí celý soubor podle prvního znaku na řádku (ID jako text).

---

## 2) Seřaďte podle jména postavy (2. sloupec)
```bash
sort -t';' -k2 postavy.txt
```

---

## 3) Seřaďte postavy podle síly jejich schopností (4. sloupec)
### vzestupně
```bash
sort -t';' -k4 -n schopnosti.txt
```

### sestupně
```bash
sort -t';' -k4 -n -r schopnosti.txt
```

---

## 4) Vypište pouze názvy postav (sloupec 2)
```bash
cut -d";" -f2 postavy.txt
```

---

## 5) Ze souboru `schopnosti.txt` vypište pouze názvy postav (sloupec 5) bez duplicit
```bash
cut -d";" -f5 schopnosti.txt | sort | uniq
```

---

## 6) Ze souboru `schopnosti.txt` vypište názvy postav, schopnost a typ (sloupce 2, 3 a 5)
```bash
cut -d";" -f2,3,5 schopnosti.txt
```

---

## 7) Kolik je typů kouzel (sloupec 3)
```bash
cut -d";" -f3 schopnosti.txt | sort | uniq
```

```bash
cut -d";" -f3 schopnosti.txt | sort | uniq -c
```

---

## 8) Spoj soubory `postavy.txt` a `schopnosti.txt` (středník `;` jako oddělovač)
```bash
paste -d";" postavy.txt schopnosti.txt
```

---

## 9) Spojíme `postavy.txt` a vybereme jen sloupce 2, 3 a 4 ze `schopnosti.txt`
```bash
paste -d";" postavy.txt <(cut -d';' -f2,3,4 schopnosti.txt)
```

---

## 10) Spojme oba soubory podle ID (1. klíč v obou souborech)
```bash
join -t';' -1 1 -2 1 postavy.txt schopnosti.txt
```

---

## 11) Spojme oba soubory podle názvu postav
```bash
join -t';' -1 2 -2 5 postavy.txt schopnosti.txt
```

### 11a) Spojení + sloučení schopností do jednoho řádku (awk)
```bash
join -t';' -1 2 -2 5 postavy.txt schopnosti.txt |
awk -F';' '
{
  key = $1 ";" $2 ";" $3
  rest = $4 ";" $5 ";" $6 ";" $7
  if (seen[key] == 0) {
    data[key] = rest
    seen[key] = 1
  } else {
    data[key] = data[key] ";" rest
  }
}
END {
  for (k in data)
    print k ";" data[k]
}'
```

---

## 12) Vypište názvy postav a nahraďte podtržítko `_` mezerou
```bash
cut -d";" -f2 postavy.txt | tr '_' ' '
```

---

## 13) Název postavy nahraďte podtržítko `_` mezerou a přepište na VELKÁ písmena
```bash
cut -d";" -f2 postavy.txt | tr '_' ' ' | tr '[:lower:]' '[:upper:]'

//případně pomocí awk
awk -F';' '{gsub("_"," ",$2); print toupper($2)}' postavy.txt

```

---

## 14) Spojení + nahrazení podtržítek ve výstupu
```bash
join -t";" -1 2 -2 5 postavy.txt schopnosti.txt | tr "_" " "
```
---

## 15) Ukázka kombinace join/paste s dalšími příkazy. 
```bash
paste -d";" \
  <(cut -d";" -f2 postavy.txt | tr '_' ' ' | tr '[:lower:]' '[:upper:]') \
  <(cut -d";" -f2,3 schopnosti.txt)

join -t";" -1 2 -2 5 \
  <(awk -F';' '{gsub("_"," ",$2); $2=toupper($2); print}' postavy.txt | sort -t";" -k2) \
  <(awk -F';' '{gsub("_"," ",$5); $5=toupper($5); print}' schopnosti.txt | sort -t";" -k5)
```




