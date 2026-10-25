# Règles du dépôt From-Above (pour Claude)

Ce dépôt partage la place Roblox de **D'EN HAUT** entre Desno972 et Arika00.
**Après chaque mise à jour du jeu, publier automatiquement une nouvelle version**, sans attendre qu'on le demande.

## Version

- Le numéro est dans `VERSION` (format `MAJEUR.MINEUR.CORRECTIF`, ex. `1.0.1`).
- Chaque publication augmente le dernier chiffre de 1 (`1.0.1` → `1.0.2`). On ne change le chiffre du milieu ou le premier que si l'utilisateur le demande.
- Le fichier de la place dans le dépôt s'appelle `From abv <version>.rbxl`. Il n'y a qu'**un seul** `.rbxl` dans le dépôt : l'ancien est supprimé (il reste dans l'historique Git).

## Procédure de publication

1. **Place enregistrée** : Studio ne peut pas être enregistré à distance. Demander à l'utilisateur de faire **Ctrl+S** (ou Fichier → Enregistrer sous dans `D:\1- ROBLOX PROJECT\`), puis attendre sa confirmation.
2. **Fichier source** : le `.rbxl` le plus récemment modifié de `D:\1- ROBLOX PROJECT\` (y compris le sous-dossier `Save.rbxl\`). Vérifier que sa date de modification est postérieure aux changements faits ; sinon redemander l'enregistrement. Ne jamais envoyer de fichier `.lock`.
3. **Vérification du contenu** : le `.rbxl` est binaire (blocs LZ4 / zstd). Le décompresser et y chercher quelques mots-clés propres aux changements (nom d'une fonction, d'un réglage). S'ils sont absents, la place n'a pas été enregistrée après la modification : ne pas publier.
4. **Fichiers** : copier la place sous `From abv <nouvelle version>.rbxl`, supprimer l'ancien `.rbxl`, mettre à jour `VERSION` et la ligne « Télécharger » du README.
5. **README** : ajouter en haut du « Journal des changements » une entrée `### <version> — <date> — <titre>` avec :
   - ce qui a été ajouté ou corrigé, en phrases courtes, avec les valeurs mesurées quand il y en a ;
   - les scripts modifiés ;
   - ce qui a été testé en jeu et ce qui ne l'a pas été (ne jamais dire « testé » pour du code seulement relu).
6. **Commit et push** sur `main` : message `Version <version> : <titre>`, un seul commit par version.
7. **Release** : le push déclenche l'action `.github/workflows/release.yml`, qui crée la release `v<version>` avec le `.rbxl`, le README et l'entrée du journal comme notes (la session Claude ne peut pas créer de release elle-même). Vérifier qu'elle est apparue (`gh run list` / `gh release view v<version>`) ; si l'action a échoué, lire son journal et corriger.
8. Donner à l'utilisateur le numéro publié et le lien de la release.

## À ne pas faire

- Ne pas réécrire l'historique (`push --force`, rebase de `main`).
- Ne pas publier de fichiers `.lock`, de sauvegardes temporaires ou de fichiers hors du jeu.
- Ne pas modifier les entrées passées du journal, sauf pour corriger une erreur factuelle.
