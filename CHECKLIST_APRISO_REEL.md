# Checklist Apriso réel - CSS et thèmes

Objectif : préparer le transfert du simulateur vers l'environnement réel.

## Accès et périmètre

| Question | Réponse |
| --- | --- |
| Version exacte de DELMIA Apriso |  |
| Portal récent ou Classic Portal |  |
| Preuve utilisée : URL, UI, config, DevTools |  |
| Quel environnement ? | Dev / recette / prod |
| Peut-on lire le dossier des thèmes ? |  |
| Peut-on copier un thème ? |  |
| Peut-on modifier un thème custom ? |  |
| Qui déploie le thème ? |  |

## Thème actif

| Élément | À vérifier |
| --- | --- |
| Nom du thème actif |  |
| Chemin physique |  |
| Activation globale |  |
| Activation par URL `Theme=` |  |
| Cache navigateur / serveur |  |

## Fichiers CSS

| Fichier | Présent | Modifiable | Rôle confirmé |
| --- | --- | --- | --- |
| `apriso.css` |  |  | Portal |
| `interpreter.css` |  |  | Function Interpreter |
| `keyboard.css` |  |  | Saisie / raccourcis |
| Autres CSS contrôle |  |  |  |

## DevTools

À capturer pour un écran représentatif :

- liste des CSS chargés ;
- ordre de chargement ;
- règle gagnante sur un titre ;
- règle gagnante sur un bouton ;
- règle gagnante sur un tableau ;
- présence de styles inline ;
- classes de contexte sur `html` ou `body`.

## Process Builder

À vérifier :

- peut-on poser une CSS class sur un contrôle ?
- convention de nommage existante ?
- Direct formatting utilisé ?
- composants custom ?
- classes générées stables ou dynamiques ?

## Règles d'équipe proposées

- Ne jamais modifier le thème livré.
- Préfixer les classes custom : `dy-`.
- Préférer custom class Process Builder aux sélecteurs profonds.
- Documenter tout `!important`.
- Documenter toute compat upgrade.
- Tester au moins un écran Portal + un écran mobile/tablette si concerné.
