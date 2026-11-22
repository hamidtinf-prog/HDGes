# HDGes — Rapports & États : logique et mode d'emploi

> Application de gestion Station / Marché de gros (achat de fournisseurs, vente à des clients, règlements, banque).
> Cette fiche décrit **comment chaque état est calculé** et **comment lire les chiffres**.

---

## 1. Où trouver les états

Menu **Rapports** (barre latérale) :

| Sous-menu | Rôle en une phrase |
|---|---|
| **Rapport** | Détail des bons filtrés, avec règlements/avances en lignes datées, solde et lettrage |
| **État Échéance** | Règlements dont l'échéance est à venir |
| **Extrait de compte** | Justification de solde (non réglés), bons réglés, ou tous les bons |
| **État Global** | Grand livre de tous les comptes, groupé par fournisseur/client avec sous-totaux mensuels |
| **État Crédit** | Crédits bancaires, filtrables par créditeur |
| **Balance âgée** | Ancienneté des impayés par tranche de jours |
| **Balance générale** | Mouvements par mois pour Fournisseurs, Clients et Banque |

Chaque état propose **Aperçu** et **Télécharger PDF**.

---

## 2. Conventions de calcul

### 2.1 Débit / Crédit / Solde (Grand livre, État Global, Extrait, Balance générale)

| Compte | Un **bon** est au… | Un **règlement** est au… | Solde |
|---|---|---|---|
| **Fournisseur** | **Crédit** (on lui doit) | **Débit** (on le paie) | Débit − Crédit |
| **Client** | **Débit** (il nous doit) | **Crédit** (il nous paie) | Débit − Crédit |

Lecture du solde (couleur) :
- **Fournisseur** : solde **négatif** (rouge) = on lui doit de l'argent ; solde **positif** (vert) = on lui a versé une **avance**.
- **Client** : solde **positif** = il nous doit ; solde **négatif** = il a versé une avance.

### 2.2 Banque

- Crédit = entrée d'argent, Débit = sortie d'argent.
- **Solde actuel** = Σ Crédit − Σ Débit (opérations réglées uniquement).
- **Situation actuelle** = Solde actuel − Σ des **soldes négatifs des fournisseurs** (ce qu'on leur doit encore).
  Les soldes positifs (avances) ne sont ni ajoutés ni retranchés.

### 2.3 Rapport principal (convention différente)

Dans **Rapport**, Solde = **Valeur − Règlement/Avance** :
- positif (rouge) = reste à payer ;
- négatif (vert) = avance versée.

---

## 3. Règlements, avances et lettrage

### 3.1 Règlement unique, plusieurs bons
Un règlement peut couvrir plusieurs bons (via le **lettrage** choisi à la saisie). Son montant est **réparti** entre ces bons :
les bons les plus anciens sont réglés en premier, avec les règlements les plus anciens. Le montant n'est **jamais compté deux fois**.

### 3.2 Avance
**Avance = la part d'un règlement qui n'est affectée à aucun bon.**
Elle apparaît comme une **ligne de règlement datée**, avec son propre montant — pas comme un total isolé.

### 3.3 Lettrage (A, B, C…)
- Chaque règlement reçoit une **lettre**, attribuée chronologiquement (date, puis ordre de saisie).
- Le lettrage est **indépendant par compte** : chaque fournisseur/client recommence à **A**.
- Un bon affiche la ou les lettres des règlements qui le couvrent ; un règlement affiche sa propre lettre.

---

## 4. Détail de chaque état

### 4.1 Rapport
- Les filtres (fournisseur, client, station, produit, variété, dates, « valeurs affichées ») **se regénèrent automatiquement** à chaque changement ; il n'y a plus de bouton *Générer*.
- Colonnes : `Date | Pièce | Produit-Variété | Station | Poids net | Prix | Valeur | Règlement/Avance | Solde | Lettrage`.
- Bons et règlements sont mélangés par date ; chaque règlement/avance est **sa propre ligne**, avec la mention *Règlement* (lié à des bons) ou *Avance* (non lié).
- ⚠️ Si un filtre **Produit / Variété / Station / côté opposé** est actif, les règlements ne sont pas affichés en lignes (ils ne se rattachent pas à un produit) ; une note l'indique.
- **Exporter CSV** exporte les bons bruts, sans règlements.

### 4.2 État Échéance
Liste les règlements (clients et fournisseurs) et les opérations bancaires avec échéance, groupés par urgence (en retard, aujourd'hui, 7 jours, 30 jours, plus tard).

- Colonnes : `Réf règl. | Date | N° pièce | Date échéance | Client / Fournisseur | À payer | À encaisser | Solde projeté | Date d'encaissement | Statut | Bons lettrés`.
- **Date d'encaissement** :
  - **client** (argent qui rentre) = date d'échéance + jours **ouvrés** selon le mode : **chèque + 2**, **effet (traite, LCN) + 3**, autres modes le jour même ; on saute les samedis, dimanches et jours fériés ;
  - **fournisseur** (argent qui sort) = **le jour même** de l'échéance.
  - Fêtes nationales comptées automatiquement : 1er janvier, 11 et 14 janvier, 1er mai, 30 juillet, 14, 20 et 21 août, 31 octobre, 6 et 18 novembre.
  - Fêtes religieuses (Aïd Al-Fitr, Aïd Al-Adha, 1er Moharram, Aïd Al-Mawlid) : dates prévues, modifiables dans **Tableau de bord → Informations société → Jours fériés religieux**.
- **Solde projeté** = solde actuel de la banque + encaissements − paiements, cumulé dans l'ordre des échéances.

### 4.3 Extrait de compte (justification de solde, comme Sage)
Choix : **Type** (Fournisseur/Client), **Compte** (un compte ou *Tous*), **Situation** :

| Situation | Contenu |
|---|---|
| **Non réglés — justification de solde** *(défaut)* | Éléments qui justifient le solde : **reste à régler sur chaque bon** + **avances / règlements non affectés** |
| **Réglés** | Bons entièrement soldés, avec la lettre du règlement qui les couvre |
| **Tous** | Tous les bons, réglés ou non, avec lettrage |

Justification de solde :
- Colonnes `Date | Pièce | Libellé | Lettre | Débit | Crédit | Solde`.
- Un bon partiellement réglé apparaît pour son **reste**, avec la mention « (réglé X sur Y) ».
- Chaque compte a une ligne **SOLDE JUSTIFIÉ** ; en fin de tableau : **TOTAL GÉNÉRAL ACHAT** ou **VENTE**.
- **Contrôle** : le solde justifié est **égal** au solde du compte dans le Grand livre.
- Les comptes déjà soldés sont masqués, sauf s'ils sont choisis explicitement.

### 4.4 État Global (Grand livre)
- Filtres : **Type** (Tout/Fournisseur/Client), **Fournisseur** et **Client** optionnels, **Date du / Date au**.
- Colonnes : `Date | Pièce | Libellé | Lettre | Débit | Crédit | Solde` (Solde cumulé par compte).
- Organisation : comptes par ordre alphabétique, écritures par date, avec :
  - **Sous-total par mois** (ligne jaune clair, libellé centré),
  - **TOTAL** par compte,
  - **TOTAL GÉNÉRAL ACHAT** et **TOTAL GÉNÉRAL VENTE** **séparés** — jamais de total mélangé.
- Les comptes qui n'ont **que des règlements/avances** (aucun bon) apparaissent aussi.
- Avec **Date du** : les écritures antérieures forment une ligne **Solde reporté** ; le solde reste exact.

### 4.5 État Crédit
Liste les crédits de la Banque (référence, date, créditeur, montant, mode, échéance, statut, réglé), avec totaux (Réglé / Non réglé). Filtres **Fournisseur** ou **Client** = nom du créditeur.

### 4.6 Balance âgée
Ancienneté des impayés, calculée depuis la **date du bon** jusqu'à aujourd'hui, sur le **reste à payer** :

`0-30 jours | 31-60 jours | 61-90 jours | +90 jours | Total dû`

Filtre par Type, et par fournisseur ou client précis.

### 4.7 Balance générale
Mouvements **par mois**, en trois sections : **FOURNISSEURS**, **CLIENTS**, **BANQUE**.

- Filtres : **Section** (Tout / Fournisseurs / Clients / Banque), **Date du / Date au**.
- Colonnes : `Compte | Débit | Crédit | Solde`.
- Pour chaque mois : un titre (ex. *Janvier 2026*), une ligne par compte qui a bougé (la banque = une ligne *Banque*), puis **Sous-total — Janvier 2026** (jaune clair).
- Fin de section : **TOTAL GÉNÉRAL FOURNISSEURS / CLIENTS / BANQUE** (séparés).
- Si **Date du** est renseignée : ligne **Report à nouveau** (solde avant la période) et **SOLDE DE CLÔTURE** en fin de section.
- Convention de solde : Fournisseurs/Clients = **Débit − Crédit** ; Banque = **Crédit − Débit**.
- **Contrôle** : le solde de clôture est égal à la somme des soldes du Grand livre (fournisseurs, clients) et au Solde actuel de la Banque, avec ou sans filtre de dates.

---

## 5. Exemples chiffrés

| Compte | Situation | Ce que montrent les états |
|---|---|---|
| **ESSDRAOUI** | Bon 14 857 DH, règlement 20 000 DH lettré au bon | Bon soldé + **avance de 5 143 DH** en ligne datée ; solde **+5 143** |
| **BOUMARG** | Aucun bon, 2 règlements de 100 000 DH non lettrés | 2 lignes **Avance** datées (lettres A, B) ; solde **+200 000** |
| **ALAMI** | Bon 10 000 (réglé 3 000) + bon 4 000 non réglé | Reste 7 000 + 4 000 ; solde **−11 000** |

Exemple de **Situation actuelle** : solde bancaire 50 000 DH, dette fournisseurs (soldes négatifs) 11 000 DH → **39 000 DH**.

---

## 6. Marge

**Marge = gain des bons = Σ (vente − achat)**, uniquement sur les vrais bons :
- exclus : **reliquats, reports, soldes de début** (case « Reliquat / report / solde de début (hors marge) » sur le bon, ou mot-clé *reliquat*, *report*, *solde de début*, *solde initial*, *à nouveau*, *ouverture* dans le produit, la variété, l'observation ou le N° de bon) ;
- exclus : les bons sans prix de vente ou sans prix d'achat (gain inconnu).

Le Tableau de bord et le Rapport indiquent sous la marge combien de bons ont été exclus.

## 7. Points d'attention

- Le lettrage est **manuel** : un règlement non lié à un bon est traité comme avance, même si des bons restent impayés.
- Les bons sans montant à régler (montant nul) sont ignorés dans les états de règlement.
- Les opérations bancaires **non réglées** ne comptent pas dans le solde bancaire ni dans la Balance générale.
- Les états se rafraîchissent après une modification (renommage d'un fournisseur/client/produit/station, règlement ajouté ou supprimé).
- Les données du compte **démo** sont isolées des données réelles.
