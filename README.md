# D'EN HAUT (From Above)

MMORPG médiéval dark fantasy sur Roblox, vue plongeante. Hub : **Aldrenord**.

Version actuelle : voir le fichier `VERSION` (chaque mise à jour augmente le dernier chiffre).

Ce dépôt sert à partager la place Roblox (`.rbxl`) et à suivre ce qui change d'une version à l'autre.
Le `.rbxl` est un fichier binaire : GitHub ne peut pas montrer ses différences. **Le journal ci-dessous est la référence.**

## Ouvrir le projet

1. Télécharger le `.rbxl` le plus récent (actuellement `From abv 1.0.1.rbxl`).
2. L'ouvrir dans Roblox Studio (Fichier → Ouvrir).
3. Avant de renvoyer une version : **Ctrl+S**, puis remplacer le fichier dans le dépôt et ajouter une entrée au journal.

## Commandes en jeu

| Touche | Action |
| --- | --- |
| ZQSD | se déplacer (touches physiques : WASD en QWERTY) |
| Shift | courir |
| V (maintenir) | sprinter |
| C | s'accroupir / se relever (glissade si lancé) |
| Espace | sauter |
| Ctrl gauche | roulade ; en garde + gauche/droite : pas de côté ; sans direction : pas en arrière |
| Clic gauche | combo ; maintenir = attaque chargée |
| Clic droit | garde (parade si levée juste avant l'impact) |
| X | sortir / ranger l'arme |
| G | intimidation |

## Scripts principaux

| Script | Emplacement | Rôle |
| --- | --- | --- |
| Locomotion | ReplicatedStorage.Combat | Micro-mouvements procéduraux, pas, saut, accroupi, mouvements impulsés (roulade, pas, glissades), pose des pieds par cinématique inverse |
| CombatClient | StarterPlayer.StarterPlayerScripts | Entrées du joueur, allures, moteur d'animation de tous les personnages |
| CombatData | ReplicatedStorage.Combat | Tous les réglages (vitesses, distances, touches, délais) |
| CombatAnims | ReplicatedStorage.Combat | Animations de combat procédurales (poses clés) |
| CombatServer | ServerScriptService | Validation serveur : coups, garde, parade, esquives, accroupi, intimidation |
| PNJAnimation | ServerScriptService | Animations de base des PNJ (repos, marche, course, saut, chute) |

Les versions précédentes des scripts sont gardées dans `ServerStorage.Sauvegardes` (un dossier par étape).

## Journal des changements

### 1.0.1 — 2026-10-25 — Réalisme du saut, de l'accroupi et de la garde

- **Pieds posés par cinématique inverse** : après toutes les couches d'animation, hanche / genou / cheville sont recalculés pour que le pied d'appui reste sur le sol. Le bassin peut bouger sans faire glisser les pieds.
- **Accroupi** : 3 pas/s de 1,3 à 1,5 stud (au lieu de 6,3 pas/s de 0,7 stud), glissement des pieds 0,05 à 0,17 stud/s (au lieu de 1,1).
- **Garde** : glissement des pieds 0,02 à 0,17 stud/s (au lieu de 0,9 à 2,8) ; pas latéraux plus longs (3,2 pas/s au lieu de 5,3) ; recul distinct (pointe posée en premier) ; plus de micro-pas à l'arrêt ni pour les petites rotations (moins de 22°) ; le rebond de boxeur s'efface en marchant.
- **Saut** : poussée, groupé et descente suivent la vitesse verticale réelle ; appel plus court quand on est lancé ; réception proportionnelle à l'impact, pieds gardés au sol pendant l'amorti ; direction 3 fois moins contrôlable en l'air (élan conservé).
- Inchangés et revérifiés : marche 12, course 17, sprint 21, accroupi 4,5 studs/s ; saut 3,95 studs de haut, portée 7,17 (course) / 8,57 (sprint) ; roulade 8,3 ; pas arrière 5,2 ; pas de côté 4,2.
- Limite connue : à l'impact d'un saut, les pieds passent sous le sol pendant une seule image (16 ms).
- Non testé : vue par un deuxième joueur, manette, tactile.

### 2026-10-24 — Refonte du mouvement (incluse dans 1.0.1)

- Sprint distinct de la course (V, 21 studs/s, montée en 0,5 s).
- Accroupi (C) avec collision réellement abaissée, impossible de se relever sous un plafond ; glissade sprint → accroupi.
- Demi-tour glissé quand on part à l'opposé en étant lancé.
- Intimidation corporelle (G), sans arme.
- Saut recalé (3,95 studs de haut), saut natif rétabli pour échelle / nage / siège.
- Roulade (8 studs), pas arrière (5), pas de côté (4) recalés ; un seul gestionnaire écrit les mouvements impulsés, les murs les arrêtent.
- Accélération et freinage progressifs réellement appliqués.
- PNJ animés (repos, marche, course, saut).
- Règles d'état : pas de saut pendant une roulade ou un coup, pas de roulade en l'air, remise à zéro à la réapparition.

### Avant le 2026-10-24

- Système de combat : combos mains nues et épée, attaque chargée, garde, parade, esquive en garde, réactions aux coups, PNJ d'entraînement.
- Micro-mouvements : respiration, regard vers la souris, inertie, transfert de poids, petits pas en garde, mimiques d'attente.
