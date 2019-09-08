# Ďábelské miny
Hrají se stejně jako běžné hledání min, ale jsou ďábelské, pokud hráč odhalí políčko, na kterém může být mina, mina tam vždy bude. Hráč tedy může vyhrát pouze tehdy, pokud odhaluje políčka, na kterých mina zaručeně není.

# Uživatelská dokumentace
### Spuštění 
Před spuštěním je třeba hru nejdříve zkompilovat: `ghc mines.hs`, poté stačí zkompilovanou hru z příkazové řádky sputit.
### Ovládání 
Hra se ovládá z příkazové řádky. Při spuštění je umožněno hráči zadat parametry hry (velikost pole, počet min, čas do zďábelštění).
```
$ mines.exe

Do you want to play with default game parameters? (y/n):
n
Choose size of playing board:
5
Input number of mines:
8
Input number of moves before devil mode starts:
4
  0 1 2 3 4
 ----------
0|* * * * *
1|* * * * *
2|* * * * *
3|* * * * *
4|* * * * *
Devil mode: INACTIVE YET
Devil mode countdown: 4
input space-separated x y coordinates:
``` 

Po zadání parametrů se spustí samotná hra. Při každém tahu se hráči zobrazí hrací plocha a hráč zadáním souřadnic zvolí, které políčko chce odkrýt.
``` 
input space-separated x y coordinates:
1 3
  0 1 2 3 4
 ----------
0|* * * * *
1|* * * * *
2|* * * * *
3|* 4 * * *
4|* * * * *
```

## Herní fáze 
Hra má 2 herní fáze - normální a ďábelskou. 
### Normální herní fáze
Hra začíná v normálním režimu a po zadaném počtu tahů se hra zďábelští. V normální fázi se hra hraje stejně, jako běžný MineSweeper. Nazačátku hry je na herní ploše náhodně rozmístěn zadaný počet min, které jsou před hráčem skryty. Hrač postupně odkrývá políčka, počet min ani jejich pozice se nemění. Tato první fáze slouží k částečnému odhalení herní plochy, aby hra byla vůbec hratelná. Poté se po určitém počtu kroků hra přepne do oné ďábelské fáze.   
### Ďábelská herní fáze
Pro jednu poloodkrytou herní plochu může existovat více rozmístění min. Pokud hráč odkryje políčko, na kterém je mina alespoň v jednom takovém rozmístění, mina tam vždy bude. Pokud hráč odkryje políčko, které neobsahuje minu ani v jednom přípustném rozmístění min, mina tam není. Jedno přípustné rozmístění min se poté použije na spočítání sousedních min právě odkrytého políčka.    

### Konec hry
Hráč prohrává v případě, že odhalil políčko s minou. Vyhrát může v normální fázi nebo v ďábelské fázi. V normální fázi hráč vyhrává tehdy, pokud odhalí všechna políčka, na kterých není mina. V ďábelské fázi nemají miny fixní polohu a ani jich není fixní počet. Proto hráč vyhrává tehdy, pokud na všech neodhalených políčkách musí být mina.

## Ukázka hry v normálním režimu
```
$ mines.exe

 0 1 2 3 4 5
 ------------
0|* * * * * *
1|* * * * * *
2|* * * * * *
3|* * * * * *
4|* * * * * *
5|* * * * * *
Devil mode: INACTIVE YET
Devil mode countdown: 3
input space-separated x y coordinates:
0 0
  0 1 2 3 4 5
 ------------
0|0 1 * * * *
1|0 1 * * * *
2|1 2 * * * *
3|* * * * * *
4|* * * * * *
5|* * * * * *
Devil mode: INACTIVE YET
Devil mode countdown: 2
input space-separated x y coordinates:
5 5
  0 1 2 3 4 5
 ------------
0|0 1 * * * *
1|0 1 * * * *
2|1 2 1 1 1 *
3|* * 1 0 1 *
4|* * 1 0 1 1
5|* * 1 0 0 0
Devil mode: INACTIVE YET
Devil mode countdown: 1
input space-separated x y coordinates:
3 0
  0 1 2 3 4 5
 ------------
0|0 1 * 1 * *
1|0 1 * * * *
2|1 2 1 1 1 *
3|* * 1 0 1 *
4|* * 1 0 1 1
5|* * 1 0 0 0
```

## Ukázka ďábelského režimu

```
$ mines.exe

  0 1 2 3 4 5
 ------------
0|* * * * * *
1|* * * * * *
2|* * * * * *
3|* * * * * *
4|* * * * * *
5|* * * * * *
Devil mode: INACTIVE YET
Devil mode countdown: 2
input space-separated x y coordinates:
0 0
  0 1 2 3 4 5
 ------------
0|0 1 * * * *
1|0 1 * * * *
2|1 2 * * * *
3|* * * * * *
4|* * * * * *
5|* * * * * *
Devil mode: INACTIVE YET
Devil mode countdown: 1
input space-separated x y coordinates:
5 5
  0 1 2 3 4 5
 ------------
0|0 1 * * * *
1|0 1 * * * *
2|1 2 1 1 1 *
3|* * 1 0 1 *
4|* * 1 0 1 1
5|* * 1 0 0 0
Devil mode: ACTIVE!
input space-separated x y coordinates:
3 0
You lost!
  0 1 2 3 4 5
 ------------
0|0 1 2 X X X
1|0 1 X 3 3 2
2|1 2 1 1 1 1
3|X 3 1 0 1 X
4|X X 1 0 1 1
5|X 3 1 0 0 0

```
Zde je vidět, že ďábel zařídil, aby na políčku 3 0 byla mina, ač tam v normálním režimu nebyla.