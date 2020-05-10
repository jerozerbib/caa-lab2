<script type="text/javascript"
        src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS_CHTML"></script>

# Auteur : Jeremy Zerbib
# Labo 2 - CAA

## Description

L'idée de l'algorithme vient de l'[article](https://eprint.iacr.org/2018/163.pdf) détaillant le protocole à la page 47.
Dans le détail de l'algorithme, nous pouvons voir qu'il existe plusieurs étapes qui seront détaillées tout au long de cette descritpion.

Afin de définir certains termes, nous allons énumérer certaines constantes et valeurs ci-dessous :   
- `F` : Fonction pseudo-aléatoire (`PRF`)
- `U` : Utilisateur défini par `F`
- `S` : Serveur
- `OPRF` : Protocole interactif entre `S` et `U`; Oblivious Pseudo Random Function 
- `k` : Entrée du serveur pour la `PRF`; clé
- `x` : Entrée utilisateur dans le domaine `F`

A la fin du protocole, l'utilisateur apprend `F(k, x)` et le serveur ne connait rien, même pas la valeur de `x` ou de `F(k, x)`.

La base de `OPAQUE` réside dans `DH-OPRF` (Diffie-Hellman-OPRF) et qui se paramétrise comme ceci :   
- `H` : Fonction de hachage (Dans le cas de ce laboratoire, nous allons utiliser `SHA-3` avec un output de *256-bits*)
- `q` : Nombre premier aléatoire
- `G` : Groupe cyclique d'ordre `q`
- `H'` : Fonction de hachage qui sert à mapper des *strings* dans `G` (oracle aléatoire)

Sachant que nous voulons implémenter une version `HMQV`, il nous faut encore définir certains termes :   
- `PubU` : g^PrivU
- `PubS` : g^PrivS
- `KE1` : g^x
- `KE2` : g^y, Mac(Km1; g^x, g^y)
- `KE3` : Mac(Km2; g^y, g^x)
-

L'algorithme se comporte de la manière suivante :

1. L'utilisateur choisit `r` et x<sub>u</sub> <- \\[\mathbb{Z}\\]

## Implémentation

