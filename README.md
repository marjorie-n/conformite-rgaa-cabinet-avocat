# Mise en conformité RGAA — Cabinet d'avocate

Étude de cas : reprise de l'accessibilité d'un site WordPress existant, de
l'évaluation des 106 critères du RGAA 4.1.2 jusqu'à la publication d'une
déclaration d'accessibilité.

**Cliente** : [Maître Emily Comyn](https://emilycomynavocat.fr), avocate au
barreau de Toulon (droit pénal, droit de la famille, droit des mineurs).
Site présenté avec son accord.

**Rôle** : évaluation, corrections et rédaction de la déclaration —
*Marjorie Ngoupende, conceptrice web indépendante*. J'avais conçu ce site en
2025 ; cette mission en est la suite, proposée à titre gracieux.

**Période** : novembre 2026

**Résultat** : 100 % des critères applicables respectés (57 conformes,
0 non conforme, 49 non applicables) — mention « totalement conforme ».

> *Périmètre : auto-évaluation réalisée par la conceptrice du site, sur
> l'ensemble des pages (accueil, mentions légales, déclaration
> d'accessibilité). Il ne s'agit pas d'un audit externe. La méthode et ses
> limites sont détaillées plus bas.*

---

## Le contexte

Le site avait été pensé dès le départ pour être accessible : structure de
titres, textes alternatifs, lien d'évitement, contrastes. Mais rien ne le
disait aux visiteurs, et rien ne le prouvait. Le pied de page affichait
« Accessibilité : le site a été conçu avec soin », avec un lien vers les
mentions légales, qui évoquaient un « score élevé » obtenu avec un outil
automatique.

Pour une avocate, l'écart entre une promesse et une preuve compte. L'objectif
était donc double : vérifier réellement le site critère par critère, et
remplacer la formule vague par une déclaration d'accessibilité conforme au
modèle officiel, avec un état de conformité mesuré.

Le site d'un cabinet individuel n'est pas soumis à l'obligation du RGAA ni à
celle de l'Acte européen sur l'accessibilité. La démarche est volontaire : les
justiciables qui cherchent un avocat pénaliste sont parfois dans l'urgence, et
parfois en situation de handicap.

## La méthode

1. **Captures avant** des pages et des zones concernées.
2. **Inventaire automatisé** dans le navigateur : images et alternatives,
   titres, zones de la page, liens, champs de formulaire, contrastes.
3. **Grille officielle** des 106 critères, issue du dépôt de la DINUM,
   remplie critère par critère.
4. **Corrections** dans WordPress, sans modifier le thème.
5. **Contrôle** : navigation complète au clavier, affichage à 320 px et à
   200 %, espacement du texte augmenté, validateur HTML du W3C sur le code
   généré, mesure des contrastes sur la photo d'accueil.
6. **Déclaration d'accessibilité** publiée, lien dédié dans le pied de page.

La grille complète, avec un constat pour chaque critère, est disponible dans
ce dépôt : [grille RGAA du site (fichier Excel)](grille-rgaa.xlsx).

## Ce que l'évaluation a révélé

Le premier passage a trouvé trois non-conformités franches. La suite de
l'évaluation en a fait apparaître d'autres, moins visibles, qu'un outil
automatique n'aurait pas signalées.

### Des boutons « Lire plus » inutilisables au clavier

Les textes des domaines de compétences sont tronqués, avec un « Lire plus »
souligné pour les déplier. Ce « Lire plus » était un simple `<span>` avec un
clic JavaScript : impossible à atteindre avec la touche Tab, et annoncé comme
du texte par les lecteurs d'écran.

Il est devenu un vrai bouton, qui indique s'il est ouvert ou fermé et précise
à quel domaine il se rapporte :

```html
<button type="button" class="kadence-read-more"
        aria-expanded="false" aria-controls="ec-texte-domaine-1">
  <span class="ec-libelle">Lire plus</span>
  <span class="screen-reader-text"> : Défense pénale — Toulon, Draguignan, Grasse (PACA)</span>
</button>
```

![Domaine « Défense pénale » sur mobile, déplié avec la touche Entrée : le texte complet s'affiche](images/apres-lire-plus-clavier.jpg)

### Un texte blanc trop peu contrasté sur la photo d'accueil

Le titre et l'accroche de la page d'accueil sont posés sur une photo
d'ordinateur portable gris clair, sous un voile sombre à 30 %. Mesuré pixel
par pixel derrière le texte, le contraste de l'accroche tombait à 3,5:1, sous
le seuil de 4,5:1.

Le voile est passé à 55 %. Le contraste remonte au-dessus de 5,3:1, sur
ordinateur comme sur mobile, sans changer la photo.

| Avant | Après |
|---|---|
| ![Page d'accueil sur mobile, avant : texte blanc « Emily Comyn, avocat à La Garde et Toulon » sur une photo de balance de la justice, voile léger](images/avant-accueil.jpg) | ![Page d'accueil sur mobile, après : même composition, photo plus sombre derrière le texte blanc](images/apres-accueil.jpg) |

### Un formulaire de contact à compléter

Le formulaire était déjà bien étiqueté, mais plusieurs détails manquaient :

- la signification de l'astérisque n'était expliquée nulle part ;
- l'astérisque rouge n'atteignait que 3,7:1 sur le fond beige ;
- les champs, blancs sur fond beige et sans bordure, ne se distinguaient pas ;
- les deux boutons radio « Téléphone » et « Email » n'étaient pas reliés à
  leur question, « Préférence de contact » ;
- le navigateur ne pouvait pas proposer de remplir automatiquement le nom,
  l'e-mail et le téléphone ;
- le message de confirmation après envoi n'était pas annoncé aux lecteurs
  d'écran.

![Section « Contact et Accès » après correction : le formulaire « Première consultation offerte » commence par la phrase « Les champs marqués d'un astérisque (*) sont obligatoires », et chaque champ a un contour gris visible sur le fond beige](images/apres-formulaire.jpg)

### Un focus clavier pas toujours visible

Le thème affichait un contour de focus sur certains éléments seulement. Un
anneau double, blanc puis foncé, s'affiche désormais sur tous les éléments
interactifs : il reste visible sur le bandeau sombre comme sur le fond beige
du formulaire.

```css
a:focus-visible, button:focus-visible, input:focus-visible,
textarea:focus-visible, select:focus-visible, [tabindex]:focus-visible {
  outline: 2px solid transparent !important; /* conservé en mode contraste élevé */
  box-shadow: 0 0 0 2px #fff, 0 0 0 4px #1a202c !important;
}
```

![Champ « Téléphone » du formulaire avec le focus clavier : anneau blanc et foncé autour du champ](images/apres-focus-clavier.jpg)

### Des détails de structure et de code

- **Titres** : le nom « Robert BADINTER » était un `<h5>` isolé et sa
  citation un `<h3>`, ce qui faussait le plan de la page. Les deux sont
  devenus des paragraphes, et la citation est balisée avec `<q>`. Leur
  typographie est conservée en CSS.
- **Pictogrammes** : les six icônes des domaines de compétences sont
  décoratives. Trois portaient une alternative du type « Avocat pénaliste à
  Toulon », lue pour rien par les lecteurs d'écran. Elles ont désormais une
  alternative vide.
- **Bouton d'appel sur mobile** : il était annoncé « Call Now Button », en
  anglais. Il est maintenant annoncé « Appeler le cabinet au 06 47 16 34 09 ».
- **Espacements** : deux paragraphes vides servaient à créer de l'espace.
  Ils sont remplacés par des blocs d'espacement.
- **Code** : le validateur du W3C relevait une vingtaine d'erreurs HTML sur la
  page d'accueil, dont des `<div>` dans des `<span>` et une étiquette reliée à
  un élément qui n'est pas un champ. Le code généré passe maintenant sans erreur.
- **Pied de page** : il contenait un bloc HTML vide, collé par une extension
  de traduction du navigateur. Il a été supprimé.

### Des corrections sans toucher au thème

Le site repose sur Kadence et ses blocs. Plutôt que de créer un thème enfant
pour une poignée de corrections, tout passe par deux emplacements de
l'extension WPCode : une feuille de style dans l'en-tête, et un script dans le
pied de page. Chaque règle est commentée avec le critère RGAA qu'elle corrige,
ce qui permet de la retrouver, de la retirer ou de la reprendre facilement.

## La déclaration d'accessibilité

Le pied de page renvoie maintenant vers une page dédiée, avec l'état de
conformité affiché dans le lien lui-même, comme le prévoit le RGAA.

| Avant | Après |
|---|---|
| ![Pied de page avant : « Accessibilité : le site a été conçu avec soin. »](images/avant-pied-de-page.jpg) | ![Pied de page après : « Accessibilité : totalement conforme »](images/apres-pied-de-page.jpg) |

La page suit le modèle officiel : état de conformité, résultats des tests,
contenus non soumis à l'obligation, méthode, contact et voies de recours.

![Page « Déclaration d'accessibilité » : titre, engagement de Maître Comyn, section « État de conformité » indiquant « totalement conforme »](images/apres-declaration.jpg)

La section des mentions légales a été réécrite pour renvoyer vers cette
déclaration, au lieu d'un texte qui ne pouvait pas être vérifié.

| Avant | Après |
|---|---|
| ![Mentions légales avant : section « ACCESIBILITÉ » évoquant un score élevé obtenu avec un outil d'analyse automatique](images/avant-mentions-legales.jpg) | ![Mentions légales après : section « ACCESSIBILITÉ » indiquant que le site est totalement conforme au RGAA 4.1.2, avec un lien vers la déclaration](images/apres-mentions-legales.jpg) |

## Limites de l'évaluation

- **Auto-évaluation** : elle a été réalisée par la conceptrice du site, pas
  par un auditeur indépendant.
- **Environnement** : navigateur Chrome sur ordinateur et affichage mobile
  simulé. La restitution a été vérifiée à partir de l'arbre d'accessibilité
  du navigateur ; un test avec NVDA et VoiceOver reste à faire pour la
  confirmer.
- **Carte Google Maps** : c'est un service tiers, non soumis à l'obligation,
  à condition que l'adresse soit donnée en texte. C'est le cas.
- **Durée de validité** : le résultat vaut pour le site tel qu'il est en
  novembre 2026. Une nouvelle page ou un nouveau bloc demande une nouvelle
  vérification.

## Ce que je retiens

**Une formule rassurante n'est pas une preuve.** « Conçu avec soin » ne dit
rien à la personne qui n'arrive pas à remplir un formulaire. Une déclaration
dit ce qui a été testé, comment, et à qui écrire si quelque chose bloque.

**Les outils automatiques passent à côté de l'essentiel.** Ici, ni le faux
bouton « Lire plus », ni le contraste du texte sur la photo, ni les boutons
radio non regroupés n'auraient été signalés par un scan. Ils se sont vus au
clavier et en mesurant.

**Un site accessible à sa création le reste rarement seul.** Un an après la
mise en ligne, des ajouts successifs (un script, une extension, un bloc collé)
avaient fait apparaître de nouveaux obstacles. L'accessibilité se maintient.

---

## Outils

WordPress · Kadence Blocks · WPCode · grille RGAA 4.1.2 (DINUM) · validateur
HTML du W3C · inspection de l'arbre d'accessibilité · mesure des contrastes

## Liens

- Site : [emilycomynavocat.fr](https://emilycomynavocat.fr)
- Déclaration d'accessibilité :
  [emilycomynavocat.fr/accessibilite](https://emilycomynavocat.fr/accessibilite/)
- Référentiel : [RGAA 4.1.2 sur le site de la DINUM](https://accessibilite.numerique.gouv.fr/)
