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
- `H'` : Fonction de hachage qui sert à mapper des *strings* dans `G` (oracle aléatoire) : m -> g^H(m)^
- `PRF` : Dans le contexte de ce laboratoire, nous allons utiliser AES-GCM 256
- `OPRF` : H(x, H'(x))^k^)

Sachant que nous voulons implémenter une version `HMQV`, il nous faut encore définir certains termes :   
- `PubU` : g^PrivU
- `PubS` : g^PrivS
- `KE1` : g^x^​
- `KE2` : g^y^​, Mac(Km1; g^x^, g^y^)
- `KE3` : Mac(Km2; g^y^, g^x^)

![KE](./assets/KE.png)

L'algorithme se comporte de la manière suivante :

1. L'utilisateur a comme entrées : son identifiant (`sid`), une constante d'identification fixée à 0 (`ssid`), le serveur à qui il envoie (`S`) et le password (`pw`)
2. L'utilisateur choisit `r` et x<sub>u</sub> dans l'ensemble des nombres Z~q~.
3. L'utilisateur assigne $\alpha$ à (H'(passw))<sup>r</sup> et X<sub>u</sub> à g<sup>x<sub>u</sub></sup>
4. L'utilisateur envoie $\alpha$ et X<sub>u</sub> au serveur S
5. Sur l'entrée α de l'utilisateur, le serveur doit faire :
   1. Vérifier que $\alpha$ ∈ G<sup>*</sup>. Si ce n'est pas le cas, on envoie ERROR
   2. Récupérer le fichier : `file[sid] = <k<sub>s</sub>, p<sub>s</sub>, P~s~, P~u~, c>
   3. Choisir x~s~ dans Z~q~ aléatoirement et calcule $\beta$ = $\alpha$^k_s^ et X~s~ = g^x_s^
   4. Calculer K = KE(p<sub>s</sub>, x<sub>u</sub>, P<sub>u</sub>, X<sub>u</sub>)
   5. Calculer `ssid`' = H(sid, ssid, $\alpha$)
   6. Calculer SK = f~k~(0, `ssid`') et A~s~ =  f~k~(1, `ssid`')
   7. Envoyer $\beta$, c, X~s~, et A~s~ à l'utilisateur
6. Sur l'entrée reçue par le serveur, l'utilisateur fait :
   1. Vérifier que $\beta$ ∈ G<sup>*</sup>. Si ce n'est pas le cas, on envoie ERROR
   2. Calculer `rw` = H(pw, $\beta$^1/r^)
   3. Il faut déchiffrer `c` avec la clé `rw`. Si le résultat est perpendiculaire (**A demander à Alex**), on envoie ERROR sinon on assigne (p~u~, P~u~, P~S~) au résultat de la fonction.
   4. Calculer K = KE(p<sub>u</sub>, x<sub>u</sub>, P<sub>s</sub>, X<sub>s</sub>)
   5. Calculer `ssid`' = H(sid, ssid, $\alpha$)
   6. Calculer SK = f~k~(0, `ssid`'), A~s~ =  f~k~(1, `ssid`') et A~u~ =  f~k~(2, `ssid`')
   7. Vérifier que A~s~ calculé est le même que A~s~ reçu, sinon on envoie ERROR
   8. Envoyer A~u~ au serveur
7. Sur l'entrée utilisateur, le serveur vérifie que A~u~ = f~k~(2, `ssid`'). si ce n'est pas le cas, on envoie ERROR, sinon OK


## Implémentation

