# Critères de Scoring des Verticales

## Version : 1.0
## Dernière mise à jour : 2026-03-18
## Prochaine revue prévue : après 5 verticales scorées

---

## Critères actifs

### 1. Complexité opérationnelle (poids: 1.2x)
Plus les opérations sont complexes, plus un ERP AI a de la valeur.
- 1 = Opérations simples, peu de flux, une seule activité linéaire
- 2 = Quelques flux parallèles, gestion basique
- 3 = Multi-flux, quelques intégrations nécessaires, saisonnalité
- 4 = Complexe, multi-parties prenantes, workflows imbriqués
- 5 = Très complexe, chaîne logistique, multi-sites, réglementations

### 2. Taille de marché adressable (poids: 1.0x)
Nombre d'entreprises cibles × revenu potentiel par client (ARPA).
- 1 = <1 000 entreprises cibles, ARPA <100€/mois
- 2 = 1 000-5 000 entreprises, ARPA 100-300€/mois
- 3 = 5 000-20 000 entreprises, ARPA 300-500€/mois
- 4 = 20 000-50 000 entreprises, ARPA 500-1000€/mois
- 5 = >50 000 entreprises, ARPA >1000€/mois

### 3. Insatisfaction outils existants (poids: 1.3x)
Le critère le plus pondéré. Spectre de satisfaction :
- 1 = Bien équipés ET satisfaits (marché saturé, loyal aux solutions en place)
- 2 = Bien équipés mais quelques lacunes mineures (pas de switching intent)
- 3 = Équipés mais outils inadaptés (Excel, bricolage multi-outils, workarounds)
- 4 = Sous-équipés, douleur consciente, recherchent activement une solution
- 5 = Aucun outil adapté, douleur forte, prêts à changer immédiatement

### 4. Capacité à payer (poids: 1.1x)
Montant ET rapidité de décision d'achat.
- 1 = Budget très limité (<100€/mois possible), cycles longs (>6 mois)
- 2 = Budget limité, cycle 3-6 mois
- 3 = Budget moyen (200-500€/mois), cycle 2-3 mois
- 4 = Budget correct (500-1000€/mois), cycle 1-2 mois
- 5 = Budget significatif (>1000€/mois), décision <1 mois

### 5. Facilité de distribution / accès (poids: 1.0x)
Comment atteindre les décideurs de cette verticale.
- 1 = Marché très fragmenté, pas de canaux identifiés, décideurs inaccessibles
- 2 = Quelques canaux existent mais accès difficile
- 3 = Canaux identifiés (salons, associations, médias spécialisés), accès moyen
- 4 = Communautés actives, quelques influenceurs, LinkedIn actif
- 5 = Communautés très actives, influenceurs identifiés, accès direct aux décideurs

### 6. Fit produit Enobase (poids: 1.2x)
Combien de modules existants sont directement applicables sans dev majeur.
- 1 = Nécessite 80%+ de développement custom
- 2 = 60-80% de dev custom nécessaire
- 3 = 50% réutilisable, 50% à adapter
- 4 = 70-80% des modules existants applicables
- 5 = 80%+ directement utilisable, adaptations mineures seulement

### 7. Densité de données / potentiel IA (poids: 0.8x)
Volume et richesse des données opérationnelles exploitables par l'IA.
- 1 = Peu de données structurées, opérations principalement manuelles
- 2 = Quelques données mais peu exploitables
- 3 = Données transactionnelles classiques (commandes, factures, stock)
- 4 = Données riches et variées, potentiel de prédiction
- 5 = Données très riches, forte automatisation possible, insights IA à forte valeur

### 8. Viralité / effet réseau (poids: 0.7x)
Le produit crée-t-il une adoption organique via l'écosystème ?
- 1 = Usage totalement isolé, pas d'effet réseau
- 2 = Interactions limitées avec l'extérieur
- 3 = Quelques interactions (fournisseurs, partenaires) pourraient tirer l'adoption
- 4 = Effet réseau modéré (clients/fournisseurs invités dans le système)
- 5 = Fort effet réseau (clients, fournisseurs, partenaires utilisent le même outil)

### 9. Vitesse de validation (poids: 1.1x)
Peut-on tester cette verticale en <4 semaines avec les moyens actuels ?
- 1 = Impossible sans développement produit significatif (>2 mois de dev)
- 2 = Nécessite du dev notable (1-2 mois)
- 3 = Faisable avec adaptations mineures (2-4 semaines de dev)
- 4 = Testable rapidement avec configuration seulement
- 5 = Testable immédiatement avec le produit actuel tel quel

---

## Formule de scoring
Score pondéré = Σ (score_i × poids_i) / Σ poids_i
Somme des poids = 1.2 + 1.0 + 1.3 + 1.1 + 1.0 + 1.2 + 0.8 + 0.7 + 1.1 = 9.4
Score max théorique = 5.0

## Seuils de décision
- ≥ 4.0 avec confidence Med+ sur 7+ critères → GO : lancer les blueprints immédiatement
- 3.5 - 3.9 → PROMISING : accélérer la collecte de data, planifier 2-3 calls ciblés
- 3.0 - 3.4 → INVESTIGATE : besoin de plus de data sur critères Low confidence
- 2.5 - 2.9 → PARK : mettre de côté, revisiter dans 3 mois
- < 2.5 → REJECT : ne pas poursuivre (documenter pourquoi)

## Indicateur de fiabilité du score
Confidence globale = nombre de critères à Med ou High / 9
- ≥ 7/9 → Score fiable, décision possible
- 4-6/9 → Score indicatif, besoin de data additionnelle
- ≤ 3/9 → Score non fiable, exploration insuffisante

## Historique des évolutions
| Date | Version | Changement | Raison |
|------|---------|-----------|--------|
| 2026-03-18 | 1.0 | Création initiale | 9 critères + pondérations hypothétiques |

## Notes
- Les poids sont des hypothèses initiales. Après 5+ verticales scorées,
  lancer /criteria pour recalibrer en fonction des résultats réels.
- Le critère "Insatisfaction outils" est le plus pondéré (1.3x) car c'est
  historiquement le meilleur prédicteur de conversion rapide.
- La confidence par critère est aussi importante que le score. Un score de
  4.2 avec 6 critères en "Low" = on n'a pas assez de data pour décider.
