**************************************************
Projet HCL : Ajout et modification d'instructions
**************************************************

Ce projet vise à modifier et enrichir un processeur Y86 écrit en HCL, 
en travaillant sur plusieurs exercices comprenant la factorisation, 
l'ajout de nouvelles instructions et l'adaptation du comportement 
des instructions existantes.

==================================================
Exercice 1 : Modification de CALL et PUSHL
==================================================

-----------------------
Q1-Factorisation de CALL et PUSHL
-----------------------

- Nous avons modifié le tableau des instructions pour attribuer à CALL 
  le même icode que PUSHL, mais avec un ifun égal à 1.

Dans le code HCL :
* Suppression de la déclaration spécifique de CALL.
* Utilisation de PUSHL avec ifun=0 (ancien PUSHL) et ifun=1 (ancien CALL).
* Lorsque les deux instructions étaient nécessaires, seule PUSHL était utilisée, 
  avec la bonne valeur d'ifun.

-----------------------
Q2-Modification du comportement
-----------------------

- Cette fois, le tableau des instructions n'a pas été modifié.

- Suppression de PUSHL dans le code HCL et introduction d'une nouvelle 
  instruction appelée PUCA utilisant le même icode.

- Toutes les occurrences de PUSHL ont été remplacées par PUCA.

==================================================
Exercice 2 : Ajout de l'instruction LODSL et STOSL
==================================================

-----------------------
Q1/Q2-Ajout de LODSL
-----------------------

- Ajout dans le tableau des instructions (icode=14, ifun=0).
- Déclaration de l'instruction dans le code HCL.

Adaptation des étages :
* Source : %esi utilisé comme srcA, srcB, et dstE (car %esi est incrémenté après lecture).
* Destination : rA (registre passé en argument).

À l'exécution :
* Addition de 4 à %esi.
* Lecture de la mémoire à l'adresse de %esi.

Test réussi : %edi incrémenté et %eax récupère la bonne valeur.

-----------------------
Q3-Ajout de STOSL
-----------------------

- Ajout de STOSL avec le même icode que LODSL, mais avec ifun=1.

Adaptation :
* srcA : registre rA (valeur à écrire).
* srcB et dstE : %edi (mis à jour après écriture).

À l'exécution :
* Incrément de %edi.
* Écriture de la valeur du registre rA dans la mémoire à %edi.

-----------------------
Q4-Clonage de strncpy
-----------------------

- Implémentation d'une fonction strncpy en Y86 utilisant LODSL et STOSL.
- La boucle copie les données de src vers dst :
  * Jusqu'à rencontrer une sentinelle (0).
  * Ou atteindre le nombre maximal d'éléments autorisé.

==================================================
Exercice 3 : Ajout de l'instruction LOOP
==================================================

- Ajout de l'instruction LOOP dans le tableau des instructions (nouveau icode).
- Déclaration de l'instruction et du registre %ecx (RECX) dans le HCL.

Fonctionnement :
* À chaque itération, décrémentation de %ecx.
* Si %ecx ≠ 0, saut à l'adresse valC.

+Pas de modification du bit d’état Z. 
+Gestion spéciale dans is_bch et new_pc.

==================================================
Exercice 4 : Ajout de LOOPE et LOOPNE
==================================================

-----------------------
Q1/Q2-Ajout dans le tableau des instructions
-----------------------

- LOOPE : même icode que LOOP, ifun=3 (comme je).
- LOOPNE : même icode que LOOP, ifun=4 (comme jne).

Le code HCL n'a pas été modifié car le branchement (BCH) dépend de l'ifun.

-----------------------
Q3-Clonage de strncpy avec LOOPE
-----------------------

- Reprise de la logique de strncpy précédente.
- Utilisation de LOOPE pour limiter la copie en fonction :
  * Du compteur.
  * Et de la détection du caractère nul.

==================================================
Remarques
==================================================

- Toutes les modifications ont été testées avec des programmes simples en Y86.
- Les fichiers sources nécessaires sont joints dans ce projet.