# Convertisseur d’unités

Application web monofichier qui convertit des valeurs entre unités SI, sous-unités, unités usuelles et unités anglo-saxonnes ou régionales. Elle s’exécute entièrement dans le navigateur, sans serveur, sans connexion et sans dépendance.

- **15 grandeurs**, **249 unités**
- **6 langues** : français (par défaut), anglais, allemand, italien, espagnol, suédois
- **3 valeurs** à convertir en parallèle
- Conçue d’abord pour le téléphone, utilisable aussi sur ordinateur
- Résultats limités à 3 décimales
- Application en un seul fichier : `convertisseur-unites.html` (ce `README.md` et sa version anglaise `README.en.md` l’accompagnent ; un lien dans le bandeau du haut y renvoie)

## Démarrage

Ouvrez `convertisseur-unites.html` dans un navigateur (double-clic). Aucune installation, aucun build.

Pour le publier, déposez simplement le fichier sur un serveur web statique, un intranet ou un partage de fichiers.

**Lien vers le README** : le lien « README » du bandeau supérieur, sous le sélecteur de langue, ouvre `README.md` (français) quand la langue de l’interface est le français, et `README.en.md` (anglais) pour toutes les autres langues. Les chemins sont relatifs à la page : le lien fonctionne donc si les deux fichiers README sont déposés **dans le même dossier** que `convertisseur-unites.html`. Dans le cas contraire (page seule, ou hébergée ailleurs), modifiez la constante `README_URL` (voir [Personnalisation](#lien-vers-le-readme)).

## Utilisation

**À l’ouverture**, la page affiche toujours la grandeur **Longueur**, de pouce (`in`) vers centimètre (`cm`), avec les valeurs **1**, **2** et **3** dans les trois champs (résultats : 2,54 ; 5,08 ; 7,62). Seule la langue choisie est conservée d’une visite à l’autre.

1. Choisissez une **grandeur** dans le bandeau du haut (longueur, température, force, etc.).
2. Choisissez l’**unité d’entrée** et l’**unité de sortie** dans les deux cartes. Le bouton central les inverse.
3. Saisissez jusqu’à **trois valeurs** : chaque ligne affiche son résultat en direct.
4. Copiez un résultat avec le bouton de copie de la ligne.

Mise en page :

- **Téléphone** : chaque ligne est une carte, avec la saisie au-dessus et le résultat en dessous.
- **Écran large (700 px et plus)** : la saisie et le résultat sont deux blocs de même largeur et de même hauteur, côte à côte. Les grandeurs passent à la ligne au lieu de défiler.

Autres fonctions :

- **Bouton « +/− »** à gauche de chaque saisie : ajoute ou retire le signe moins (utile sur téléphone, où le clavier numérique n’a pas toujours cette touche).
- **Catalogue des unités** en bas de page : liste toutes les unités de la grandeur avec leur usage, leur région et leur équivalent. Les pastilles « entrée » et « sortie » de chaque unité la sélectionnent directement. Le catalogue est replié par défaut sur petit écran.
- **Clavier** : sur les onglets de grandeur, flèches gauche et droite, Début et Fin.
- **Mode sombre** automatique, selon le réglage du système.
- **Lien « README »** dans le bandeau du haut, sous le sélecteur de langue : ouvre le README dans un nouvel onglet, en français si la langue est le français (libellé « README (FR) »), en anglais sinon (libellé « README (EN) »). L’info-bulle et le nom accessible sont traduits. Sur écran étroit (moins de 480 px), le sélecteur de langue affiche son code (FR, EN, DE…) et le lien affiche « README » seul, pour que le titre garde sa place.

### Saisie des nombres

- Le séparateur décimal peut être `,` ou `.` quelle que soit la langue de l’interface.
- Si les deux apparaissent, le dernier est le séparateur décimal : `1.234,5` et `1,234.5` valent tous deux 1234,5.
- Un séparateur isolé est toujours lu comme **décimal** : `1,234` vaut 1,234 (et non 1234).
- Les espaces (y compris insécables et fines) et les apostrophes sont ignorés : `1 000` vaut 1000.
- La notation scientifique est acceptée : `1e-3`, `2.5e6`.

## Grandeurs et unités

| Grandeur | Unités | Unité de référence (calculs) |
|---|---:|---|
| Longueur | 20 | mètre (m) |
| Température | 8 | degré Celsius (°C) |
| Force | 17 | newton (N) |
| Élongation | 16 | mètre par mètre (m/m) |
| Vitesse | 15 | mètre par seconde (m/s) |
| Pression | 19 | pascal (Pa) |
| Débit | 18 | mètre cube par seconde (m³/s) |
| Puissance | 17 | watt (W) |
| Surface | 17 | mètre carré (m²) |
| Volume | 20 | mètre cube (m³) |
| Angle | 15 | radian (rad) |
| Énergie | 18 | joule (J) |
| Masse | 20 | kilogramme (kg) |
| Accélération | 13 | mètre par seconde carrée (m/s²) |
| Couple | 16 | newton-mètre (N·m) |

Les unités sont regroupées par système ou par région (SI et multiples, États-Unis et Royaume-Uni, marine, Asie, astronomie, etc.). Chaque unité du catalogue indique son usage.

## Fonctionnement des calculs

- **Unités linéaires** : `résultat = valeur × facteur_entrée / facteur_sortie`, où le facteur est la valeur de l’unité dans l’unité de référence du tableau ci-dessus.
- **Température** : la conversion passe par le degré Celsius (fonctions `toC` et `fromC` de chaque échelle). Une valeur inférieure au zéro absolu déclenche un avertissement, mais le calcul est tout de même effectué. Les résidus d’arrondi de l’ordre de 10⁻¹³ sont ramenés à 0 : le zéro absolu saisi dans une échelle donne bien `0` dans une autre, et non `5,684 × 10⁻¹⁴`.
- **Précision de calcul** : arithmétique en virgule flottante double précision (IEEE 754).
- **Affichage des résultats** : **3 décimales au maximum**, sans zéros inutiles (`200` et non `200,000`). Les valeurs inférieures à 0,0005 ou supérieures ou égales à 10¹² passent en notation scientifique, avec 3 décimales au maximum sur la mantisse (par exemple `1 × 10⁻⁹` pour 1 nm en mètres), afin d’éviter d’afficher un faux zéro.
- **Facteurs de référence** : la ligne d’équivalence sous les cartes d’unités (« 1 ft = 0,3048 m ») et la colonne d’équivalents du catalogue conservent toute leur précision (12 chiffres significatifs). Ils ne sont pas limités à 3 décimales.
- **Copie** : la valeur copiée est identique à la valeur affichée (3 décimales au maximum), avec le séparateur décimal de la langue active et sans séparateur de milliers.

### Hypothèses de définition

- **Élongation** : allongement relatif ε = ΔL / L₀, grandeur sans dimension (essais de traction, jauges de contrainte). Ce n’est pas un déplacement exprimé en unité de longueur.
- **Débit** : débit volumique (volume par unité de temps). Le débit massique n’est pas inclus.
- **Calorie** : calorie internationale (4,1868 J). La kilocalorie vaut 4186,8 J.
- **Kilogramme-force** : pesanteur normale g = 9,806 65 m/s².
- **Mach** : vitesse du son dans l’atmosphère standard au niveau de la mer, à 15 °C (340,294 m/s). Elle varie en réalité avec la température.
- **Unités anglo-saxonnes** : définitions internationales exactes (1 in = 25,4 mm, 1 lb = 0,453 592 37 kg, etc.).
- **Unités régionales** (mu, rai, feddan, tola, dunam, etc.) : valeur légale ou conventionnelle actuelle.
- **Tonne d’équivalent TNT** : convention thermochimique de 4,184 GJ.

## Limites

- Outil d’aide au calcul courant. Il n’est pas prévu pour de la métrologie étalonnée ni pour des calculs réglementaires sans vérification indépendante.
- Les résultats sont arrondis à 3 décimales à l’affichage et à la copie. Pour une précision supérieure, convertissez dans une unité plus petite (par exemple des millimètres plutôt que des mètres) ou utilisez les facteurs du catalogue.
- Les traductions et les descriptions d’usage régional ont été rédigées sans relecture par des locuteurs natifs. Le README anglais est une traduction du README français : en cas de divergence, la version française fait foi.
- Aucun test automatisé n’est fourni avec le fichier.

## Langues

Menu en haut à droite : Français, English, Deutsch, Italiano, Español, Svenska. Le français est la langue par défaut, quelle que soit la langue du navigateur.

Sont traduits : l’interface, les messages, les noms des grandeurs et de toutes les unités, les groupes des listes, les descriptions d’usage et la note de bas de page. Le README existe en français (`README.md`) et en anglais (`README.en.md`) : le lien du bandeau ouvre la version française si la langue est le français, la version anglaise pour toutes les autres. Les nombres suivent les conventions de la langue (par exemple 4 050,19 en français, 4,050.19 en anglais, 4.050,19 en allemand). Quelques symboles changent selon la langue (cheval-vapeur : ch, PS, CV ou hk ; année-lumière : al, ly, Lj ou ljusår ; « par jour » : /j, /d ou /g).

## Confidentialité et dépendances

- **Aucune requête réseau** : le fichier ne charge ni script, ni police, ni image, ni feuille de style externes. Les polices sont celles du système.
- **Aucune dépendance** (pas de bibliothèque, pas de gestionnaire de paquets, pas de build).
- **Aucun cookie, aucun suivi, aucune donnée personnelle collectée.**
- **Lien vers le README** : c’est un lien ordinaire. Il n’émet aucune requête tant qu’on ne clique pas dessus, et il s’ouvre dans un nouvel onglet (`rel="noopener"`).
- **Stockage local** : le navigateur mémorise uniquement la **langue** choisie, sous la clé `localStorage` `convertisseur-unites-v1`. La grandeur, les unités et les valeurs saisies ne sont pas conservées : la page s’ouvre toujours dans son état initial. Si le stockage est indisponible, l’application fonctionne normalement sans mémoriser la langue.
- Le code est lisible tel quel : il n’est ni minifié ni obscurci, ce qui facilite une revue de sécurité.

## Structure du code

Tout est dans `convertisseur-unites.html`, dans l’ordre suivant :

| Élément | Rôle |
|---|---|
| `<style>` | Thème clair et sombre (variables CSS), mise en page mobile d’abord |
| `<body>` | Bandeau (titre, langue, lien README, grandeurs), cartes d’unités, lignes de conversion, catalogue |
| `README_URL` | Adresses des deux README (français, anglais) ciblées par le lien du bandeau |
| `QUANTITIES` | Données : grandeurs et unités, avec les facteurs de conversion |
| `defLang(...)` | Une entrée par langue : interface, symboles, noms et usages traduits |
| `convert`, `fmtResult`, `fmtHTML`, `parseValue` | Calcul, affichage des résultats (3 décimales), affichage des facteurs, lecture de la saisie |
| `applyLang`, `setQuantity`, `update` | Rendu et mise à jour de l’interface |

## Personnalisation

### Ajouter une unité

Dans `QUANTITIES`, ajoutez une ligne dans la grandeur voulue :

```js
U("id", "nom", "symbole", "Groupe", "Région. Usage", facteur)
```

- `id` : identifiant unique dans la grandeur.
- `facteur` : valeur de l’unité dans l’unité de référence (voir le tableau des grandeurs).
- Les unités d’un même groupe doivent rester **contiguës**.
- Pour la température, utilisez `T(id, nom, symbole, groupe, usage, toC, fromC, formule)`.

**Traductions** : dans chaque langue, `names` et `uses` sont des listes séparées par `|`, **alignées par position** sur les unités de la grandeur. Insérez la traduction à la même position dans chaque langue, sinon les libellés seront décalés. Les groupes (`groups`) suivent l’ordre d’apparition des groupes.

### Ajouter une grandeur

Ajoutez un objet à `QUANTITIES` :

```js
{ key: "ma_grandeur", label: "Ma grandeur", base: "unité de référence", from: "id-entrée", to: "id-sortie",
  desc: "Description affichée sous les onglets.", units: [ /* U(...) */ ] }
```

Puis ajoutez une entrée `ma_grandeur: { label, desc, groups, names, uses }` dans chaque `defLang` (sinon l’affichage retombe sur le français).

### Ajouter une langue

1. Copiez un bloc `defLang("xx", { ... })` existant et traduisez `name`, `locale` (code BCP 47, par exemple `"pt-PT"`), `ui`, `sym` et `q`.
2. Ajoutez `<option value="xx">Nom</option>` dans le menu `<select id="lang">`.
3. Vérifiez que, pour chaque grandeur, `names` et `uses` contiennent autant d’éléments que d’unités, et `groups` autant que de groupes.
4. Les clés manquantes de `ui` (y compris `readme`, l’info-bulle du lien) retombent sur le français. Toute langue autre que le français ouvre le README anglais.

Les symboles propres à une langue se déclarent dans `sym`, avec la clé `"grandeur.id"` (par exemple `"puissance.ch": "PS"`).

### Lien vers le README

Le lien du bandeau est piloté par la constante `README_URL`, au début du script :

```js
const README_URL = { fr: "README.md", en: "README.en.md" };
```

`fr` est utilisé quand la langue de l’interface est le français, `en` pour toutes les autres langues. Pour pointer ailleurs, remplacez-les par des adresses complètes (par exemple celles du dépôt ou de l’intranet) :

```js
const README_URL = {
  fr: "https://exemple.intranet/docs/convertisseur/README.md",
  en: "https://exemple.intranet/docs/convertisseur/README.en.md"
};
```

Le texte visible est `README` suivi de `(FR)` ou `(EN)` : le mot figure dans le HTML, sur la balise `<a id="readme">`, et le suffixe est calculé par le script à chaque changement de langue. L’info-bulle et le nom accessible viennent de la clé `readme` de `ui`, à adapter dans chaque langue.

### Changer les unités par défaut

Pour chaque grandeur, `from` et `to` désignent l’unité d’entrée et de sortie proposées à l’ouverture de la page et à chaque changement de grandeur (par exemple `from: "in", to: "cm"` pour la longueur).

La grandeur affichée à l’ouverture est la première de la liste `QUANTITIES` (Longueur). Les valeurs initiales 1, 2 et 3 sont fixées à la création des trois lignes de saisie, dans la ligne `r.input.value = String(i + 1);` du script.

## Compatibilité

Navigateurs récents sur ordinateur, iOS et Android. L’application utilise notamment `Intl.NumberFormat`, les grilles CSS, `inset` et `mask-image`. Sans JavaScript, la page ne fonctionne pas.

## Licence

À définir par le propriétaire du projet.
