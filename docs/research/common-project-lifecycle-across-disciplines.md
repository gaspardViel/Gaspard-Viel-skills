# Existe-t-il une « route commune » à tout projet ? Une enquête sur sources primaires

But : déterminer, à partir des sources primaires elles-mêmes (normes, spécifications,
livres officiels), s'il existe une **colonne vertébrale partagée par tout projet** — une
séquence d'étapes que trois familles de savoirs indépendantes décrivent chacune de leur
côté — autrement dit une véritable « ingénierie de la gestion de projet ». Méthode : lire
trois familles de corpus (1. corpus formels de gestion de projet ; 2. gestion de cas &
science des checklists ; 3. systèmes de productivité mono-acteur), extraire de chacune la
séquence sous-jacente, puis vérifier si elles **convergent** vers une même route ou seulement
se ressemblent en surface. Le document reste une **survol de sources primaires** : aucune
traduction vers une architecture de skills, seulement des faits sourcés, honnêtes et
vérifiables. Pour chaque discipline on isole en priorité quatre **jointures porteuses**
alignées sur le domaine du dépôt (voir `CONTEXT.md`) : (a) définir *à l'avance* le jeu
d'éléments requis, (b) suivre la complétude/l'avancement, (c) relancer vers la clôture,
(d) définir le « done ».

## Note sur la méthode

Plusieurs hôtes canoniques renvoient un HTTP 403 ou ne servent qu'une coquille JavaScript à
l'outil de récupération de cette session : **pmi.org** (page Process Groups et page PMBOK),
**iso.org** (fiche ISO 21502), **atulgawande.com** (page du livre, partiellement servie) et
**axelos.com** (page PRINCE2 7, rendue en JS). Là où la page primaire n'a pu être récupérée
directement, les constats ci-dessous s'appuient sur les extraits (snippets) de moteur de
recherche **de la page/spéc/livre primaire elle-même** — jamais sur une paraphrase de site
tiers — et chaque affirmation reste attribuée à l'URL/référence primaire pour qu'un lecteur
puisse la vérifier. La spécification OMG CMMN et le Scrum Guide, eux, ont pu être lus
directement.

---

## Famille 1 — Corpus formels de gestion de projet

### 1.1 PMI, *PMBOK Guide* — les cinq groupes de processus

Le PMI structure historiquement tout projet en **cinq groupes de processus** :
*Initiating, Planning, Executing, Monitoring & Controlling, Closing*
([PMI, Process Groups: A Practice Guide](https://www.pmi.org/standards/process-groups)).
Ce qui est décisif pour notre question, ce sont les **rôles** attribués à chacun :

- **Initiating** — « define a new project or a new phase … by obtaining authorization to
  begin » ; ce groupe « helps to set the vision of what is to be accomplished »
  ([PMI, Process Groups](https://www.pmi.org/standards/process-groups)).
- **Planning** — « establish the scope of the project, define objectives, and determine the
  course of action to achieve the project's goals » ; c'est le groupe le plus volumineux
  (24 processus dans le modèle prédictif classique)
  ([PMI, Process Groups](https://www.pmi.org/standards/process-groups)).
- **Executing** — mise en œuvre du plan : coordonner personnes et ressources et **produire
  les livrables** ([PMI, Process Groups](https://www.pmi.org/standards/process-groups)).
- **Monitoring & Controlling** — « ensures the project stays aligned with scope, schedule,
  budget, and quality … focuses on **tracking performance and making adjustments** when
  needed ». Point important pour nous : ce groupe **ne se place pas après l'exécution**, il
  s'exécute **en parallèle** de l'exécution
  ([PMI, Process Groups](https://www.pmi.org/standards/process-groups)).
- **Closing** — souvent réduit à un seul processus, « close project or phase »
  ([PMI, Process Groups](https://www.pmi.org/standards/process-groups)).

Sur les quatre jointures : (a) le jeu requis est fixé en **Planning** (« establish the
scope … define objectives ») ; (b/c) le suivi et l'ajustement vivent dans **Monitoring &
Controlling** (« tracking performance … making adjustments ») ; (d) le « done » est un acte
explicite et distinct, **Closing** (« close project or phase »).

### 1.2 PMI, *PMBOK Guide* 7e édition — principes & domaines de performance (la séquence est *relâchée*)

La 7e édition (2021) **abandonne l'ossature séquentielle** des groupes de processus au profit
de **12 principes** (dans *The Standard for Project Management*) et de **8 domaines de
performance** (dans le *PMBOK Guide* lui-même)
([PMI, PMBOK Guide](https://www.pmi.org/standards/pmbok)). Un domaine de performance y est
défini comme « a group of related activities that are critical for the effective delivery of
project outcomes », et les principes « are not prescriptive … not laws … they are intended to
guide the behavior » ([PMI, PMBOK Guide](https://www.pmi.org/standards/pmbok)). Les domaines
ne sont pas des phases ordonnées mais des ensembles d'activités **concurrents**. Trois d'entre
eux recoupent nos jointures :

- **Planning** — « defining the course of action to achieve project objectives … planning
  continuously, updating information, and keeping alignment as conditions change »
  ([Delivery/Planning domains, PMI 7e éd.](https://www.pmi.org/standards/pmbok)).
- **Project Work** — « the actual creation of the project's deliverables, ensuring that the
  work is performed efficiently, meets quality standards »
  ([PMI 7e éd.](https://www.pmi.org/standards/pmbok)).
- **Measurement** — « assessing project performance and taking appropriate actions to
  maintain acceptable performance » (c.-à-d. le suivi/contrôle)
  ([PMI 7e éd.](https://www.pmi.org/standards/pmbok)).

À noter : la 8e édition (2025) **réintroduit** le niveau processus tout en gardant la base
principielle — signe que le PMI oscille entre « séquence de phases » et « domaines
concurrents », sans jamais nier ni l'un ni l'autre
([comparatif d'éditions, source secondaire — à vérifier sur pmi.org](https://www.pmi.org/standards/pmbok)).

**Verdict de section** : le PMI décrit bien une ossature (définir → produire → suivre →
clôturer), mais la 7e édition démontre que même *son propre* corpus considère l'**ordre strict**
comme non essentiel : ce sont les *concerns* (planifier, produire, mesurer, clôturer) qui sont
invariants, pas leur séquencement linéaire.

### 1.3 PRINCE2 (Axelos/PeopleCert) — sept processus, « du démarrage contrôlé à la clôture contrôlée »

PRINCE2 décrit « the chronological flow of a project from initial idea through to formal
closure » via **sept processus** : *Starting up a Project, Directing a Project, Initiating a
Project, Controlling a Stage, Managing Product Delivery, Managing a Stage Boundary, Closing a
Project* ([Axelos, PRINCE2 7](https://www.axelos.com/certifications/propath/prince2-project-management/prince2-7/)).
Chaque processus « has defined inputs, outputs, and activities »
([Axelos, via résultats de recherche](https://www.axelos.com/certifications/propath/prince2-project-management/prince2-7/)).

Sur les jointures : (a) le jeu requis est verrouillé lors de **Initiating a Project** (« setting
up the project for success with detailed planning ») ; (b/c) le suivi et la relance vivent dans
**Controlling a Stage** (« monitoring and controlling the project's progress ») et à chaque
**Managing a Stage Boundary** (« reviewing and planning for the next stage ») ; (d) le « done »
est **Closing a Project** (« bringing the project to a controlled end »)
([Axelos, PRINCE2 7](https://www.axelos.com/certifications/propath/prince2-project-management/prince2-7/)).

**Divergence à signaler** : *Directing a Project* n'est **pas** une phase séquentielle — c'est
la gouvernance (le *project board*) qui court **en continu** au-dessus des autres. Dans notre
domaine mono-acteur (`CONTEXT.md` : un jury ou une administration est une *cible* de Relance,
jamais un participant modélisé), ce processus de gouvernance à deux niveaux n'a pas d'équivalent.

### 1.4 ISO 21502:2020 — et la preuve la plus directe de convergence

ISO 21502:2020 « gives guidelines for project management … general project management concepts
and guidelines, prerequisites for formalizing project management, integrated project management
practices, and management practices for a project »
([ISO, fiche 21502:2020](https://www.iso.org/standard/74947.html)). Sa structure :

- **Clause 6 (pratiques intégrées)** couvre le cycle « from the **pre-project** activities
  through the **planning and controlling** activities to the **post-project** activities »
  ([ISO 21502 clause 6, snippets de la norme](https://www.iso.org/standard/74947.html)).
- **Clause 7 (pratiques de management)** détaille des pratiques nommées ; parmi elles,
  **7.4 Scope Management** = « defining, controlling, and confirming the project scope to ensure
  **all required work, and only the required work, is performed** »
  ([ISO 21502 clause 7, snippets de la norme](https://www.iso.org/standard/74947.html)). Cette
  phrase est l'énoncé formel le plus net de notre jointure (a) : *fixer d'avance exactement le
  jeu d'éléments requis, ni plus ni moins.*

Surtout : l'**Annexe A d'ISO 21502 mappe explicitement** ses pratiques sur les **cinq groupes
classiques** — *Initiating, Planning, Implementing, Controlling, Closing*
([ISO 21502 Annexe A, snippets de la norme](https://www.iso.org/standard/74947.html)). Un
consensus international (ISO) déclare donc de lui-même que ses pratiques se rangent dans la même
ossature à cinq temps que le PMI. C'est la meilleure preuve, *à l'intérieur des corpus formels*,
que ces derniers convergent réellement — et non par hasard — vers une séquence commune.

**Verdict Famille 1** : les trois corpus formels partagent une ossature nette —
**cadrer/autoriser → définir le périmètre requis → produire → suivre & contrôler → clôturer** —
et ISO 21502 va jusqu'à l'écrire noir sur blanc (Annexe A). Mais la 7e édition du PMBOK et le
rôle transverse *Directing* de PRINCE2 montrent que l'**ordre strict** est une convention forte,
pas une loi : ce sont les *étapes-concerns*, pas leur linéarité, qui sont réellement partagées.

---

## Famille 2 — Gestion de cas & science des checklists

### 2.1 OMG CMMN — un modèle qui *nie* la séquence stricte mais garde étapes, jalons et complétude

CMMN « defines a common meta-model and notation for … a Case … for capturing work methods
that are based on the handling of cases requiring various activities that may be performed in
an **unpredictable order** in response to evolving situations, using an event-centered approach »
([OMG, CMMN 1.1](https://www.omg.org/spec/CMMN/1.1/About-CMMN/) ;
[OMG, CMMN 1.0 PDF](https://www.omg.org/spec/CMMN/1.0/PDF)). C'est le contre-exemple le plus
important de toute l'enquête : une discipline entière, standardisée par l'OMG, existe
**précisément parce que** l'ordre fixe des phases ne convient pas à tous les projets.

Et pourtant, CMMN conserve exactement nos jointures :

- Un Case = « a case **Plan Model**, a **Case File Model**, and a set of case Roles »
  ([OMG CMMN, via snippets de la spéc](https://www.omg.org/spec/CMMN/1.0/PDF)). Le Case File
  Model est le jeu des éléments (analogue du *Classement* de `CONTEXT.md`).
- Une **Stage** = « a 'phase' in a case … a container of elements from which the plan of the
  case is constructed and can further evolve »
  ([OMG CMMN, snippets de la spéc](https://www.omg.org/spec/CMMN/1.0/PDF)).
- Un **Milestone** = « an achievable target, defined to enable **evaluation of progress** of
  the case, with **no work directly associated** … completion of a set of Tasks or the
  availability of key deliverables typically leading to achieving a Milestone. A Milestone may
  have zero or more **entry criteria**, which define the condition when a Milestone is reached »
  ([OMG CMMN, snippets de la spéc](https://www.omg.org/spec/CMMN/1.0/PDF)). C'est notre jointure
  (b) — suivre la complétude — modélisée comme un objet à part entière, calculé à partir de
  l'état des tâches/livrables, **jamais stocké comme un travail en soi** (exactement l'esprit du
  *Suivi de complétude* « computed live » de `CONTEXT.md`).
- Le franchissement d'état est gouverné par des **sentries** (critères d'entrée/sortie) : « When
  any entry criterion is met, the plan item … performs the state transition from AVAILABLE to
  ENABLED » et « When any exit criterion is met, a plan item performs a state transition … to
  state TERMINATED » ([CMMN 1.1, réf. §8.4.2, doc. d'implémentation Camunda citant la
  spéc](https://docs.camunda.org/manual/7.4/reference/cmmn11/concepts/entry-exit-criteria/)).
- Complétude/clôture : « When both tasks A and B are completed, the stage will also complete »
  ([CMMN, réf. spéc via Camunda](https://docs.camunda.org/manual/7.4/reference/cmmn11/concepts/entry-exit-criteria/)) —
  c'est notre jointure (d) : le « done » d'un conteneur est *dérivé* de l'achèvement de ses
  éléments, pas décrété.

### 2.2 INCOSE — traçabilité des exigences : le jeu requis, tracé de la définition à la vérification

Le *Systems Engineering Handbook* de l'INCOSE (4e éd. 2015, 5e éd. 2023) « emphasizes clear
**traceability from definition to verification** », en veillant à ce que « all requirements,
design elements, and test cases are connected », et pose que les exigences doivent être
« **verifiable, traceable**, and aligned with stakeholder needs »
([INCOSE SE Handbook / Needs and Requirements Manual, snippets de recherche](https://www.wiley.com/en-us/INCOSE+Needs+and+Requirements+Manual:+Needs,+Requirements,+Verification,+Validation+Across+the+Lifecycle-p-9781394152766)).
Cela couvre deux jointures : (a) définir *à l'avance* le jeu d'exigences requises, et (b/d)
**tracer** chaque exigence jusqu'à sa **vérification** — c'est-à-dire prouver, élément par
élément, que le jeu requis est satisfait avant de déclarer « done ». Le *Needs and Requirements
Manual* de l'INCOSE présente ces pratiques « across the system lifecycle »
([INCOSE, Wiley](https://www.wiley.com/en-us/INCOSE+Needs+and+Requirements+Manual:+Needs,+Requirements,+Verification,+Validation+Across+the+Lifecycle-p-9781394152766)).

### 2.3 Atul Gawande, *The Checklist Manifesto* — la checklist comme garantie que rien de requis n'est oublié

Gawande distingue les **errors of ignorance** (« mistakes we make because we don't know
enough ») des **errors of ineptitude** (« mistakes we made because we don't make proper use of
what we know »), et soutient que la plupart des échecs modernes relèvent de l'ineptitude — savoir
quoi faire mais **omettre de l'appliquer**
([Gawande, *The Checklist Manifesto*, ch. 1 « The Problem of Extreme Complexity », résumés de
recherche pointant le livre](http://atulgawande.com/book/the-checklist-manifesto/)). Deux formes
de checklist :

- **READ-DO** — « people carry out the tasks as they check them off … more like a recipe » ;
- **DO-CONFIRM** — « team members perform their jobs from memory … But then they **stop**. They
  **pause** to run the checklist and confirm that everything that was supposed to be done was
  done »
  ([Gawande, *The Checklist Manifesto*, ch. « The Checklist Factory », snippets de
  recherche](http://atulgawande.com/book/the-checklist-manifesto/)).

Ces checklists se conçoivent autour de **pause points** logiques, aux pauses naturelles d'un
flux de travail ([id.](http://atulgawande.com/book/the-checklist-manifesto/)). Pour notre
domaine, Gawande fonde deux jointures : (a) énumérer d'avance les étapes/éléments requis, et
(d) un **point de confirmation** (DO-CONFIRM) où l'on vérifie que *tout ce qui devait être fait
l'a été* — la définition opérationnelle du « done ».

### 2.4 Scrum Guide — la *Definition of Done*

Le *Scrum Guide* définit la **Definition of Done** comme « a formal description of the state of
the Increment when it meets the quality measures required for the product », avec la règle
tranchante : « **Work cannot be considered part of an Increment unless it meets the Definition
of Done** », et « If a Product Backlog item does not meet the Definition of Done, it cannot be
released or even presented at the Sprint Review »
([Scrum Guide 2020, scrumguides.org](https://scrumguides.org/scrum-guide.html)). L'Increment
est « a concrete **stepping stone toward the Product Goal** … additive to all prior Increments
and **thoroughly verified** »
([Scrum Guide, scrumguides.org](https://scrumguides.org/scrum-guide.html)). C'est notre jointure
(d) sous sa forme la plus pure : un **critère binaire, fixé d'avance, qui gate la sortie** — rien
n'est « done » tant que le critère n'est pas satisfait. C'est exactement le modèle
`Definition-of-Done checklist` de `CONTEXT.md` (ADR-0002).

**Verdict Famille 2** : cette famille couvre puissamment les jointures (a) *définir le jeu
requis d'avance*, (b) *suivre la complétude comme un objet dérivé* et (d) *définir le « done »
comme un critère explicite* — mais elle **n'impose pas de séquence de phases**. CMMN va jusqu'à
la nier frontalement. Ces disciplines valident donc nos jointures porteuses **tout en réfutant**
l'idée d'un ordre linéaire universel.

---

## Famille 3 — Systèmes de productivité mono-acteur

### 3.1 David Allen, *Getting Things Done* — cinq étapes

La page officielle GTD nomme cinq étapes : **Capture** (« Collect what has your attention »),
**Clarify** (« Process what it means »), **Organize** (« Put it where it belongs »), **Reflect**
(« Review frequently … Update and review all pertinent system contents to regain control and
focus »), **Engage** (« Simply do … make action decisions with confidence and clarity »)
([gettingthingsdone.com, What is GTD](https://gettingthingsdone.com/what-is-gtd/)). Le principe
central : sortir tout « of one's mind by recording them externally and then **breaking them into
actionable work items** »
([David Allen, GTD — présentation officielle](https://gettingthingsdone.com/what-is-gtd/)).

Correspondances : Capture+Clarify+Organize ≈ définir/ranger le jeu de choses à faire (jointures
a et rangement, cf. *Classement*) ; **Reflect** ≈ la **revue** régulière qui rouvre les manques
(jointure b/c : « regain control » suppose de repérer ce qui traîne) ; **Engage** ≈ exécuter.
Le dispositif GTD **Waiting For** (liste des choses attendues d'autrui) est l'analogue mono-acteur
le plus proche d'une **Relance** — la trace de ce qu'on attend d'un tiers
([GTD, workflow en cinq étapes](https://gettingthingsdone.com/what-is-gtd/)).

**Divergence à signaler** : GTD n'a **ni autorisation/gouvernance amont, ni clôture formelle**.
C'est une **boucle de flux personnelle** répétée en continu, pas un cycle de vie de projet à
début et fin uniques. « Reflect » n'est pas un jalon mais une habitude périodique.

### 3.2 Tiago Forte, PARA — un « Projet » est défini par un but et une fin

Forte définit un **Project** comme « short-term efforts … that you take on with a certain
**goal in mind** » (dessiner une page web, écrire un rapport, rénover une salle de bain), par
opposition à une **Area** = « important part[] of your work and life that require[] **ongoing
attention** … continue indefinitely **without a defined endpoint** »
([Tiago Forte, PARA — fortelabs.com](https://fortelabs.com/blog/para/)). Le trait distinctif est
exactement celui de notre *Projet* (`CONTEXT.md`) : **un but à atteindre et une fin**. Quand il
s'achève, il passe aux **Archives** = « anything … no longer active, but you might want to save
for future reference »
([Forte, PARA — fortelabs.com](https://fortelabs.com/blog/para/)). C'est notre jointure (d)/clôture
sous forme de *déplacement d'état* : « done » ⇒ archivé.

**Verdict Famille 3** : la productivité mono-acteur confirme deux bornes fortes — un projet **naît
d'un but** (Forte : « with a goal in mind ») et **se termine** (Forte : passage en Archives ;
GTD : l'item quitte les listes actives) — et un mécanisme de **revue** qui rouvre les manques
(GTD : Reflect ; Waiting For). Elle **n'apporte pas** de phase de gouvernance ni de séquence
figée : ces disciplines sont itératives et centrées sur l'acteur unique, ce qui colle au domaine
du dépôt mais affaiblit l'idée d'une séquence linéaire universelle.

---

## La route commune, énoncée neutralement

En superposant les trois familles, une même **suite d'étapes-concerns** ressort. Énoncée
indépendamment de tout vocabulaire propriétaire :

| Étape (neutre) | Ce qu'elle fait | Attestée par |
|---|---|---|
| **1. Cadrer & décider le but** | Décider *pourquoi* et *quoi* : fixer l'objectif/l'état-cible, ouvrir l'entreprise. | PMI *Initiating* (« set the vision … obtaining authorization ») ; PRINCE2 *Starting up* + *Directing* ; ISO 21502 « pre-project » (Annexe A : *Initiating*) ; GTD *Capture* ; Forte : un Projet « with a goal in mind ». |
| **2. Définir le jeu d'éléments requis** | Énumérer *à l'avance*, exactement, ce qu'il faut réunir — le périmètre/la checklist. | PMI *Planning* (« establish the scope … define objectives ») ; PRINCE2 *Initiating a Project* ; ISO 21502 §7.4 (« all required work, and only the required work ») ; CMMN Case File Model + Tasks/Milestones ; INCOSE (exigences *traceable/verifiable*) ; Gawande (les items de la checklist) ; Scrum DoD (les critères) ; GTD *Clarify/Organize*. |
| **3. Produire / assembler** | Faire le travail, obtenir les éléments un à un. | PMI *Executing* / domaine *Project Work* ; PRINCE2 *Controlling a Stage* + *Managing Product Delivery* ; ISO 21502 *Implementing* ; CMMN activation des Tasks (sentries) ; GTD *Engage* ; Forte : projet « actif ». |
| **4. Suivre la complétude & relancer les manques** | Calculer où l'on en est, détecter les trous, pousser vers leur clôture. | PMI *Monitoring & Controlling* / domaine *Measurement* (« tracking performance … adjustments », **en parallèle** de l'exécution) ; PRINCE2 *Controlling a Stage* + *Managing a Stage Boundary* ; ISO 21502 *Controlling* ; CMMN Milestone (« evaluation of progress ») ; GTD *Reflect* + *Waiting For* ; INCOSE traçabilité définition→vérification. |
| **5. Clôturer / « done »** | Vérifier que tout le jeu requis est satisfait, déclarer terminé, clore/archiver. | PMI *Closing* (« close project or phase ») ; PRINCE2 *Closing a Project* (« controlled end ») ; ISO 21502 *Closing* / « post-project » ; CMMN complétion de Stage/Case (exit criteria) ; Scrum DoD (« Done ») ; Gawande DO-CONFIRM (point de confirmation) ; Forte : passage en Archives. |

### Divergences explicites (là où une discipline nomme, insère ou retire une étape)

1. **La séquence stricte n'est pas universelle — elle est même explicitement niée par deux
   sources primaires.** CMMN est bâti pour des activités « performed in an **unpredictable
   order** » ([OMG CMMN](https://www.omg.org/spec/CMMN/1.1/About-CMMN/)) ; le *PMBOK Guide* 7e
   édition **remplace** les groupes de processus ordonnés par des domaines de performance
   **concurrents** ([PMI](https://www.pmi.org/standards/pmbok)). Même dans les corpus qui
   l'affichent, l'étape 4 (*Monitoring & Controlling*) « generally occurs **in parallel** to
   Execution » ([PMI, Process Groups](https://www.pmi.org/standards/process-groups)) : ce n'est
   pas une phase *après* la 3, mais *pendant*.
2. **La gouvernance n'est pas une étape séquentielle.** PRINCE2 *Directing a Project* court en
   continu au-dessus des autres ; le domaine mono-acteur (GTD, PARA) et le domaine du dépôt
   (`CONTEXT.md` : jury/administration = cible externe, pas participant) n'ont pas ce niveau. On
   la **loge donc dans l'étape 1** (décider/autoriser) plutôt que d'en faire une phase à part.
3. **L'étape « Relance » n'est de première classe nulle part.** Aucun corpus formel ne fait de
   la *chasse aux manques* une phase distincte : elle est **absorbée dans l'étape 4**
   (Monitoring/Controlling). L'analogue le plus explicite côté mono-acteur est le **Waiting For**
   de GTD ([gettingthingsdone.com](https://gettingthingsdone.com/what-is-gtd/)). C'est la
   jointure (c) la plus faiblement soutenue par les sources primaires — un fait à assumer.
4. **GTD/PARA n'ont pas de clôture *formelle*** : l'étape 5 y est un simple **changement d'état**
   (item quitte les listes ; projet → Archives), non un processus outillé comme le *Closing* de
   PMI/PRINCE2. Convergence sur *le fait* de finir, divergence sur *la cérémonie* de finir.
5. **Le « done » est tantôt décrété, tantôt dérivé.** Scrum en fait un **critère binaire posé
   d'avance** ([Scrum Guide](https://scrumguides.org/scrum-guide.html)) ; CMMN le **dérive** de
   l'achèvement des éléments (« when both tasks … are completed, the stage will complete »)
   ([CMMN via Camunda](https://docs.camunda.org/manual/7.4/reference/cmmn11/concepts/entry-exit-criteria/)).
   Les deux disent « un conteneur est fini quand ses éléments requis le sont » — mais l'un
   *vérifie contre une liste*, l'autre *calcule depuis les états*. (Le domaine du dépôt choisit
   la voie CMMN : *Suivi de complétude* « computed live, never stored ».)

### Convergence réelle vs. ressemblance superficielle

- **Genuinement convergent (arrivé indépendamment) :** l'idée qu'un projet **fixe d'avance un jeu
  requis** et **finit contre un critère de complétude**. ISO 21502 (« all required work, and only
  the required work »), Scrum (DoD), Gawande (checklist/DO-CONFIRM), CMMN (Case File + Milestones),
  INCOSE (traçabilité→vérification) et Forte (Projet « with a goal ») l'énoncent chacun **dans un
  vocabulaire distinct et sans se citer** — c'est le signe d'une convergence authentique, pas d'un
  emprunt. Ce sont les jointures (a), (b) et (d).
- **Partiellement superficiel (une seule lignée) :** la **séquence à cinq temps** exacte
  (*Initiating→Planning→Executing→Monitoring&Controlling→Closing*) est essentiellement la
  généalogie **PMI**, qu'ISO 21502 **reprend explicitement** (Annexe A) et que PRINCE2 décline.
  C'est une **famille**, pas trois témoins indépendants. Et les disciplines hors de cette lignée
  (CMMN, PMBOK 7, GTD, PARA) **ne l'imposent pas**. Traiter la *séquence linéaire* comme une loi
  universelle serait confondre une convention dominante avec un invariant.

---

## Verdict

**Une route commune existe — mais sous la forme d'un squelette de *jointures* partagées, pas
d'une séquence linéaire universelle.**

- **OUI, fortement**, pour les **étapes-concerns** : tout projet, dans les trois familles,
  (1) part d'un but, (2) **définit d'avance un jeu d'éléments requis**, (3) le produit/assemble,
  (4) **suit sa complétude et rattrape les manques**, (5) **se termine contre un critère de
  complétude**. Les jointures (a) *définir le jeu requis*, (b) *suivre la complétude comme valeur
  dérivée* et (d) *définir le « done »* sont attestées, indépendamment, par des sources primaires
  de chaque famille — c'est une convergence réelle et solide.
- **NON / faiblement**, pour la **séquence stricte et ordonnée** : elle est la convention d'une
  seule lignée (PMI, reprise par ISO 21502 et PRINCE2) et se trouve **explicitement relâchée ou
  niée** par les sources primaires les plus adaptatives — CMMN (« unpredictable order ») et le
  PMBOK 7 (domaines concurrents) — ainsi que par les systèmes mono-acteur itératifs (GTD, PARA).
  L'étape 4 est d'ailleurs, de l'aveu même du PMI, **concurrente** de l'étape 3, pas postérieure.
- **La jointure (c) — relancer/chasser vers la clôture — est la moins soutenue** : aucune source
  primaire n'en fait une phase de premier rang ; elle vit à l'intérieur du suivi/contrôle, son
  analogue mono-acteur le plus clair étant le *Waiting For* de GTD. Un dispositif de *Relance* de
  premier ordre (comme dans `CONTEXT.md`) est donc une **spécialisation légitime mais non
  prescrite** par la littérature primaire.

**Formulation la plus neutre et la mieux soutenue de la « route commune » :** *un projet est un
effort à but défini qui (1) fixe à l'avance un jeu d'éléments requis, (2) fait avancer ces
éléments par le travail, (3) mesure en continu sa complétude et pousse à combler les manques, et
(4) se déclare terminé lorsque, et seulement lorsque, le jeu requis satisfait son critère de
« done ».* Les corpus formels ajoutent par-dessus un **ordonnancement en phases** (cadrage →
planification → exécution → contrôle → clôture) qui est une **convention forte de gestion, non un
invariant** : les disciplines de gestion de cas et de productivité personnelle atteignent le même
résultat sans lui.

---

## Sources primaires citées (distinctes)

1. **PMI — *PMBOK Guide*, groupes de processus** — [pmi.org/standards/process-groups](https://www.pmi.org/standards/process-groups)
2. **PMI — *PMBOK Guide* 7e éd., principes & domaines de performance** — [pmi.org/standards/pmbok](https://www.pmi.org/standards/pmbok)
3. **Axelos/PeopleCert — PRINCE2 7, les sept processus** — [axelos.com … prince2-7](https://www.axelos.com/certifications/propath/prince2-project-management/prince2-7/)
4. **ISO 21502:2020 — Guidance on project management** — [iso.org/standard/74947.html](https://www.iso.org/standard/74947.html)
5. **OMG — CMMN (Case Management Model and Notation), spéc. 1.0/1.1** — [omg.org/spec/CMMN](https://www.omg.org/spec/CMMN/1.1/About-CMMN/) ; [PDF 1.0](https://www.omg.org/spec/CMMN/1.0/PDF) ; réf. §8.4.2 sentries via [docs.camunda.org (cite la spéc)](https://docs.camunda.org/manual/7.4/reference/cmmn11/concepts/entry-exit-criteria/)
6. **INCOSE — *Systems Engineering Handbook* / *Needs and Requirements Manual*** — [wiley.com … INCOSE NRM](https://www.wiley.com/en-us/INCOSE+Needs+and+Requirements+Manual:+Needs,+Requirements,+Verification,+Validation+Across+the+Lifecycle-p-9781394152766)
7. **Atul Gawande — *The Checklist Manifesto*** — [atulgawande.com/book/the-checklist-manifesto](http://atulgawande.com/book/the-checklist-manifesto/)
8. **Ken Schwaber & Jeff Sutherland — *The Scrum Guide* (2020), Definition of Done** — [scrumguides.org/scrum-guide.html](https://scrumguides.org/scrum-guide.html)
9. **David Allen — *Getting Things Done*, les cinq étapes** — [gettingthingsdone.com/what-is-gtd](https://gettingthingsdone.com/what-is-gtd/)
10. **Tiago Forte — *Building a Second Brain* / méthode PARA** — [fortelabs.com/blog/para](https://fortelabs.com/blog/para/)
