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
Hra začíná v normálním režimu a po zadaném počtu tahů se hra zďábelští. V normální fázi se hra hraje stejně, jako běžný MineSweeper. Na začátku hry je na herní ploše náhodně rozmístěn zadaný počet min, které jsou před hráčem skryty. Hrač postupně odkrývá políčka, počet min ani jejich pozice se nemění. Tato první fáze slouží k částečnému odhalení herní plochy, aby hra byla vůbec hratelná. Poté se po určitém počtu kroků hra přepne do oné ďábelské fáze.   
### Ďábelská herní fáze
Pro jednu poloodkrytou herní plochu může existovat více rozmístění min. Pokud hráč odkryje políčko, na kterém je mina alespoň v jednom takovém rozmístění, mina tam vždy bude. Pokud hráč odkryje políčko, které neobsahuje minu ani v jednom přípustném rozmístění min, mina tam není. Jedno přípustné rozmístění min se poté použije na spočtení počtu sousedních min právě odkrytého políčka.    

### Konec hry
Hráč prohrává v případě, že odhalil políčko s minou. Vyhrát může v normální fázi nebo v ďábelské fázi. V normální fázi hráč vyhrává tehdy, pokud odhalí všechna políčka, na kterých není mina. V ďábelské fázi nemají miny fixní polohu a ani jejich počet není fixní. Proto hráč vyhrává tehdy, pokud na všech neodhalených políčkách musí být mina.

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


# Technická dokumentace
## Custom datové typy
#### Point
Ukládá souřadnice x,y nějakého políčka.

#### Mines
Představuje rozmístění min na herní ploše.

#### Info
Představuje stav nějakého políčka. Políčko může být neodkryté (Covered), nebo může být odkryté, a v tom případě má kolem sebe nějaký n sous edních min (Uncovered n)

#### MinesInfo
Ukládá Info o každém políčku herní plochy.

#### MinePossibility
Při ďábelském rozmísťování min je zakrytým políčkům postupně přiřazován stav, zdali zde mina je, či není. Políčko může mít stav Mine, NotMine nebo Unassigned. 


## Funkce
 
#### askPlayerForGameParams
Po spuštění hry se nejdříve vyžádají herní parametry od uživatele, funkce vrátí trojici velikost_pole, počet min, počet tahů normální fáze. Funkce validuje zadané hodnoty.

#### safelyGetNumberFromPlayer
Vyžádá si od hráče číslo, kontroluje, zda hráč opravdu zadal číslo.

#### getPlayerChoice
Vrací políčko, které chce hráč odkrýt. Robustně ošetřuje vstup.

#### placeMinesRandomly
Vrátí náhodné rozmístění min. Žádné 2 miny se nepřekrývají.

#### getRandomPoint
Vrátí náhodné políčko na herní ploše a nový stav náhodného generátoru.

#### getCoveredBoard
Zinicializuje MinesInfo, všechna políčka jsou zakrytá.

#### getNeighbouringPoints
Vrátí seznam políček, které sousedí s daným políčkem

#### playGame
Hlavní herní smyčka (herní rekurze). Pamatuje si herní stav (tj. Odkrytí plochy, počty okolních min odkrytých políček, rozložení min, kolik tahů zbývá do zďábelštění)

#### printBoard
Vykresluje herní stav.

#### showMinePlacement
Po skončení hry odhalí hráči rozložení min.

#### isMineHere
Zjistí, zdali je na zadaném políčku mina.

#### uncoverSafeCells
Odkryje požadované políčko. Pokud toto políčko nesousedí s žádnou minou, odkryje rekurzivně všechna sousední políčka. Volá funkci `recursivelyUncoverSafeCells` 

#### playerHasWonNormal
Vrací Bool, zdali hráč vyhrál v normálním režimu hry.

#### playerHasWonDevil
Vrací Bool, zdali hráč vyhrál v ďábelském režimu hry.

#### generateDevilMines
Funkce vrátí Maybe rozložení min, které má na danném políčku minu. Pokud žádné takové neexistuje, vrátí Nothing. Je to high level interface k funkci `generateMines`

#### generateMines
Funkce bere jako parametry informaci, kterou má hráč (MinesInfo), seznam zakrytých políček s informací, zda na tomto políčku má/nemá být mina, či zda o tom ještě nebylo rozhodnuto a index, od kterého prvku v seznamu může začít měnit stavy políčkům. Vrací Maybe seznam min, odpovídající constrains v parametrech. Vrací Nothing, pokud žádná konfigurace min odpovíající požadavkům neexistuje.

#### checkPartialPlacement  
Zkontroluje, zdali částečné přiřazení stavů zakrytým políčkům není v rozporu s už odhalenou informací, kterou vidí hráč. Kontroluje políčko po políčku, na každé volá `checkOneSquare`

#### getInfo a changeInfo
Pracuje s objektem MinesInfo. Vytahuje/ukládá informace o jednotlivých políčkách.

 
 
 
