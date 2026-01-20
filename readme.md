# Lekce 11 - sort uniq cut paste join tr

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

## tr – překlad znaků

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
