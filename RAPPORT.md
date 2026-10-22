# Rapport par repo — 22 oct. 2026

Les fichiers vides ne portent pas de nom de repo : je les ai attribués d'après `info.json` et les fichiers uniques (`prototypes/` → ore, `utils/` → utils…). À vérifier si un cas semble mal placé.

## Tous les repos

- Presque tous les fichiers standards sont vides : `control.lua`, `data-updates.lua`, `data-final-fixes.lua`, `settings-*.lua`, `migrations.lua`, tous les `locale/*/config.cfg`. À supprimer tant qu'ils ne servent pas.
- `migrations.lua` à la racine ne sert à rien : Factorio lit un dossier `migrations/`.
- Un `settings copy.lua` (doublon) existe en plusieurs exemplaires. `local bool = {}` passé à `data:extend` fait planter le chargement, et `RM-stp-order` n'a ni préfixe cohérent ni locale.
- `CHANGELOGS.md` ne contient que `---`, doublon de `changelog.txt`.
- Versions : `main` est en 2.0, les autres en 2.1.

## assets

- **Corriger** : les `icon.png` (iron/copper) sont des fichiers texte, pas des PNG. Factorio plantera dès qu'un prototype les référence.
- **Corriger** : `graphics/README.md` est obsolète (cite `prototypes/util.lua`, absent, et du 32×32). Les icônes vanilla sont en 64×64.
- **Corriger** : arborescence `graphics/ores/realistic-iron-ore/icon.png` ≠ chemin attendu par `icon_path`. Les noms `realistic-*` ne correspondent à aucun prototype (`iron-ore` vanilla, `hematite`…).
- **Ajouter** : icônes de base par forme (roche, minerai, lingot, fluide, gaz) à teinter, plutôt que 88+ PNG ; sons et sprites plus tard.
- **Retirer** : `control.lua`, `migrations.lua`, `settings*.lua` inutiles pour un mod de ressources graphiques.

## utils

- **Corriger** `icon_path` : variable globale `folder` jamais déclarée, paramètre `mod` ignoré.
- **Corriger** `trancate` : fonction vide.
- **Aligner** les noms entre TODO et code : `add_proto`, `copy_proto`, `get_settings`.
- **Ajouter** tout ce qui est vide : `utils/ore.lua`, `assets.lua`, `building.lua`, `main.lua`, `function/multi.lua`, `function/world.lua`.
- **Ajouter** un loader des `*_data.lua` générés (c'est le vrai chaînon manquant avec ore).
- Les autres mods l'utilisent via `require("__realistic_production_utils__.utils.general")` : à documenter dans un README.

## ore

C'est le repo le plus avancé, mais rien n'est encore chargé.

- **Bloquant** : pas de `data.lua`. Les fichiers `prototypes/entity|item|recipe|technologie|globals|util.lua` sont tous vides (`technologie` → `technology`).
- **Données incomplètes** :
  - `materials_data` et `components_data` sont vides.
  - Sans composition : bauxite, wolframite, pentlandite, laterite, lepidolite, coltan, monazite, bastnasite, carnotite, apatite, charbons, tourbe, bitumes, tous les fluides sauf l'air, toutes les roches.
  - Conséquences : l'aluminium n'a de source que dans des silicates avancés. Le nickel n'a aucune source, donc pas d'acier inoxydable. Le silicium n'a de source qu'avancée alors qu'il est « basic ». Phosphore, vanadium, niobium/tantale et lanthanides n'ont aucune source.
- **Cohérence** :
  - Étain `advanced` mais cassitérite et bronze `basic`.
  - Thorium/uranium `advanced` mais minerais `experimental`.
  - `astatine` et `francium` sans `origin`.
  - Mélange `iron-ore` / `native_gold` (tirets/underscores).
  - `elements_data` est rangé dans `resources/` alors que ce sont des matériaux.
- **Risques Space Age** : `carbon`, `lithium`, `fluorine`, `sulfur` existent déjà dans le jeu. `add_proto` ignore alors tes versions en silence.
- **Settings** : « Allow pure ore nodes » et « Ore density multiplier » sans préfixe ni locale, et rien ne les lit encore.
- **Hors repo** : `generate_elements.py` et le tableur source n'étaient pas dans les fichiers fournis ; il faut les versionner.
- **Changelog** : daté 2025-11-05, à mettre à jour.

## building

- Tout est à faire : seul `data.lua` existe (vide).
- **Corriger** le nom `realistic_production_buidling` avant publication (renommer un mod plus tard casse les sauvegardes).
- À partir de `utils/building.lua` : `create_building`, catégories de craft, `fluid_boxes`, `create_machine`, multiblocs.

## core (`realistic_production_main`)

- `info.json` : `factorio_version` 2.0 et `base >= 2.0.69` (les autres : 2.1), description copiée d'assets, dépendance `buidling` fautive.
- Aucune logique : `data-updates` / `data-final-fixes` vides. Les fonctions d'intégration vanilla (`remove_resource`, `replace_ingredient`…) n'existent pas non plus.
- Nom du repo (`core`) ≠ nom du mod (`main`) : à unifier.

## Priorités

1. Faux PNG, `settings copy.lua`, faute `buidling`, `migrations/`.
2. `data.lua` d'ore + loader dans utils.
3. Compositions manquantes (aluminium, nickel, silicium, phosphore).
4. Locale générée + icônes teintées.
