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

Sois concis, donne ta réponse sous la forme règles métier et dictionnaire de données. Précise la nature des données. Récupère tes informations dans les références données uniquement.


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

Dans ce modèle, un **match** est une rencontre entre deux équipes ; une **manche** est une partie jouée dans ce match. Le calendrier officiel distingue les compétitions, leurs phases, les dates, les équipes, les scores et les formats tels que Bo3 et Bo5. Les règlements de Riot traitent notamment des effectifs et de l’éligibilité des joueurs. :chatgpt-content-reference{index="0"}

1. Une région peut organiser plusieurs compétitions ; une compétition régionale appartient à une région. Une compétition internationale réunit des équipes de plusieurs régions et n’est rattachée à aucune région unique.
2. Une compétition possède des éditions datées. Chaque édition comporte une ou plusieurs phases ; une phase peut être une sous-phase d’une autre phase de la **même édition**.
3. Un match appartient à une phase et oppose exactement deux équipes distinctes. Une équipe peut disputer plusieurs matchs, y compris contre la même équipe.
4. Un match possède une date et une heure prévues, un fuseau horaire et un statut. Son lieu physique peut être inconnu ou sans objet. Une modification du calendrier conserve la nouvelle date prévue.
5. Le format d’un match fixe le nombre maximal de manches prévues. Une manche est identifiée par son numéro **à l’intérieur de son match** ; elle ne peut exister sans ce match.
6. Une manche jouée oppose les deux équipes du match et possède une équipe gagnante. Pour un match terminé normalement, le score de chaque équipe est le nombre de manches qu’elle a gagnées ; l’équipe ayant atteint le nombre de victoires requis gagne le match. Un forfait ou une annulation exige un statut distinct afin de ne pas inventer des manches jouées.
7. Un joueur peut appartenir à plusieurs équipes **à des périodes différentes**. Ses périodes d’appartenance à deux équipes ne peuvent pas se chevaucher si le projet retient une appartenance exclusive.
8. La participation à une manche associe **un joueur, une équipe et une manche**. L’équipe doit être l’une des deux équipes du match, et le joueur doit être éligible dans son effectif à la date de la manche. Chaque équipe aligne cinq joueurs distincts dans une manche effectivement jouée. Les postes sont `TOP`, `JUNGLE`, `MID`, `BOT` et `SUPPORT`. :chatgpt-content-reference{index="1"}
9. Les noms de ligues, les phases et les formats sont enregistrés par édition ou par match : ils ne sont pas supposés identiques dans toutes les régions ni constants sur dix ans. Le calendrier montre, par exemple, des Bo3 en phase régulière et des Bo5 en playoffs de LEC ; Riot décrit également des phases et qualifications propres aux événements internationaux. :chatgpt-content-reference{index="2"}

## 2. Dictionnaire de données

`PK` désigne un identifiant, `FK` une référence à une autre entité. Les identifiants techniques proposés servent au projet ; ils ne prétendent pas être des identifiants publiés par Riot.

| Entité ou association | Données et nature |
|---|---|
| **Région** | `id_region` (PK, entier) ; `nom` (texte). |
| **Compétition** | `id_competition` (PK, entier) ; `nom` (texte) ; `portee` (énumération : régionale, internationale) ; `id_region` (FK, entier, nul pour une compétition internationale). |
| **Édition** | `id_edition` (PK, entier) ; `id_competition` (FK) ; `annee` (entier) ; `date_debut`, `date_fin` (dates). |
| **Phase** | `id_phase` (PK, entier) ; `id_edition` (FK) ; `nom` (texte : saison régulière, playoffs, etc.) ; `id_phase_parente` (FK vers **Phase**, facultative). |
| **Équipe** | `id_equipe` (PK, entier) ; `nom` (texte) ; `sigle` (texte, facultatif). |
| **Joueur** | `id_joueur` (PK, entier) ; `pseudo` (texte) ; `nom_public` (texte, facultatif). Le pseudo seul n’est pas une clé fiable sur dix ans. |
| **Effectif** | `id_joueur` (FK) ; `id_equipe` (FK) ; `date_debut` (date) — **clé composée** ; `date_fin` (date, facultative). Représente l’historique d’appartenance. |
| **Lieu** | `id_lieu` (PK, entier) ; `nom_site`, `ville`, `pays` (textes). |
| **Match** | `id_match` (PK, entier) ; `id_phase` (FK) ; `debut_prevu` (date et heure avec fuseau) ; `statut` (énumération : prévu, en cours, terminé, forfait, annulé) ; `format_max_manches` (entier) ; `id_lieu` (FK, facultative). |
| **Équipe du match** | `id_match` (FK) ; `id_equipe` (FK) — **clé composée**. Exactement deux lignes par match ; pour un forfait, `issue_exceptionnelle` (énumération : victoire, défaite, sans objet, facultative). |
| **Manche** | `id_match` (FK) ; `numero_manche` (entier positif) — **clé composée** ; `debut_reel` (date et heure avec fuseau, facultatif) ; `statut` (énumération : prévue, en cours, terminée) ; `id_equipe_gagnante` (FK vers Équipe, renseignée à la fin). |
| **Participation** | `id_match`, `numero_manche` (FK composée vers Manche) ; `id_equipe` (FK) ; `id_joueur` (FK) — **clé composée** ; `poste` (énumération : TOP, JUNGLE, MID, BOT, SUPPORT). Association entre **trois objets métier** : manche, équipe et joueur. |

Le **score du match** et son **vainqueur normal** sont calculés à partir des manches terminées ; les enregistrer une seconde fois comme faits indépendants créerait un risque de contradiction. Les lieux physiques ne doivent pas être déduits de la région : Riot annonce, par exemple, un site et des dates propres au MSI 2026. :chatgpt-content-reference{index="3"}

## 3. Vérification

Le modèle permet de construire le MCD demandé : **Équipe** et **Joueur** sont des entités fortes ; **Manche** est une entité faible identifiée par `(id_match, numero_manche)` ; **Phase → Phase** est une association récursive ; **Participation(Manche, Équipe, Joueur)** est une association ternaire.

La **3FN se vérifie sur les relations obtenues du MCD**, et non sur le MCD lui-même. Avec les clés indiquées, chaque attribut non clé dépend de la clé de sa relation, sans dépendance transitive volontaire : le nom de région reste dans Région, le lieu dans Lieu, et les informations du joueur dans Joueur. Le score calculé évite une donnée redondante. Les contraintes « deux équipes », « cinq joueurs par équipe et par manche », « équipe gagnante parmi les participantes » et « périodes d’effectif sans chevauchement » devront être contrôlées lors de l’implémentation ; la 3FN, à elle seule, ne les impose pas.

## I.C. Règles métier


## I.D. Dictionnaire de données

