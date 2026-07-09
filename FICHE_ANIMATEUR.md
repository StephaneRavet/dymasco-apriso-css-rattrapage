# Fiche animateur - journée CSS Apriso Dymasco

## Intention

- Réparer la confiance.
- Montrer une préparation visible.
- Rester concret.
- Ne pas promettre Apriso live.
- Rendre la méthode transférable.

## Ouverture courte

Dire :

> Avant de démarrer, je veux recadrer la journée. La première session n'a pas répondu à vos attentes, notamment sur la préparation perçue, la progressivité des exercices et le lien avec votre contexte. J'ai repris le dispositif dans ce sens. Aujourd'hui, je vous propose une journée de consolidation très concrète : peu de théorie, des exercices guidés, un fil rouge proche Apriso, et des checkpoints réguliers pour vérifier que ça répond bien à vos besoins.

Ne pas faire :

- se justifier longuement ;
- refaire l'historique ;
- s'auto-flageller ;
- promettre une grande formation CSS avancé.

## Contrat feedback

Dire :

> Aujourd'hui, je veux qu'on évite deux écueils : aller trop vite sur des bases qui bloquent, ou rester trop longtemps sur des rappels qui ne vous servent pas. Donc je vais poser des checkpoints réguliers : rythme, niveau, applicabilité. Si quelque chose bloque vraiment, vous m'arrêtez. Sinon, on le note et on ajuste au checkpoint suivant.

Question de checkpoint :

> Sur cette séquence : trop basique, trop rapide, ou suffisamment utile pour continuer ?

## Déroulé proposé

| Horaire | Séquence | Objectif |
| --- | --- | --- |
| 09:00 | Ouverture + contrat | Reposer le cadre |
| 09:15 | Chapitre 1 Apriso réel | Carte du terrain sans accès |
| 10:00 | Pause courte | Respiration |
| 10:15 | Simulateur local | Fichiers, contexte, règles |
| 10:30 | Exercice 2.1 | `!important` diagnostiqué |
| 11:00 | Exercice 2.2 | Statuts et cartes |
| 11:30 | Exercice 2.3 | Inline style / Direct formatting |
| 12:00 | Checkpoint long | Ajuster niveau |
| 12:30 | Déjeuner |  |
| 13:30 | Exercice 3.1 | Custom class Process Builder |
| 14:15 | Exercice 3.2 | Contextes Apriso |
| 15:00 | Organisation thème | Maintenabilité |
| 15:45 | Bonus upgrade ou consolidation | Selon rythme |
| 16:30 | Checklist finale | Transfert au réel |
| 16:50 | Clôture | Retour concret |

## Phrases utiles

Si le groupe conteste la fidélité Apriso :

> Vous avez raison de questionner la fidélité. Le but ici n'est pas de prétendre remplacer Apriso. Le but est de simuler les douleurs CSS qu'on retrouve dans Apriso : thème, Function Interpreter, classes générées, styles directs, contexte d'affichage.

Si quelqu'un veut du CSS moderne :

> On peut en parler en bonus, mais pour votre contexte, le premier levier n'est pas `@layer` ou Container Queries. Le premier levier est de savoir où Apriso branche le CSS et pourquoi une règle gagne.

Si quelqu'un repart sur la session précédente :

> C'est justement pour ça que j'ai réduit le périmètre aujourd'hui. On ne va pas courir après tout CSS avancé ; on va stabiliser une méthode utilisable.

## Points de vigilance

- Ne pas dire "ça va ?"
- Ne pas improviser un gros détour CSS.
- Ne pas vendre `@layer` comme solution Apriso.
- Ne pas critiquer Apriso.
- Toujours revenir à DevTools.
- Toujours demander : fichier, sélecteur, propriété, cause.

## Démo minimale à réussir

1. Ouvrir `Portal/Screens/atelier.html`.
2. Inspecter `.FIPageTitle`.
3. Montrer `Default/interpreter.css` avec `!important`.
4. Ajouter `!important` dans `Dymasco/interpreter.css`.
5. Recharger.
6. Verbaliser : diagnostic avant correction.

## Si le rythme est trop lent

Basculer vers :

- exercices en binôme ;
- donner le diagnostic, faire écrire la correction ;
- garder le bonus upgrade pour les rapides.

## Si le rythme est trop rapide

Ajouter :

- inspection DevTools détaillée ;
- comparaison Portal / M&M / mobile ;
- bonus upgrade ;
- refactor de `interpreter.css` en sections.

