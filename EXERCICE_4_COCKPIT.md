# Exercice 4 - Challenge client cockpit superviseur

Durée :

- exercice : 2h ;
- correction / partage : 45 à 60 min.

Objectif : répondre à une demande client Apriso avec une surcharge CSS propre.

## Demande client

> En 10 secondes, un superviseur doit savoir quoi traiter en priorité.

Constat :

- écran actuel trop plat ;
- priorité A3-03 pas assez visible ;
- KPI difficiles à comparer ;
- tableau OF trop large ;
- actions mal alignées ;
- tablette atelier pénible ;
- maintenance CSS risquée.

## Mise en place

Depuis la racine :

```text
cp checkpoints/05-cockpit-start/interpreter.css Portal/Styles/Dymasco/interpreter.css
```

Ouvrir :

```text
Portal/Screens/exercice-4-cockpit.html
```

Fichier de travail :

```text
Portal/Styles/Dymasco/interpreter.css
```

## Contraintes

- Ne pas modifier `Portal/Styles/Default/`.
- Ne pas modifier la structure HTML.
- Ne pas transformer l'exercice en intégration libre.
- Si une classe métier semble nécessaire, la noter comme action Process Builder.
- `!important` seulement si une règle gagnante l'impose.
- Tester en largeur desktop et tablette en redimensionnant le navigateur.

## Paliers

| Palier | Objectif | Critères |
| --- | --- | --- |
| Bronze | Lisibilité client | priorité A3-03 visible, KPI lisibles, actions compréhensibles |
| Silver | Layout responsive | `grid` robuste, `flex` qui wrap, pas de scroll horizontal global en largeur tablette |
| Gold | Maintenabilité | tokens, sections, sélecteurs courts, règles lisibles |
| Bonus | Finesse CSS | `clamp()` bien borné, fallback `var()`, subtilité inline documentée |

- Bronze minimum.
- Silver / Gold = solution solide.
- Bonus = rapides.

## Travail demandé

1. Diagnostiquer l'écran de départ.
2. Identifier ce qui empêche la décision en 10 secondes.
3. Corriger `Dymasco/interpreter.css`.
4. Garder un CSS lisible.
5. Remplir la mini-note d'arbitrage.

## Déroulé conseillé

| Temps | Travail |
| --- | --- |
| 0-15 min | Observer, reformuler, lister les irritants |
| 15-35 min | Inspecter : règles gagnantes, largeurs, débordements |
| 35-70 min | Bronze : priorité, KPI, actions |
| 70-105 min | Silver : tablette, actions, scroll local |
| 105-120 min | Viser Gold : tokens, sections, dette Process Builder, mini-note |

## Points d'attention

- Priorité réelle.
- Hiérarchie visuelle.
- Layout stable.
- Pas de scroll horizontal global en tablette.
- Scroll local accepté pour le tableau.
- Dette Process Builder identifiée.

## Critères d'acceptation

Bronze :

- A3-03 ressort sans lire tout le tableau.
- Escalade = action sensible.
- KPI comparables.
- Les autres actions restent normales.

Silver :

- Pas de scroll horizontal global.
- Scroll local du tableau OK.
- Actions utilisables en largeur réduite.
- Cartes lisibles.

Gold :

- Couleurs métier via tokens.
- Sections lisibles.
- Pas de sélecteur dépendant d'une position de colonne.
- Inline style contourné = dette Process Builder documentée.

## Indices progressifs

Bronze :

- Qu'est-ce qui attire l'oeil en premier ?
- Quelle action est vraiment critique ?
- La priorité est-elle seulement textuelle ?

Silver :

- Réduire la largeur avant de coder.
- Trouver l'élément qui force la page.
- Choisir : réorganiser ou scroller localement.

Gold :

- Valeurs répétées ?
- Règle structurelle au lieu de métier ?
- Style inline avant `!important` ?

## Mini-note d'arbitrage

À remplir par les apprenants :

Livrable obligatoire : elle vérifie que la solution est transposable dans Apriso.

```text
Risques identifiés :
- 
- 
- 

Choix CSS :
- 
- 
- 

Dette / Process Builder :
- 

Tests faits :
- Desktop :
- Tablette :
```

## Règles d'équipe attendues

- Toute couleur de statut passe par un token.
- Les layouts cockpit utilisent `grid`; les groupes d'actions utilisent `flex`.
- Les règles responsive restent liées au besoin réel de l'écran.
- Aucun sélecteur `nth-child` pour porter une intention métier.
- Tout contournement d'inline style mentionne la dette Process Builder.
