# D'EN HAUT (From Above)

MMORPG médiéval dark fantasy sur Roblox, caméra à la troisième personne. Hub : **Aldrenord**.

Version actuelle : voir le fichier `VERSION` (chaque mise à jour augmente le dernier chiffre).

Ce dépôt sert à partager la place Roblox (`.rbxl`) et à suivre ce qui change d'une version à l'autre.
Le `.rbxl` est un fichier binaire : GitHub ne peut pas montrer ses différences. **Le journal ci-dessous est la référence.**

## Ouvrir le projet

1. Télécharger le `.rbxl` le plus récent (actuellement `From abv 1.0.11.rbxl`).
2. L'ouvrir dans Roblox Studio (Fichier → Ouvrir).
3. Avant de renvoyer une version : **Ctrl+S**, puis remplacer le fichier dans le dépôt et ajouter une entrée au journal.

## Commandes en jeu

| Touche | Action |
| --- | --- |
| Souris | tourner la caméra autour du personnage, regarder en haut / en bas (un clic ne fait pas tourner la caméra) |
| Molette | rapprocher / éloigner la caméra |
| Alt gauche (maintenir) | afficher le curseur |
| ZQSD | se déplacer (touches physiques : WASD en QWERTY) |
| Shift (maintenir) | courir, puis sprint progressif |
| C | s'accroupir / se relever (glissade si lancé) |
| Espace | sauter |
| Ctrl gauche | roulade ; en garde + gauche/droite : pas de côté ; sans direction : pas en arrière |
| Clic gauche | combo ; maintenir = attaque chargée |
| Clic droit | garde (parade si levée juste avant l'impact) |
| X | sortir / ranger l'arme |
| G | intimidation |
| Tab | inventaire (clic : mettre en main ou porter une armure ; clic droit : donner / jeter) |
| `,` | lire la carte : le personnage sort le parchemin de sa ceinture et la carte plein écran s'ouvre ; la même touche la ferme. Bouger ou sauter la ferme aussi. Dans la carte : clic gauche ou molette (clic) maintenu = déplacer, molette = zoom, clic droit = poser un ping, clic sur un ping = ses options. La touche est lue selon la disposition du clavier (AZERTY : la virgule) |
| Flèche haut / bas | objet « quick use » suivant / précédent (les flèches ne font plus avancer ni reculer) |
| V | utiliser l'objet « quick use » sélectionné |
| A, E, R | sorts 1, 2 et 3 (les losanges s'éclairent ; aucun sort derrière pour l'instant) |
| F | ramasser un objet au sol |
| F2 | menu admin (apparence, cheveux, armes) |

## Scripts principaux

| Script | Emplacement | Rôle |
| --- | --- | --- |
| Locomotion | ReplicatedStorage.Combat | Micro-mouvements procéduraux, pas, saut, accroupi, mouvements impulsés (roulade, pas, glissades), pose des pieds par cinématique inverse |
| CombatClient | StarterPlayer.StarterPlayerScripts | Entrées du joueur, allures, moteur d'animation de tous les personnages |
| CombatData | ReplicatedStorage.Combat | Tous les réglages (vitesses, distances, touches, délais) |
| CombatAnims | ReplicatedStorage.Combat | Animations de combat procédurales (poses clés) |
| CombatServer | ServerScriptService | Validation serveur : coups, garde, parade, esquives, accroupi, intimidation |
| PNJAnimation | ServerScriptService | Animations de base des PNJ (repos, marche, course, saut, chute) |
| BackSheathServer | ServerScriptService | Armes rangées sur le personnage : fourreau d'épée dans le dos, étuis des dagues croisés au creux des reins, dague de la main gauche |
| EquipementServer | ServerScriptService | Armes placées dans les mains (inventaire), donner / jeter, retour des armes à la réapparition |
| ThirdPersonCamera + Camera.CameraConfig | StarterPlayer.StarterPlayerScripts / ReplicatedStorage.Camera | Caméra à la troisième personne : orbite à la souris, zoom, obstacles, cadrage de combat. Tous les réglages dans `CameraConfig` (l'ancienne `TopDownCamera` est désactivée) |
| EnduranceService + Endurance.EnduranceConfig / EnduranceRules | ServerScriptService / ReplicatedStorage.Endurance | Endurance validée par le serveur : coûts des actions, régénération, épuisement, garde brisée |
| EnduranceJauge | StarterPlayer.StarterPlayerScripts | Arc d'endurance à côté du personnage |
| PoidsServer + Poids.PoidsConfig / PoidsRules | ServerScriptService / ReplicatedStorage.Poids | Poids de l'équipement porté : mobilité (vitesses, saut, roulade) et bonus de puissance |
| HudLosanges + HUD.QuickUse | StarterPlayer.StarterPlayerScripts / ReplicatedStorage.HUD | HUD à 4 losanges en pixel art (bas gauche) : 3 sorts A / E / R et le grand losange « quick use ». `QuickUse` tient la liste des objets (3 objets de test ; `QuickUse.setItems({...})` pour la remplir, l'événement `QuickUse.used` signale chaque utilisation). Remplace l'ancien HUD `HudDenHaut` (supprimé ; `HudClient` ne garde que l'inventaire) |
| MiniMap + MiniMapPings | StarterPlayer.StarterPlayerScripts | Mini-carte ronde en haut à droite : relevé du terrain en pixels fins, contour gris avec N / E / S / W, flèche rouge du joueur, aucune icône de mob. `MiniMapPings` y affiche les pings. Réglages dans `CFG` en tête de `MiniMap` |
| WorldMap + HUD.PingStore | StarterPlayer.StarterPlayerScripts / ReplicatedStorage.HUD | Carte plein écran (touche `,`) : tuiles dont le niveau de détail suit le zoom, déplacement, zoom, pings (8 couleurs, 8 icônes, verrouillage permanent ; un ping non verrouillé disparaît à l'arrivée ou après 3 min). `PingStore` partage la liste des pings et leurs textures avec la mini-carte |
| Combat.MapRead + Combat.ToggleArme | ReplicatedStorage.Combat | Animation « lire la carte » et parchemin à la ceinture : geste fonction d'une progression `u` (le geste inverse est le même geste à l'envers), mains posées par `ArmIK`, couche additive appelée par `CombatClient`. `ToggleArme` permet à la carte de ranger puis de ressortir l'arme. Réglages dans `MapRead.CFG` |
| UI.Police | ReplicatedStorage.UI | Police unique du jeu (Caesar Dressing, identifiant `rbxassetid://12187368843`). Tout texte d'interface doit utiliser `Police.FONT` ; pour changer de police partout, modifier cet identifiant |
| AdminMenuServer / AdminMenuClient | ServerScriptService / StarterPlayer.StarterPlayerScripts | Menu admin (F2) : peau, yeux, sourcils, coupe et couleur des cheveux, moustaches, barbes et couleur de la pilosité, armes. Catalogue dans `ReplicatedStorage.AdminMenu.Config` |
| Pilosite | ReplicatedStorage | Moustaches et barbes construites en blocs (sans mesh importé), avec leurs couleurs. Attributs joueur : `Look_Moustache`, `Look_Barbe`, `Look_PilositeCouleur` |

Les versions précédentes des scripts sont gardées dans `ServerStorage.Sauvegardes` (un dossier par étape).

## Journal des changements

### 1.0.11 — 2026-10-30 — Nouveau HUD à losanges, mini-carte, carte plein écran et lecture de carte

- **Ancien HUD supprimé** : `HudDenHaut` et `HUD.HudConfig` (vie, mana, niveau, mini-carte, raccourcis, arme) et tout le dessin de l'ancien `HudClient`, qui ne garde plus que l'inventaire et le masquage de la barre de vie Roblox. Seuls la jauge d'endurance (`EnduranceJauge`) et l'inventaire sont conservés. Anciens scripts gardés dans `ServerStorage.Sauvegardes.avant_refonte_hud`.
- **HUD à 4 losanges** (`HudLosanges`, en bas à gauche), en pixel art comme la mini-carte : une seule image de 127 × 93 px agrandie par un entier (hauteur visée = 15 % de l'écran). Géométrie reprise de la maquette : grand losange de rayon 39, petits de rayon 22, 3 px d'écart, les 3 petits en triangle à droite du grand. Les 3 petits sont les sorts A (haut), E (milieu), R (bas) : la lettre s'éclaire quand la touche est enfoncée, aucun sort n'est branché. Le grand losange montre l'objet « quick use » sélectionné (icône, 3 points de position, rappel de la touche V, sans nom ni quantité).
- **Quick use** (`HUD.QuickUse`) : 3 objets de test (potion de vie × 3, potion de mana × 5, élixir d'endurance × 2). Flèche haut = suivant, flèche bas = précédent (liste circulaire), V = utiliser (la quantité baisse). Il n'y a aucun effet de soin ni de mana : l'événement `QuickUse.used` est prévu pour les brancher. Les flèches sont absorbées (elles ne font plus avancer ni reculer).
- **Mini-carte** (`MiniMap`, haut droite) : ronde, nord fixe, contour gris biseauté avec N / E / S / W en texte, relevé du terrain en pixels de 2 studs avec ombrage de relief et grain, flèche rouge qui tourne avec le personnage, aucune tête de mob. Taille : 27 % de la hauteur de l'écran. `MiniMapPings` y affiche les pings (hors portée : collés au bord, à moitié transparents).
- **Carte plein écran** (`WorldMap`, touche `,` lue selon la disposition du clavier) : clic gauche ou clic molette maintenu = déplacer (la carte suit le curseur), molette = zoom autour du curseur (0,06 à 6 px par stud), bouton RECENTRER, coordonnées en bas à gauche. Le niveau de détail suit le zoom : un pixel de carte fait toujours 3 à 6 px à l'écran, moyenné sur 4 relevés aux niveaux larges. Clic droit = ping (8 couleurs, 8 icônes, case « verrouillé · permanent », suppression). Un ping non verrouillé disparaît quand on arrive à 6 studs (message « DESTINATION ATTEINTE ») ou après 3 minutes. Marqueurs de 22 px pour un écran de 1080 px de haut. Réglages dans `CFG`.
- **Lecture de carte animée** (`Combat.MapRead`) : la main droite prend le parchemin à la ceinture (dans le dos, à droite), le ramène devant le torse, la main gauche le rejoint, il se déroule et la tête se penche ; la carte plein écran s'ouvre à 78 % du geste (1,5 s en tout). À la fermeture, le même geste est rejoué à l'envers (1,1 s). Les mains sont posées par `ArmIK`. Bouger, sauter ou mourir annule. Si une arme est en main, elle est rangée avant (`Combat.ToggleArme`) puis ressortie après.
- **Police unique Caesar Dressing** (`UI.Police`, identifiant `12187368843`) : tout le texte d'interface l'utilise (HUD, mini-carte, carte, menu des pings, inventaire, menu admin F2, chiffres de dégâts) ; les lettres dessinées en pixels (A / E / R, V, N / E / S / W) sont maintenant du vrai texte. Tailles agrandies pour cette police : × 1,5 dans le menu admin, 6 → 9, 7 → 10 et 10 → 15 pour les petits textes de l'inventaire.
- **Liste des joueurs Roblox masquée** : elle s'ouvrait aussi avec Tab, en même temps que l'inventaire.
- Scripts créés : `HudLosanges`, `MiniMap`, `MiniMapPings`, `WorldMap`, `HUD.QuickUse`, `HUD.PingStore`, `UI.Police`, `Combat.MapRead`, `Combat.ToggleArme`. Scripts modifiés : `HudClient`, `CombatClient` (couche de lecture de carte, rangement de l'arme, police des dégâts), `Camera.CameraConfig` (la carte libère le curseur), `AdminMenuClient`, `HUD.Inventaire`, `UI.LosangeKit`. Supprimés : `HudDenHaut`, `HUD.HudConfig`. Sauvegardes ajoutées : `avant_refonte_hud`, `avant_anim_carte`.
- Testé en jeu : console sans erreur à chaque lancement. Carte plein écran : ouverture et fermeture, déplacement au clic gauche (décalage exact de 200 × 100 px pour 200 × 100 px de souris), zoom avant et arrière (tuiles du niveau de détail courant présentes), ping posé sous le curseur, couleur / icône / verrou du menu, marqueur sur la mini-carte, disparition d'un ping à l'arrivée avec son message, fermeture de la carte quand on marche. Animation de lecture : poses de la saisie dans le dos, du passage sur le côté, du portage et de la lecture, vues en captures. HUD : disposition, flèche haut (vie → mana), touche V (quantité × 3 → × 2), personnage immobile pendant les flèches. Police : 263 textes d'interface en Caesar Dressing, aucun autre ; inventaire (Tab) et menu admin (F2) vus en captures ; Tab n'ouvre plus que l'inventaire. Le contenu du `.rbxl` publié a été vérifié (nouveau code et police présents, absents de la 1.0.10).
- Non testé : déplacement au clic molette (l'outil de test ne le simule pas), rangement et ressortie automatiques de l'arme en main, geste inverse vu à l'œil (seule la fin du geste a été contrôlée), rendu à l'œil de la carte plein écran et de ses tuiles aux zooms extrêmes (tests dans une petite fenêtre Studio, agrandissement × 1), taille réelle du HUD et de la mini-carte sur un écran 1080p, ce que voient les autres joueurs (le geste et le parchemin ne sont pas répliqués), sauvegarde des pings d'une partie à l'autre (non faite), manette et tactile.

### 1.0.10 — 2026-10-30 — Mains nues en captures et nettoyage de la place

- **Mains nues en captures Mixamo** (Standing Idle → Fight Idle, Fight Idle → Standing Idle, Body Block), adaptées au squelette R15 (module `MocapMainsNues`, haut du corps seulement) : la garde se lève et se rabaisse au rythme de l'acteur, les jambes descendent dans la garde du jeu en suivant la flexion des jambes de la capture, et le blocage (clic droit) joue la capture du Body Block. Les bras sont écartés s'ils entrent dans le torse ou la tête. Réglages dans `CombatData` : `FIST_GUARD_MOCAP`, `FIST_GUARD_IN_SPEED` 1,5, `FIST_GUARD_OUT_SPEED` 1,3, `FIST_BLOCK_ANIM`, `FIST_BLOCK_SPEED` 1,25. Animations ajoutées dans `CombatAnims` : `fist_guard_in`, `fist_guard_out`, `fist_block`.
- **Menu admin (F2)** : anciennes peaux et anciens sourcils (dont « Court ») retirés de la liste ; nouvelles peaux (Blanche, Olive, Foncée) et nouveaux sourcils (Blanc, Blond, Gris, Noir, Roux) listés.
- **Nettoyage de la place** : suppression des scripts désactivés `MedievalCharacter` et `TopDownCamera`, de la `MiniCarte` désactivée, des outils de test `ServerStorage.ArmuresEssai`, de l'aperçu `Workspace.PersoBloc_Apercu`, du ciel et de l'atmosphère par défaut en double dans Lighting. Dans `ServerStorage.Sauvegardes`, 40 des 43 dossiers supprimés (il reste `avant_captures_deplacement`, `avant_f2_textures`, `avant_anims_mains_nues`) : les anciennes versions restent dans l'historique Git (les sauvegardes `avant_marteau` à `avant_garde_marteau` citées dans 1.0.9 n'existent plus dans la place). `HideDefaultHealth` et `LosangeKit` sont conservés.
- Scripts créés : `MocapMainsNues`. Scripts modifiés : `CombatClient`, `CombatAnims`, `CombatData`, `AdminMenu.Config`.
- Testé en jeu : lancement d'une partie après le nettoyage, console sans erreur ; pendant l'intégration des mains nues, aucune erreur en console et saut de poing d'au plus 0,25 stud par image. Le contenu du `.rbxl` publié a été vérifié (nouveau code présent, scripts supprimés absents).
- Non testé : vrai combat à mains nues contre un PNJ, garde levée en marchant, rendu à l'œil des 3 animations en jeu, ce que voient les autres joueurs, manette et tactile.

### 1.0.9 — 2026-10-30 — Marteau en capture, roulades lourd / léger et déplacements capturés

- **Marteau** (nouvelle arme, 7 kg) : endurance (coup 18, finisher 26, chargé 24 à 40), porté dans le dos et en main ; trot quand on est épuisé (Shift ne fait plus courir, allure `TROT_K` 0,42).
- **Coups du marteau en captures Mixamo** (Great Sword Slash découpé en 3 coups, 1 clic = 1 coup ; Heavy Weapon Swing ; Great Sword Jump Attack), adaptées au squelette R15 (module `MocapMarteau`) : poids de la masse (ressort, affaissement, contact au sol, étincelles), arrêt sur image et secousse de caméra aux impacts, gros impact au sol pour le coup sauté et le coup unique, frappes au sol devant le perso (bras par cinématique inverse), 2e coup plus lent qui racle le sol (étincelles, son de métal), pause essoufflée puis relevé lourd après les coups au sol, délai avant de refrapper, coups impossibles à interrompre (super-armure, dégâts quand même pris). Port en diagonale à deux mains à l'arrêt, garde (clic droit) cohérente avec le port, marteau posé et traîné au sol en maintenant Shift (posé / ramassé sans que l'arme « vole »), bras qui ne traversent plus le corps pendant la charge.
- **Textures** : nouvelles peaux (blanc, foncée, olive), yeux (6 couleurs) et sourcils (5 couleurs) dans `ReplicatedStorage.Apparence` ; appliquées au perso de départ, à l'aperçu, au Mannequin et au Combattant.
- **Roulades** (Universal Animation Library 1 de Quaternius, CC0, module `MocapRoulade`) : poids de référence 75 kg (corps 70 + équipement). Lourd : ROLL (roulade sur place, déplacement donné par le jeu, environ 7,5 studs avec le marteau). Léger : ROLL RM (vrai déplacement de la capture, ramené à 12 studs), une seule fois : une roulade enchaînée pendant la précédente ou moins de 0,45 s après est une ROLL normale.
- **Déplacements en captures Mixamo** (module `MocapLoco`) : marche, départ, arrêt, marche arrière en garde (jambes), demi-tour en marchant, saut sur place et en mouvement, montée de marches. La cadence suit la distance réellement parcourue ; les sauts suivent la vitesse verticale réelle. À la conversion : bras écartés s'ils entrent dans le torse ou la tête, hanches écartées si les jambes se croisent, bassin recalé pour que le pied le plus bas touche le sol. En jeu, les pieds sont soulevés s'ils entreraient dans le sol ou une marche. La course et le sprint gardent l'animation de course.
- **Vitesses** : marche 6 studs/s (avant 12) pour coller à la capture de marche ; épée 5,5 (avant 8), marteau 5 (avant 9) ; course et sprint inchangés. Garde et charge gardent à peu près leur allure grâce à `GUARD_MOVE_K` 0,9 et `CHARGE_MOVE_K` 0,8. Préparation du saut sur place 0,3 s (avant 0,24). Le demi-tour capturé ne se lance que si l'on avançait face à son regard (pas en relâchant la garde en reculant).
- Scripts créés : `MocapMarteau`, `MocapRoulade`, `MocapLoco`. Scripts modifiés : `CombatClient`, `CombatServer`, `CombatAnims`, `CombatData`, `Locomotion`, `PoidsConfig`, `EnduranceConfig`, `EnduranceRules`, `BackSheathServer`, `HUD.Inventaire`, `HudDenHaut`. Sauvegardes dans `ServerStorage.Sauvegardes` (de `avant_marteau` à `avant_captures_deplacement`).
- Testé en jeu :
  - marteau : coups, impacts, positions et vitesses des articulations, contact au sol, traîné au sol, garde ;
  - roulades : sans arme (70 kg) ROLL RM ~13 studs, puis ROLL ~7,8 studs si enchaînée, puis ROLL RM après une pause ; avec le marteau (77 kg) ROLL ~7,5 studs à chaque fois ;
  - marche sur sol plat : aucun pied sous le sol, pied d'appui qui glisse ~0,6 stud/s à 6 studs/s ;
  - escaliers de 0,8 stud : 14 images sur 391 avec un pied à plus de 0,1 stud dans une marche (0,18 au pire) ;
  - demi-tour 0 à 180° en 1,2 s ; sauts sur place et en marchant (jambes repliées, réception amortie) ; marche arrière en garde à l'épée (pieds au sol) ; course et marche avec le marteau ; garde relâchée en reculant sans demi-tour ;
  - console sans erreur.
- Non testé : coup sauté du marteau en jeu, rendu à l'œil de la plupart des gestes, ce que voient les autres joueurs, manette et tactile.
- Limites connues : le départ de marche est joué environ 2,5 fois plus vite que la capture ; les bras de la capture de marche sont presque tendus ; à l'arrêt en garde à l'épée, les pieds entrent de 0,25 à 0,3 stud dans le sol (déjà le cas avant cette version).

### 1.0.8 — 2026-10-29 — Moustaches, barbes et têtes carrées

- **Pilosité faciale** : nouveau module `Pilosite` (moustaches et barbes construites en blocs, sans mesh importé) avec une liste de couleurs. Le choix est gardé en attributs du joueur (`Look_Moustache`, `Look_Barbe`, `Look_PilositeCouleur`) et posé sur le personnage à l'apparition (`poserPilosite`).
- **Menu admin (F2)** : nouvel onglet « Pilosité » (barbes, moustaches, couleur de la pilosité), avec aperçu. Les attributs `Look_*` de pilosité sont remis à zéro avec le reste de l'apparence.
- **Têtes carrées** : le fichier contient une note de sauvegarde « avant import pilosité + têtes carrées » (anciennes têtes : taille 0,75 / 1 / 0,875) ; les anciennes valeurs sont conservées dans la place.
- Scripts créés : `Pilosite`. Scripts modifiés : le menu admin (partie serveur et partie client). Liste établie en comparant le contenu avec la 1.0.7, pas en relisant chaque script.
- Vérifié : le `.rbxl` décompressé contient bien le module `Pilosite`, l'onglet « Pilosité » et les trois attributs `Look_*`, absents de la 1.0.7.
- Non testé en jeu par Claude : l'affichage des barbes et moustaches, les couleurs, l'onglet du menu F2, les têtes carrées, la compatibilité avec les coupes de cheveux. Les mesures du journal de la 1.0.7 ne sont pas refaites.

### 1.0.7 — 2026-10-28 — Caméra 3e personne, endurance, poids et nouvel HUD

- **Caméra à la troisième personne** (inspirée de Where Winds Meet, sans en reprendre aucun élément) : derrière et au-dessus du personnage, angle plongeant de 14°, distance 11 (5 à 16 à la molette), personnage à 41 % de la largeur de l'écran. Rotation à la souris (curseur bloqué au centre ; Alt, Tab ou F2 le libèrent), angle vertical de -65° à +30°, lissage réglable. Obstacles : la caméra se rapproche vite et repart doucement ; dos au mur, elle passe au-dessus de l'épaule. En combat, le personnage se décale un peu plus et la caméra plonge de 10° de plus pour voir l'adversaire. Les attaques et la garde visent dans la direction de la caméra.
- **Endurance** (100 points, validée par le serveur) : coups 7 à 20 selon l'arme et le coup, coups chargés 11 à 32, roulade 20, pas en arrière 14, pas de côté 11, sprint 10 par seconde ; marche et course gratuites. Régénération de 32 par seconde après 0,9 s (3 fois plus lente en garde). À 0 : épuisé, plus de sprint jusqu'à 30. Coup bloqué : coûte ses dégâts (6 au minimum) ; sans assez d'endurance, la garde se brise (50 % des dégâts, étourdi 0,9 s). Une action est acceptée s'il reste au moins la moitié de son coût. Jauge : petit arc doré à gauche du personnage.
- **Poids de l'équipement** : poids total = 70 kg + objets équipés (dague 0,8, épée 3,5 ; le sac ne compte pas). Mobilité = 1 - 1,5 % par kg (70 % au minimum) appliquée à la course et au sprint, et en partie à la marche, au saut, à la roulade, à l'accroupi et au démarrage. Bonus de dégâts +0,7 % par kg (+15 % au plus), calculé par le serveur. Les cases d'armure de l'inventaire fonctionnent (4 armures d'essai dans `ServerStorage.ArmuresEssai`, non distribuées). L'inventaire affiche le poids des objets, la charge, la mobilité et la puissance. L'endurance utilise ce poids.
- **Nouvel HUD** d'après les maquettes, dans le style de la jauge d'endurance (traits en segments fins, palette terreuse de l'inventaire uniquement) : vie et mana en bas, niveau avec anneau d'expérience, mini-carte ronde au bord irrégulier, raccourcis A / E / R et arme en bas à droite. Presque invisible à l'arrêt (10 %), visible quand on bouge ou agit, pleinement visible en combat (dégâts reçus ou infligés, ennemi qui attaque à moins de 12 studs) pendant 6 s après la dernière action hostile. Rouge seulement à 25 % de vie ou moins. Tous les réglages dans `HudConfig`.
- Scripts créés : `ThirdPersonCamera`, `Camera.CameraConfig`, `Camera.CameraState`, `EnduranceService`, `Endurance.EnduranceConfig`, `Endurance.EnduranceRules`, `EnduranceJauge`, `PoidsServer`, `Poids.PoidsConfig`, `Poids.PoidsRules`, `HudDenHaut`, `HUD.HudConfig`. Scripts modifiés : `CombatClient`, `CombatServer`, `CombatData`, `EquipementServer`, `HUD.Inventaire`, `HudClient` (ancien HUD caché). Désactivés : `TopDownCamera`, `StarterGui.MiniCarte`.
- Testé en jeu :
  - caméra : immobile, marche / course / saut / roulade dans plusieurs directions, rotation complète, regard haut et bas, mur, couloir étroit, pente, combat, réapparition, Tab / Alt / F2 ;
  - endurance : coûts des attaques et des roulades mesurés, sprint 10 par seconde sur 10 s, épuisement et reprise, garde brisée (seuil abaissé pour le test), changement d'arme sans remise à zéro, mort et réapparition, 40 roulades envoyées en 2 s par un client modifié dont 4 acceptées ;
  - poids : vitesses mesurées (sprint 19,5 sans équipement, 13,6 avec épée + cuirasse), saut 3,95 / 2,94 studs, roulade 7,93 / 6,70 studs, dégâts d'un jab 6,000 / 6,756 avec la cuirasse, remplacement d'armure sans double comptage, retour exact à la base ;
  - HUD : fondu à l'arrêt et en mouvement, combat en infligeant et en recevant des coups, barre de vie fidèle aux vrais PV, vie basse, mode rouge à 22 et 18 PV (pas à 26 ni en combat à 92 PV) ;
  - console sans erreur.
- Non testé : sensation de la souris à la main (rotation testée par un signal interne), manette et tactile, multijoueur et PvP, autres résolutions d'écran (seulement calculées), accélération au démarrage selon le poids (calculée), fondus de l'HUD après sa dernière refonte visuelle.
- Limites connues : l'image de référence de la caméra n'avait pas été fournie ; au corps à corps, notre personnage cache encore une partie du torse de l'adversaire ; un client modifié qui sprinte épuisé est seulement signalé dans la console ; les armures d'essai n'ont ni modèle ni défense et disparaissent à la mort ; mobilité et puissance atteignent leur plafond vers 20 kg ; le niveau, l'XP et le mana n'existent pas encore dans le jeu (l'HUD affiche 1, 0/100 et 80/80) ; la mini-carte est orientée nord en haut.

### 1.0.6 — 2026-10-25 — Sauts, roulade et sprint progressif

- **Saut sur place en 5 temps** : préparation (0,16 s : genoux et hanches fléchis, bassin 0,78 stud plus bas, pieds à plat, bras tirés en arrière avec un léger retard), poussée au sol (0,08 s : hanches, genoux et chevilles tendus jusqu'à la pointe des pieds, bras qui balancent vers le haut ; les pieds quittent le sol après l'extension, avant le personnage décollait encore accroupi), phase aérienne (jambes repliées, bras écartés au sommet), descente (jambes qui se déplient, pieds remis à plat), réception (genoux jusqu'à 86° au lieu de 48°, tronc droit, retour stable en ~0,25 s).
- **Saut en courant en appel sur une jambe** : dernière foulée qui charge la jambe d'appel (bassin 0,31 stud plus bas, jambe libre derrière, bras opposé en arrière), poussée au sol (jambe d'appel tendue, genou libre qui monte, bras opposé vers le haut), foulée en l'air avec un écart des cuisses de 116° (au lieu d'environ 70°), réception en courant. La jambe d'appel alterne à chaque saut ; corrigé : les jambes s'inversaient au décollage.
- **Roulade décomposée** en 6 phases enchaînées (élan, abaissement, contact en diagonale sur l'épaule droite menton rentré et tête décalée, rotation groupée, sortie accroupie, reprise de course) ; chaque partie du corps part avec son propre décalage (jambes, bassin, buste, tête, bras droit, bras gauche). 81 clés, hauteur du bassin calculée sur le rig. L'animation dure 0,74 s (le déplacement reste de 8,1 studs en 0,6 s).
- **Sprint** : la touche V est supprimée. Shift maintenu : course en ~1 s (17), puis sprint qui s'installe en ~1,5 s jusqu'à 19,5 (au lieu de 21 avec V) ; buste plus penché, bassin un peu plus bas, bras plus fléchis. Dagues 19, épée 16,5.
- **Genou au sol** : arrive 2,65 s après s'être accroupi (au lieu d'environ 7 s), descente en 0,9 s.
- Scripts modifiés : `Locomotion` (poses et phases des deux sauts, côté de la jambe d'appel, sprint visible, réglages `JUMP_PREP` 0,24, `JUMP_PREP_RUN` 0,15, `KNEEL_IDLE` 2, `KNEEL_IN` 0,9, `SPRINT_LEAN` 12), `CombatAnims` (`roll_forward`), `CombatClient` (touche V retirée, sprint progressif sous Shift), `CombatData` (vitesses de sprint, `SPRINT_BUILD`).
- Testé en jeu : saut sur place et saut en courant enregistrés image par image puis revus de profil, hauteur 3,95 et portée en sprint 8,4 inchangées, écart des cuisses 116° sur deux sauts (une jambe d'appel de chaque côté), roulade (8,1 studs en 0,6 s, corps jamais sous le sol à plus de 0,07 stud), V sans effet, montée de vitesse sous Shift (13 → 17 en 1 s → 19,5), genou au sol, console sans erreur.
- Non testé : roulade observée à l'œil en jeu (vérifiée image par image hors jeu et mesurée en jeu), sauts avec une arme en main, vue par un deuxième joueur, manette, tactile.
- Limites connues : à l'arrêt, le saut part 0,24 s après l'appui (0,15 s en courant), le temps de la préparation ; pendant la rotation de la roulade, le corps flotte d'environ 0,2 stud une ou deux images ; la roulade bloque les attaques environ 0,15 s de plus qu'avant.

### 1.0.5 — 2026-10-25 — Franges et longueurs des coupes

- **Franges** ajoutées aux coupes 2 à 9 pour moins montrer le front : mèches en désordre (2, 4, 8), mèche retombant sur le côté (3), rideau avec raie au milieu (5), frange balayée (6, 7), frange posée sur le front (9). Les mèches ne descendent jamais sur les sourcils.
- **Longueurs raccourcies** : coupe 5 (longs détachés) 1,69 stud de haut au lieu de 2,25 ; coupe 6 (queue-de-cheval) 1,44 au lieu de 1,75 ; coupe 8 (tressés) 1,69 au lieu de 2,38. Les cheveux tombent maintenant sur le haut du dos.
- Coupes 1 et 10 inchangées. Même texture qu'avant : les couleurs du menu F2 s'appliquent toujours aux 10 coupes. La coupe 9 garde `Transparency = 0.01` (zone rasée semi-transparente).
- Modèles remplacés dans `ReplicatedStorage > Cheveux` (`Coupe2` à `Coupe9`, nouveaux attributs `Decalage`) et sur les PNJ (Mannequin coupe 9, Combattant coupe 6). Aucun script modifié.
- Un fichier « Front Fix » fourni à part n'a pas été utilisé : ses faces pointaient vers les mauvais sommets (coupes 6 à 9 mélangées entre elles) et il reprenait les anciennes longueurs. Les franges ont été refaites dans le générateur des coupes.
- Testé en jeu : coupes 5 et 9 avec frange posées depuis le menu F2 (position sur la tête correcte, frange au-dessus des sourcils, zone rasée transparente) ; coupes 5, 6 et 8 raccourcies posées et vues de dos avant l'ajout des franges ; coupes des deux PNJ remplacées.
- Non testé : coupes 2, 3, 4, 6, 7 et 8 avec frange sur le personnage en jeu (vues seulement en aperçu), vue par un deuxième joueur, manette, tactile.

### 1.0.4 — 2026-10-25 — Coupes de cheveux

- **10 coupes de cheveux** en blocs, au même grain que le visage (1 bloc = 1 pixel de la face, 1/16 stud) : courts classiques, courts ébouriffés, mi-longs en arrière, mi-longs ébouriffés, longs détachés, queue-de-cheval, chignon, tressés, rasés sur les côtés, bouclés. De 838 à 4 254 triangles par coupe.
- **Couleur séparée de la coupe** : une seule texture-palette (256 × 128, 7 cases : 5 tons, zone rasée, attache) sert aux 10 coupes. Une nouvelle couleur = une seule image à importer. Seule « Brun » existe pour l'instant.
- **Coupes par défaut** : joueur coupe 1 (posée sur `StarterCharacter`), Mannequin coupe 9, Combattant coupe 6.
- **Menu admin (F2) > Apparence** : sections « Coupe » (les 10 + « Aucune », avec aperçu 3D) et « Couleur des cheveux ». Le choix est gardé dans les attributs du joueur `Look_Coupe` et `Look_CouleurCheveux` et remis à chaque réapparition ; « Tout réinitialiser » revient à la coupe 1.
- **Coupe 9** : la zone rasée est une fine couche semi-transparente posée sur la peau. La pièce a `Transparency = 0.01`, sinon Roblox ignore la transparence de la texture et la zone s'affiche en noir.
- Modèles : `ReplicatedStorage > Cheveux` (`Coupe1` à `Coupe10`, attributs `Decalage`, `NomCoupe`, `Ordre`), soudés à la tête (`Head > Hair`, soudure `HairWeld`).
- Scripts modifiés : `AdminMenuServer` (pose / changement de coupe et de couleur), `AdminMenuClient` (sections Coupe et Couleur des cheveux), `AdminMenu > Config` (catalogue `CouleursCheveux`).
- Testé en jeu : coupe 1 au spawn (visible aussi dans le buste du HUD et dans l'aperçu de l'inventaire), aperçus du menu, passage aux coupes 9 et 1 depuis le menu, coupe gardée après une mort (coupe 5), « Aucune », retour à la coupe 1 par défaut, coupes des PNJ présentes, zone rasée transparente.
- Non testé : chaque coupe une par une sur le personnage en mouvement (seulement 1, 5 et 9 ; les autres vues uniquement en aperçu), autre couleur que Brun, vue par un deuxième joueur, manette, tactile.
- Limites connues : les cheveux sont soudés à la tête ; les cheveux longs (5, 8) peuvent traverser le dos pendant certaines animations (non vérifié). Si `Transparency` de la coupe 9 est remis à 0, la zone rasée redevient noire.

### 1.0.3 — 2026-10-25 — Dagues jumelles et attente genou au sol

- **Dagues jumelles (nouvelle arme)** : deux « Dague d'Aldrenord » (type Dagger), une par main dans l'inventaire (Tab). Modèle 3D fait dans Blender d'après l'image de référence : lame à arête, garde courbe à têtes de lion, poignée torsadée, pommeau à croix ; étui en cuir bordeaux à chape et bouterolle en bronze. Longueur 1,75 stud.
- **Port** : étuis croisés au creux des reins (sur les hanches, ils traversaient les bras et les cuisses). Dague droite = outil Roblox, dague gauche = copie soudée à la main gauche pendant qu'elle est sortie.
- **X** : les deux mains vont chercher les poignées dans le dos ; les dagues passent dans les mains au moment où les mains sont sur les poignées, puis reviennent devant. Rangement : geste inverse.
- **Clic gauche** : combo de 4 coups (taille droite, taille gauche, estoc droit, tourbillon à deux lames) ; maintien = double estoc chargé ; attaques après roulade et en l'air. **Clic droit** : garde / parade (lames dressées devant le visage). Dégâts validés par le serveur.
- **Démarche** : lames prêtes devant, bras qui accompagnent le pas ; vitesses d'arme légère (marche 12,4, course 18, sprint 20,5 mesurés, au lieu des vitesses de l'épée).
- **Attente genou au sol** : après 6 s accroupi sans bouger, le personnage pose le genou gauche (descente 1,1 s, relevé 0,45 s dès qu'on bouge, tourne, saute, se met en garde ou appuie sur C). Genou, pointe du pied gauche et pied droit à 0,00 stud du sol.
- Scripts modifiés : `CombatAnims` (animations des dagues), `CombatData` (arme Dagger, gestes de dégainage par arme, vitesses par arme), `CombatClient` (dégainage / rangement selon l'arme, traînées des deux lames, démarche dagues, vitesses), `CombatServer` (gestes de dégainage acceptés selon l'arme), `BackSheathServer` (étuis et dague de la main gauche), `Locomotion` (genou au sol). Modèles : `ServerStorage > Armes` (Dague, EtuiDague, Import_FBX) ; outils dans `StarterPack`. Sources Blender / FBX : `D:\1- ROBLOX PROJECT\Armes\` chez Desno972.
- Testé en jeu : équiper / retirer depuis l'inventaire (étuis qui suivent), dégainer / ranger, combo sur le mannequin (7, 7, 10, 9 + 9 dégâts), attaque chargée (30), garde, vitesses, accroupi dagues sorties, mort dagues sorties puis réapparition (3 objets, aucun doublon, dagues remises en main), épée toujours fonctionnelle, genou au sol (descente, relevé en marchant et avec C), console sans erreur.
- Non testé : attaque après roulade et attaque en l'air avec les dagues, vue par un deuxième joueur, manette, tactile.
- Limites connues : épée en main droite + dague en main gauche → la dague reste rangée (X sort seulement la main droite) ; pas d'AnimationId Roblox (animations procédurales, comme l'épée) ; en s'accroupissant ou en se relevant avec C, les pieds passent environ 0,3 stud sous le sol pendant environ 0,15 s (déjà présent avant).

### 1.0.2 — 2026-10-25 — Roulade, accroupi, bras du saut et dérapages

- **Roulade** refaite (45 poses clés) : appel, enroulement compact, membres décalés, sortie avec bras d'équilibre. Le corps ne passe plus sous le sol (−0,02 stud au plus bas, contre −0,95 avant) ; les coudes bougent de 31° à 89°, le bras gauche suit le droit avec un léger retard.
- **Accroupi** : haut du corps voûté, relâché et asymétrique (torse −20° à −23°, léger roulis ±3°, balancement des bras en marchant). Jambes inchangées.
- **Repos accroupi** (nouveau) : regards irréguliers de la tête (toutes les 1,2 à 3,8 s), léger balancement du bassin, petits ajustements d'épaules et d'avant-bras. Les pieds ne glissent pas (0,000 stud). Les mimiques debout sont coupées en position accroupie.
- **Bras du saut** : impulsion vers l'arrière, phase en l'air asymétrique (écart moyen entre les bras 24,6° à 33°, contre 3,8° avant), préparation à la réception juste avant le sol et amorti proportionnel à l'impact.
- **Dérapages D↔Q** : partent du bon côté dans les deux sens ; bassin abaissé (2,13 au lieu d'environ 2,68 en course) ; bras extérieur écarté pour l'équilibre, bras intérieur replié ; jambes, puis torse, puis bras ; phase de relance de 0,2 à 0,3 s sans délai ajouté.
- Scripts modifiés : `Locomotion` (poses SKID, SKID_REC, PRELAND, CROUCH, saut ; groupes décalés des mouvements impulsés ; repos accroupi ; inertie des bras ; réglages `SKID_RECOVER`, `CROUCH_IDLE`, `ARM_INERTIA`), `CombatAnims` (`roll_forward`), `CombatClient` (mimiques coupées en accroupi).
- Testé en jeu : roulade, marche et repos accroupis, deux sauts (course et sprint), dérapages D→Q et Q→D, glissade, vitesses (marche 12, course 17, sprint 21, accroupi 4,5 — inchangées), console sans erreur.
- Non testé : roulade épée en main, chute de grande hauteur, portées du saut et de la roulade (non remesurées), vue par un deuxième joueur, manette, tactile.
- Limites connues : pendant la glissade (C), le corps passe 0,17 stud sous le sol un court instant ; à l'impact d'un saut, les pieds passent sous le sol une seule image (16 ms).

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
