# Le Magazine Lulli — blog GEO/SEO (Hugo)

Blog editorial de Lulli sur la Toile (concept store mode et lifestyle), genere avec Hugo et heberge sur GitHub Pages. DA Lulli, methode 2 evergreen. Duplique depuis le template PBN datashake puis habille aux couleurs de la marque.

## Comment ca marche

Ce repo ne contient pas de site. Il contient les **instructions et templates** pour que Claude Code genere un site complet automatiquement.

### Premier lancement

1. L'utilisateur connecte Claude Code a ce repo
2. L'utilisateur tape `/create-site`
3. Claude pose les questions necessaires (nom du site, couleurs, categories, etc.)
4. Claude genere tout le site Hugo, les fichiers SEO, et configure le deploiement
5. L'utilisateur push sur GitHub, active GitHub Pages, le site est en ligne

### Utilisation courante

- `/create-article` : creer un nouvel article de blog (choix parmi plusieurs types : article standard, comparatif). Push automatiquement sur GitHub si le repo est configure
- `/create-article-seo` : **production evergreen SEO en batch** (methode 2 du reseau PBN datashake). Brief SEO complet avant chaque redaction, modele Opus 4.8 obligatoire, articles bilingues FR+EN avec `publishDate` futur, publication automatique via le cron GitHub Actions (mardi + vendredi 3h Paris, cadence 2 articles/semaine). Pilote par `roadmap.yaml` a la racine. Voir section "Publications evergreen automatiques" plus bas
- `/seo-setup` : generer ou mettre a jour les fichiers SEO techniques de base (robots.txt, llms.txt, sitemap, structured data)
- `/seo` : mode interactif pour modifier/ajouter des elements SEO (meta tags, JSON-LD, audit on-page, etc.)
- `/serve` : lancer le serveur Hugo en local (previsualisation sur `http://localhost:1313/`)
- `/share` : lancer Hugo + ngrok pour partager le site via un lien public (accessible par n'importe qui)
- `/github-setup` : creer un repo GitHub, push le code et activer GitHub Pages (mise en ligne du site)
- `/github-deploy` : push les modifications vers GitHub et declencher le deploiement

## Structure du repo

```
.claude/
├── scripts/
│   └── fetch-image.sh           ← Image libre de droit : cascade Pexels/Unsplash/Openverse + visuel genere
├── skills/
│   ├── create-site.md           ← Workflow creation de site complet
│   ├── create-article.md        ← Workflow creation d'article (multi-types)
│   ├── seo-setup.md             ← Workflow fichiers SEO techniques (baseline)
│   ├── seo.md                   ← Mode interactif SEO (modifications ponctuelles)
│   ├── serve.md                 ← Lancer le serveur Hugo en local
│   ├── share.md                 ← Lancer Hugo + ngrok (partage public)
│   ├── github-setup.md          ← Creer un repo GitHub + activer GitHub Pages
│   └── github-deploy.md         ← Push et deployer sur GitHub Pages
└── templates/
    ├── data/
    │   ├── authors.yaml          ← 6 auteurs partages avec bios FR/EN, expertise, topics
    │   └── avatar-prompts.md     ← Prompts de generation des 6 avatars (Midjourney/DALL-E)
    ├── hugo-workflow.yml         ← GitHub Actions CI/CD
    ├── main.css                  ← Design system CSS complet (variables, composants, responsive, a11y)
    ├── articles/                 ← Templates d'articles par type
    │   ├── article-standard.md   ← Article informatif SEO + GEO (type par defaut)
    │   └── geo-comparatif.md     ← Article comparatif avec mise en avant
    ├── seo/                      ← Fichiers SEO techniques (editables)
    │   ├── robots.txt            ← Modele robots.txt
    │   ├── llms.txt              ← Modele llms.txt
    │   └── structured-data/      ← Schemas JSON-LD
    │       ├── article.json      ← Article (avec image et @id croise)
    │       ├── organization.json ← Organization (avec @id, logo, expertise)
    │       ├── author.json       ← Person (auteur)
    │       ├── breadcrumb.json   ← BreadcrumbList
    │       ├── website.json      ← WebSite (avec publisher @id)
    │       └── faq.json          ← FAQPage (genere auto depuis frontmatter)
    ├── layouts/
    │   ├── baseof.html           ← Layout de base (skip-to-content, fonts non-bloquantes, favicon)
    │   ├── home.html             ← Page d'accueil
    │   ├── list.html             ← Pages de liste
    │   ├── single.html           ← Page article (breadcrumb, TOC sticky, image hero, auteur, articles similaires)
    │   ├── sitemap-html.html     ← Page plan du site (liste toutes les pages)
    │   └── 404.html              ← Page 404 custom
    └── partials/
        ├── header.html           ← Header sticky (backdrop-filter, aria-labels, mobile menu)
        ├── footer.html           ← Footer (aria-label, lien plan du site)
        └── seo-head.html         ← Meta tags SEO complets + JSON-LD auto (OG avec image, Twitter, canonical, hreflang, author, article:section, FAQPage auto, Organization @id)
```

## Contexte du site

> Cette section est remplie automatiquement par le skill `/create-site`.
> Elle permet a Claude de connaitre le contexte du site pour les futures actions.

- **Nom du site** : Le Magazine Lulli
- **Description (FR)** : Le magazine de Lulli sur la Toile : createurs, style, art de vivre et idees cadeaux par le concept store.
- **Description (EN)** : The magazine of Lulli sur la Toile: designers, style, art de vivre and gift ideas from the concept store.
- **Type** : A (site client, exclusif Lulli sur la Toile) — sous-domaine prevu `magazine.lulli-sur-la-toile.com`
- **URL (phase 1)** : https://analytics-ds.github.io/lulli-magazine/ (GitHub Pages, apres `/github-setup`). Domaine custom `magazine.lulli-sur-la-toile.com` a connecter ensuite (CNAME cote client ; le `baseURL` de hugo.toml pointe deja dessus). Le fichier `static/CNAME` est deja pose.
- **Couleurs** : DA Lulli, monochrome chaleureux noir/creme. Tokens semantiques dans `main.css` : `--bg / --fg / --bg-elevated / --fg-muted / --line`. **Mode CLAIR par defaut** (bg #FCFCFE, fg #1D1D1B) ; **mode sombre** via `[data-theme="dark"]` (bg #141312, fg #F5F3EF). Bascule utilisateur (bouton lune/soleil dans le header) memorisee en localStorage, appliquee avant peinture (anti-flash) par un script inline dans `baseof.html`.
- **Logo** : vrai wordmark Lulli (`logo-lulli.svg`, recupere sur lulli-sur-la-toile.com, fills passes en `currentColor` pour s'adapter au theme). Inline via `resources.Get` dans le header (+ tagline "Le Magazine") et le footer. Favicon = vrai symbole/monogramme Lulli (les 4 boucles) recupere du site officiel : `static/favicon.ico` (multi-tailles) + `favicon.png` + `apple-touch-icon.png` + `images/logos/lulli-icon-192/512.png` (ce dernier sert de logo dans le schema Organization).
- **Polices** : Fraunces (serif editoriale, TITRES — substitut libre d'Atacama, la serif de marque de Lulli) + Inter (grotesque, CORPS + UI — substitut libre d'Antarctica, la grotesque de marque). Chargees en non-bloquant.
- **Bonnes pratiques Google** : `color-scheme` + `theme-color` metas, `site.webmanifest` + apple-touch-icon, hreflang, canonical, JSON-LD (Organization/Article/FAQPage/BreadcrumbList/WebSite), sitemaps par langue + robots.txt, images alt + lazy-load + dimensions (CLS), fonts non-bloquantes, responsive, liens crawlables.
- **Langue principale** : Francais (fr), servie a la racine. Version EN dans `/en/` (TOUJOURS active). Organisation par dossier : `content/fr/` et `content/en/`, chacun declare via `contentDir` dans hugo.toml (evite la fuite de taxonomie cross-langue).
- **Rubriques de header (FR / EN)** : Conseils / Advice, Inspirations / Inspiration, Créateurs / Designers, Idées Cadeaux / Gift Guides. Rubriques GENERIQUES editoriales (surtout PAS des familles produits qui recopieraient la nav de la boutique). Les univers / clusters de prompts GEO (bijoux, chaussures, sacs, vetement, maison, local, comparatifs, marque, marque createur) vivent en TAGS dans les articles et pilotent la roadmap. Des SOUS-CATEGORIES par univers seront ajoutees ensuite sous ces 4 portes.
- **Auteur principal du site** : Magalie Ergoz (`magalie-ergoz`, mode/beaute). Avatars vides (placeholder initiale) ; a remplacer par de vrais visuels si besoin.
- **DA / regles de rendu** : monochrome chaleureux (noir #1D1D1B sur creme/blanc), aucun arrondi, aucune ombre, hairlines 1px, beaucoup d'air, nav uppercase letter-spacing, **titres en serif (Fraunces)**, **images en COULEUR** (la mode = couleur, PAS de grayscale, contrairement a Jitrois). Rester fidele a lulli-sur-la-toile.com.
- **Voix editoriale Lulli (override du ton impersonnel par defaut)** : registre elegant et raffine, mais chaleureux et proche, avec un ADN familial revendique. On s'adresse au lecteur en **"vous"** et la maison parle en **"nous" / "notre"**. Ton desirable, sensible, jamais froid ni distant, jamais salesy. Eviter le ton dictionnaire impersonnel. Pas de deux-points en milieu de phrase de prose (le ":" sert uniquement a introduire une liste a puces).
- **Regle de marque (STRICT)** : parler de **createurs** et de **selection**, JAMAIS de "marques" pour designer l'offre. Marqueur identitaire fort cote client. Champ lexical : createur, selection, piece, savoir-faire, raffine, elegant, intemporel, feminin, desirable, idee cadeau, art de vivre, decouverte.
- **Anti-cannibalisation (STRICT)** : le magazine ne doit JAMAIS reproduire une page de lulli-sur-la-toile.com (pages createur `/createurs/*`, categories produit, collections, requetes commerciales pures type "robe tendance", "baskets femme"...). Il cible UNIQUEMENT des requetes complementaires que le site marchand ne possede pas : guides et conseils ("comment porter", "comment choisir", "comment associer"), portraits editoriaux de createurs, idees cadeaux et occasions, tendances, art de vivre, formats GEO conseil/occasion. Pour les termes possedes par la marque, on LINKE vers la page lulli-sur-la-toile.com correspondante (ex : l'article exemple `garde-robe-capsule-elegante` linke vers la selection sur lulli-sur-la-toile.com). Avant tout nouvel article : verifier les pages du site marchand.
- **Donnees structurees** (via `seo-head.html`) : Organization = entite **Lulli sur la Toile** (url lulli-sur-la-toile.com, sameAs Instagram/Facebook/TikTok/YouTube) ; articles en **BlogPosting** ; pages de liste/rubriques en **WebPage/CollectionPage** ; accueil en **WebSite + WebPage** ; **FAQPage** auto depuis le frontmatter `faq` ; **BreadcrumbList** sur les articles.
- **Cache / ETag / Last-Modified** : geres automatiquement par GitHub Pages (non personnalisables). Le signal "derniere modif" cote contenu vient du `lastmod` du frontmatter (sitemap XML + `dateModified` JSON-LD).
- **Images** : images en COULEUR (mode). Pour les articles evergreen, privilegier des visuels editoriaux / lifestyle de qualite (banque images datashake `assets/banque-images/`, visuels Lulli, ou Openverse en licence commerciale via `fetch-image.sh`). IMPORTANT : les chemins d'images dans le frontmatter doivent etre **relatifs sans slash initial** (`image: "images/blog/x.webp"`), sinon `relURL` ne prefixe pas le sous-chemin GitHub Pages et l'image 404. Idem pour `default_og_image` / `logo` dans hugo.toml.
- **A faire (post-launch)** : creer le repo GitHub `analytics-ds/lulli-magazine` + activer Pages (`/github-setup`), puis connecter `magazine.lulli-sur-la-toile.com` (CNAME cote client). Remplir `roadmap.yaml` (vide) via Haloscan avant le premier `/create-article-seo`.

## Suivi des publications (MEMORY.md)

Le fichier `MEMORY.md` a la racine trace tous les articles publies, classes par semaine. Il est mis a jour automatiquement par `/create-article`.

**Limite de publication : 4 articles par semaine maximum.** Avant chaque creation d'article, le systeme verifie le quota. Si 4 articles sont deja publies dans la semaine en cours, l'utilisateur est averti.

Cette limite sert a eviter la publication en masse et a maintenir un rythme de publication regulier, ce qui est meilleur pour le SEO.

## Regles generales

- **Bilinguisme obligatoire (langue principale + EN)** : tous les blogs generes par ce template sont bilingues. La langue principale est servie a la racine (`/`), la version anglaise en sous-dossier `/en/`. Hugo gere le multilingue via la convention de dossiers `content/` (principale) et `content/en/` (anglais). Chaque article et page a une paire de fichiers avec un `translationKey` identique dans le frontmatter. Le header contient un language switcher automatique. Les balises hreflang sont generees automatiquement par le partial `seo-head.html`. **Ne JAMAIS generer un site ou un article dans une seule langue** — c'est systematiquement FR + EN (ou la langue principale + EN)
- Toujours utiliser `relURL` dans les templates Hugo pour les liens (compatibilite GitHub Pages)
- Les articles vont dans `content/blog/` (langue principale) et `content/en/blog/` (anglais)
- Les slugs sont en minuscules, sans accents, mots separes par des tirets
- Ne JAMAIS utiliser `&` dans les noms de categories ou de tags — toujours remplacer par "et" (Hugo genere un double tiret `--` dans le slug, ce qui casse les URLs)
- Le ton des articles est impersonnel (pas de je/tu/nous/vous) sauf instruction contraire
- Les specs d'article (mots minimum, H2, blocs obligatoires) dependent du type choisi — lire les `<!-- NOTES POUR CLAUDE -->` dans chaque template d'article
- Chaque article doit contenir au minimum 3 liens internes contextuels vers d'autres articles du blog. L'ancre de chaque lien doit contenir le mot-cle principal de l'article cible. **Maillage intra-langue uniquement** : un article FR ne mail que des articles FR, un article EN ne mail que des articles EN (le lien vers la traduction est gere par le language switcher du header)
- **Systeme d'auteurs partage** : 6 auteurs fictifs definis dans `data/authors.yaml` (copie depuis `.claude/templates/data/authors.yaml` a la creation du site). Chaque auteur a un id-slug, un nom, un type (person/organization), un avatar, des `jobTitle`/`role`/`bio` bilingues FR/EN, une liste d'`expertise` et une liste de `topics` (mots-cles pour la selection automatique). Les auteurs disponibles : `thomas-durand` (tech), `magalie-ergoz` (mode/beaute), `claire-beaumont` (maison/habitat), `laura-verdier` (sante/bien-etre), `kevin-moreau` (transport/mobilite), `sophie-martin` (finance/patrimoine)
- **Selection automatique de l'auteur** : dans le frontmatter d'un article, le champ `author` contient l'**ID slug** de l'auteur (ex: `author: thomas-durand`), pas son nom complet. La skill `/create-article` selectionne automatiquement l'auteur le plus pertinent selon les `topics` et `expertise` qui matchent avec le sujet de l'article. Si aucun match clair, l'auteur principal du site (defini dans la section "Contexte du site" de ce CLAUDE.md) est utilise
- **Avatars des auteurs** : fichiers WebP 512x512 dans `static/images/authors/[id].webp`. Style unifie "flat illustration portrait". Prompts de generation documentes dans `.claude/templates/data/avatar-prompts.md`. Si l'avatar est manquant, un placeholder coloree avec la 1ere lettre du nom s'affiche
- **JSON-LD Author** : le partial `seo-head.html` genere automatiquement un schema.org/Person (ou Organization) complet depuis les donnees de `data/authors.yaml` (name, jobTitle, description, knowsAbout, image, sameAs, worksFor)
- **Bloc auteur en bas d'article** : le layout `single.html` affiche automatiquement un encart avec avatar, nom, role, bio complete et expertise de l'auteur, traduit dans la langue de l'article (FR ou EN)
- Les templates SEO dans `.claude/templates/seo/` sont editables par l'utilisateur — toujours lire la version en place avant de generer
- Pour ajouter un nouveau type d'article, creer un `.md` dans `.claude/templates/articles/` — il sera automatiquement propose par `/create-article`
- Pour ajouter un schema JSON-LD, creer un `.json` dans `.claude/templates/seo/structured-data/` et utiliser `/seo` pour l'integrer
- Chaque article doit avoir un champ `lastmod` dans le frontmatter (= date de derniere modification). Il est utilise par le sitemap XML, le sitemap HTML et le schema JSON-LD
- Quand un article est modifie, toujours mettre a jour le champ `lastmod` avec la date du jour
- Le sitemap HTML (`/plan-du-site/`) se regenere automatiquement a chaque build Hugo
- Toujours build et verifier (`hugo`) avant de commit
- Chaque article doit avoir un champ `faq` dans le frontmatter (liste de questions/reponses) pour generer automatiquement le schema FAQPage JSON-LD. Minimum 3 questions
- Chaque article a une image hero OBLIGATOIRE, recuperee automatiquement par `.claude/scripts/fetch-image.sh` au moment de `/create-article`. L'image vient de la **cascade Pexels -> Unsplash -> Openverse -> visuel de charte genere** (revue le 2026-09-04 : Openverse n'est plus la source nominale, il ne produisait que 15 photos pertinentes sur 45 heros mesures). Les cles `PEXELS_API_KEY` et `UNSPLASH_ACCESS_KEY` viennent du **prompt de la routine** ou du `.env` local, **jamais du repo** qui est public ; sans cle la cascade demarre a Openverse et l'article sort quand meme. Le registre `.claude/hero-sources.json` empeche deux articles de porter la meme photo. **Controle visuel obligatoire avant publication, quelle que soit la banque.** Convertie en WebP automatiquement si `cwebp` est installe. Stockee dans `static/images/blog/[slug].webp`
- Le frontmatter contient 3 champs lies a l'image : `image` (chemin Hugo), `imageAlt` (texte alternatif FR, max 125 car), `imageCredit` (attribution du photographe). Ces 3 champs sont remplis automatiquement par le script
- L'image est affichee : (1) dans les cards de la homepage et des pages de liste, (2) en bannière cote a cote avec le titre sur la page article, (3) dans og:image pour les partages sociaux, (4) dans le schema Article JSON-LD
- Le credit photo est affiche sous l'image de l'article (petite mention en italique alignee a droite). Obligatoire pour respecter les licences CC BY et CC BY-SA
- Les Google Fonts sont chargees en non-bloquant (media="print" + swap JS) pour de meilleures performances
- Le layout inclut un lien "Skip to content" pour l'accessibilite
- Les navigations ont des `aria-label` pour les lecteurs d'ecran
- Le CSS respecte `prefers-reduced-motion` pour desactiver les animations si l'utilisateur le demande
- Les articles affichent une table des matieres (TOC) sticky en sidebar, generee automatiquement par Hugo
- Les articles similaires sont affiches en bas de page, calcules par Hugo via la config `[related]` dans hugo.toml

## Publications evergreen automatiques (methode 2 : batch local + GitHub Actions cron)

Ce blog utilise la **methode 2** du systeme PBN GEO datashake (comme jitrois-atelier / ma-bonne-sante) :

- **Modele : Opus 4.8** pour le brief et la redaction (qualite editoriale max)
- **Brief SEO complet avant chaque redaction** (analyse SERP + fetch concurrents, title/meta/H1, plan Hn avec consignes par section, FAQ, maillage, champ semantique), puis redaction conforme au brief, puis auto-controle bloquant (interdits, typo, densite kw, liens)

### Principe

1. Charlie renseigne les mots-cles dans `roadmap.yaml` (`status: todo`, scheduled_date)
2. Environ 1x/mois, Charlie lance `/create-article-seo` en local : brief + redaction FR/EN pour chaque entree, articles ecrits avec `publishDate` futur, status passe a `queued`
3. Relecture possible, puis commit + push sur main (compte GitHub `analytics-ds`)
4. Le cron GitHub Actions (`.github/workflows/hugo.yml`, `0 1 * * 2,5` = mardi + vendredi 3h Paris) rebuild le site : les articles dont `publishDate <= today` apparaissent automatiquement

### Roadmap et cocon (doctrine Bourrelly)

Fichier : `roadmap.yaml` a la racine (VIDE au 2026-07-22, a remplir via Haloscan). Statuts : `todo`, `queued`, `failed`.

La roadmap doit etre structuree en **cocon semantique** convergeant vers l'entite que Lulli possede (le concept store / la selection de createurs mode et lifestyle). Chaque entree porte un champ `cluster`. Regles :
- Chaque brief passe le **filtre Bourrelly** (pourquoi Lulli ? quelle demande du client final ? quelle place dans le cocon ?)
- Chaque article linke le pilier quand le contexte le permet + en priorite son propre cluster
- Jamais de lien interne vers un article a publishDate posterieure (404 jusqu'a publication)

Regles propres a la roadmap Lulli :
- Vocabulaire : **createurs** et **selection**, JAMAIS "marques"
- Ton elegant + raffine + chaleureux/familial, jamais froid ni salesy
- JAMAIS de requete commerciale possedee par lulli-sur-la-toile.com (anti-cannibalisation) : on cible guides/conseils, portraits editoriaux de createurs, idees cadeaux, occasions, tendances, art de vivre, GEO conseil/occasion
- Rubriques (categories header, generiques) : Conseils, Inspirations, Créateurs, Idées Cadeaux. Les univers produits (clusters de prompts : bijoux, chaussures, sacs, vetement, maison, local, comparatifs, marque createur) = TAGS, pas categories.
- Diversite des rubriques : jamais 2 articles consecutifs de la meme rubrique, au moins 3 rubriques distinctes sur toute fenetre de 5 articles
- Cadence 2/semaine (mardi + vendredi), quota global 4 articles/semaine (articles GEO manuels inclus)

### Images

Les articles evergreen utilisent des visuels editoriaux / lifestyle EN COULEUR (banque images datashake, visuels Lulli, ou Openverse en licence commerciale via `fetch-image.sh`). Pas de contrainte monochrome (contrairement a jitrois-atelier).

## Comment repondre a l'utilisateur

- Tutoiement, ton decontracte
- Pas de jargon technique sans explication
- Reponses structurees avec listes a puces
- Pas d'emoji sauf demande explicite
