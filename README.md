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
FICHE_ANIMATEUR.md
CHECKLIST_APRISO_REEL.md
EXERCICES.md
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
checkpoints/
  00-start/
  01-diagnostic/
  02-interpreter/
  03-theme/
  04-bonus-upgrade/
```

## Supports Markdown

| Fichier | Usage |
| --- | --- |
| `FORMATION.md` | Support principal, futur contenu Notion |
| `FICHE_ANIMATEUR.md` | Déroulé, phrases, vigilance |
| `CHECKLIST_APRISO_REEL.md` | Transfert vers l'environnement Apriso réel |
| `EXERCICES.md` | Version compacte des exercices et corrigés |

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

## Competences visees

| Competence | Geste observable |
| --- | --- |
| Diagnostiquer | Trouver la regle gagnante dans DevTools |
| Surcharger | Corriger dans `apriso.css` ou `interpreter.css` |
| Maintenir | Garder un theme `Dymasco` lisible et testable |

## Progression

1. Comprendre ou se branche le CSS dans Apriso
2. Diagnostiquer pourquoi une regle gagne
3. Corriger `interpreter.css`
4. Structurer un theme maintenable
5. Bonus : survivre a une mise a jour Apriso
