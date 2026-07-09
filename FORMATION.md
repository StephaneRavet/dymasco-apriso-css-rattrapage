# Journée de rattrapage - CSS Apriso - Dymasco / DELMIA

Date : jeudi 9 juillet 2026  
Format : consolidation concrète, exercices guidés, simulateur local

> [!NOTE]
> **Cadre formateur — Ouverture**
>
> Avant de démarrer, je veux recadrer la journée.
>
> La première session n'a pas répondu à vos attentes, notamment sur la préparation perçue, la progressivité des exercices et le lien avec votre contexte.
>
> J'ai repris la préparation dans ce sens.

## Journée de consolidation très concrète :

- peu de théorie ;
- des exercices guidés ;
- un projet fil rouge proche Apriso ;
- des points réguliers ;
- une méthode réutilisable sur vos surcharges CSS.

## Contrat de séance

Aujourd'hui, je veux qu'on évite deux écueils :

- aller trop vite sur des bases qui bloquent ;
- rester trop longtemps sur des rappels qui ne vous servent pas.

Donc je poserai des points réguliers : rythme, niveau, applicabilité.

## Objectifs finaux

| Compétence | Geste observable |
| --- | --- |
| Diagnostiquer | Trouver dans DevTools pourquoi une règle CSS ne passe pas |
| Surcharger | Corriger dans le bon fichier, avec un sélecteur stable |
| Maintenir | Savoir organiser un thème sans empiler des règles chronologiques. Garder un thème lisible. |

## Ce que la journée n'est pas

- Pas une formation CSS avancé complète.
- Pas un tour de Grid / Container Queries / animations.

## Ce que la journée produit

- Un simulateur local proche du contexte.
- Des exercices courts corrigés dans la fiche formateur.
- Une checklist de diagnostic.
- Un modèle de thème réutilisable.

---

# 01 - Où se branche le CSS dans Apriso ?

Objectif : comprendre le terrain réel, sans accès à Apriso.

## Point de prudence version

Les noms et chemins ci-dessous sont des repères issus de documentations Apriso récentes. Cela peut changer selon la version et les personnalisations Dymasco :

- version exacte de DELMIA Apriso ;
- version Portal utilisée ;
- thème réellement actif ;
- fichiers CSS réellement chargés ;
- personnalisations historiques ;
- droits de modification.

Ces repères sont une carte de lecture. Ils servent à auditer l'environnement réel, pas à remplacer l'audit.

## Carte de lecture

Dans Apriso, on ne raisonne pas seulement "une page HTML + un fichier CSS". On doit d'abord situer où le style se branche.

| Repère | Dans Apriso | Sert aujourd'hui à |
| --- | --- | --- |
| Portal | Cadre web qui affiche l'application : header, navigation, frame, session | savoir si on touche au cadre général |
| Thème | Ensemble de fichiers CSS/images activé pour personnaliser le rendu | comprendre pourquoi on copie `Default` vers `Dymasco` |
| Function Interpreter | Moteur qui qui **affiche/exécute** un écran dans le Portal, donc ce qui produit le HTML/CSS visible côté navigateur. | savoir pourquoi `interpreter.css` est le fichier central |
| Process Builder | là où on **conçoit/configure** l'écran ou l'opération. | comprendre les custom classes sur les contrôles |
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

Donc le fichier le plus utile est `interpreter.css`.

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

```css
/* Fragile : dépend de la structure HTML générée */
.FIContentPanel .FITable tr td:nth-child(4) span {
  color: #b91c1c;
}

/* Plus stable : dépend d'une intention métier posée dans Process Builder */
.dy-status-blocked {
  color: #b91c1c;
}
```

Raison : si la structure HTML change, le premier sélecteur casse facilement. Si la classe métier reste posée sur le composant, le second continue à porter l'intention.

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

Cela veut dire : corriger la configuration qui génère le HTML ou le style direct, plutôt qu'ajouter une règle CSS plus forte.

Exemples :

- retirer un Direct formatting ;
- poser une classe CSS sur le composant dans Process Builder ;
- éviter qu'une couleur soit générée directement en `style="..."`.

## Contextes Apriso

Apriso peut ajouter des contextes d'affichage.

```html
<html class="PortalContext">
<html class="MMScreenContext">
<html class="MobileAppScreenContext">
```

Conséquence :

- une règle correcte en Portal peut être trop large ailleurs ;
- scoper par contexte si nécessaire.

## Point

> Est-ce que cette carte du terrain correspond suffisamment à ce que vous voyez dans votre Apriso ?

---

# 02 - Kit local

Les exercices se font dans le kit fourni par le formateur.

Dépôt : <https://github.com/StephaneRavet/dymasco-apriso-css-rattrapage>

Options de récupération :

```text
git clone https://github.com/StephaneRavet/dymasco-apriso-css-rattrapage.git
```

ou téléchargement ZIP depuis GitHub.

Dossier de travail :

```text
dymasco-apriso-css-rattrapage/
```

Depuis la racine du dépôt, avant de commencer ou pour se recaler :

```text
cp checkpoints/00-start/interpreter.css Portal/Styles/Dymasco/interpreter.css
```

Ouvrir dans le navigateur :

```text
Portal/Screens/atelier.html
```

Ouvrir dans l'éditeur de code :

```text
Portal/Styles/Dymasco/interpreter.css
```

Ne pas modifier :

```text
Portal/Styles/Default/
```

---

# Atelier 1 - Diagnostiquer

Objectif : comprendre pourquoi une règle CSS ne passe pas avant d'écrire une correction.

## Pour les 4 exercices suivants :

1. inspecter l'élément ;
2. trouver la règle gagnante ;
3. trouver fichier + sélecteur + propriété ;
4. formuler la cause ;
5. proposer la correction minimale.

## Niveau 1 - Le titre reste bleu

Départ :

```text
cp checkpoints/00-start/interpreter.css Portal/Styles/Dymasco/interpreter.css
```

Symptôme :

- Le titre `Poste atelier - suivi OF et alertes qualité` reste bleu.
- Une règle Dymasco existe dans `Portal/Styles/Dymasco/interpreter.css` :

```css
.FIPageTitle {
  color: var(--dy-color-text);
}
```

À inspecter :

```css
.FIPageTitle
```

Question : quelle règle gagne, dans quel fichier, sur quelle propriété, et pourquoi ?

> [!NOTE]
> **Cadre formateur — Réponse**
>
> La règle gagnante est `.FIPageTitle` dans `Default/interpreter.css`, sur la propriété `color`, parce qu'elle utilise `!important`.
>
> **Corrigé**
>
> ```css
> .FIPageTitle {
>   color: var(--dy-color-text) !important;
> }
> ```

Avant de passer au niveau suivant :

```text
git add Portal/Styles/Dymasco/interpreter.css
git commit -m "Diagnose title color override"
```

## Niveau 2 - Les cartes critiques gardent le fond Apriso

Symptôme :

- `blocked` reste rouge pâle.
- `quality-alert` reste orange pâle.
- Une règle Dymasco générale existe :

```css
.FIMachineCard {
  background: var(--dy-color-surface);
  border-color: var(--dy-color-border);
  border-radius: 6px;
}
```

À inspecter :

```css
.FIMachineCard
.FIMachineCard--blocked
.FIMachineCard--quality-alert
```

Question : pourquoi la règle générale `.FIMachineCard` ne suffit-elle pas pour les états critiques ?

> [!NOTE]
> **Cadre formateur — Réponse**
>
> Les modifiers `.FIMachineCard--blocked` et `.FIMachineCard--quality-alert` définissent leurs propres couleurs avec `!important`, donc ils battent la règle générale `.FIMachineCard`.
>
> **Corrigé**
>
> ```css
> .FIMachineCard--blocked {
>   border-left-color: var(--dy-status-blocked) !important;
>   background: #fff1f2 !important;
> }
>
> .FIMachineCard--quality-alert {
>   border-left-color: var(--dy-status-quality) !important;
>   background: #fffbeb !important;
> }
> ```

Avant de passer au niveau suivant :

```text
git add Portal/Styles/Dymasco/interpreter.css
git commit -m "Diagnose critical card states"
```

## Niveau 3 - Le badge `blocked` résiste

Symptôme : le badge `blocked` ne suit pas la règle `.FIStatus`.

Règle Dymasco :

```css
.FIStatus {
  color: var(--dy-color-text);
  background: var(--dy-color-soft);
}
```

Dans le HTML du badge bloqué :

```html
style="background:#fee2e2;color:#7f1d1d"
```

À inspecter :

```css
.FIStatus
.dy-u-critical .FIStatus
```

Question : est-ce un problème de cascade CSS, ou un problème de HTML/style généré ?

> [!NOTE]
> **Cadre formateur — Réponse**
>
> C'est d'abord un problème de style direct dans le HTML généré. La correction idéale est dans Apriso ou Process Builder ; la correction CSS est un contournement local si on ne peut pas changer la source.
>
> **Corrigé**
>
> ```css
> .dy-u-critical .FIStatus {
>   color: #7f1d1d !important;
>   background: #ffe4e6 !important;
> }
> ```

Avant de passer au niveau suivant :

```text
git add Portal/Styles/Dymasco/interpreter.css
git commit -m "Diagnose generated badge style"
```

## Niveau 4 - Diagnostic libre

Symptômes à classer :

- le titre reste bleu ;
- les cartes critiques gardent leur fond Apriso ;
- le badge `blocked` résiste ;
- la variante M&M est trop arrondie.

Pour chaque symptôme, remplir :

| Élément | Règle gagnante | Fichier | Propriété | Cause | Correction probable |
| --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |

> [!NOTE]
> **Cadre formateur — Corrigé attendu**
>
> - Titre : règle `.FIPageTitle` dans `Default/interpreter.css`, propriété `color`, cause `!important`.
> - Cartes critiques : modifiers `.FIMachineCard--blocked` / `.FIMachineCard--quality-alert`, propriétés `background` et `border-left-color`, cause `!important`.
> - Badge : style direct `style="..."`, cause HTML généré / Direct formatting.
> - M&M : règle générale trop large, cause absence de scope `.MMScreenContext`.

---

# Atelier 2 - Surcharger

Objectif : écrire une correction courte, au bon endroit, sans modifier le thème livré.

## Niveau 1 - Corriger le titre

À modifier :

```text
Portal/Styles/Dymasco/interpreter.css
```

Objectif : faire gagner la couleur Dymasco sur `.FIPageTitle`.

Contrainte : ne pas modifier `Portal/Styles/Default/`.

> [!NOTE]
> **Cadre formateur — Corrigé**
>
> ```css
> .FIPageTitle {
>   color: var(--dy-color-text) !important;
> }
> ```

Avant de passer au niveau suivant :

```text
git add Portal/Styles/Dymasco/interpreter.css
git commit -m "Fix title color override"
```

## Niveau 2 - Corriger les états métier

Objectif : corriger les cartes :

```css
.FIMachineCard--blocked
.FIMachineCard--quality-alert
```

Contraintes :

- conserver les tokens Dymasco ;
- ne pas toucher au HTML ;
- justifier tout `!important`.

> [!NOTE]
> **Cadre formateur — Corrigé**
>
> ```css
> .FIMachineCard--blocked {
>   border-left-color: var(--dy-status-blocked) !important;
>   background: #fff1f2 !important;
> }
>
> .FIMachineCard--quality-alert {
>   border-left-color: var(--dy-status-quality) !important;
>   background: #fffbeb !important;
> }
> ```

Avant de passer au niveau suivant :

```text
git add Portal/Styles/Dymasco/interpreter.css
git commit -m "Fix critical card states"
```

## Niveau 3 - Corriger un style direct

Objectif : traiter le badge `blocked`.

Question à trancher :

- correction idéale dans Apriso / Process Builder ?
- correction CSS locale si on ne peut pas changer le HTML généré ?

À modifier si correction CSS locale :

```css
.dy-u-critical .FIStatus
```

> [!NOTE]
> **Cadre formateur — Réponse et corrigé**
>
> Idéalement, supprimer le style direct dans Apriso / Process Builder et poser une classe métier. Si ce n'est pas possible, contournement CSS local :
>
> ```css
> .dy-u-critical .FIStatus {
>   color: #7f1d1d !important;
>   background: #ffe4e6 !important;
> }
> ```

Avant de passer au niveau suivant :

```text
git add Portal/Styles/Dymasco/interpreter.css
git commit -m "Handle generated badge style"
```

## Niveau 4 - Choisir le bon fichier

Pour chaque symptôme, choisir le bon point d'intervention.

| Symptôme | Point d'intervention |
| --- | --- |
| Le header n'a pas la bonne couleur |  |
| Le tableau d'opérations est trop serré |  |
| Le badge `blocked` a un style inline |  |
| Le rendu mobile doit être plus compact |  |
| Un raccourci clavier est mal lisible |  |

Réponses possibles :

- `apriso.css` ;
- `interpreter.css` ;
- `keyboard.css` ;
- Process Builder ;
- règle scopée par contexte.

> [!NOTE]
> **Cadre formateur — Corrigé**
>
> | Symptôme | Point d'intervention |
> | --- | --- |
> | Le header n'a pas la bonne couleur | `apriso.css` |
> | Le tableau d'opérations est trop serré | `interpreter.css` |
> | Le badge `blocked` a un style inline | Process Builder d'abord, CSS ensuite |
> | Le rendu mobile doit être plus compact | règle scopée par contexte |
> | Un raccourci clavier est mal lisible | `keyboard.css` |

Avant de passer à l'atelier suivant :

```text
git add Portal/Styles/Dymasco/interpreter.css
git commit -m "Choose CSS intervention points"
```

---

# Atelier 3 - Maintenir

Objectif : transformer des corrections ponctuelles en thème lisible et transmissible.

## Niveau 1 - Ranger une section

Départ :

```text
cp checkpoints/02-interpreter/interpreter.css Portal/Styles/Dymasco/interpreter.css
```

Objectif : regrouper les règles par intention :

- tokens ;
- titres ;
- cartes ;
- statuts ;
- custom classes ;
- contextes.

> [!NOTE]
> **Cadre formateur — Corrigé attendu**
>
> Structure attendue :
>
> ```text
> 1. Tokens
> 2. Function Interpreter - titres
> 3. Function Interpreter - cartes poste
> 4. Custom classes Process Builder
> 5. Contextes Apriso
> ```

Avant de passer au niveau suivant :

```text
git add Portal/Styles/Dymasco/interpreter.css
git commit -m "Organize theme sections"
```

## Niveau 2 - Nommer les intentions

Objectif : éviter les valeurs magiques.

À repérer :

- couleurs directes ;
- valeurs répétées ;
- règles métier implicites.

À produire :

- un token si la valeur est réutilisable ;
- un commentaire court si la règle est une exception ;
- une classe métier si le style porte une intention stable.

> [!NOTE]
> **Cadre formateur — Corrigé attendu**
>
> Exemples de bonnes transformations :
>
> - `#b91c1c` -> `var(--dy-status-blocked)` si c'est un statut métier ;
> - `!important` -> commentaire si l'escalade est imposée par `Default/interpreter.css` ;
> - sélecteur structurel -> classe métier stable comme `.dy-status-blocked`.

Avant de passer au niveau suivant :

```text
git add Portal/Styles/Dymasco/interpreter.css
git commit -m "Name theme intentions"
```

## Niveau 3 - Adapter le contexte M&M

Fichier complémentaire à ouvrir :

```text
Portal/Screens/atelier-mm.html
```

Règles de départ :

```css
.dy-u-critical {
  outline: 2px solid var(--dy-status-quality);
}

.FIMachineCard {
  border-radius: 6px;
}
```

Objectif :

- rendre `.dy-u-critical` plus métier ;
- ajouter une règle scopée pour `.MMScreenContext` ;
- garder le rendu Portal standard intact.

> [!NOTE]
> **Cadre formateur — Corrigé**
>
> ```css
> .dy-u-critical {
>   border-color: var(--dy-status-quality) !important;
>   outline: 0;
>   box-shadow: inset 0 0 0 2px rgb(180 83 9 / 0.28);
> }
>
> .MMScreenContext .FIMachineCard {
>   border-radius: 2px;
>   box-shadow: none;
> }
> ```

Avant de passer au niveau suivant :

```text
git add Portal/Styles/Dymasco/interpreter.css
git commit -m "Scope M&M context styles"
```

## Niveau 4 - Mini-audit final

Départ :

```text
cp checkpoints/03-theme/interpreter.css Portal/Styles/Dymasco/interpreter.css
```

Travail demandé :

- repérer 5 risques de maintenance ;
- corriger 2 risques ;
- documenter 1 arbitrage ;
- proposer 1 règle d'équipe.

Avant de passer au bonus ou à la clôture :

```text
git add Portal/Styles/Dymasco/interpreter.css
git commit -m "Audit theme maintainability"
```

Exemples de risques :

- `!important` non justifié ;
- sélecteur trop structurel ;
- style direct qui devrait venir de Process Builder ;
- règle non scopée par contexte ;
- compat upgrade non datée.

> [!NOTE]
> **Cadre formateur — Corrigé attendu**
>
> Exemples de risques acceptables :
>
> - `!important` sans commentaire ;
> - valeur hex qui devrait devenir token ;
> - classe trop technique au lieu d'une classe métier ;
> - absence de section `Contextes Apriso` ;
> - compat temporaire sans date de revue.

---

# Bonus - Upgrade Apriso

Objectif : occuper les plus rapides avec un problème réaliste.

Scénario : une montée de version Apriso change certaines classes.

Fichier à modifier :

```text
Portal/Styles/Dymasco/interpreter.css
```

Zone conseillée : en bas du fichier, dans une section `Compat upgrade` datée.

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

Exercice : ajouter une compat temporaire sans casser l'existant.

> [!NOTE]
> **Cadre formateur — Corrigé**
>
> ```css
> /* Compat upgrade Apriso - à supprimer après migration complète.
>    Hypothèse atelier : FIMachineCard renommé en FIWorkstationCard.
>    Revue : 2026-07-09.
> */
> .FIMachineCard,
> .FIWorkstationCard {
>   background: var(--dy-color-surface);
>   border-color: var(--dy-color-border);
> }
>
> .FIMachineCard-title,
> .FIWorkstationCard-title {
>   color: var(--dy-color-text);
> }
> ```

Avant de clôturer le bonus :

```text
git add Portal/Styles/Dymasco/interpreter.css
git commit -m "Add temporary upgrade compatibility"
```

---

# Checklist Apriso réel

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

## DevTools

À capturer pour un écran représentatif :

- liste des CSS chargés ;
- ordre de chargement ;
- règle gagnante sur un titre ;
- règle gagnante sur un bouton ;
- règle gagnante sur un tableau ;
- présence de styles inline ;
- classes de contexte sur `html` ou `body`.

## Règles d'équipe proposées

- Ne jamais modifier le thème livré.
- Préfixer les classes custom : `dy-`.
- Préférer custom class Process Builder aux sélecteurs profonds.
- Documenter tout `!important`.
- Documenter toute compat upgrade.
- Tester au moins un écran Portal + un écran mobile/tablette si concerné.

---

# Clôture

Aujourd'hui, on n'a pas essayé de couvrir tout CSS avancé.

On a travaillé trois gestes :

- diagnostiquer ;
- surcharger ;
- maintenir.

Question finale :

> Quelle méthode ou checklist repartez-vous avec, concrètement utilisable dans votre environnement Apriso ?
