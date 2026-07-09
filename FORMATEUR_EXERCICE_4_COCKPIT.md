# Formateur - Exercice 4 - Cockpit superviseur

Support interne. Ne pas distribuer tel quel aux apprenants.

## Intention pédagogique

L'exercice final doit vérifier que les apprenants savent transformer une demande client Apriso en CSS maintenable.

Ce n'est pas un exercice de copie de maquette.

Critère central :

> En 10 secondes, un superviseur doit savoir quoi traiter en priorité.

## Mise en place

Départ :

```text
cp checkpoints/05-cockpit-start/interpreter.css Portal/Styles/Dymasco/interpreter.css
```

Écrans :

```text
Portal/Screens/exercice-4-cockpit.html
Portal/Screens/exercice-4-cockpit-mm.html
Portal/Screens/exercice-4-cockpit-mobile.html
```

Correction :

```text
checkpoints/05-cockpit-solution/interpreter.css
```

## Équilibre attendu

| Dimension | Niveau voulu |
| --- | --- |
| Diagnostic | Moyen à difficile |
| Layout moderne | Difficile mais faisable |
| Cascade Apriso | Moyen |
| Maintenabilité | Difficile |
| Responsive | Difficile |
| Pièges cachés | Présents mais justifiables |

Le départ doit être exploitable en desktop, mais casser clairement en tablette/mobile.

La solution de référence est une trajectoire, pas une copie obligatoire.

## Risques attendus

| Priorité | Risque | Indice DevTools |
| --- | --- | --- |
| 1 | Priorité client trop faible | `.FIPriorityStrip` ressemble à un message ordinaire |
| 2 | KPI trop figés | `repeat(4, 1fr)` |
| 3 | Zone de travail trop rigide | `grid-template-columns: 2fr 1fr 1fr` |
| 4 | Actions qui ne respirent pas | `flex` sans `flex-wrap` |
| 5 | Tableau qui force la largeur | `min-width` sans conteneur utile |
| 6 | Couleurs métier directes | hex répétés |
| 7 | Absence de `clamp()` | tailles fixes sur titres/KPI |
| 8 | Sélecteur structurel | `td:nth-child(5)` |
| 9 | Inline style simulé | `style="background:#fee2e2;color:#7f1d1d"` |
| 10 | Contextes non testés | Portal seul fonctionne |

## Paliers de correction

### Bronze - Lisibilité client

But :

- A3-03 ressort en priorité ;
- l'action d'escalade est visible ;
- les KPI sont comparables ;
- l'écran reste exploitable en desktop.

Sélecteurs probables :

- `.FIPriorityStrip`
- `.FIButton--danger`
- `.FIKpiGrid`
- `.FIKpiCard`
- `.FIKpiValue`

Points de vigilance :

- ne pas rendre tous les boutons rouges ;
- ne pas faire reposer toute la priorité sur la couleur ;
- ne pas oublier le bouton secondaire dans la priorité.

### Silver - Layout responsive

But :

- pas de scroll horizontal global ;
- les actions reviennent à la ligne ;
- la table scrolle localement ;
- le cockpit reste lisible tablette/mobile.

Sélecteurs probables :

- `.FICockpitHeader`
- `.FICockpitActions`
- `.FIWorkArea`
- `.FILineMap`
- `.FIQueueTableWrap`
- `@media (max-width: 920px)`
- `@media (max-width: 560px)`

Points de vigilance :

- le scroll horizontal local du tableau est acceptable ;
- le scroll global de page ne l'est pas ;
- `grid` pour structure, `flex` pour groupes d'actions.

### Gold - Maintenabilité

But :

- tokens nommés ;
- sections lisibles ;
- sélecteurs courts ;
- contextes Apriso séparés ;
- dette Process Builder nommée.

Sélecteurs / tokens probables :

- `--dy-cockpit-gap`
- `--dy-critical-soft`
- `--dy-quality-soft`
- `.dy-client-priority .FIStatus`
- `.MMScreenContext`
- `.MobileAppScreenContext`

Points de vigilance :

- ne pas accepter `nth-child` comme intention métier ;
- documenter tout contournement inline ;
- ne pas mélanger M&M et mobile dans une même règle trop large.

### Bonus - Finesse CSS

But :

- `clamp()` utile, borné ;
- `var()` avec fallback au moins une fois ;
- priorité visible sans surcharger tout l'écran ;
- dette Process Builder formulée clairement.

Exemples :

```css
gap: var(--dy-cockpit-gap, 12px);
font-size: clamp(26px, 4vw, 44px);
```

## Correction commentée

### Priorité

Avant :

```css
.FIPriorityStrip {
  margin: 12px 0;
  padding: 12px;
  background: #f7fafc;
  border: 1px solid #c9d3df;
}
```

Après :

```css
.FIPriorityStrip {
  display: grid;
  grid-template-columns: minmax(0, 1fr) auto;
  gap: 8px 16px;
  align-items: center;
  padding: clamp(10px, 1vw, 14px);
  background: var(--dy-critical-soft);
  border: 1px solid #fecdd3;
  border-left: 8px solid var(--dy-status-blocked);
  border-radius: var(--dy-cockpit-radius);
}
```

Pourquoi :

- la priorité devient scannable ;
- le bouton reste aligné en desktop ;
- la règle prépare le responsive.

### KPI

Avant :

```css
.FIKpiGrid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
}
```

Après :

```css
.FIKpiGrid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(100%, 150px), 1fr));
  gap: var(--dy-cockpit-gap, 12px);
}
```

Pourquoi :

- le nombre de colonnes s'adapte ;
- les KPI restent comparables ;
- le fallback garde le layout si le token manque.

### Table OF

Avant :

```css
.FIQueueTable {
  min-width: 720px;
}
```

Après :

```css
.FIQueueTableWrap {
  overflow-x: auto;
}

.FIQueueTable {
  min-width: 680px;
}
```

Pourquoi :

- le tableau peut rester dense ;
- le scroll devient local ;
- la page ne déborde plus globalement.

### Dette inline

Le statut `blocked` contient un style inline simulé.

Correction idéale en Apriso réel :

- supprimer le Direct formatting ;
- poser une classe métier dans Process Builder.

Contournement acceptable dans le kit :

```css
.FIStatus--critical,
.dy-client-priority .FIStatus {
  color: #7f1d1d !important; /* HTML simule un Direct formatting Apriso. */
  background: #ffe4e6 !important;
}
```

## Questions de débrief

1. Qu'est-ce qui rend A3-03 prioritaire sans lire tout l'écran ?
2. Où le scroll horizontal est-il acceptable ?
3. Quelle règle CSS serait dangereuse si une colonne OF était ajoutée ?
4. Quelle correction devrait être faite dans Process Builder plutôt qu'en CSS ?
5. Quelle règle d'équipe évite de refaire le même patch dans 6 mois ?

## Signaux d'une bonne solution

- L'écran répond à la demande client avant d'être joli.
- Les valeurs métier passent par tokens.
- Les contextes sont séparés.
- Les `!important` sont rares et justifiés.
- Les zones denses restent denses sans casser la page.
- La mini-note d'arbitrage mentionne au moins une dette Process Builder.

## Signaux d'une solution faible

- Tout est rendu rouge.
- Le tableau force encore la largeur globale.
- La solution dépend de `nth-child`.
- Les contextes M&M/mobile sont ignorés.
- Les tokens existants sont contournés.
- Le CSS final est une suite de patchs chronologiques.
