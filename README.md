[README.md](https://github.com/user-attachments/files/34272969/README.md)
# Convertisseur d’unités

Application web monofichier qui convertit des valeurs entre unités SI, sous-unités, unités usuelles et unités anglo-saxonnes ou régionales. Elle s’exécute entièrement dans le navigateur, sans serveur, sans connexion et sans dépendance.

- **15 grandeurs**, **249 unités**
- **6 langues** : français (par défaut), anglais, allemand, italien, espagnol, suédois
- **3 valeurs** à convertir en parallèle
- Conçue d’abord pour le téléphone, utilisable aussi sur ordinateur
- Un seul fichier : `convertisseur-unites.html`

## Démarrage

Ouvrez `convertisseur-unites.html` dans un navigateur (double-clic). Aucune installation, aucun build.

Pour le publier, déposez simplement le fichier sur un serveur web statique, un intranet ou un partage de fichiers.

## Utilisation

1. Choisissez une **grandeur** dans le bandeau du haut (longueur, température, force, etc.).
2. Choisissez l’**unité d’entrée** et l’**unité de sortie** dans les deux cartes. Le bouton central les inverse.
3. Saisissez jusqu’à **trois valeurs** : chaque ligne affiche son résultat en direct.
4. Copiez un résultat avec le bouton de copie de la ligne.

Autres fonctions :

- **Bouton « +/− »** à gauche de chaque saisie : ajoute ou retire le signe moins (utile sur téléphone, où le clavier numérique n’a pas toujours cette touche).
- **Catalogue des unités** en bas de page : liste toutes les unités de la grandeur avec leur usage, leur région et leur équivalent. Les pastilles « entrée » et « sortie » de chaque unité la sélectionnent directement. Le catalogue est replié par défaut sur petit écran.
- **Clavier** : sur les onglets de grandeur, flèches gauche et droite, Début et Fin.
- **Mode sombre** automatique, selon le réglage du système.

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
- **Température** : la conversion passe par le degré Celsius (fonctions `toC` et `fromC` de chaque échelle). Une valeur inférieure au zéro absolu déclenche un avertissement, mais le calcul est tout de même effectué.
- **Précision** : arithmétique en virgule flottante double précision (IEEE 754). Les résultats sont affichés avec **12 chiffres significatifs** au maximum, en notation scientifique en dehors de l’intervalle [10⁻⁴ ; 10¹²[.
- **Copie** : la valeur copiée a 12 chiffres significatifs, le séparateur décimal de la langue active et aucun séparateur de milliers.

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
- La précision est celle de la virgule flottante double précision ; ne comptez pas sur plus de 15 chiffres.
- Les traductions et les descriptions d’usage régional ont été rédigées sans relecture par des locuteurs natifs.
- Aucun test automatisé n’est fourni avec le fichier.

## Langues

Menu en haut à droite : Français, English, Deutsch, Italiano, Español, Svenska. Le français est la langue par défaut, quelle que soit la langue du navigateur.

Sont traduits : l’interface, les messages, les noms des grandeurs et de toutes les unités, les groupes des listes, les descriptions d’usage et la note de bas de page. Les nombres suivent les conventions de la langue (par exemple 4 050,19 en français, 4,050.19 en anglais, 4.050,19 en allemand). Quelques symboles changent selon la langue (cheval-vapeur : ch, PS, CV ou hk ; année-lumière : al, ly, Lj ou ljusår ; « par jour » : /j, /d ou /g).

## Confidentialité et dépendances

- **Aucune requête réseau** : le fichier ne charge ni script, ni police, ni image, ni feuille de style externes. Les polices sont celles du système.
- **Aucune dépendance** (pas de bibliothèque, pas de gestionnaire de paquets, pas de build).
- **Aucun cookie, aucun suivi, aucune donnée personnelle collectée.**
- **Stockage local** : le navigateur mémorise la langue, la grandeur active et les unités choisies pour chaque grandeur, sous la clé `localStorage` `convertisseur-unites-v1`. Les valeurs saisies ne sont pas conservées. Si le stockage est indisponible, l’application fonctionne normalement sans mémoriser.
- Le code est lisible tel quel : il n’est ni minifié ni obscurci, ce qui facilite une revue de sécurité.

## Structure du code

Tout est dans `convertisseur-unites.html`, dans l’ordre suivant :

| Élément | Rôle |
|---|---|
| `<style>` | Thème clair et sombre (variables CSS), mise en page mobile d’abord |
| `<body>` | Bandeau (titre, langue, grandeurs), cartes d’unités, lignes de conversion, catalogue |
| `QUANTITIES` | Données : grandeurs et unités, avec les facteurs de conversion |
| `defLang(...)` | Une entrée par langue : interface, symboles, noms et usages traduits |
| `convert`, `fmtHTML`, `parseValue` | Calcul, affichage des nombres, lecture de la saisie |
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

Les symboles propres à une langue se déclarent dans `sym`, avec la clé `"grandeur.id"` (par exemple `"puissance.ch": "PS"`).

### Changer les unités par défaut

Pour chaque grandeur, `from` et `to` désignent l’unité d’entrée et de sortie proposées au premier lancement.

## Compatibilité

Navigateurs récents sur ordinateur, iOS et Android. L’application utilise notamment `Intl.NumberFormat`, les grilles CSS, `inset` et `mask-image`. Sans JavaScript, la page ne fonctionne pas.

## Licence

À définir par le propriétaire du projet.
