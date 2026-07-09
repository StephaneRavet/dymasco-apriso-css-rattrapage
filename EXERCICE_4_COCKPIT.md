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

Comparer avec :

```text
Portal/Screens/exercice-4-cockpit-mm.html
Portal/Screens/exercice-4-cockpit-mobile.html
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
- Tester Portal, M&M et mobile.

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
2. Identifier les problèmes qui empêchent la décision en 10 secondes.
3. Corriger le thème Dymasco sans modifier le thème livré.
4. Garder une architecture CSS lisible.
5. Remplir la mini-note d'arbitrage.

## Points d'attention

- Priorité réelle de l'écran.
- Hiérarchie des informations.
- Stabilité du layout.
- Densité en contexte M&M.
- Usage mobile sans scroll horizontal global.
- Différence entre dette CSS et dette Process Builder.

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
- Portal :
- M&M :
- Mobile :
```

## Règles d'équipe attendues

- Toute couleur de statut passe par un token.
- Les layouts cockpit utilisent `grid`; les groupes d'actions utilisent `flex`.
- Les règles contexte sont séparées : Portal, M&M, mobile.
- Aucun sélecteur `nth-child` pour porter une intention métier.
- Tout contournement d'inline style mentionne la dette Process Builder.
