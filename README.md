# Journee de rattrapage CSS Apriso - Dymasco

Date cible : jeudi 9 juillet 2026.

Objectif : simuler un environnement DELMIA Apriso sans acces a Apriso.

## Contrat

- Ne pas modifier `Portal/Styles/Default/`
- Travailler dans `Portal/Styles/Dymasco/`
- Lire le rendu via DevTools avant de corriger
- Corriger court, verifier, noter la regle

## Fichiers

```text
FORMATION.md
EXERCICE_4_COCKPIT.md
FORMATEUR_EXERCICE_4_COCKPIT.md
CHECKLIST_APRISO_REEL.md
CONTEXT.md
Portal/
  Styles/
    Default/
      apriso.css
      interpreter.css
      keyboard.css
    Dymasco/
      apriso.css
      interpreter.css
      keyboard.css
  Screens/
    atelier.html
    atelier-mm.html
    atelier-mobile.html
    exercice-4-cockpit.html
checkpoints/
  00-start/
  01-diagnostic/
  02-interpreter/
  03-theme/
  04-audit/
  05-cockpit-start/
  05-cockpit-solution/
```

## Supports Markdown

| Fichier | Usage |
| --- | --- |
| `FORMATION.md` | Support principal, futur contenu Notion |
| `EXERCICE_4_COCKPIT.md` | Fiche apprenant du challenge cockpit |
| `FORMATEUR_EXERCICE_4_COCKPIT.md` | Corrigé et animation formateur |
| `CHECKLIST_APRISO_REEL.md` | Transfert vers l'environnement Apriso réel |
| `CONTEXT.md` | Glossaire pédagogique du kit |

## Lancer

Ouvrir directement :

```text
Portal/Screens/atelier.html
```

Puis comparer avec :

```text
Portal/Screens/atelier-mm.html
Portal/Screens/atelier-mobile.html
```

Exercice final :

```text
Portal/Screens/exercice-4-cockpit.html
```

## Competences visees

| Competence | Geste observable |
| --- | --- |
| Diagnostiquer | Trouver la regle gagnante dans DevTools |
| Surcharger | Corriger dans `apriso.css` ou `interpreter.css` |
| Maintenir | Garder un theme `Dymasco` lisible et testable |

## Progression

1. Comprendre ou se branche le CSS dans Apriso
2. Atelier 1 : diagnostiquer pourquoi une regle gagne
3. Atelier 2 : surcharger dans le bon fichier
4. Atelier 3 : passer d'un CSS qui marche a un theme maintenable
5. Atelier 4 : challenge client cockpit superviseur
