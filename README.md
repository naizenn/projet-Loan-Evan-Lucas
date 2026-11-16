# Le Donjon des Gobelins

Projet synthétique PRA 5 : jeu de rôle textuel en Python, jouable en console.

**Équipe :** Evan Kalaycioglu et 2 autres membres _(à compléter)_
**Établissement :** La Manu, campus de Chartres
**Matière :** Algorithmique / Pratique (ALGOPRA5)

---

## Présentation

Le Héros doit traverser un donjon de 3 salles. Dans chaque salle, un monstre l'attend : un Gobelin, puis un Orc, puis le Boss. Le joueur doit gérer ses points de vie, son stock limité de potions et son score d'expérience (XP) pour survivre jusqu'au bout.

## Lancer le jeu

Prérequis : Python 3.8 ou plus récent. Aucune bibliothèque externe n'est nécessaire (seul le module standard `random` est utilisé).

```bash
python donjon.py
```

## Règles du jeu

À chaque tour, le joueur choisit une action dans un menu :

| Choix | Action            | Effet                                                                                                        |
| ----- | ----------------- | ------------------------------------------------------------------------------------------------------------ |
| 1     | Attaquer à l'épée | Inflige des dégâts aléatoires au monstre                                                                     |
| 2     | Boire une potion  | Restaure des PV (sans dépasser le maximum) ; consomme 1 potion du stock limité ; le tour est perdu           |
| 3     | Tenter de fuir    | Une chance sur deux de réussir et d'abandonner le donjon ; en cas d'échec, le Héros subit une pénalité de PV |

Après l'action du joueur, si le monstre est toujours en vie, il riposte avec des dégâts aléatoires.

### Fin de partie

- **Victoire totale** : les 3 monstres sont vaincus.
- **Défaite** : les PV du Héros tombent à 0.
- **Abandon** : fuite réussie.
  Le score d'XP est affiché à la fin de la partie.

## Données du jeu

Valeurs d'équilibrage par défaut (modifiables en haut du fichier `donjon.py`) :

| Élément           | Valeur     |
| ----------------- | ---------- |
| PV du Héros       | 100        |
| Potions de départ | 3          |
| Soin d'une potion | 25 à 40 PV |
| Dégâts du Héros   | 10 à 20    |
| Chance de fuite   | 50 %       |

| Salle | Monstre | PV  | Dégâts  | XP gagnée |
| ----- | ------- | --- | ------- | --------- |
| 1     | Gobelin | 40  | 5 à 10  | 20        |
| 2     | Orc     | 70  | 8 à 15  | 50        |
| 3     | Boss    | 120 | 12 à 22 | 150       |

## Architecture du code

Le programme tient dans un seul fichier, organisé en sections qui suivent les étapes du sujet.

| Section                    | Contenu                                                          |
| -------------------------- | ---------------------------------------------------------------- |
| Données (étape 1)          | Constantes, `creer_heros()`, liste `MONSTRES`, `creer_monstre()` |
| Affichage                  | `afficher_etat()`, `afficher_menu()`, `demander_choix()`         |
| Actions (étape 3)          | `attaquer()`, `utiliser_potion()`, `tenter_fuite()`              |
| Combat et donjon (étape 2) | `gerer_tour_combat()`, `combattre_salle()`, `boucle_donjon()`    |
| Fin de partie (étape 4)    | `afficher_fin()`                                                 |
| Point d'entrée             | `main()` et `if __name__ == "__main__":`                         |

### Boucles imbriquées

- Boucle externe : parcourt les salles du donjon tant que le Héros est vivant.
- Boucle interne : enchaîne les tours de combat tant que le Héros et le monstre sont en vie.

### Sécurisation des variables

- Les PV ne descendent jamais sous 0 (`max(0, ...)`) et ne dépassent jamais le maximum lors d'un soin.
- Le stock de potions est vérifié avant chaque utilisation : il ne peut pas devenir négatif.
- Toute saisie invalide du joueur est refusée et redemandée, sans faire planter le programme.

## Répartition du travail

Le projet est réalisé en équipe de 3. Chaque membre développe un bloc, puis l'assemblage final est fait dans `donjon.py`.

| Membre                         | Bloc                                           | Contenu                                                                                                              |
| ------------------------------ | ---------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| Membre 1 : _(nom à compléter)_ | Données et affichage (étape 1)                 | Constantes, `creer_heros()`, `MONSTRES`, `creer_monstre()`, `afficher_etat()`, `afficher_menu()`, `demander_choix()` |
| Membre 2 : _(nom à compléter)_ | Actions tactiques (étape 3)                    | `attaquer()`, `utiliser_potion()`, `tenter_fuite()`, sécurisation des PV et du stock                                 |
| Membre 3 : _(nom à compléter)_ | Progression, fin et assemblage (étapes 2 et 4) | `gerer_tour_combat()`, `combattre_salle()`, `boucle_donjon()`, `afficher_fin()`, `main()`, tests, README             |

### Format de données commun

```python
heros   = {"pv": 100, "pv_max": 100, "potions": 3, "xp": 0, "degats": (10, 20)}
monstre = {"nom": "Gobelin", "pv": 40, "degats": (5, 10), "xp": 20}
```

### Valeurs de retour convenues

| Fonction                            | Retour                                                        |
| ----------------------------------- | ------------------------------------------------------------- |
| `utiliser_potion(heros)`            | `True` si une potion a été bue, `False` si le stock est vide  |
| `tenter_fuite(heros)`               | `True` si la fuite réussit, `False` sinon                     |
| `gerer_tour_combat(heros, monstre)` | `"continue"`, `"monstre_vaincu"`, `"heros_mort"` ou `"fuite"` |

## Tests réalisés

Les scénarios testés en console sont listés dans `tests_manuels.md` :

- victoire totale ;
- défaite ;
- fuite réussie et fuite ratée ;
- potion avec stock vide ;
- potion à PV maximum ;
- saisies invalides (lettres, chiffre hors menu, entrée vide).
