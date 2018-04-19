# Entrega de les pràctiques de Base de Dades Orientades a objectes.

## Pràctica 1: 

Enunciat i resolució de problema amb una d'aquestes tècniques:

* Taula d'objectes
* Taula clàssica amb columna tipus objecte
* Taula d'objectes + Taula clàssica amb referència a la taula d'objectes

Entrega a la carpeta [pràctica 1](./practica01).

## Pràctica 2: 

Pensa i redacta un tipus col·lecció i declara'l. Després crea una taula que el faci servir. Inserta dades a la taula. Actualitza una filera amb una nova col·lecció. Mostra les dades de la col·lecció juntament amb la resta de dades de la taula fent servir `TABLE`.

Entrega a la carpeta [pràctica 2](./practica02).

## Pràctica 3:

Modifica la pràctica anterior per tal de fer [DML](https://docs.oracle.com/database/121/ADOBJ/adobjcol.htm#CIHJHIGG) sobre les col·leccions que heu creat. Entregueu la pràctica a la carpeta [pràctica 3](./practica03).

## Pràctica 4:

Mira la pràctica [Oracle Objecte-Relacional - Els soldats - Treballar amb col·leccions](https://uf.ctrl-alt-d.net/material/mostra/153/oracle-objecte-relacional-els-soldats-treballar-amb-colleccions)

Cal fer un objecte que contingui una col·lecció amb un mètode que retorni un dels objectes de la col·lecció. Després tindrem una taula d'aquests objectes.

Propostes:

A l'Exemple |  Taxistes |  Alumnes  |  Gossos
-----|----|----|---
soldat_tip  | Taxista | Alumne | Gos
divisio_tip | Taxistes | Alumnes | Gossos
exercit_tip (getCapità)| Flota (getLliure)| Classe (getDelegat) | Gossera (getElMesGros)
exercits    |   Flotes | Institut | Gosseres

Entrega a la carpeta [pràctica 4](./practica04).

## Pràctica 5:

El mètode de la pràctica anterior, el que hem fet amb un `FOR i IN 1..` el podiem haver fet amb un `TABLE`. Es demana:
* Crea un segon mètode ( sense esborrar el mètode que ja tens programat ) que retorni el mateix objecte que el mètode que ja tens però ara l'ha de recuperar de la col·lecció mitjançant `TABLE`.
* Invoca els dos mètodes, el de la pràctica anterior i el nou. Posa el resultat dins variables.
* Amb un `IF` comprova si l'objecte que ens retornan els dos mètodes és el mateix. Per fer-ho, recorda modificar l'estructura de l'objecte retornat de manera que tingui un mètode `MAP`. Recorda que aquest tipus de mètode serveix per quan comparem un objecte amb un altre saber si són o no el mateix.

