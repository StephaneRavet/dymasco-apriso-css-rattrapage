# Journée de rattrapage - CSS Apriso - Dymasco / DELMIA

Date : jeudi 9 juillet 2026  
Format : consolidation concrète, exercices guidés, simulateur local

## Journée de consolidation très concrète :

- peu de théorie ;
- des exercices guidés ;
- un projet fil rouge proche Apriso ;
- des points réguliers ;
- une méthode réutilisable sur vos overrides CSS.

## Contrat de séance

Aujourd'hui, je veux qu'on évite deux écueils :

- aller trop vite sur des bases qui bloquent ;
- rester trop longtemps sur des rappels qui ne vous servent pas.

Donc je poserai des points réguliers :

- rythme ;
- niveau ;
- applicabilité.

## Objectifs finaux

| Compétence | Geste observable |
| --- | --- |
| Diagnostiquer | Trouver dans DevTools pourquoi une règle CSS ne passe pas |
| Surcharger | Corriger dans le bon fichier, avec un sélecteur stable |
| Maintenir | Garder un thème Dymasco lisible |

## Où ces objectifs sont travaillés

| Objectif promis | Séquences qui le travaillent | Preuve attendue en fin de journée |
| --- | --- | --- |
| Diagnostiquer | Chapitre 02, exercices 2.1 à 2.3 | Savoir nommer la règle gagnante, le fichier, la propriété et la cause |
| Surcharger | Chapitre 03, exercices 3.1 et 3.2 | Savoir choisir `apriso.css` ou `interpreter.css`, puis écrire une correction courte |
| Maintenir | Chapitre 04 + checklist finale | Savoir organiser un thème Dymasco sans empiler des règles chronologiques |

## Ce que la journée n'est pas

- Pas une formation CSS avancé complète.
- Pas un tour de Grid / Container Queries / animations.

## Ce que la journée produit

- Un simulateur local proche du contexte.
- Des exercices courts corrigés.
- Une checklist de diagnostic.
- Un modèle de thème réutilisable.

---

# 01 - Où se branche le CSS dans Apriso ?

Objectif : comprendre le terrain réel, sans accès à Apriso.

## Point de prudence version

Les noms et chemins ci-dessous sont des repères issus de documentations Apriso récentes. Cela peut changer selon la version et les personnalisations Dymasco, en fonction de :

- version exacte de DELMIA Apriso ;
- version Portal utilisée ;
- thème réellement actif ;
- fichiers CSS réellement chargés ;
- personnalisations historiques ;
- droits de modification.

## Idée principale

Dans Apriso, on ne raisonne pas seulement "une page HTML + un fichier CSS".

On doit d'abord situer où le style se branche.

| Repère | Dans Apriso | Sert aujourd'hui à |
| --- | --- | --- |
| Portal | Cadre web qui affiche l'application : header, navigation, frame, session | savoir si on touche au cadre général |
| Thème | Ensemble de fichiers CSS/images activé pour personnaliser le rendu | comprendre pourquoi on copie `Default` vers `Dymasco` |
| Function Interpreter | Moteur qui rend les écrans/opérations dans la zone de contenu | savoir pourquoi `interpreter.css` est le fichier central |
| Process Builder | Outil où les écrans/process sont construits et où des classes peuvent être posées | comprendre les custom classes sur les contrôles |
| Classes générées | Classes produites par Apriso ou par les contrôles affichés | éviter les sélecteurs trop fragiles |
| Styles directs | Mise en forme portée directement par un contrôle ou le HTML généré | diagnostiquer les inline styles / Direct formatting |
| Contexte d'affichage | Mode de rendu : Portal standard, M&M, mobile/tablette selon l'écran | tester Portal, M&M, mobile séparément |

## Thème Apriso

Principe réaliste :

- un thème livré existe ;
- on copie un thème ;
- on modifie la copie ;
- on n'édite pas le livré.

Chemin typique :

```text
WebSite/
  Portal/
    Styles/
      Default/
      Dymasco/
```

Dans le simulateur :

```text
Portal/
  Styles/
    Default/
    Dymasco/
```

## Fichiers principaux

| Fichier | Rôle |
| --- | --- |
| `apriso.css` | Shell Portal : header, navigation, frame |
| `interpreter.css` | Contenu Function Interpreter : contrôles, tableaux, messages |
| `keyboard.css` | Indices clavier / saisie |

Règle pratique :

- problème autour du portail -> `apriso.css` ;
- problème dans l'écran opération -> `interpreter.css` ;
- problème de rendu généré par Process Builder -> souvent `interpreter.css` + classe custom.

## Pourquoi `interpreter.css` est central aujourd'hui

Dans cette journée, on travaille surtout sur les écrans métier.

Ce sont les écrans affichés dans la zone de contenu Apriso :

- contrôles issus des opérations ;
- tableaux ;
- messages ;
- boutons ;
- statuts ;
- classes posées dans Process Builder ;
- styles directs ou générés.

Donc le fichier le plus utile pour nos exercices est `interpreter.css`.

`apriso.css` reste important, mais plutôt pour le cadre Portal :

- header ;
- navigation ;
- frame ;
- habillage général.

Phrase à retenir :

> Si le problème est dans l'écran opération, commencer par regarder `interpreter.css`. Si le problème est autour de l'écran, regarder `apriso.css`.

## Process Builder

Dans le vrai environnement :

- un contrôle peut recevoir une classe CSS ;
- cette classe doit exister dans le thème ;
- mieux vaut une classe métier stable qu'un sélecteur structurel profond.

Exemple :

```css
.dy-u-critical {
  border-color: var(--dy-status-quality) !important;
}
```

## Direct formatting / styles inline

Symptôme :

```html
<span class="FIStatus" style="background:#fee2e2;color:#7f1d1d">
  blocked
</span>
```

Diagnostic :

- le style est porté par l'élément ;
- le CSS classique perd souvent ;
- la meilleure correction est parfois dans Apriso, pas dans CSS.

Méthode :

1. Vérifier si le style vient d'un Direct formatting.
2. Retirer le format direct si possible.
3. Remplacer par une custom class.
4. Escalader en CSS seulement si nécessaire.

## Contextes Apriso

Apriso peut ajouter des contextes d'affichage.

Exemples simulés :

```html
<html class="PortalContext">
<html class="MMScreenContext">
<html class="MobileAppScreenContext">
```

Conséquence :

- une règle correcte en Portal peut être trop large ailleurs ;
- scoper par contexte si nécessaire.

## (a passer si tout va bien) À vérifier dans l'environnement réel

- Où sont stockés les thèmes ?
- Quel thème est actif ?
- Peut-on copier le thème livré ?
- Quels fichiers sont chargés dans le navigateur ?
- `apriso.css` et `interpreter.css` sont-ils modifiables ?
- Les écrans utilisent-ils des custom classes Process Builder ?
- Le Direct formatting est-il utilisé ?
- Quels contextes existent : Portal, M&M, mobile ?

## point

Question :

> Est-ce que cette carte du terrain correspond suffisamment à ce que vous voyez dans votre Apriso ?

---

# 02 - Diagnostiquer pourquoi un style gagne

Objectif : ne pas écrire de CSS avant d'avoir lu la cascade.

## Fichier à ouvrir

```text
Portal/Screens/atelier.html
```

## Règle de travail

Avant chaque correction :

1. inspecter l'élément ;
2. trouver la règle gagnante ;
3. noter fichier + sélecteur + propriété ;
4. corriger le minimum ;
5. recharger ;
6. vérifier.

## Exercice 2.1 - Le titre reste bleu

### Symptôme

- Le titre `Poste atelier - suivi OF et alertes qualité` reste bleu.
- Une règle Dymasco existe pourtant.

### À inspecter

```css
.FIPageTitle
```

### Question

Pourquoi la règle Dymasco ne gagne pas ?

<details>
<summary>Corrigé</summary>

La règle livrée dans `Default/interpreter.css` contient :

```css
.FIPageTitle {
  color: #214d7a !important;
}
```

La règle Dymasco ne contient pas `!important`.

Correction minimale :

```css
.FIPageTitle {
  color: var(--dy-color-text) !important;
}
```

Règle à retenir :

- `!important` doit être diagnostiqué, pas deviné ;
- si Apriso impose `!important`, l'escalade peut être acceptable ;
- commentaire utile obligatoire dans le thème final.

</details>

## Exercice 2.2 - Les cartes critiques gardent le fond Apriso

### Symptôme

- `blocked` reste rouge pâle.
- `quality-alert` reste orange pâle.

### À inspecter

```css
.FIMachineCard--blocked
.FIMachineCard--quality-alert
```

<details>
<summary>Corrigé</summary>

Dans `Default/interpreter.css` :

```css
.FIMachineCard--blocked {
  border-left-color: #c53030 !important;
  background: #fff5f5 !important;
}

.FIMachineCard--quality-alert {
  border-left-color: #dd6b20 !important;
  background: #fffaf0 !important;
}
```

Correction :

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

Règle à retenir :

- `!important` n'est pas interdit ;
- il doit être rare, localisé, expliqué.

</details>

## Exercice 2.3 - Le badge blocked résiste

### Symptôme

- Le badge `blocked` ne suit pas la règle `.FIStatus`.

### À inspecter

```html
style="background:#fee2e2;color:#7f1d1d"
```

<details>
<summary>Corrigé</summary>

Cause :

- style inline simulé ;
- équivalent Apriso possible : Direct formatting ou style généré.

Correction recommandée dans le vrai Apriso :

- retirer le Direct formatting ;
- poser une custom class stable.

Correction locale :

```css
.dy-u-critical .FIStatus {
  color: #7f1d1d !important;
  background: #ffe4e6 !important;
}
```

Règle à retenir :

- si le problème vient du HTML généré, le CSS n'est pas toujours la meilleure première réponse.

</details>

## point

Question :

> Sur cette séquence : trop basique, trop rapide, ou suffisamment utile pour continuer ?

---

# 03 - Surcharger `interpreter.css` proprement

Objectif : transformer des corrections isolées en règles maintenables.

## Avant de corriger : choisir le bon fichier

| Symptôme | Fichier à regarder d'abord |
| --- | --- |
| Header, navigation, cadre Portal | `apriso.css` |
| Titre, tableau, bouton, message dans l'écran métier | `interpreter.css` |
| Raccourci, aide clavier, saisie spécialisée | `keyboard.css` |
| Style porté directement par un contrôle | Process Builder d'abord, CSS ensuite si nécessaire |
| Variante Portal / M&M / mobile | même fichier, mais règle scopée par contexte |

Mini-drill :

- Le header n'a pas la bonne couleur -> `apriso.css`.
- Le tableau d'opérations est trop serré -> `interpreter.css`.
- Le badge `blocked` a un style inline -> vérifier Process Builder / Direct formatting.

## Structure cible

```css
@import url("../Default/interpreter.css");

/* Tokens */
:root {
  --dy-color-text: #172026;
}

/* Function Interpreter - titres */
.FIPageTitle {
  color: var(--dy-color-text) !important;
}

/* Custom classes Process Builder */
.dy-u-critical {
  ...
}

/* Contextes Apriso */
.MMScreenContext .FIMachineCard {
  ...
}
```

## Exercice 3.1 - Custom class Process Builder

### Symptôme

- `.dy-u-critical` existe.
- Elle ressemble encore à un patch technique.

### Objectif

- En faire une classe métier réutilisable.

<details>
<summary>Corrigé</summary>

```css
.dy-u-critical {
  border-color: var(--dy-status-quality) !important;
  outline: 0;
  box-shadow: inset 0 0 0 2px rgb(180 83 9 / 0.28);
}
```

Règle à retenir :

- une custom class Process Builder est souvent plus stable qu'une chaîne CSS profonde.

</details>

## Exercice 3.2 - Contexte M&M dense

### Fichier à ouvrir

```text
Portal/Screens/atelier-mm.html
```

### Symptôme

- Une règle pensée pour Portal peut être trop confortable pour un contexte dense.

<details>
<summary>Corrigé</summary>

```css
.MMScreenContext .FIMachineCard {
  border-radius: 2px;
  box-shadow: none;
}
```

Règle à retenir :

- scoper par contexte quand Apriso fournit une classe de contexte.

</details>

## point

Question :

> Ce niveau de correction ressemble-t-il à un geste applicable demain chez vous ?

---

# 04 - Maintenir un thème Dymasco

Objectif : éviter le fichier CSS chronologique impossible à relire.

## Organisation recommandée

```text
interpreter.css
  1. Header commentaire
  2. Tokens
  3. Titres
  4. Cartes / panels
  5. Tableaux
  6. Messages
  7. Custom classes Process Builder
  8. Contextes Apriso
  9. Compat upgrade temporaire
```

## Commentaire de tête

```css
/* ==========================================================================
   DYMASCO THEME - interpreter.css
   Derniere revue : 2026-07-09

   Regles :
   - Ne pas modifier Default/
   - Preferer custom classes Process Builder
   - Diagnostiquer avant d'ajouter !important
   - Documenter toute compat upgrade
   ========================================================================== */
```

## Checklist avant modification

- Ai-je inspecté la règle gagnante ?
- Le problème est-il Portal ou Function Interpreter ?
- Le style vient-il d'un inline style ?
- Existe-t-il une custom class Process Builder ?
- Le sélecteur est-il court ?
- Le `!important` est-il justifié ?
- La règle doit-elle être scopée par contexte ?
- Ai-je testé Portal + mobile si concerné ?

## point final

Question :

> Si vous deviez reprendre un thème existant demain, quelle partie de la méthode vous servirait en premier ?

---

# 05 - Bonus - Upgrade Apriso

Objectif : occuper les plus rapides avec un problème réaliste.

## Scénario

Une montée de version Apriso change certaines classes.

Avant :

```css
.FIMachineCard
.FIMachineCard-title
```

Après :

```css
.FIWorkstationCard
.FIWorkstationCard-title
```

## Exercice

Ajouter une compat temporaire sans casser l'existant.

<details>
<summary>Corrigé</summary>

```css
/* Compat upgrade Apriso - à supprimer après migration complète.
   Hypothèse atelier : FIMachineCard renommé en FIWorkstationCard.
   Revue : 2026-07-09.
*/
.FIMachineCard,
.FIWorkstationCard {
  background: var(--dy-color-surface);
  border-color: var(--dy-color-border);
}

.FIMachineCard-title,
.FIWorkstationCard-title {
  color: var(--dy-color-text);
}
```

Règle à retenir :

- compat courte ;
- datée ;
- visible ;
- supprimable.

</details>

---

# Clôture

## Récap

Aujourd'hui, on n'a pas essayé de couvrir tout CSS avancé.

On a travaillé trois gestes :

- diagnostiquer ;
- surcharger ;
- maintenir.

## Question finale

> Quelle méthode ou checklist repartez-vous avec, concrètement utilisable dans votre environnement Apriso ?
