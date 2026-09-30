# I. Première étape : analyse des besoins

## I.A. Prompt engineering

### R — Rôle

Tu es un expert en esport League of Legend, analyste professionnel et concepteur de bases de données spécialisé dans l'e-sport. Tu as été joueur professionnel une partie de ta carrière avant de devenir analyste. 


### I — Instructions

Analyse le domaine de l'e-sport professionnel sur League of Legends dans le monde entier afin de construire une base de données relationnelle.

Commence par identifier les principales informations qui doivent être stockées pour gérer des compétitions et des matchs professionnels de League of Legends.

Identifie ensuite les règles métier qui régissent les relations entre ces informations.

Enfin, construis un dictionnaire de données structuré regroupant les données nécessaires à la conception d'un futur MCD.

Les informations doivent être suffisamment précises pour permettre ensuite de créer un MCD normalisé en troisième forme normale (3FN).


### C — Contexte

Réalise cette analyse dans le cadre d'un projet étudiants de l'EFREI dans la matière Base de données utilisant la méthode MERISE.

Le projet doit pouvoir être compris par des professeurs n'ayant aucune connaissance sur League of legends ni sur l'esport en général.


### A — Contraintes additionnelles

* Le domaine doit rester centré sur l'e-sport professionnel de League of Legends.
* Les règles métier doivent être formulées clairement.
* Les données proposées doivent être adaptées à une base de données relationnelle.
* Le dictionnaire de données doit pouvoir être utilisé pour réaliser un MCD en 3FN.
* Distinguer les identifiants des autres attributs.
* Chaque attribut doit être unique, pour ce faire, ajoute dans le nom de l'attribut le nom de l'entité à laquelle il appartient.
* Préciser le type de chaque donnée.
* Le type de chaque donnée doit être compatible avec le logiciel Looping.
* Préciser si une donnée est obligatoire ou facultative lorsque cela est pertinent.
* Prévoir suffisamment de données pour représenter l'historique des équipes et des joueurs.
* Ne pas inventer de règles qui ne sont pas nécessaires au fonctionnement du domaine.


### R — Références

Utilise les références suivantes pour ton analyse : 

* Riot Games — LoL Esports : https://www.leagueoflegends.com/fr-fr/news/esports/
* Riot Games Competitive Operations — League of Legends : https://competitiveops.riotgames.com/
* LoL Esports — calendrier officiel : https://lolesports.com/


### D — Rendement désiré

Retourne exactement les éléments suivants : 


#### 1. Règles métier

Les règles métiers doivent être formulées sous la forme de phrases précises.

Chaque règle doit décrire une contrainte ou une relation entre les données du domaine.

#### 2. Dictionnaire de données

Construis un tableau contenant 15 entités et relations.

Le tableau doit comporter les colonnes suivantes :

| Donnée | Description | Type | Identifiant | Obligatoire/Facultatif |
| ------ | ----------- | ---- | ----------- | ---------------------- |

Pour chaque donnée, indique clairement à quelle entité ou information elle appartient.


#### 3. Vérification

Vérifie :

* que le dictionnaire des données et les règles métier permettent de produire un MCD qui respecte la 3FN sans ajouter de nouvelle donnée
* que le dictionnaire des données contient au moins une association récursive, n-aire avec n>2, une entité faible et forte
* que les règles métier sont cohérentes entre elles
* que les types sont compatibles avec le logiciel Looping


### O — Objectif

Fournis une analyse complète et exploitable pour la réalisation du MCD d'une base de données de gestion de matchs d'e-sport professionnel sur League of Legends.

## I.B. Prompt obtenu

# Base de données de l'e-sport professionnel League of Legends

**Projet EFREI — Base de données (méthode MERISE)** — Analyse du domaine en vue de la construction d'un MCD normalisé en troisième forme normale (3FN).

---

## 0. Le domaine en quelques lignes (pour les non-initiés)

- **League of Legends** : jeu vidéo de stratégie opposant deux équipes de **cinq joueurs** ; chaque joueur contrôle un **champion**, un personnage aux capacités uniques.
- **Poste** : chaque joueur occupe un poste fixe — Toplane, Jungle, Midlane, ADC (tireur) ou Support.
- **Partie (game)** : une manche complète ; l'équipe qui détruit la base adverse remporte la partie. Une partie se joue d'un côté de la carte : **bleu** ou **rouge**.
- **Match (série)** : rencontre entre deux équipes, disputée en « **best-of** » : BO1, BO3 ou BO5 — la première équipe à remporter la majorité des parties gagne le match.
- **Pick &amp; ban** : avant chaque partie, les équipes **bannissent** des champions (interdits pour tous), puis les **sélectionnent** ; un champion ne peut apparaître qu'une fois par partie.
- **Patch** : version du jeu (ex. 26.19) ; les parties professionnelles se jouent sur un patch précis.
- **Ligue** : championnat régional (ex. LEC pour l'Europe, LCK pour la Corée) ; les meilleurs équipes des différentes régions s'affrontent lors de tournois internationaux (First Stand, MSI, championnat du monde « Worlds »).
- **Saison** : l'année compétitive est découpée en **segments** (hiver, printemps, été).

---

## 1. Règles métier

### 1.1 Circuit et compétitions

- **R1.** Une ligue représente une région du monde (ex. LEC = EMEA, LCK = Corée) et porte un nom unique.
- **R2.** Une saison correspond à un segment de l'année compétitive (ex. « Été 2026 ») ; le couple (année, segment) identifie une saison de manière unique.
- **R3.** Un tournoi se déroule pendant exactement une saison.
- **R4.** Un tournoi régional est organisé par exactement une ligue ; un tournoi international (First Stand, MSI, Worlds) n'est rattaché à aucune ligue.
- **R5.** Un tournoi réunit au moins deux équipes ; une équipe peut participer à plusieurs tournois, mais au plus une fois à un tournoi donné.
- **R6.** Une équipe possède au plus un classement final par tournoi ; ce classement n'est renseigné qu'à l'issue du tournoi.

### 1.2 Équipes et joueurs

- **R7.** Une équipe possède un nom et un tag d'affichage ; sa création est datée, et sa dissolution éventuelle est également datée — l'historique est conservé même après dissolution.
- **R8.** Une équipe principale peut être rattachée à plusieurs équipes affiliées (ex. équipe académique) ; une équipe affiliée est rattachée à au plus une équipe principale (association récursive sur EQUIPE).
- **R9.** Un joueur ne peut être sous contrat qu'avec une seule équipe à une date donnée ; l'ensemble de ses contrats successifs, datés, constitue l'historique de sa carrière.
- **R10.** Un contrat précise le poste du joueur (Toplane, Jungle, Midlane, ADC ou Support) et son statut (titulaire ou remplaçant) ; sa date de fin est facultative tant que le contrat est en cours.
- **R11.** Un joueur commence sa carrière à une date donnée et peut prendre sa retraite à une date donnée ; il reste enregistré dans la base après sa retraite.

### 1.3 Matchs et parties

- **R12.** Un match oppose exactement deux équipes et se dispute dans exactement un tournoi.
- **R13.** Un match est joué selon un format « best-of » : BO1, BO3 ou BO5, c'est-à-dire au plus 1, 3 ou 5 parties.
- **R14.** Un match possède un statut — planifié, en cours, terminé ou reporté — et sa date et son heure sont connues dès sa planification.
- **R15.** Une partie est identifiée par son numéro d'ordre au sein de son match ; une partie ne peut exister sans son match (entité faible).
- **R16.** Chaque partie est jouée sur un patch précis du jeu.
- **R17.** Une partie est remportée par exactement une équipe, forcément l'une des deux équipes du match.
- **R18.** L'équipe qui remporte le plus de parties remporte le match ; le score d'un match n'est donc pas stocké, il se calcule à partir des parties remportées.
- **R19.** Au cours d'une partie, chaque équipe joue d'un côté de la carte — bleu ou rouge ; le côté de l'équipe vainqueure est enregistré, l'autre équipe jouant le côté opposé.

### 1.4 Sélections et bannissements

- **R20.** Une partie est disputée par dix joueurs, cinq par équipe ; chaque joueur sélectionne exactement un champion et occupe un poste.
- **R21.** Un joueur ne peut être sélectionné que s'il est sous contrat, à la date du match, avec l'une des deux équipes qui disputent ce match.
- **R22.** Avant chaque partie, les équipes bannissent des champions lors de la phase de pick &amp; ban ; une équipe peut bannir plusieurs champions par partie.
- **R23.** Un même champion ne peut apparaître qu'une seule fois au cours d'une même partie, que ce soit comme sélection ou comme bannissement.

---

## 2. Dictionnaire de données

**Conventions.** Le dictionnaire couvre **15 entités et relations porteuses** : 9 entités et 6 relations (associations porteuses d'attributs). Chaque attribut est préfixé par le nom de son entité ou de sa relation, ce qui garantit l'unicité des noms. Les types utilisés — **Chaîne de caractères, Entier, Date, Heure** — sont tous des types proposés par le logiciel Looping. La colonne « Identifiant » indique si la donnée participe à l'identifiant ( clé) de son entité ou de sa relation ; « relatif » désigne l'identifiant d'une entité faible, « composé » une donnée qui complète l'identifiant d'une relation.

> **Note.** Les associations qui ne portent **aucune donnée** (ORGANISER, SE\_DÉROULER\_PENDANT, SE\_DÉROULER\_DANS, DISPUTER, COMPORTER, ÊTRE\_JOUEE\_SUR) n'ont pas de ligne dans ce dictionnaire : elles existeront dans le MCD, mais ne stockent rien. Elles sont récapitulées avec leurs cardinalités en fin de section.

### 1 — LIGUE *(entité forte — identifiant : ligue\_id)*


| Donnée                | Description                                             | Type                 | Identifiant | Obligatoire/Facultatif |
| --------------------- | ------------------------------------------------------- | -------------------- | ----------- | ---------------------- |
| ligue\_id             | Identifiant unique de la ligue                          | Entier               | Oui         | Obligatoire            |
| ligue\_nom            | Nom officiel de la ligue (ex. : LEC, LCK, LPL)          | Chaîne de caractères | Non         | Obligatoire            |
| ligue\_region         | Région couverte par la ligue (ex. : EMEA, Corée, Chine) | Chaîne de caractères | Non         | Obligatoire            |
| ligue\_date\_creation | Date de création de la ligue                            | Date                 | Non         | Facultatif             |


### 2 — SAISON *(entité forte — identifiant : saison\_id ; (saison\_annee, saison\_segment) est clé alternative)*


| Donnée              | Description                                                  | Type                 | Identifiant | Obligatoire/Facultatif |
| ------------------- | ------------------------------------------------------------ | -------------------- | ----------- | ---------------------- |
| saison\_id          | Identifiant unique de la saison                              | Entier               | Oui         | Obligatoire            |
| saison\_annee       | Année civile de la saison (ex. : 2026)                       | Entier               | Non         | Obligatoire            |
| saison\_segment     | Segment de l'année compétitive (ex. : Hiver, Printemps, Été) | Chaîne de caractères | Non         | Obligatoire            |
| saison\_date\_debut | Date d'ouverture de la saison                                | Date                 | Non         | Obligatoire            |
| saison\_date\_fin   | Date de clôture de la saison                                 | Date                 | Non         | Obligatoire            |


### 3 — TOURNOI *(entité forte — identifiant : tournoi\_id)*


| Donnée               | Description                                                                    | Type                 | Identifiant | Obligatoire/Facultatif |
| -------------------- | ------------------------------------------------------------------------------ | -------------------- | ----------- | ---------------------- |
| tournoi\_id          | Identifiant unique du tournoi                                                  | Entier               | Oui         | Obligatoire            |
| tournoi\_nom         | Nom du tournoi (ex. : « LEC Été 2026 », « Worlds 2026 »)                       | Chaîne de caractères | Non         | Obligatoire            |
| tournoi\_date\_debut | Date de début du tournoi                                                       | Date                 | Non         | Obligatoire            |
| tournoi\_date\_fin   | Date de fin du tournoi                                                         | Date                 | Non         | Obligatoire            |
| tournoi\_lieu        | Ville ou pays hôte du tournoi (utile surtout pour les tournois internationaux) | Chaîne de caractères | Non         | Facultatif             |


### 4 — EQUIPE *(entité forte — identifiant : equipe\_id)*


| Donnée                    | Description                                    | Type                 | Identifiant | Obligatoire/Facultatif |
| ------------------------- | ---------------------------------------------- | -------------------- | ----------- | ---------------------- |
| equipe\_id                | Identifiant unique de l'équipe                 | Entier               | Oui         | Obligatoire            |
| equipe\_nom               | Nom complet de la structure (ex. : G2 Esports) | Chaîne de caractères | Non         | Obligatoire            |
| equipe\_tag               | Abréviation d'affichage (ex. : G2)             | Chaîne de caractères | Non         | Obligatoire            |
| equipe\_date\_creation    | Date de création de l'équipe                   | Date                 | Non         | Obligatoire            |
| equipe\_date\_dissolution | Date de dissolution, si l'équipe n'existe plus | Date                 | Non         | Facultatif             |
| equipe\_pays              | Pays d'attache de l'équipe                     | Chaîne de caractères | Non         | Facultatif             |


### 5 — JOUEUR *(entité forte — identifiant : joueur\_id)*


| Donnée                        | Description                         | Type                 | Identifiant | Obligatoire/Facultatif |
| ----------------------------- | ----------------------------------- | -------------------- | ----------- | ---------------------- |
| joueur\_id                    | Identifiant unique du joueur        | Entier               | Oui         | Obligatoire            |
| joueur\_pseudonyme            | Pseudonyme en jeu (ex. : Faker)     | Chaîne de caractères | Non         | Obligatoire            |
| joueur\_nom                   | Nom de famille                      | Chaîne de caractères | Non         | Obligatoire            |
| joueur\_prenom                | Prénom                              | Chaîne de caractères | Non         | Obligatoire            |
| joueur\_date\_naissance       | Date de naissance                   | Date                 | Non         | Obligatoire            |
| joueur\_nationalite           | Nationalité                         | Chaîne de caractères | Non         | Obligatoire            |
| joueur\_date\_debut\_carriere | Date du premier match professionnel | Date                 | Non         | Facultatif             |
| joueur\_date\_fin\_carriere   | Date de retraite sportive           | Date                 | Non         | Facultatif             |


### 6 — MATCH *(entité forte — identifiant : match\_id)*


| Donnée         | Description                                       | Type                 | Identifiant | Obligatoire/Facultatif |
| -------------- | ------------------------------------------------- | -------------------- | ----------- | ---------------------- |
| match\_id      | Identifiant unique du match                       | Entier               | Oui         | Obligatoire            |
| match\_libelle | Libellé de la rencontre (ex. : « Grande finale ») | Chaîne de caractères | Non         | Facultatif             |
| match\_date    | Date à laquelle le match est disputé              | Date                 | Non         | Obligatoire            |
| match\_heure   | Heure de début du match                           | Heure                | Non         | Obligatoire            |
| match\_format  | Format de la série : BO1, BO3 ou BO5              | Chaîne de caractères | Non         | Obligatoire            |
| match\_statut  | Statut : planifié, en cours, terminé ou reporté   | Chaîne de caractères | Non         | Obligatoire            |


### 7 — PARTIE *(entité faible de MATCH — identifiant : (match\_id, partie\_numero))*


| Donnée         | Description                                          | Type   | Identifiant               | Obligatoire/Facultatif |
| -------------- | ---------------------------------------------------- | ------ | ------------------------- | ---------------------- |
| partie\_numero | Numéro d'ordre de la partie dans le match (1, 2, 3…) | Entier | Oui (identifiant relatif) | Obligatoire            |
| partie\_duree  | Durée de la partie, en secondes                      | Entier | Non                       | Facultatif             |


### 8 — CHAMPION *(entité forte — identifiant : champion\_id)*


| Donnée                 | Description                                          | Type                 | Identifiant | Obligatoire/Facultatif |
| ---------------------- | ---------------------------------------------------- | -------------------- | ----------- | ---------------------- |
| champion\_id           | Identifiant unique du champion                       | Entier               | Oui         | Obligatoire            |
| champion\_nom          | Nom du personnage jouable (ex. : Ahri)               | Chaîne de caractères | Non         | Obligatoire            |
| champion\_classe       | Classe principale du champion (ex. : Mage, Assassin) | Chaîne de caractères | Non         | Obligatoire            |
| champion\_date\_sortie | Date d'ajout du champion au jeu                      | Date                 | Non         | Obligatoire            |


### 9 — PATCH *(entité forte — identifiant : patch\_numero)*


| Donnée              | Description                            | Type                 | Identifiant | Obligatoire/Facultatif |
| ------------------- | -------------------------------------- | -------------------- | ----------- | ---------------------- |
| patch\_numero       | Numéro de version du jeu (ex. : 26.19) | Chaîne de caractères | Oui         | Obligatoire            |
| patch\_date\_sortie | Date de mise en service de la version  | Date                 | Non         | Obligatoire            |


### 10 — CONTRAT *(relation porteuse JOUEUR – EQUIPE — identifiant : (joueur\_id, equipe\_id, contrat\_date\_debut))*

Historise l'effectif des équipes et la carrière des joueurs.


| Donnée               | Description                                             | Type                 | Identifiant   | Obligatoire/Facultatif |
| -------------------- | ------------------------------------------------------- | -------------------- | ------------- | ---------------------- |
| contrat\_date\_debut | Date de signature (début) du contrat                    | Date                 | Oui (composé) | Obligatoire            |
| contrat\_date\_fin   | Date de fin du contrat (absente = contrat en cours)     | Date                 | Non           | Facultatif             |
| contrat\_poste       | Poste occupé : Toplane, Jungle, Midlane, ADC ou Support | Chaîne de caractères | Non           | Obligatoire            |
| contrat\_statut      | Statut dans l'équipe : titulaire ou remplaçant          | Chaîne de caractères | Non           | Facultatif             |


### 11 — AFFILIATION *(relation récursive EQUIPE – EQUIPE, porteuse — identifiant : (equipe\_principale, equipe\_affiliee, affiliation\_date\_debut))*

Rôles : équipe principale / équipe affiliée (ex. équipe académique).


| Donnée                   | Description                                        | Type | Identifiant   | Obligatoire/Facultatif |
| ------------------------ | -------------------------------------------------- | ---- | ------------- | ---------------------- |
| affiliation\_date\_debut | Date de début du rattachement                      | Date | Oui (composé) | Obligatoire            |
| affiliation\_date\_fin   | Date de fin du rattachement (absente = lien actif) | Date | Non           | Facultatif             |


### 12 — PARTICIPATION *(relation porteuse EQUIPE – TOURNOI — identifiant : (equipe\_id, tournoi\_id))*


| Donnée                    | Description                                                  | Type   | Identifiant | Obligatoire/Facultatif |
| ------------------------- | ------------------------------------------------------------ | ------ | ----------- | ---------------------- |
| participation\_classement | Classement final de l'équipe dans le tournoi (1 = vainqueur) | Entier | Non         | Facultatif             |


### 13 — REMPORTER *(relation porteuse EQUIPE – PARTIE — identifiant : (equipe\_id, match\_id, partie\_numero))*

Chaque partie a exactement un vainqueur (R17).


| Donnée          | Description                                                   | Type                 | Identifiant | Obligatoire/Facultatif |
| --------------- | ------------------------------------------------------------- | -------------------- | ----------- | ---------------------- |
| remporter\_side | Côté de la carte joué par l'équipe vainqueure : Bleu ou Rouge | Chaîne de caractères | Non         | Obligatoire            |


### 14 — SELECTION *(relation ternaire JOUEUR – PARTIE – CHAMPION, porteuse — identifiant : (joueur, partie))*

Clé réelle : le couple (joueur, partie), car un joueur sélectionne exactement un champion par partie (R20) ; le champion est fonctionnellement déterminé.


| Donnée             | Description                                   | Type                 | Identifiant | Obligatoire/Facultatif |
| ------------------ | --------------------------------------------- | -------------------- | ----------- | ---------------------- |
| selection\_poste   | Poste occupé pendant la partie                | Chaîne de caractères | Non         | Obligatoire            |
| selection\_kills   | Nombre d'éliminations réalisées par le joueur | Entier               | Non         | Facultatif             |
| selection\_deaths  | Nombre de morts du joueur                     | Entier               | Non         | Facultatif             |
| selection\_assists | Nombre d'aides à l'élimination                | Entier               | Non         | Facultatif             |


### 15 — BANNIR *(relation ternaire EQUIPE – PARTIE – CHAMPION, porteuse — identifiant : (equipe\_id, match\_id, partie\_numero, champion\_id))*


| Donnée        | Description                                           | Type   | Identifiant | Obligatoire/Facultatif |
| ------------- | ----------------------------------------------------- | ------ | ----------- | ---------------------- |
| bannir\_ordre | Ordre du bannissement dans la phase de pick &amp; ban | Entier | Non         | Facultatif             |


### Associations sans attribut (futur MCD, aucune donnée stockée)


| Association                         | Entités liées    | Cardinalités (indicatives) | Règle |
| ----------------------------------- | ---------------- | -------------------------- | ----- |
| ORGANISER                           | LIGUE — TOURNOI  | (0,n) — (0,1)              | R4    |
| SE\_DÉROULER\_PENDANT               | SAISON — TOURNOI | (0,n) — (1,1)              | R3    |
| SE\_DÉROULER\_DANS                  | TOURNOI — MATCH  | (1,n) — (1,1)              | R12   |
| DISPUTER                            | EQUIPE — MATCH   | (0,n) — (1,n)\*            | R12   |
| COMPORTER *(lien d'identification)* | MATCH — PARTIE   | (1,n) — (1,1)              | R15   |
| ÊTRE\_JOUEE\_SUR                    | PATCH — PARTIE   | (0,n) — (1,1)              | R16   |


\* Looping ne propose pas de cardinalité maximale « 2 » : la contrainte « exactement deux équipes par match » (R12) est portée par la règle métier, avec une cardinalité (1,n) sur l'association DISPUTER.

---

## 3. Vérification

### 3.1 Le dictionnaire permet un MCD en 3FN, sans donnée supplémentaire

1. **Dépendances directes.** Pour chaque entité, tout attribut non identifiant dépend fonctionnellement de l'identifiant, et de rien d'autre : par exemple `joueur_nationalite` dépend de `joueur_id`, pas du pseudonyme ; `ligue_region` dépend de `ligue_id`. Aucun attribut ne dépend d'un autre attribut non identifiant (pas de dépendance transitive).
2. **Aucune donnée calculée stockée.** Le score et le vainqueur d'un match se déduisent des parties remportées (R18) ; le nombre d'équipes d'un tournoi se déduit de PARTICIPATION ; l'activité d'une équipe se déduit de `equipe_date_dissolution`. Ces valeurs ne figurent donc pas dans le dictionnaire.
3. **Aucun attribut multivalué.** `champion_classe` est limitée à la classe principale ; les occurrences multiples (plusieurs contrats, participations, affiliations) sont portées par des relations **datées**, ce qui historise sans dupliquer.
4. **PATCH élevé au rang d'entité.** La date de sortie d'une version ne dépend que du numéro de patch : la stocker dans PARTIE aurait créé une dépendance transitive (partie → patch → date de sortie). Sa promotion en entité supprime cette redondance.
5. **Clés des relations porteuses complètes.** Pour CONTRAT, la clé est (joueur, équipe, date de début) : date de fin, poste et statut en dépendent entièrement. Pour SELECTION, la clé réelle est (joueur, partie) puisque le champion est fonctionnellement déterminé par ce couple (R20) : poste, kills, deaths et assists dépendent de cette clé entière — aucune dépendance partielle ni transitive. Pour BANNIR, la clé (équipe, partie, champion) est minimale : l'ordre de bannissement en dépend entièrement.

**Conclusion :** le MCD dérivé de ce dictionnaire se traduit en relations directement normalisées en 3FN, sans ajouter de nouvelle donnée.

### 3.2 Contraintes structurelles demandées — toutes présentes


| Contrainte                        | Élément du dictionnaire                                                                            | Justification                                                                                                                                |
| --------------------------------- | -------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| Association **récursive**         | AFFILIATION (EQUIPE – EQUIPE, §2.11)                                                               | Une équipe principale peut avoir plusieurs équipes affiliées, chacune rattachée à au plus une équipe principale (R8)                         |
| Association **n-aire (n &gt; 2)** | SELECTION (JOUEUR – PARTIE – CHAMPION, §2.14) et BANNIR (EQUIPE – PARTIE – CHAMPION, §2.15), n = 3 | Une sélection associe simultanément un joueur, une partie et un champion (R20) ; un bannissement une équipe, une partie et un champion (R22) |
| **Entité faible**                 | PARTIE, identifiée relativement à MATCH par (match\_id, partie\_numero) (§2.7)                     | Une partie ne peut exister sans son match (R15)                                                                                              |
| **Entité forte**                  | LIGUE, SAISON, TOURNOI, EQUIPE, JOUEUR, MATCH, CHAMPION, PATCH (§2.1 à 2.9)                        | Identifiants propres, existence indépendante                                                                                                 |


### 3.3 Cohérence des règles métier

- **R3 + R4** : tout tournoi a exactement une saison ; la ligue est optionnelle pour les tournois internationaux — ce qui correspond à la cardinalité (0,1) de ORGANISER.
- **R9 + R21** : la sélection d'un joueur en partie exige un contrat actif à la date du match ; l'historique des contrats est donc pleinement exploitable.
- **R12 → R13 → R15 → R17 → R18** : chaîne cohérente match → parties → vainqueurs → score. Un BO3 terminé correspond exactement à une équipe ayant remporté 2 parties sur au plus 3.
- **R19 ↔ R12** : les deux côtés (bleu/rouge) existent précisément parce qu'un match oppose exactement deux équipes.
- **R20 + R22 + R23** : un champion ne peut figurer qu'une fois par partie, que ce soit par sélection ou par bannissement — l'unicité est garantie sur l'ensemble des deux relations.
- Aucune règle ne contredit une autre, et chaque règle justifie une entité, une cardinalité ou une contrainte d'intégrité du futur MCD : aucune règle superflue n'a été inventée.

### 3.4 Compatibilité avec le logiciel Looping

- **Types** : Chaîne de caractères, Entier, Date et Heure sont tous des types proposés par Looping ; aucun type exotique n'est utilisé.
- **Identifiants** : identifiants simples, identifiant relatif pour l'entité faible PARTIE (lien d'identification COMPORTER, cardinalités (1,1)–(1,n)) et identifiants composés pour les relations porteuses — autant de fonctionnalités natives de Looping.
- **Associations** : les associations ternaires (SELECTION, BANNIR) et l'association récursive (AFFILIATION) sont prises en charge directement.
- **Limites assumées** : Looping ne gère que les cardinalités usuelles (0,1 / 0,n / 1,1 / 1,n) ; la contrainte « exactement deux équipes par match » (R12) ne peut pas s'exprimer par une cardinalité (2,2) et reste portée par la règle métier — c'est précisément le rôle des règles de gestion dans MERISE.
- **Unicité des noms** : chaque donnée est préfixée par son entité ou sa relation ; le dictionnaire peut être saisi dans Looping sans conflit de nom.

---

## Sources

- [LoL Esports — League Handbook (calendrier et structure des ligues)](https://lolesports.com/en-GB/season/113470241010388072/handbook)
- [Riot Games — LoL Esports (actualités)](https://www.leagueoflegends.com/fr-fr/news/esports/)
- [Riot Games Competitive Operations](https://competitiveops.riotgames.com/)
- [Wikipedia — List of League of Legends leagues and tournaments](https://en.wikipedia.org/wiki/List_of_League_of_Legends_leagues_and_tournaments)
- [Leaguepedia — exemple de suivi pick &amp; ban et patch par partie (MSI 2025)](https://lol.fandom.com/wiki/2025_Mid-Season_Invitational/Picks_and_Bans)

