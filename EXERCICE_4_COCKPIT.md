# Exercice 4 - Challenge client cockpit superviseur

Durée :

- exercice : 2h ;
- correction / partage : 45 à 60 min.

Objectif : faire passer les devs d'une surcharge CSS ponctuelle à une réponse structurée à une demande client Apriso.

## Demande client

> En 10 secondes, un superviseur doit savoir quoi traiter en priorité.

Constat client :

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

Correction de référence :

```text
checkpoints/05-cockpit-solution/interpreter.css
```

## Contraintes

- Ne pas modifier `Portal/Styles/Default/`.
- Ne pas modifier la structure HTML.
- Ne pas transformer l'exercice en intégration libre.
- Si une classe métier semble nécessaire, la noter comme action Process Builder.
- `!important` seulement si une règle gagnante l'impose.
- Tester la largeur desktop, tablette, mobile.

## Paliers

| Palier | Objectif | Critères |
| --- | --- | --- |
| Bronze | Lisibilité client | priorité A3-03 visible, KPI lisibles, actions compréhensibles |
| Silver | Layout responsive | `grid` robuste, `flex` qui wrap, pas de scroll horizontal global |
| Gold | Maintenabilité | tokens, sections, sélecteurs courts, contextes séparés |
| Bonus | Finesse CSS | `clamp()` bien borné, fallback `var()`, subtilité inline documentée |

Bronze minimum attendu.

Silver / Gold valorisent les solutions solides.

Bonus pour les plus rapides.

## Travail demandé

1. Diagnostiquer l'écran de départ.
2. Identifier 5 problèmes CSS concrets.
3. Corriger le thème Dymasco.
4. Garder une architecture lisible.
5. Remplir la mini-note d'arbitrage.

## Problèmes attendus

- `grid-template-columns: repeat(4, 1fr)` fragile sur KPI.
- `FIWorkArea` en 3 colonnes trop rigide.
- actions en `flex` sans wrap utile.
- tableau avec `min-width` mais pas de wrapper utile.
- valeurs couleur directes au lieu de tokens.
- `clamp()` absent sur titres / KPI.
- sélecteur `nth-child` fragile.
- inline style simulé sur statut `blocked`.
- contexte mobile / M&M non traité.

## Mini-note d'arbitrage

À remplir par les apprenants :

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
- Mobile :
```

## Correction - trajectoire formateur

### Bronze

- Rendre la priorité A3-03 immédiatement visible.
- Mettre le bouton d'escalade en action dangereuse.
- Clarifier les KPI.
- Éviter que le tableau casse la page.

Points CSS :

- `.FIPriorityStrip`
- `.FIButton--danger`
- `.FIKpiGrid`
- `.FIQueueTableWrap`

### Silver

- Remplacer les colonnes fixes par des grilles adaptatives.
- Autoriser les actions à revenir à la ligne.
- Adapter `FIWorkArea` sous 920px.
- Gérer le très petit écran sous 560px.

Points CSS :

- `repeat(auto-fit, minmax(...))`
- `flex-wrap`
- `@media`
- `overflow-x: auto`

### Gold

- Ajouter des tokens utiles.
- Regrouper les sections.
- Remplacer les couleurs métier directes.
- Supprimer le sélecteur `nth-child`.
- Scoper les contextes Apriso.

Points CSS :

- `--dy-cockpit-gap`
- `--dy-critical-soft`
- `--dy-quality-soft`
- `.dy-client-priority .FIStatus`
- `.MMScreenContext`
- `.MobileAppScreenContext`

### Bonus

- Utiliser `clamp()` sans créer de taille extrême.
- Mettre un fallback `var()` si une valeur peut manquer.
- Documenter la dette inline / Direct formatting.
- Proposer une classe Process Builder stable.

## Règles d'équipe attendues

- Toute couleur de statut passe par un token.
- Les layouts cockpit utilisent `grid`; les groupes d'actions utilisent `flex`.
- Les règles contexte sont séparées : Portal, M&M, mobile.
- Aucun sélecteur `nth-child` pour porter une intention métier.
- Tout contournement d'inline style mentionne la dette Process Builder.
