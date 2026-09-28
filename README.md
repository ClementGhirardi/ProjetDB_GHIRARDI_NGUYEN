# I. Première étape : analyse des besoins

## I.A. Prompt engineering

### R — Rôle

Tu es un expert en esport League of Legend, analyste professionnel. Tu as suivi l'ensemble des matchs esportif des 10 dernières années dans les différentes régions. Tu as été joueur professionnel une partie de ta carrière avant d'être analyste. 


### I — Instructions

Tu dois nous fournir les règles métier de l'esport league of legend.
Fait le en respectant la forme illustré dans ces exemples : “Un employé ne peut pas être aﬀilié à plus d’un service”, “Les employés ont un manager” ou bien “Un service a un nom qui a la forme”S” + numéro sur 2 digits” sont des règles métier.
Fournit un dictionnaire de données de ce domaine contenant des informations sur les différents match dans toutes les régions, les joueurs, les résultats des matchs, les dates où se déroulent les matchs, les lieu, etc. 


### C — Contexte

Dans le cadre d'un projet de conception et développement d'une base de données réalisé par des étudiants de l'EFREI qui sera corrigé par des professeurs qui ne connaissent pas du tout League of Legends.  


### A — Contraintes additionnelles

Sois concis, donne ta réponse sous la forme règles métier et dictionnaire de données. Précise la nature des données. Récupère tes informations dans les références données uniquement. Pour toute les clés assure toi 
que la clé est de la forme nomclé_nomentité. Fournis nous uniquement les clés essentielles de la table. 
Fournis nous 15 entités et associations. 


### R — Références

* Riot Games — LoL Esports : https://www.leagueoflegends.com/fr-fr/news/esports/
* Riot Games Competitive Operations — League of Legends : https://competitiveops.riotgames.com/
* LoL Esports — calendrier officiel : https://lolesports.com/


### D — Rendement désiré

Retourne exactement les éléments suivants : 


* 1. Règles métier


* 2. Dictionnaire de données


* 3. Vérification 
Vérifie que le dictionnaire des données et les règles métier permettent de produire le MCD qui respecte la 3FN
et qui contient au moins une association récursive, n-aire avec n>2, une entité faible et forte.
Cette base doit contenir toute les données fournis.


### O — Objectif

Fournis les règles métier et le dictionnaire de données issue de ton analyse ainsi que la vérification afin d'exploiter le résultat dans notre projet étudiant.

## I.B. Prompt obtenu

## 1. Règles métier

Les références distinguent les compétitions régionales et internationales, leurs phases, ainsi que des rencontres en **Bo1, Bo3 ou Bo5**. Le calendrier affiche les équipes, les dates et les scores ; Riot publie aussi des informations sur les effectifs et les lieux d’événements. Les règles ci-dessous traduisent ces informations en **choix de modélisation** pour le projet, sans supposer que tous les détails sont publiés pour chaque rencontre. :chatgpt-content-reference{index="0"}

- Une compétition possède des éditions datées. Une édition appartient à une seule compétition et comporte des phases.
- Une compétition régionale est rattachée à une région ; une compétition internationale peut ne relever d’aucune région unique.
- Une équipe peut s’inscrire à plusieurs éditions. Une rencontre appartient à une seule phase et oppose **deux équipes distinctes** dès que ses adversaires sont connus.
- Une rencontre peut être programmée sans adversaires définitifs ni score. Une rencontre terminée possède un score pour chacune de ses deux équipes et un vainqueur unique.
- Le format d’une rencontre est Bo1, Bo3 ou Bo5. Ses manches portent un numéro unique **dans cette rencontre** ; une manche ne peut exister sans elle.
- Une manche enregistrée comme terminée a pour vainqueur l’une des deux équipes de sa rencontre. Si les résultats de toutes les manches sont disponibles, leur décompte doit correspondre au score de la rencontre.
- Un joueur peut être lié successivement à plusieurs équipes par des contrats datés. Pour une manche donnée, son alignement indique **le joueur, l’équipe et la manche** ; cette équipe doit participer à la rencontre.
- Une rencontre peut avoir un lieu renseigné. Plusieurs rencontres peuvent se tenir au même lieu.
- Une phase peut précéder une autre phase de la même édition ; une phase ne peut pas se précéder elle-même.

Les exemples de régions, ligues, compétitions internationales et phases figurent dans les pages Riot ; les contraintes de cohérence entre enregistrements sont des règles proposées pour la base étudiante. :chatgpt-content-reference{index="1"}

## 2. Dictionnaire de données

**Notation :** `PK` = clé primaire ; `FK` = clé étrangère ; `?` = donnée facultative. Une date et une heure de rencontre sont stockées en `TIMESTAMP` avec fuseau horaire. Le modèle compte **9 entités et 6 associations nommées**, soit **15 objets**.

| N° | Nature | Entité ou association | Données essentielles et nature |
|---:|---|---|---|
| 1 | Entité | **Région** | `id_region` **PK**, entier ; `nom_region`, texte (ex. EMEA). |
| 2 | Entité | **Compétition** | `id_competition` **PK**, entier ; `nom_competition`, texte ; `portee_competition`, énumération *régionale/internationale*. |
| 3 | Entité | **Édition** | `id_edition` **PK**, entier ; `id_competition` FK ; `annee_edition`, entier ; `nom_edition`, texte ; `date_debut_edition` et `date_fin_edition`, dates facultatives. |
| 4 | Entité | **Phase** | `id_phase` **PK**, entier ; `id_edition` FK ; `nom_phase`, texte ; `ordre_phase`, entier facultatif. |
| 5 | Entité | **Rencontre** | `id_rencontre` **PK**, entier ; `id_phase` FK ; `id_lieu` FK facultative ; `debut_prevu_rencontre`, horodatage avec fuseau ; `format_rencontre`, énumération *Bo1/Bo3/Bo5* ; `statut_rencontre`, énumération *programmée/en cours/terminée*. |
| 6 | **Entité faible** | **Manche** | **PK composée** : `id_rencontre` FK + `numero_manche`, entier positif ; `id_gagnante_equipe` FK facultative ; `statut_manche`, énumération *prévue/en cours/terminée*. |
| 7 | Entité | **Lieu** | `id_lieu` **PK**, entier ; `nom_lieu`, texte ; `ville_lieu` et `pays_lieu`, textes facultatifs. |
| 8 | Entité | **Équipe** | `id_equipe` **PK**, entier ; `nom_equipe`, texte ; `sigle_equipe`, texte facultatif. |
| 9 | Entité | **Joueur** | `id_joueur` **PK**, entier ; `pseudo_joueur`, texte ; `nom_joueur`, texte facultatif. |
| 10 | Association | **Rattachement** : compétition–région | **PK composée** : `id_competition` FK + `id_region` FK. Une compétition régionale a exactement un rattachement ; une internationale, aucun. |
| 11 | Association | **Inscription** : édition–équipe | **PK composée** : `id_edition` FK + `id_equipe` FK. |
| 12 | Association | **Participation** : rencontre–équipe | **PK composée** : `id_rencontre` FK + `id_equipe` FK ; `score_participation`, entier positif ou nul, facultatif avant le résultat. |
| 13 | **Association ternaire** | **Alignement** : manche–équipe–joueur | **PK composée** : `id_rencontre` FK + `numero_manche` FK + `id_equipe` FK + `id_joueur` FK ; `role_alignement`, texte facultatif. Le couple (`id_rencontre`, `numero_manche`, `id_joueur`) est unique. |
| 14 | Association | **Contrat** : joueur–équipe | `id_contrat` **PK**, entier ; `id_joueur` FK ; `id_equipe` FK ; `date_debut_contrat`, date ; `date_fin_contrat`, date facultative. |
| 15 | **Association récursive** | **Précédence** : phase–phase | **PK composée** : `id_amont_phase` FK + `id_aval_phase` FK. Les deux phases appartiennent à la même édition et sont distinctes. |

Dans `Alignement`, les deux champs qui identifient la manche forment **ensemble** sa clé étrangère. Les noms des clés suivent la convention demandée : un nom de clé suivi de `_` et du nom de l’entité, y compris pour les rôles `amont` et `aval` de **Phase**.

## 3. Vérification

**Oui, ce dictionnaire permet de construire un MCD répondant aux critères**, sous réserve d’appliquer les règles de cohérence indiquées :

- **Entités fortes :** notamment `Équipe`, `Joueur` et `Rencontre`, chacune identifiable par sa propre clé.
- **Entité faible :** `Manche`, identifiée par `id_rencontre` **et** `numero_manche`.
- **Association n-aire, n > 2 :** `Alignement` relie simultanément une manche, une équipe et un joueur.
- **Association récursive :** `Précédence` relie deux occurrences distinctes de `Phase`.
- **3FN :** chaque attribut descriptif est placé auprès de l’objet dont il dépend : la ville dépend du lieu, le format de la rencontre, le score du couple rencontre–équipe, et la date de début d’un contrat particulier. Les noms d’équipes, de joueurs et de compétitions ne sont pas recopiés dans les résultats.

Les dates, lieux, joueurs et résultats **peuvent être conservés** par ce schéma lorsqu’ils sont connus. Le calendrier consulté ne fournit pas systématiquement le détail des manches, des alignements ou du lieu de chaque rencontre : ces données doivent donc rester facultatives tant qu’une source ne les renseigne pas. :chatgpt-content-reference{index="2"}

## I.C. Règles métier


## I.D. Dictionnaire de données

