# The Last Dawn — Catalogue d'assets de map (résumé de conv, WIP)

## Priorités
🔴 indispensable pour Aldrenord · 🟠 zones extérieures · 🟡 plus tard.
On démarre par la catégorie 1 : architecture modulaire 🔴 (remparts, grande porte + herse, tours de guet, escaliers, murs pierre/bois/colombage, toits chaume/bardeaux/ardoise).

## Fiche de génération d'asset
Objet / Style voxel Vintage Story médiéval rustique forgé main / Format cuboïdes sur grille, 16 voxels = 1 bloc, rotations 22,5° max / Dimensions / Matériaux + palette (couleurs plates par voxel) / Détails / Interdit : courbes lisses, dégradés, brillance, éléments < 1 voxel / Variantes / lister éléments + dimensions avant de générer.

## Méthode
Si un asset déplaît en jeu : on le signale, on identifie la règle de la fiche en cause, et on la modifie dans le wiki dev.

## Étape 1 — règles de base (validées)
- 1 voxel = 0,25 stud ; 1 bloc = 4 studs
- Mur 64×64×8 voxels · porte 24×40 · fenêtre 20×24 · marche 16×4×8
- Pentes de toit : 22,5° et 45°
- Palette : pierre gris froid (3 tons), bois brun sombre (3), chaume ocre grisé (3), ardoise gris-bleu (2), colombage (poutres brun noir + remplissage crème sale), accents fer mat + rivets cuivre

## Direction : états des bâtiments
Chaque bâtiment est dérivé en états : vivant/siège → usé → abandonné → ruine → brûlé. « Maisons abandonnées/hantées » devient un état de pack. Assets défensifs (palissades, barricades, fortins, pièges) prioritaires pour le hub. Textures de vieillissement : pierre moussue, bois pourri, chaume noirci, suie ; lierre/ronces/mousse.

## En cours
- Question ouverte : ton d'Aldrenord (prospère malgré la menace vs marqué par le siège)
- Prochaine étape : étape 2 (murs et toits, dimensions listées avant génération)
- Idée : script Studio créant l'aile « Architecture » du musée
