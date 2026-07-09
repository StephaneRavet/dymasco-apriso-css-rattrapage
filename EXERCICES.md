# Exercices - format animateur

## Exercice 2.1 - Le titre ne prend pas la couleur Dymasco

### Symptome

- `FIPageTitle` reste bleu Apriso.
- La regle Dymasco existe pourtant dans `Dymasco/interpreter.css`.

### Diagnostic

- DevTools > Styles.
- Regle gagnante : `Default/interpreter.css`.
- Cause : `color: #214d7a !important`.

### Correction minimale

```css
.FIPageTitle {
  color: var(--dy-color-text) !important;
}
```

### Regle a retenir

- `!important` ne se combat pas par specificite normale.
- Dans Apriso, certains styles livres ou generes imposent une escalade.
- Escalade acceptee si elle est documentee et localisee.

---

## Exercice 2.2 - Les cartes critiques gardent le fond Apriso

### Symptome

- `FIMachineCard--blocked` garde le fond rouge pale livre.
- `FIMachineCard--quality-alert` garde le fond orange pale livre.

### Diagnostic

- Les backgrounds de statut sont en `!important` dans `Default/interpreter.css`.

### Correction minimale

```css
.FIMachineCard--blocked {
  border-left-color: var(--dy-status-blocked) !important;
  background: #fff1f2 !important;
}

.FIMachineCard--quality-alert {
  border-left-color: var(--dy-status-quality) !important;
  background: #fffbeb !important;
}
```

### Regle a retenir

- Un override propre n'est pas toujours sans `!important`.
- Le vrai critere : diagnostic explicite + selecteur court + commentaire utile.

---

## Exercice 2.3 - Le badge blocked resiste

### Symptome

- Le badge `blocked` a un style inline dans le HTML.

### Diagnostic

- Attribut HTML : `style="background:#fee2e2;color:#7f1d1d"`.
- Dans Apriso, equivalent probable : Direct formatting ou style genere.

### Correction recommandee en vrai Apriso

- Retirer le Direct formatting dans Process Builder si possible.
- Preferer une custom class stable.

### Correction locale si on ne controle pas le HTML

```css
.dy-u-critical .FIStatus {
  color: #7f1d1d !important;
  background: #ffe4e6 !important;
}
```

### Regle a retenir

- Face a un inline style, la meilleure correction est souvent dans Apriso, pas dans CSS.
- Si impossible : `!important` cible, documente.

---

## Exercice 3.1 - Custom class Process Builder

### Symptome

- La classe `dy-u-critical` existe mais ne porte pas assez de sens visuel.

### Objectif

- Transformer cette classe en marqueur metier reutilisable.

### Correction minimale

```css
.dy-u-critical {
  border-color: var(--dy-status-quality) !important;
  box-shadow: inset 0 0 0 2px rgb(180 83 9 / 0.28);
}
```

### Regle a retenir

- Dans Apriso, une custom class posee dans Process Builder est souvent plus stable qu'un ciblage structurel profond.

---

## Exercice 3.2 - Contexte M&M trop espace

### Symptome

- La variante `atelier-mm.html` doit rester dense.
- Les styles generaux Dymasco ne doivent pas rendre le contexte M&M trop "carte".

### Correction minimale

```css
.MMScreenContext .FIMachineCard {
  border-radius: 2px;
  box-shadow: none;
}
```

### Regle a retenir

- Scoper par contexte quand Apriso ajoute une classe de contexte.
- Ne pas supposer qu'un style Portal convient a tous les rendus.

---

## Bonus - Upgrade Apriso

### Scenario

- Une mise a jour renomme `FIMachineCard` en `FIWorkstationCard`.

### Objectif

- Adapter sans casser l'existant.

### Correction transitoire

```css
.FIMachineCard,
.FIWorkstationCard {
  background: var(--dy-color-surface);
  border-color: var(--dy-color-border);
}
```

### Regle a retenir

- Une compat temporaire doit etre datee et supprimee.
- Garder une section `Compat upgrade` courte.

