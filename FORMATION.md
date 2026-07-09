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

Résultat attendu :

- savoir expliquer pourquoi le titre reste bleu ;
- identifier `Default/interpreter.css` comme source de la règle gagnante ;
- proposer la correction minimale dans `Dymasco/interpreter.css`.

> [!NOTE]
> **Cadre formateur — Réponse**
>
> La règle gagnante est `.FIPageTitle` dans `Default/interpreter.css`, sur la propriété `color`, parce qu'elle utilise `!important`.
>
> Point à montrer dans DevTools :
>
> - règle source : `Portal/Styles/Default/interpreter.css`
> - règle candidate : `Portal/Styles/Dymasco/interpreter.css`
> - fichier à modifier : `Portal/Styles/Dymasco/interpreter.css`
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

Résultat attendu :

- comprendre que les modifiers de statut battent la règle générale ;
- identifier les règles gagnantes dans `Default/interpreter.css` ;
- proposer une surcharge ciblée pour les états `blocked` et `quality-alert`.

> [!NOTE]
> **Cadre formateur — Réponse**
>
> Les modifiers `.FIMachineCard--blocked` et `.FIMachineCard--quality-alert` définissent leurs propres couleurs avec `!important`, donc ils battent la règle générale `.FIMachineCard`.
>
> Point à montrer dans DevTools :
>
> - `.FIMachineCard` existe dans `Portal/Styles/Dymasco/interpreter.css`
> - `.FIMachineCard--blocked` et `.FIMachineCard--quality-alert` existent dans `Portal/Styles/Default/interpreter.css`
> - les propriétés gagnantes sont `background` et `border-left-color`
> - fichier à modifier : `Portal/Styles/Dymasco/interpreter.css`
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

Résultat attendu :

- reconnaître que le style direct dans le HTML gagne ;
- expliquer que la correction idéale est côté Apriso / Process Builder ;
- proposer un contournement CSS seulement si le HTML généré n'est pas modifiable.

> [!NOTE]
> **Cadre formateur — Réponse**
>
> C'est d'abord un problème de style direct dans le HTML généré. La correction idéale est dans Apriso ou Process Builder ; la correction CSS est un contournement local si on ne peut pas changer la source.
>
> Point à montrer dans DevTools :
>
> - règle candidate : `.FIStatus` dans `Portal/Styles/Dymasco/interpreter.css`
> - valeur gagnante : attribut HTML `style="background:#fee2e2;color:#7f1d1d"`
> - source idéale : configuration Apriso / Process Builder
> - contournement local : `Portal/Styles/Dymasco/interpreter.css`
>
> **Correction idéale dans le simulateur local**
>
> Dans `Portal/Screens/atelier.html`, remplacer :
>
> ```html
> <span class="FIStatus" style="background:#fee2e2;color:#7f1d1d">blocked</span>
> ```
>
> par :
>
> ```html
> <span class="FIStatus dy-status-blocked">blocked</span>
> ```
>
> Puis ajouter la règle métier dans `Portal/Styles/Dymasco/interpreter.css`.
>
> **Corrigé**
>
> ```css
> .dy-status-blocked,
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

## Niveau 4 - Demande client : le bouton `Incident` ne ressort pas assez

Demande client :

> Quand un opérateur doit déclarer un incident, le bouton ne ressort pas assez. On veut qu'il soit plus visible, mais sans rendre tous les boutons rouges.

Première étape : reformuler le besoin.

- Quel bouton est concerné ?
- Est-ce un problème sur tous les boutons ou seulement sur l'action `Incident` ?
- Quelle classe permet de cibler ce bouton sans toucher aux autres ?

Dans `Portal/Screens/atelier.html`, le bouton concerné est :

```html
<button class="FIButton FIButton--secondary">Incident</button>
```

À inspecter :

```css
.FIButton
.FIButton--secondary
```

Question : quelle règle explique que le bouton garde un rendu secondaire ? Où faut-il écrire la surcharge ?

Résultat attendu :

- traduire la demande client en problème CSS précis ;
- identifier `.FIButton--secondary` comme modifier responsable du rendu secondaire ;
- identifier `Default/interpreter.css` comme source de la règle à battre ;
- proposer une surcharge ciblée dans `Dymasco/interpreter.css`, sans modifier tous les boutons.

> [!NOTE]
> **Cadre formateur — Réponse**
>
> Le client ne demande pas "mettre tous les boutons en rouge". Il demande de rendre l'action `Incident` plus visible.
>
> Le bouton concerné combine deux classes : `.FIButton` et `.FIButton--secondary`. Le bon diagnostic consiste à inspecter le modifier `.FIButton--secondary`, pas à modifier tous les boutons `.FIButton`.
>
> La règle gagnante est `.FIButton--secondary` dans `Portal/Styles/Default/interpreter.css`. Elle donne au bouton un rendu secondaire (`color: #214d7a`, `background: white`).
>
> La correction doit être écrite dans `Portal/Styles/Dymasco/interpreter.css`, en ciblant le modifier `.FIButton--secondary`.
>
> **Corrigé possible**
>
> ```css
> .FIButton--secondary {
>   color: white;
>   background: var(--dy-status-blocked);
>   border-color: var(--dy-status-blocked);
> }
> ```

Avant de passer à l'atelier suivant :

```text
git add Portal/Styles/Dymasco/interpreter.css
git commit -m "Diagnose secondary incident button"
```

---

# Atelier 2 - Transformer une demande client en surcharge propre

Objectif : partir d'une demande client, clarifier le besoin, choisir le bon sélecteur, puis écrire une surcharge ciblée sans modifier le thème livré.

## Niveau 1 - Demande client : le titre doit reprendre la couleur texte Dymasco

Demande client :

> Le titre principal de l'écran doit utiliser la couleur texte Dymasco, pas le bleu Apriso.

Données nécessaires :

| Élément | Valeur |
| --- | --- |
| Élément visé | `.FIPageTitle` |
| Couleur souhaitée | `var(--dy-color-text)` |
| Fichier à modifier | `Portal/Styles/Dymasco/interpreter.css` |
| Fichier interdit | `Portal/Styles/Default/interpreter.css` |

Travail demandé :

- identifier pourquoi la couleur Dymasco ne gagne pas ;
- écrire la surcharge minimale ;
- ne pas modifier le thème livré.

Résultat attendu :

- le titre n'est plus bleu Apriso ;
- la couleur vient de `Dymasco/interpreter.css` ;
- `Default/` reste inchangé.

> [!NOTE]
> **Cadre formateur — Réponse**
>
> La règle à battre est `.FIPageTitle` dans `Portal/Styles/Default/interpreter.css`, sur `color: #214d7a !important`.
>
> **Corrigé**
>
> À écrire dans `Portal/Styles/Dymasco/interpreter.css`.
>
> ```css
> .FIPageTitle {
>   color: var(--dy-color-text) !important;
> }
> ```

Avant de passer au niveau suivant :

```text
git add Portal/Styles/Dymasco/interpreter.css
git commit -m "Apply Dymasco title color"
```

## Niveau 2 - Demande client : les incidents doivent ressortir immédiatement

Demande client :

> Quand un poste est bloqué ou en alerte qualité, l'opérateur doit le voir immédiatement.

Données nécessaires :

| État | Classe | Bordure souhaitée | Fond souhaité |
| --- | --- | --- | --- |
| Poste bloqué | `.FIMachineCard--blocked` | `var(--dy-status-blocked)` | `#fff1f2` |
| Alerte qualité | `.FIMachineCard--quality-alert` | `var(--dy-status-quality)` | `#fffbeb` |

Fichier à modifier :

```text
Portal/Styles/Dymasco/interpreter.css
```

Contraintes :

- ne pas modifier toutes les cartes ;
- ne pas toucher au HTML ;
- justifier tout `!important`.

Résultat attendu :

- les cartes `blocked` et `quality-alert` utilisent les couleurs Dymasco ;
- les fonds demandés sont visibles ;
- les surcharges sont écrites dans `Dymasco/interpreter.css` ;
- les `!important` ajoutés sont limités aux cas nécessaires.

> [!NOTE]
> **Cadre formateur — Réponse**
>
> Les règles à battre sont dans `Portal/Styles/Default/interpreter.css`. Elles utilisent déjà `!important` sur `border-left-color` et `background`, donc la surcharge Dymasco doit en utiliser aussi pour ces propriétés.
>
> **Corrigé**
>
> À écrire dans `Portal/Styles/Dymasco/interpreter.css`.
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
git commit -m "Emphasize critical workstation states"
```

## Niveau 3 - Demande client : le bouton Incident doit être plus visible

Demande client :

> Le bouton Incident ne ressort pas assez, mais on ne veut pas rendre tous les boutons agressifs.

Données nécessaires :

| Élément | Valeur |
| --- | --- |
| Bouton concerné | `<button class="FIButton FIButton--secondary">Incident</button>` |
| Sélecteur à cibler | `.FIButton--secondary` |
| Couleur texte souhaitée | `white` |
| Fond souhaité | `var(--dy-status-blocked)` |
| Bordure souhaitée | `var(--dy-status-blocked)` |
| Fichier à modifier | `Portal/Styles/Dymasco/interpreter.css` |

Travail demandé :

- ne pas modifier `.FIButton` globalement ;
- cibler le bouton secondaire ;
- vérifier que les autres boutons `.FIButton` ne changent pas.

Résultat attendu :

- le bouton `Incident` est identifiable comme action sensible ;
- la surcharge cible `.FIButton--secondary` ;
- les autres boutons `.FIButton` restent inchangés.

> [!NOTE]
> **Cadre formateur — Réponse**
>
> Le bouton concerné combine `.FIButton` et `.FIButton--secondary`. Modifier `.FIButton` toucherait tous les boutons. Le bon point d'entrée est donc le modifier `.FIButton--secondary`.
>
> La règle de base est dans `Portal/Styles/Default/interpreter.css`.
>
> **Corrigé**
>
> À écrire dans `Portal/Styles/Dymasco/interpreter.css`.
>
> ```css
> .FIButton--secondary {
>   color: white;
>   background: var(--dy-status-blocked);
>   border-color: var(--dy-status-blocked);
> }
> ```

Avant de passer au niveau suivant :

```text
git add Portal/Styles/Dymasco/interpreter.css
git commit -m "Emphasize incident action button"
```

## Niveau 4 - Demande client : harmoniser les statuts

Demande client :

> Les statuts running, waiting, blocked et quality alert doivent être cohérents visuellement. Aujourd'hui, on a l'impression que chaque statut a été traité séparément.

Données nécessaires :

| Statut | Classe | Token couleur |
| --- | --- | --- |
| running | `.FIMachineCard--running` | `var(--dy-status-running)` |
| waiting | `.FIMachineCard--waiting` | `var(--dy-status-waiting)` |
| blocked | `.FIMachineCard--blocked` | `var(--dy-status-blocked)` |
| quality alert | `.FIMachineCard--quality-alert` | `var(--dy-status-quality)` |

Travail demandé :

- vérifier que les 4 états utilisent les tokens Dymasco ;
- modifier 2 états maximum si nécessaire ;
- ajouter un court commentaire de convention au-dessus du groupe de règles.

Résultat attendu :

- les 4 statuts utilisent les tokens Dymasco cohérents ;
- la convention est visible dans le fichier ;
- les corrections restent limitées aux statuts métier.

> [!NOTE]
> **Cadre formateur — Réponse**
>
> L'exercice force à sortir du mode "patch isolé" : on regroupe les statuts métier et on rend la convention lisible pour la maintenance.
>
> **Corrigé**
>
> À écrire ou harmoniser dans `Portal/Styles/Dymasco/interpreter.css`.
>
> ```css
> /* Status metier : tous les etats poste utilisent les tokens Dymasco. */
> .FIMachineCard--running {
>   border-left-color: var(--dy-status-running) !important;
> }
>
> .FIMachineCard--waiting {
>   border-left-color: var(--dy-status-waiting) !important;
> }
>
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

Avant de passer à l'atelier suivant :

```text
git add Portal/Styles/Dymasco/interpreter.css
git commit -m "Harmonize workstation status colors"
```

---

# Atelier 3 - Passer d'un CSS qui marche à un thème maintenable

Objectif : reprendre un thème qui fonctionne visuellement, mais qui reste fragile, puis le rendre lisible, testable et transmissible.

Le geste professionnel attendu n'est pas "ranger pour ranger". C'est :

- repérer les risques ;
- décider ce qui mérite correction ;
- corriger sans changer le kit ;
- vérifier Portal, M&M et mobile ;
- formuler une règle d'équipe réutilisable.

## Départ

```text
cp checkpoints/04-audit/interpreter.css Portal/Styles/Dymasco/interpreter.css
```

Écrans à ouvrir :

```text
Portal/Screens/atelier.html
Portal/Screens/atelier-mm.html
Portal/Screens/atelier-mobile.html
```

Cible de référence :

```text
checkpoints/03-theme/interpreter.css
```

> [!NOTE]
> **Cadre formateur — Intention**
>
> `04-audit` est volontairement imparfait : il donne un CSS plausible après plusieurs corrections rapides.
>
> `03-theme` représente la forme maintenable attendue : tokens, sections, statuts métier, custom classes, contextes.

## Niveau 1 - Audit silencieux

Objectif : lire le fichier comme un mainteneur, pas comme l'auteur du patch.

Travail demandé :

- ouvrir `Portal/Styles/Dymasco/interpreter.css` ;
- repérer 5 risques de maintenance ;
- ne pas corriger tout de suite ;
- noter pour chaque risque : ligne, type, impact probable.

Grille :

| Ligne | Risque | Impact | Correction probable |
| --- | --- | --- | --- |
|  |  |  |  |

Types de risques attendus :

- couleur directe alors qu'un token existe ;
- `!important` non justifié ;
- sélecteur structurel fragile ;
- classe technique peu métier ;
- règle contexte trop large ;
- patch temporaire non daté ;
- contournement d'un style inline non documenté.

Résultat attendu :

- au moins 5 risques identifiés ;
- aucune correction prématurée ;
- distinction claire entre bug visible et dette maintenable.

> [!NOTE]
> **Cadre formateur — Risques à faire émerger**
>
> Classement du plus dangereux au moins dangereux :
>
> | Priorité | Risque | Pourquoi c'est dangereux |
> | --- | --- | --- |
> | 1 | Sélecteur structurel `td:nth-child(4)` | casse dès que le tableau change |
> | 2 | Contextes M&M et mobile mélangés | peut créer une régression invisible sur un autre écran |
> | 3 | Contournement inline non documenté | masque une dette Apriso / Process Builder |
> | 4 | Patch temporaire non daté | devient permanent sans responsable |
> | 5 | Classe `.dy-u-critical` trop technique | intention métier peu transmissible |
> | 6 | Couleurs directes sur `quality-alert` | dérive progressive du thème |
> | 7 | Couleurs directes sur `blocked` | dette simple, facile à corriger |
>
> ### 1. Sélecteur structurel fragile
>
> Avant :
>
> ```css
> .FIContentPanel .FIDataTable tr td:nth-child(4) .FIStatus {
>   font-weight: 700 !important;
> }
> ```
>
> Pourquoi ce n'est pas bien :
>
> - dépend de la position de colonne ;
> - casse si une colonne est ajoutée, retirée ou déplacée ;
> - ne dit pas quelle intention métier est visée.
>
> Après :
>
> ```css
> .dy-status-critical .FIStatus,
> .dy-client-priority .FIStatus {
>   font-weight: 700;
> }
> ```
>
> Pourquoi c'est mieux :
>
> - dépend d'une intention métier ;
> - survit aux changements de structure du tableau ;
> - peut être posée proprement dans Process Builder.
>
> ### 2. Contextes M&M et mobile mélangés
>
> Avant :
>
> ```css
> .MMScreenContext .FIMachineCard,
> .MobileAppScreenContext .FIMachineCard {
>   border-radius: 2px !important;
> }
> ```
>
> Pourquoi ce n'est pas bien :
>
> - M&M et mobile n'ont pas forcément les mêmes contraintes ;
> - une correction M&M peut casser le mobile ;
> - le `!important` empêche d'ajuster finement un contexte.
>
> Après :
>
> ```css
> .MMScreenContext .FIMachineCard {
>   border-radius: 2px;
>   box-shadow: none;
> }
>
> .MobileAppScreenContext .FIButton {
>   width: 100%;
> }
> ```
>
> Pourquoi c'est mieux :
>
> - chaque contexte Apriso porte ses propres règles ;
> - on limite les effets de bord ;
> - la review peut tester Portal, M&M et mobile séparément.
>
> ### 3. Contournement inline non documenté
>
> Avant :
>
> ```css
> .dy-u-critical .FIStatus {
>   color: #7f1d1d !important;
>   background: #ffe4e6 !important;
> }
> ```
>
> Pourquoi ce n'est pas bien :
>
> - le CSS masque probablement un Direct formatting Apriso ;
> - on ne sait pas si c'est une vraie règle ou un contournement ;
> - le prochain développeur risque d'empiler un autre `!important`.
>
> Après :
>
> ```css
> .dy-u-critical .FIStatus {
>   color: #7f1d1d !important; /* HTML simule Direct formatting Apriso. */
>   background: #ffe4e6 !important;
> }
> ```
>
> Pourquoi c'est mieux :
>
> - la dette est visible ;
> - la vraie correction reste identifiée : enlever le style inline côté Apriso / Process Builder ;
> - le `!important` est justifié.
>
> ### 4. Patch temporaire non daté
>
> Avant :
>
> ```css
> .FIHighDensityMode .FIMachineCard {
>   padding: 10px;
> }
> ```
>
> Pourquoi ce n'est pas bien :
>
> - on ne sait pas pourquoi le patch existe ;
> - on ne sait pas quand le supprimer ;
> - le temporaire devient souvent définitif.
>
> Après :
>
> ```css
> /* Patch temporaire densite atelier.
>    Cause : attente arbitrage écran compact Apriso.
>    Revue : 2026-07-09.
> */
> .FIHighDensityMode .FIMachineCard {
>   padding: 10px;
> }
> ```
>
> Pourquoi c'est mieux :
>
> - la cause est explicite ;
> - une date de revue existe ;
> - le patch est supprimable.
>
> ### 5. Classe technique peu métier
>
> Avant :
>
> ```css
> .dy-u-critical {
>   outline: 2px solid #b45309;
> }
> ```
>
> Pourquoi ce n'est pas bien :
>
> - `u` ressemble à une classe utilitaire ;
> - l'intention métier n'est pas claire ;
> - la couleur directe mélange technique et métier.
>
> Après :
>
> ```css
> .dy-client-priority {
>   border-color: var(--dy-status-quality) !important;
>   outline: 0;
>   box-shadow: inset 0 0 0 2px rgb(180 83 9 / 0.28);
> }
> ```
>
> Pourquoi c'est mieux :
>
> - le nom dit pourquoi l'élément est stylé ;
> - la classe peut être posée dans Process Builder ;
> - la couleur principale revient au token.
>
> Variante acceptable si le HTML du kit ne doit pas bouger :
>
> ```css
> .dy-u-critical {
>   border-color: var(--dy-status-quality) !important;
>   outline: 0;
>   box-shadow: inset 0 0 0 2px rgb(180 83 9 / 0.28);
> }
> ```
>
> ### 6. `quality-alert` utilise des couleurs directes
>
> Avant :
>
> ```css
> .FIMachineCard--quality-alert {
>   border-left-color: #b45309 !important;
>   background: #fffbeb !important;
> }
> ```
>
> Pourquoi ce n'est pas bien :
>
> - la couleur métier est dupliquée ;
> - si le thème Dymasco change, cette règle ne suit pas ;
> - le `!important` n'explique pas quelle règle il bat.
>
> Après :
>
> ```css
> .FIMachineCard--quality-alert {
>   border-left-color: var(--dy-status-quality) !important;
>   background: #fffbeb !important;
> }
> ```
>
> Pourquoi c'est mieux :
>
> - le statut qualité dépend du token Dymasco ;
> - la règle reste alignée avec le thème ;
> - seule la nuance de fond reste locale.
>
> ### 7. `blocked` utilise des couleurs directes
>
> Avant :
>
> ```css
> .FIMachineCard--blocked {
>   border-left-color: #b91c1c !important;
>   background: #fff1f2 !important;
> }
> ```
>
> Pourquoi ce n'est pas bien :
>
> - même couleur que le token `--dy-status-blocked`, mais recopiée en dur ;
> - risque de divergence si le rouge Dymasco change ;
> - intention métier moins visible.
>
> Après :
>
> ```css
> .FIMachineCard--blocked {
>   border-left-color: var(--dy-status-blocked) !important;
>   background: #fff1f2 !important;
> }
> ```
>
> Pourquoi c'est mieux :
>
> - la règle lit clairement "statut bloqué" ;
> - le thème reste piloté par les tokens ;
> - correction simple, faible risque.

## Niveau 2 - Classer avant de corriger

Objectif : éviter le réflexe "je corrige tout". Prioriser.

Travail demandé :

- classer les risques trouvés ;
- choisir 2 corrections à faire maintenant ;
- choisir 1 dette à documenter ;
- choisir 1 règle d'équipe à proposer.

Grille :

| Risque | Catégorie | Décision |
| --- | --- | --- |
|  | Bug visible / dette / convention / compat | Corriger / documenter / laisser |

Contraintes :

- ne pas modifier `Portal/Styles/Default/` ;
- ne pas modifier les écrans HTML ;
- garder le kit tel quel ;
- corriger uniquement dans `Portal/Styles/Dymasco/interpreter.css`.

Résultat attendu :

- 2 corrections ciblées ;
- 1 arbitrage documenté ;
- 1 règle d'équipe formulée.

> [!NOTE]
> **Cadre formateur — Bon arbitrage**
>
> Corriger maintenant :
>
> - couleurs métier directes -> tokens ;
> - règle contexte trop large -> contexte séparé.
>
> Documenter plutôt que masquer :
>
> - style inline simulé : dette Process Builder / Direct formatting.
>
> Règle d'équipe possible :
>
> - tout patch temporaire doit être daté et placé dans une section dédiée.

## Niveau 3 - Refactor guidé

Objectif : transformer les patchs en intentions lisibles.

Travail demandé :

- créer ou vérifier les sections ;
- remplacer les couleurs métier directes par tokens ;
- regrouper les statuts poste ;
- isoler les custom classes Process Builder ;
- scoper les règles par contexte.

Structure attendue :

```text
1. Tokens
2. Function Interpreter - titres
3. Function Interpreter - cartes poste
4. Custom classes Process Builder
5. Contextes Apriso
6. Patches temporaires si nécessaire
```

Résultat attendu :

- le CSS reste visuellement équivalent ;
- les intentions métier sont lisibles ;
- les exceptions sont localisées ;
- un autre développeur peut trouver rapidement où modifier.

> [!NOTE]
> **Cadre formateur — Correction cible**
>
> La cible n'est pas une copie obligatoire au caractère près.
>
> Le fichier doit tendre vers `checkpoints/03-theme/interpreter.css` :
>
> - tokens en haut ;
> - statuts regroupés ;
> - `.dy-u-critical` dans une section Process Builder ;
> - `.MMScreenContext` et `.MobileAppScreenContext` séparés ;
> - dette inline visible si elle reste contournée en CSS.

Avant de passer au niveau suivant :

```text
git add Portal/Styles/Dymasco/interpreter.css
git commit -m "Refactor theme maintainability"
```

## Niveau 4 - Vérifier les contextes

Objectif : vérifier qu'une correction maintenable ne casse pas un autre contexte Apriso.

Écrans à comparer :

```text
Portal/Screens/atelier.html
Portal/Screens/atelier-mm.html
Portal/Screens/atelier-mobile.html
```

Travail demandé :

- vérifier le titre ;
- vérifier les cartes `running`, `waiting`, `blocked`, `quality-alert` ;
- vérifier le bouton `Incident` ;
- vérifier le rendu compact M&M ;
- vérifier le bouton mobile pleine largeur si la règle existe.

Grille :

| Écran | À vérifier | Résultat |
| --- | --- | --- |
| Portal | titre, statuts, bouton Incident |  |
| M&M | cartes compactes, pas de règle mobile parasite |  |
| Mobile | bouton pleine largeur, alerte visible |  |

Résultat attendu :

- aucune régression Portal ;
- contexte M&M plus compact ;
- contexte mobile séparé de M&M ;
- les règles de contexte ne débordent pas.

> [!NOTE]
> **Cadre formateur — Point clé**
>
> Une règle peut être correcte en Portal et trop large ailleurs.
>
> Le bon réflexe est de tester les classes de contexte :
>
> ```css
> .MMScreenContext .FIMachineCard
> .MobileAppScreenContext .FIButton
> ```

## Niveau 5 - Formaliser la règle d'équipe

Objectif : repartir avec une convention utilisable dans Apriso réel.

Travail demandé :

- écrire 3 règles d'équipe courtes ;
- les relier à un risque vu dans l'atelier ;
- garder des règles vérifiables en review.

Exemples acceptables :

- Ne jamais modifier `Portal/Styles/Default/`.
- Préférer une classe métier Process Builder à un sélecteur `nth-child`.
- Toute couleur de statut passe par un token `--dy-status-*`.
- Tout `!important` doit battre une règle identifiée dans `Default`.
- Tout patch temporaire est daté, isolé, supprimable.
- Tout contournement de style inline mentionne la dette Process Builder.

Résultat attendu :

- 3 règles courtes ;
- applicables en review ;
- transférables vers `CHECKLIST_APRISO_REEL.md`.

Avant de passer à l'atelier 4 ou à la clôture :

```text
git add Portal/Styles/Dymasco/interpreter.css
git commit -m "Audit theme maintainability"
```

---

# Atelier 4 - Challenge client cockpit superviseur

Objectif : appliquer les gestes de la journée sur une demande client plus grosse, sous contrainte Apriso.

Durée :

- exercice : 2h ;
- correction / partage : 45 à 60 min.

Demande client :

> En 10 secondes, un superviseur doit savoir quoi traiter en priorité.

Contexte :

- écran cockpit superviseur ligne A3 ;
- HTML contraint, proche Function Interpreter ;
- thème Dymasco seul modifiable ;
- CSS de départ fonctionnel mais insuffisant ;
- correction par paliers Bronze / Silver / Gold / Bonus.

## Kit

Fiche dédiée :

```text
EXERCICE_4_COCKPIT.md
```

Écran :

```text
Portal/Screens/exercice-4-cockpit.html
```

Départ :

```text
cp checkpoints/05-cockpit-start/interpreter.css Portal/Styles/Dymasco/interpreter.css
```

Correction de référence :

```text
checkpoints/05-cockpit-solution/interpreter.css
```

Fiche formateur :

```text
FORMATEUR_EXERCICE_4_COCKPIT.md
```

## Paliers

| Palier | Objectif | Critères |
| --- | --- | --- |
| Bronze | Lisibilité client | priorité A3-03 visible, KPI lisibles, actions compréhensibles |
| Silver | Layout responsive | `grid` robuste, `flex` qui wrap, pas de scroll horizontal global en largeur tablette |
| Gold | Maintenabilité | tokens, sections, sélecteurs courts, règles lisibles |
| Bonus | Finesse CSS | `clamp()` bien borné, fallback `var()`, subtilité inline documentée |

## Compétences travaillées

- transformer une demande client en priorités CSS ;
- choisir `grid` pour la structure ;
- choisir `flex` pour les actions ;
- utiliser `clamp()` sans rendre le rendu instable ;
- utiliser `var()` et tokens métier ;
- éviter les sélecteurs structurels fragiles ;
- traiter le responsive utile sans sur-traiter M&M/mobile ;
- documenter la dette Process Builder quand le HTML impose un inline style.

## Sortie attendue

- CSS final dans `Portal/Styles/Dymasco/interpreter.css` ;
- mini-note d'arbitrage dans `EXERCICE_4_COCKPIT.md` ou à part ;
- Bronze minimum atteint ;
- Silver / Gold discutés en correction collective.

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
- Documenter tout patch temporaire.
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

Réponses attendues :

- Toujours inspecter avant de corriger : règle gagnante, fichier, propriété, cause.
- Ne jamais modifier le thème livré `Default/`.
- Choisir le bon fichier : `apriso.css`, `interpreter.css`, `keyboard.css`, ou Process Builder.
- Préférer une classe métier Process Builder à un sélecteur structurel profond.
- Utiliser les tokens Dymasco pour les couleurs et statuts métier.
- Limiter `!important` aux cas justifiés par le diagnostic.
- Identifier les styles inline / Direct formatting comme dette Apriso, pas comme simple bug CSS.
- Tester le contexte réellement utilisé : Portal desktop, largeur tablette, ou M&M/mobile seulement si l'écran existe dans ces contextes.
- Documenter les exceptions : patch temporaire, dette Process Builder, convention d'équipe.
- Garder le thème lisible par sections : tokens, composants, statuts, custom classes, contextes.
