# Passation : site « Clés de détermination » pour enseignants

Ce document résume le projet « Qui vit ici ? » mené avec Jean, enseignant de SVT en collège (sortie 6e, Bagnols-sur-Cèze). Il sert de point de départ à une nouvelle discussion qui construira un **site web pour d'autres enseignants**. Ce site permettra de configurer, puis de générer, leur propre livret papier et leur propre version interactive.

---

## 1. Ce qui existe déjà

### Livret papier « Qui vit ici ? »

- Format A5, 76 pages, en noir et blanc, page de 559 × 794 px.
- Trois milieux (« biomes ») :
  - Forêt de l'Ancyse ;
  - Ripisylve ;
  - Rivière (la Cèze).
- **Pages 1 à 5 :**
  - p. 1 : couverture ;
  - p. 2 : « Comment utiliser ce carnet », avec un exemple de chemin dans la clé (balises 7a, 8b, 9b et 10b, qui mènent au lierre) ;
  - p. 3 : « Deux noms pour chaque être vivant » et « D'où viennent ces noms ? » (étymologie) ;
  - p. 4 : le paysage ;
  - p. 5 : règles de l'observateur et sommaire.
- **Forêt :**
  - p. 6 : « Clé de la forêt », avec les questions de départ ;
  - p. 6 à 23 : végétaux (35 espèces, dont 3 mousses) ;
  - p. 24 à 27 : champignons et lichens (« Ni plante, ni animal ») ;
  - p. 28 à 37 : animaux (30) ;
  - p. 38 à 43 : traces (14).
- **Ripisylve :**
  - p. 44 : « Clé de la ripisylve » ;
  - végétaux (24) jusqu'à la p. 55 ;
  - p. 56 à 59 : animaux (8) ;
  - p. 60 et 61 : traces (6).
- **Rivière :**
  - p. 62 : « Clé de la rivière » ;
  - végétaux (8) jusqu'à la p. 65 ;
  - p. 66 à 71 : petites bêtes (18) ;
  - p. 72 et 73 : traces (6).
- **Fin du livret :**
  - p. 74 : qualité de l'eau ;
  - p. 75 : mots à connaître ;
  - p. 76 : à ne pas toucher (datura, hellébore, chenilles processionnaires, abeilles, guêpes, frelon).

### Version interactive (un seul fichier HTML, environ 18 Mo, fonctionne hors ligne)

- À gauche : un sommaire avec recherche.
- Au centre : le livret qui se feuillette (bibliothèque page-flip 2.0.7, intégrée au fichier).
- À droite : une **clé interactive** au look papier.
  - Choix du milieu, puis du type (végétaux, champignons et lichens, animaux, traces).
  - Une question à la fois ; les réponses déjà données restent visibles.
  - À la fin : une fiche résultat avec le dessin et un bouton « Voir la planche p. X ».
  - Les questions de départ (« Clé des traces p. 38 ») basculent vers la bonne clé.
  - Les espèces « en ligne seulement » affichent la mention « seulement dans la version en ligne ».

### PDF à imprimer

- **A5** : une page par feuille.
- **A4 livret** : 4 pages A5 par feuille A4, recto verso. On plie les feuilles, on les emboîte et on agrafe au milieu.
- **A3 livret** : 4 pages au format A4 par feuille A3, recto verso.

### Artifacts claude.ai (URL stables)

- Design du livret, page par page : https://claude.ai/artifact/WVxBreKqGcrMxdQNcEyEY7. Les fichiers sont dans `project/*.dc.html` et `project/canvas.json` ; les images sont des assets `/_blob/<id>`.
- Flipbook partageable : https://claude.ai/artifact/FQipDaFGKVGronrz6J7Wtr
- Livret oiseaux (projet voisin, « Qui vole ici ? », 16 p.) : https://claude.ai/artifact/RBGraiyD7Qq2ydhRhL8iAJ

### Fichiers sur l'ordinateur de Jean

Dossier `H:\_DEV\_projets\DEV_CleDetSortie` :

- les PDF et la version interactive ;
- toutes les illustrations d'origine ;
- les fichiers de prompts ;
- le dossier `_handoff_site/`, qui contient ce dossier de passation.

---

## 2. Contenu du dossier `_handoff_site/`

| Élément | Contenu |
|---|---|
| `HANDOFF.md` | Ce document. |
| `livret_db.json` | **La base de données complète**, voir la section 3. Elle est aussi enregistrée dans le Project claude.ai. |
| `handoff_images.zip` → `images/` | 159 illustrations JPG de 1024 px, nommées `<biome>-<type>-<nom>.jpg`. Leur chemin est indiqué dans `livret_db.json`. |
| `handoff_data.zip` → `livret_pages/` | Les 76 pages du livret actuel au format `.dc.html` (HTML avec styles inline), plus `canvas.json` et `blob_map.json` (correspondance entre un identifiant `/_blob/…` et un fichier image). C'est la **référence visuelle exacte** de chaque type de page. |
| `handoff_data.zip` → `scripts/` | Les scripts Python et Node utilisés pour générer le livret, voir la section 5. |
| `handoff_data.zip` → `prompts/` | Les 3 fichiers de prompts d'illustrations pour Claude dans Chrome et Gemini. |

---

## 3. Modèle de données (`livret_db.json`)

```json
{
  "title": "Qui vit ici ?", "subtitle": "...", "level": "6e",
  "biomes": [
    { "id": "foret", "name": "Forêt de l'Ancyse", "tab": "Forêt",
      "keys": [
        { "type": "vegetaux" | "npa" | "animaux" | "traces",
          "label": "Végétaux", "title": "Clé de la forêt", "intro": "Pars de la balise 1 : ...",
          "root": 1,
          "couplets": {
            "1": [ ["Un être vivant, ou un morceau ...", "go", 2],
                   ["Une trace : empreinte, crotte ...", "ext", "foret-traces", "Clé des traces", 38] ],
            "8": [ ["Tiges couvertes de petits bouquets ...", "sp", 4], ["Tiges avec de vraies feuilles", "go", 9] ]
          },
          "species": {
            "4": { "id": "foret-vegetaux-asperge-sauvage", "name": "Asperge sauvage",
                   "lat": "Asparagus acutifolius", "cap": "Bouquets d'aiguilles piquantes",
                   "image": "images/foret-vegetaux-asperge-sauvage.jpg",
                   "print": true, "online": true, "print_page": 9 }
          } } ] } ]
}
```

- Chaque **couplet** (une « balise ») contient 2 options, a et b. Une option est de l'un de ces trois types :
  - `go` : elle renvoie vers un autre couplet ;
  - `sp` : elle donne un résultat, c'est-à-dire une espèce ;
  - `ext` : elle renvoie vers une autre clé (questions de départ). Ses champs sont : clé cible, libellé, page dans le livret actuel.
- Les clés de la base sont les **versions complètes, en ligne**. 3 espèces y ont `print: false` : le barbon pied-de-poule, la galle en corne du térébinthe et le tronc rongé par le castor. La version papier s'obtient en les retirant, avec l'algorithme de la section 4.4.
- Les questions de départ (« vivant ou trace ? », « yeux et bouche ? », « tige et feuilles ? ») sont les premiers couplets de la clé `vegetaux` de chaque milieu.
- Les numéros de page `print_page` et les pages des options `ext` correspondent au livret actuel. Ils doivent être **recalculés** à chaque génération.

---

## 4. Règles de mise en page et de pédagogie (validées par Jean)

### 4.1 Règles de fond

- **Les élèves n'écrivent pas dans le livret.** C'est une ressource : pas de page « mes observations », pas de crayon dans la liste du matériel.
- Langage simple pour des élèves de 11 ans. Chaque espèce a un nom vulgaire, un nom latin en italique (genre avec une majuscule, puis espèce) et une légende courte qui donne un critère visible.
- Les plantes toxiques ou les animaux qui piquent sont signalés sur la page « À ne pas toucher » et dans les règles.
- Les mousses sont des végétaux, donc dans la clé des végétaux. Les champignons et lichens forment la section « Ni plante, ni animal ».

### 4.2 Pages

- Page A5 de 559 × 794 px.
  - Les pages paires ont leurs onglets de milieu à gauche ; les pages impaires, à droite.
  - Les onglets (Forêt, Ripisylve, Rivière) sont verticaux ; celui du milieu en cours est noir.
- Polices (Google Fonts) :
  - **Fraunces** pour les titres et les noms latins ;
  - **Atkinson Hyperlegible** pour le texte.
- Couleurs : encre `#1B1B18`, gris `#55554F`, pointillés `#8A8A83`, fond des onglets inactifs `#ECEAE3`, fond des renvois `#F1F0EA`.
- **Double page = clé à gauche (page paire), planche à droite (page impaire).** Une section commence toujours sur une page paire.
- En-tête d'une page de clé :
  - en italique, le nom du milieu, suivi de pastilles de type (Végétaux, Animaux, Traces) ; la pastille du type en cours est noire ;
  - puis le titre (h1 sur la première page, « (suite) » sur les suivantes) ;
  - puis une phrase d'introduction.
- Une **balise** se compose d'un carré numéroté et d'un trait pointillé vertical qui la relie à la suivante. Ses options a et b se terminent par :
  - « va à [N] », suivi de « p. X » si la balise N est sur une autre page ;
  - ou une pastille ronde « (N) Nom », suivie de « p. X » si la planche n'est pas en face ;
  - ou un renvoi encadré « Clé des traces p. 38 » (fond `#F1F0EA`).
- Une **planche** est une grille de cartes. Chaque carte a un numéro dans un rond noir, le dessin (`object-fit: contain`), le nom en gras, le nom latin en italique et une légende grise.
  - Au maximum **4 cartes par page pour les végétaux** (2 × 2), et au maximum 6 pour les animaux et les traces (2 × 3).
  - Champignons et lichens : 4 au maximum.
- Pied de page : le numéro de page, et « Planche : page X » ou « Clé : page X ».

### 4.3 Placement des dessins en face des résultats

But : le dessin de chaque résultat est sur la planche **en face** de la page de clé où il apparaît. Aujourd'hui, 141 résultats sur 143 respectent cette règle.

1. Mesurer la hauteur réelle de chaque balise : rendu dans Chromium sur une largeur de 467 px, sans les « p. X ».
2. Programmation dynamique sur le découpage en pages :
   - hauteur utile = 702 px − en-tête (124 px sur la première page, 64 px sur les suivantes) − 24 px pour la ligne « La clé continue page X » − 8 px de marge ;
   - pénalité de 10 par résultat au-delà de la capacité de la planche ;
   - pénalité de 30 pour une planche vide ;
   - petite pénalité pour une page trop vide.
3. Les espèces en trop vont sur la planche libre la plus proche.
4. **Renuméroter les espèces dans l'ordre des planches.** Les numéros des pastilles et ceux des cartes restent cohérents.
5. Toujours vérifier qu'aucune page ne déborde (rendu Playwright avec détection d'overflow).

Le code est dans `scripts/gen11.py` (fonctions `meas_heights` et `optimise`).

### 4.4 Retirer une espèce d'une clé (cases à cocher)

1. Trouver le couplet c dont une option mène à l'espèce retirée.
2. Remplacer, partout, les renvois vers c par l'autre option de c.
3. Supprimer c.
4. Renuméroter les couplets en ordre préfixe (option a d'abord, à partir de la racine).
5. Renuméroter les espèces.

Le code est dans `scripts/gen10.py` (fonctions `remove_species` et `number`).

Si toutes les espèces d'une branche sont retirées, il faut répéter l'opération. Si une clé devient vide, il faut retirer la section et les renvois `ext` qui pointaient vers elle.

### 4.5 Nombre de pages et impression

- Le nombre total de pages doit être un **multiple de 4** (impression en livret).
- Imposition : pour la feuille k (en partant de 0) d'un livret de N pages, le recto porte les pages N−2k et 2k+1, le verso les pages 2k+2 et N−2k−1. Chaque page est mise à l'échelle dans une moitié de la feuille paysage :
  - A4 paysage : 841,89 × 595,28 pt ;
  - A3 paysage : 1190,55 × 841,89 pt.
- Script : `scripts/impose.py` (pypdf).
- Les images sont redimensionnées à 640 px (JPEG qualité 82) pour que le PDF reste sous 20 Mo environ.

### 4.6 Renvois de pages

Dès que la pagination change, il faut recalculer :

- le sommaire (p. 5) ;
- les renvois `ext` des questions de départ ;
- les « va à N p. X » ;
- les « La clé continue page X » ;
- les pieds de page ;
- les titres du sommaire de la version interactive ;
- les raccourcis du flipbook.

---

## 5. Chaîne de génération actuelle (scripts)

1. Pages `.dc.html` générées par `gen.py` : `build_section(tpl, key_pages, pl_pages, species, couplets, key_split, pl_split, titre_clé, titre_planche)`. Les fonctions `couplet_html`, `card_html`, `opt_html`, `page_skeleton` et `pagenum` produisent le HTML exact.
2. `build.py` assemble les 76 pages dans un seul `livret.html`. Il remplace les `/_blob/<id>` par des images locales en base64 (640 px), grâce à la correspondance image → fichier.
3. `render.js` (Playwright, Chromium) produit `livret.pdf` et une image par page (`pNN.jpg`), et signale les pages qui débordent.
4. `impose.py` produit le livret A4 et le livret A3.
5. `build_keys.py` construit `keys_data.json` (les clés pour la version interactive, avec des vignettes de 420 px en base64).
6. `build_flipexport.py` et `flip_export_tpl.html` produisent le HTML interactif autonome. Ils y intègrent les pages en JPG (900 px), le sommaire, la recherche, la clé interactive et page-flip.

Pour le site, l'idéal est de **refaire cette chaîne en JavaScript côté navigateur ou serveur**, à partir de `livret_db.json` et des modèles HTML de `livret_pages/`, et non de repartir des scripts Python, qui servent de référence.

---

## 6. Illustrations

- Style imposé (à reprendre pour toute nouvelle espèce) : *illustration naturaliste en noir et blanc façon gravure ancienne (taille-douce), traits fins à l'encre et hachures croisées, aucune couleur, fond blanc pur, sujet centré et isolé, format carré, lisible une fois imprimé en petit (5 cm de large) dans un livret A5 pour des élèves de 11 ans. Aucun texte, aucune légende, aucune signature.*
- Chaque prompt décrit les critères de la clé (forme des feuilles, fruits, etc.) et souvent un **encart** : un détail agrandi, ou la silhouette de l'arbre en trait léger.
- Méthode de production :
  1. Claude dans Chrome ouvre Gemini (Nano Banana) dans une nouvelle conversation par image.
  2. Il colle le prompt, vérifie que l'image est en noir et blanc et sans texte, puis télécharge l'image dans `DEV_CleDetSortie` sous le nom indiqué.
  3. Ensuite, on éclaircit le fond si besoin pour obtenir un blanc pur, et on recadre les cadres parasites.
- Les modèles de prompts sont dans `prompts/`.

---

## 7. Cahier des charges du nouveau site (demande de Jean)

Un site interactif pour **d'autres enseignants**, qui contient :

1. **Une page d'administration** pour :
   - choisir le nom des milieux (biomes) ;
   - pour chaque espèce, cocher ou décocher « dans le livret papier » et « dans la version interactive » ;
   - voir l'effet sur le nombre de pages (multiple de 4).
2. **Une page de création** de nouvelles espèces ou de nouveaux milieux :
   - fiche espèce : nom, nom latin, légende, image (envoi d'un fichier), milieu, type ;
   - **insertion dans la clé** : choisir la balise où brancher la nouvelle espèce, et écrire la question a/b qui la distingue de sa voisine ;
   - aide aux prompts d'illustration : générer automatiquement le prompt dans le style imposé, à partir de la description.
3. **La génération pour cet enseignant :**
   - la page HTML interactive (feuilletage, sommaire avec recherche, clé cliquable) ;
   - les PDF à imprimer : A5, livret A4 et livret A3 recto verso, avec les règles de la section 4.

Points à décider dans la nouvelle discussion :

- **Hébergement.** Jean utilise GitHub (jeancazes), Vercel et Hostinger. Options : un site statique avec génération dans le navigateur (pdf-lib, et html2canvas ou rendu vectoriel), ou un serveur avec Playwright pour des PDF de qualité identique.
- **Stockage des configurations par enseignant** : comptes, base de données (Supabase par exemple), ou simple export/import JSON.
- Les comptes enseignants et le partage des espèces créées entre enseignants.

---

## 8. Préférences de Jean

- Il communique en français.
- Il veut des livrets imprimables (A4 ou A3 recto verso), un look papier soigné, et des dessins assez grands pour reconnaître les espèces.
- Il veut garder une version en ligne plus complète que la version papier.
- Il dépose les illustrations dans `DEV_CleDetSortie`.
