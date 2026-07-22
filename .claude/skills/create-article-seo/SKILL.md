# Skill : Production evergreen SEO L'Atelier Jitrois (batch local, brief + redaction Opus 4.8)

Cette skill produit en **local** un ou plusieurs articles evergreen SEO bilingues FR + EN pour L'Atelier Jitrois, avec un niveau de qualite maximum. C'est la **methode 2** du systeme PBN GEO datashake (batch local + publication differee via GitHub Actions cron), durcie pour Jitrois sur deux points :

1. **Modele : Opus 4.8 par defaut (OBLIGATOIRE)**. Verifier le modele de la session au lancement. Si la session ne tourne pas sur Opus 4.8, proposer de basculer (`/model opus`) OU deleguer les etapes brief + redaction a un sous-agent avec `model: opus`. Ne JAMAIS rediger un article Jitrois avec un modele inferieur sans accord explicite de Charlie.
2. **Brief SEO complet avant chaque redaction**. Pas de redaction au fil de l'eau : chaque article passe par un brief formalise (analyse SERP, title/meta, plan Hn avec consignes par section, FAQ, maillage, champ semantique), puis la redaction suit le brief, puis un auto-controle de conformite.

La publication reelle se fait via `publishDate` + le cron GitHub Actions (`.github/workflows/hugo.yml`, `0 1 * * 2,5` = mardi + vendredi 3h Paris). Hugo (`buildFuture: false`) masque les articles a publishDate future ; ils apparaissent automatiquement au rebuild suivant leur date.

## Quand l'utiliser

- 1x/mois environ : production en batch des prochaines entrees `todo` de `roadmap.yaml` (mode A)
- Ponctuel : 1 ou plusieurs KW a la demande (mode C)

## Contexte a charger AVANT tout (obligatoire)

1. `CLAUDE.md` du blog (voix editoriale Jitrois, interdits, anti-cannibalisation, regles techniques)
2. `roadmap.yaml` (entrees, dates, regle de diversite des categories)
3. `MEMORY.md` (quota 4 articles/semaine max, articles GEO manuels inclus)
4. La liste des articles existants (`content/fr/blog/`, `content/en/blog/`) pour le maillage et l'anti-doublon

## Regles editoriales Jitrois (STRICT, non negociables)

- **Voix de la Maison** : le lecteur est vouvoye ("vous"), la maison parle en "nous" / "chez Jitrois" / "la Maison". Ton chic, aspirationnel, sensoriel. Jamais salesy, jamais plat, jamais dictionnaire.
- **Interdits absolus** : registre sexuel/fetiche/"sexy" ; sections entretien/nettoyage/"comment entretenir" (meme en FAQ, meme en H3).
- **Anti-cannibalisation jitrois.com** : avant de produire, verifier que le sujet n'est pas couvert par une page de jitrois.com (/pages/materials, /pages/jean-claude-jitrois, /pages/our-heritage, /blogs/journal, pages produit/collection). Le blog cible des requetes complementaires (guides, comparatifs, questions, styling, matiere, heritage). Pour les termes possedes par la marque, on LINKE vers jitrois.com, on ne concurrence pas.
- **Liens sortants** : chaque article cite et linke jitrois.com au moins 1 fois (page pertinente : /pages/materials pour la matiere, /pages/jean-claude-jitrois ou /pages/our-heritage pour l'heritage, une collection pour le styling). Ancres naturelles, jamais suroptimisees.
- **Typo** : tous les accents francais ; jamais de tiret cadratin ni demi-cadratin ; le deux-points sert uniquement a introduire une liste a puces, jamais en milieu de phrase de prose.
- **Test "fais-moi deviner le KW"** : le mot-cle exact n'apparait que dans les zones strategiques (title, meta, H1, intro, 1 H2, slug, alt, ancres entrantes) et 0 a 2 fois max dans le corps. Le reste du texte porte le sujet par synonymes, entites et cooccurrences.

## Etape 0 — Lancement

1. **Verifier le modele** : si la session n'est pas sur Opus 4.8, proposer `/model opus` ou basculer les etapes 2-3 en sous-agent `model: opus`. Bloquant.
2. **Mode** :
   > "(A) Suivre la roadmap.yaml [defaut] : combien d'articles ?
   >  (C) KW a la demande : donne les KW + categories + dates"
3. **Verifier le quota** : croiser MEMORY.md et les publishDate planifiees. Max 4 articles/semaine toutes sources confondues (evergreen auto + GEO manuels). Si depassement, decaler au prochain vendredi libre et le dire.
4. **Git** : `git pull --rebase origin main` avant toute modif. En cas de conflit : STOP, avertir Charlie/Damien.
5. Recapituler les N articles prevus (kw, categorie, publishDate) et demander confirmation.

## Etape 1 — Scheduling

- Defaut : garder les `scheduled_date` de la roadmap.
- Slots valides : mardis et vendredis (cadence 2 articles/semaine, quota 4/semaine max).
- Apres tout ajout ou recalage, valider la **regle de diversite** (jamais 2 categories identiques consecutives ; au moins 3 categories distinctes sur toute fenetre de 5). Permuter les dates des entrees en conflit si besoin (jamais celles deja `queued`), logger les permutations.

## Etape 2 — BRIEF SEO (pour chaque article, avant toute redaction)

Le brief est construit selon la methode datashake. Il est redige en interne (pas de fichier livrable) mais doit etre complet avant de passer a l'etape 3.

### 2.0 Filtre Bourrelly (bloquant, avant tout le reste)

Le blog n'est pas une liste de mots-cles, c'est un cocon qui converge vers l'entite "cuir couture", la categorie que Jitrois possede (article pilier : `/blog/qu-est-ce-que-le-cuir-couture/`). Demonstration centrale : **Jitrois est la reference du cuir couture**.

Avant d'analyser la SERP, repondre a 3 questions. Si une reponse est faible, re-angler le sujet (ou le rejeter et le signaler a Charlie), ne jamais rediger "pour remplir un creneau".

1. **Pourquoi toi ?** En quoi cet article renforce la demonstration centrale ? Quel fait, quelle histoire, quel savoir-faire de la Maison cet article met-il en scene ? (Un guide styling repond aussi a cette question : il montre comment la Maison pense le vetement.)
2. **Quelle demande ?** Quel probleme reel du client final cet article resout-il ? Formuler la question telle qu'une cliente la poserait a une vendeuse ou a ChatGPT. Si le sujet ne correspond a aucune vraie question, c'est un sujet de catalogue, pas de demande.
3. **Quelle place dans le cocon ?** Lire le champ `cluster` de l'entree roadmap (pilier / matiere / savoir-faire / style / maison). L'angle et le maillage doivent servir ce cluster et remonter vers le pilier.

Consigner les 3 reponses en une ligne chacune dans le brief.

### 2.1 Analyse SERP

- `mcp__serpapi__search` si disponible, sinon WebSearch natif : requete = kw, gl=fr, hl=fr, top 10.
- Extraire URLs, titles, snippets, PAA (related_questions), recherches associees.
- **Fetch de 3 a 5 concurrents pertinents** (WebFetch) : title, meta, H1, structure H2/H3, longueur, presence FAQ/tableau.
  - Filtres : langue FR, contenu editorial (pas e-commerce pur, pas PDF), 500+ mots.
  - Si <3 concurrents pertinents : mode degrade (titles + snippets + PAA), le noter.
- **Check anti-cannibalisation** : rechercher `site:jitrois.com <kw>` (ou verifier les pages connues). Si jitrois.com possede deja la requete, marquer l'entree `failed` avec `error: "cannibalisation jitrois.com"` et passer a la suivante.

### 2.2 Synthese

- Intention de recherche (informationnelle / comparative / mixte).
- Patterns Hn recurrents (H2 presents chez 2+ concurrents).
- Longueur cible : moyenne des concurrents +/- 10% (1200-1800 mots si mode degrade).
- FAQ : 3 a 6 questions issues des PAA + FAQ concurrentes, dedupliquees, reformulees. JAMAIS de question entretien.
- Champ semantique : entites, cooccurrences, vocabulaire a couvrir (lexique cuir couture, seconde peau, savoir-faire...).

### 2.3 Brief formalise

Produire le brief complet :
- **Title** (<=60 car., kw dans le premier tiers, angle editorial Maison)
- **Meta description** (<=155 car., kw inclus, promesse claire)
- **H1** (different du title, kw ou variante proche)
- **Plan Hn** : 4-7 H2 (patterns SERP + 1-2 H2 de valeur ajoutee signature Jitrois, ex. le regard de la Maison, le point de vue atelier), H3 si utile. H2 explicites et autosuffisants, pas de "Introduction"/"Conclusion", pas de "&".
- **Consignes par section** : pour chaque H2, 1-2 lignes sur ce que la section doit couvrir (faits, entites, angle).
- **FAQ** (si pertinente) : questions + angle de reponse.
- **Maillage interne (logique cocon)** : 3-5 articles cibles du blog. Priorite 1 : l'article pilier `/blog/qu-est-ce-que-le-cuir-couture/` (ancre "cuir couture") quand le contexte le permet. Priorite 2 : les articles du meme `cluster` (scoring : meme cluster +5, meme categorie +3, tags partages +1, mots du kw +2 ; inclure les articles du batch en cours pour le maillage cross-batch). Ancre = mot-cle principal de l'article cible. Intra-langue uniquement. IMPORTANT : ne linker que des articles dont la publishDate est anterieure ou egale a celle de l'article en cours (sinon lien 404 jusqu'a la publication de la cible).
- **Lien(s) sortant(s) jitrois.com** : page cible + ancre.
- **Longueur cible, ton, interdits rappeles.**

## Etape 3 — Redaction (Opus 4.8)

### 3.1 Auteur et image

- Auteur : matching `topics`/`expertise` dans `data/authors.yaml`, sinon `magalie-ergoz` (auteur principal). Meme ID en FR et EN.
- **Image hero (OBLIGATOIRE, pool Jitrois uniquement)** : reutiliser un visuel existant de `static/images/blog/` ou recuperer un visuel sur jitrois.com (credit "© Jitrois"). JAMAIS Openverse/fetch-image.sh sur ce blog (DA monochrome, visuels marque uniquement). Chemins relatifs sans slash initial (`image: "images/blog/x.jpg"`).

### 3.2 Redaction FR (`content/fr/blog/<slug>.md`)

Frontmatter :

```yaml
---
title: "[Title du brief]"
translationKey: "[slug-generique]"
date: "[YYYY-MM-DD aujourd'hui]"
lastmod: "[YYYY-MM-DD aujourd'hui]"
publishDate: "[scheduled_date]"
description: "[Meta du brief]"
categories: ["[Categorie FR]"]
tags: ["tag1", "tag2", "tag3", "tag4", "tag5"]
author: "[id-slug]"
image: "images/blog/[fichier]"
imageAlt: "[Alt FR <=125 car, kw ou variante]"
imageCredit: "© Jitrois"
faq:
  - question: "[Q]"
    answer: "[R, 3-5 phrases]"
readingTime: true
---
```

Body :
- Suivre STRICTEMENT le plan Hn du brief.
- Intro : kw naturellement place, promesse, voix de la Maison.
- Longueur cible du brief +/- 10%.
- Kw exact 0-2 fois dans le corps (test "devine le KW"), synonymes et entites partout ailleurs.
- 1 tableau si l'intention est comparative.
- Maillage et lien jitrois.com conformes au brief.
- FAQ en dernier H2 "Questions frequentes" en accordeon `<details><summary>` si prevue au brief. Q/R identiques au frontmatter.
- Pas de separateur `---` dans le body.

### 3.3 Redaction EN (`content/en/blog/<slug-en>.md`)

- Meme `translationKey`, meme `publishDate`, meme `author`, meme `image`/`imageCredit`.
- Traduction editoriale (pas litterale) de la version FR, voix de la Maison en anglais ("we", "the House", vouvoiement implicite).
- Slug, categories, tags, `imageAlt` et FAQ traduits (mapping categories FR/EN du CLAUDE.md).
- Maillage vers les articles EN correspondants uniquement.

### 3.4 Auto-controle de conformite (bloquant)

Avant de passer a l'article suivant, verifier :
- [ ] Brief respecte (title, meta, H1, plan Hn, longueur)
- [ ] Aucun terme interdit (registre sexy/fetiche, entretien/nettoyage)
- [ ] Deux-points uniquement devant des listes ; aucun tiret cadratin/demi-cadratin ; accents partout
- [ ] Kw exact <= 2 occurrences dans le body
- [ ] 3+ liens internes intra-langue + 1+ lien jitrois.com
- [ ] Filtre Bourrelly consigne au brief + lien vers l'article pilier cuir couture si le contexte le permet
- [ ] Aucun lien interne vers un article a publishDate posterieure
- [ ] FAQ frontmatter == FAQ body
- [ ] `publishDate` = scheduled_date, un vendredi

Si un point echoue : corriger avant de continuer.

## Etape 4 — Mises a jour systemes (pour chaque article)

1. `roadmap.yaml` : `status: queued`, `queued_date: today`, `published_url_fr`/`published_url_en` = URLs finales prevues.
2. `static/llms.txt` : ajouter `- <Titre complet> : <URL absolue>` dans les sections FR et EN.
3. `MEMORY.md` : ligne dans la semaine de la publishDate, format `- YYYY-MM-DD | <Titre FR> / <Titre EN> (FR+EN) | <Categorie> | evergreen (queued)`.

## Etape 5 — Build de verification

```bash
hugo
```

Exit 0 obligatoire. Les articles a publishDate future sont absents du rendu (attendu). Verifier pour un article a publishDate passee : sitemap FR/EN, plan du site, page auteur, llms.txt.

## Etape 6 — Recap + commit/push

Afficher le recap (tableau : kw, slug, categorie, publishDate, mots, auteur, liens) puis demander :
> "Commit + push ? (1) oui [defaut] (2) non, relecture d'abord (3) commit local seulement"

Si push : `git add -A && git commit -m "evergreen: <N> articles (batch <date>)" && git pull --rebase origin main && git push origin main`.

Rappel : le push sur l'org `analytics-ds` necessite le compte GitHub `analytics-ds` (`gh auth switch`).

Apres push : les articles restent invisibles jusqu'au cron de leur publishDate (mardi + vendredi 3h Paris).

## Gestion des erreurs

Article en echec : marquer `status: failed` + `error: "<etape> <message>"` dans roadmap.yaml, supprimer les fichiers partiels, continuer le batch, signaler dans le recap. Jamais de retry automatique.
