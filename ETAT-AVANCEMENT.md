# Etat d'avancement — Le Magazine Lulli

Derniere mise a jour : 2026-07-22

## Le projet en une phrase

Blog editorial evergreen (Hugo, methode 2 PBN datashake) pour Lulli sur la Toile, habille a la DA de la marque, destine au sous-domaine `magazine.lulli-sur-la-toile.com`. Duplique et adapte depuis `jitrois-atelier`.

Infos cles :
- Emplacement : `site web/lulli-magazine/`
- Theme : `themes/magazine-lulli/`
- Domaine cible : `magazine.lulli-sur-la-toile.com` (baseURL + CNAME deja poses)
- Repo GitHub a creer : `analytics-ds/lulli-magazine`
- Site marchand : https://www.lulli-sur-la-toile.com/
- Consultant : Charlie

## Ce qui est FAIT

**DA Lulli (habillage complet)**
- Mode CLAIR par defaut (bg #FCFCFE, fg #1D1D1B) + bascule sombre `[data-theme="dark"]`, memorisee localStorage, anti-flash
- Titres en serif Fraunces (substitut Atacama), corps + UI en Inter (substitut Antarctica)
- Zero arrondi, hairlines 1px, nav uppercase, beaucoup d'air, images EN COULEUR (grayscale Jitrois retire)
- Logo wordmark "lulli" (SVG officiel recupere sur le site, fills en currentColor) dans header (tagline "Le Magazine") + footer
- Favicon SVG + PNG (64) + apple-touch-icon (180) regeneres depuis le wordmark
- og-default.svg refait (mode clair, serif)

**Structure / config**
- hugo.toml : baseURL, titles, descriptions FR/EN, params (expertise, parentBrandUrl), 4 rubriques en menus FR/EN
- 4 rubriques de header GENERIQUES/editoriales : Conseils / Advice — Inspirations / Inspiration — Créateurs / Designers — Idées Cadeaux / Gift Guides (choix Charlie : PAS de familles produits qui recopieraient la boutique). Les clusters de prompts GEO (bijoux, chaussures, sacs, vetement, maison, local, comparatifs, marque, marque createur) = TAGS + roadmap. Sous-categories par univers a ajouter ENSUITE sous ces 4 portes.
- Source des prompts / clusters GEO : Sheet "Prompts a valider" (https://docs.google.com/spreadsheets/d/1_xVRBjTjnQhllJadQo_S4fUda3SIv0by9OicxrK-CeA/)
- Home FR/EN (hero + rubriques + derniers articles), blog index FR/EN, plan du site FR/EN
- seo-head.html : Organization = Lulli sur la Toile (sameAs Instagram/Facebook/TikTok/YouTube reels)
- robots.txt, llms.txt, site.webmanifest, CNAME : tous en magazine.lulli-sur-la-toile.com
- Workflow GitHub Actions : baseURL corrige (etait reste sur jitrois)
- CLAUDE.md : sections "Contexte du site" + "Publications evergreen" reecrites pour Lulli
- Auteur principal : Magalie Ergoz (mode/beaute)

**Contenu**
- 1 article exemple bilingue : "Garde-robe capsule : composer un vestiaire elegant et durable" (FR) / "Capsule wardrobe..." (EN), rubrique Le Vestiaire, image couleur, FAQ 3 Q, liens sortants vers le site marchand
- roadmap.yaml : VIDE (structure + regles Lulli en commentaire, `articles: []`)

**Verifie**
- Build Hugo OK (32 pages FR, 30 EN), zero residu "jitrois" hors mentions comparatives du CLAUDE.md
- Rendu visuel valide (screenshot) : conforme DA Lulli

## Ce qui RESTE

- [ ] Ajouter les SOUS-CATEGORIES par univers (bijoux, chaussures, sacs, vetement, maison...) sous les 4 rubriques de header, une fois la roadmap calee
- [ ] Remplir `roadmap.yaml` a partir du Sheet de prompts GEO (clusters valides par le client) + Haloscan, filtre Bourrelly par entree
- [x] Repo GitHub `analytics-ds/lulli-magazine` cree + GitHub Pages actif (Actions). Git en place dans le Drive (remote analytics-ds@, credential store).
- [x] Deploye en PHASE PREVIEW sur https://analytics-ds.github.io/lulli-magazine/ (baseURL github.io, CNAME retire temporairement). Build OK, tout repond 200.
- [x] GO-LIVE domaine custom fait le 2026-09-11 : `static/CNAME` re-ajoute, baseURL du workflow rebascule sur `https://magazine.lulli-sur-la-toile.com/`, custom domain defini dans les settings Pages.
- [x] Cote client : DNS CNAME `magazine` -> `analytics-ds.github.io` pose, verifie le 2026-09-11.
- [ ] Valider la DA avec le client (ou en interne) avant mise en ligne publique
- [ ] Remplacer/ameliorer l'image de l'article exemple (visuel editorial Lulli plutot que la photo Openverse actuelle)
- [ ] 1er batch `/create-article-seo` (Opus 4.8) une fois la roadmap remplie
- [ ] Optionnel : creer une categorie banque images `assets/banque-images/lulli-*` (visuels mode/lifestyle en couleur)

## Prochaine action recommandee

Remplir la roadmap (recherche KW Haloscan mode/createurs/idees cadeaux, requetes complementaires non possedees par le site marchand), puis `/github-setup`.

## Decisions cles

- Nom : "Le Magazine Lulli" sur `magazine.lulli-sur-la-toile.com` (choix Charlie, 2026-07-22)
- Type A (site client exclusif Lulli), comme jitrois-atelier
- Serif Fraunces + grotesque Inter = substituts libres des polices de marque Atacama + Antarctica
- Mode CLAIR par defaut (l'inverse de Jitrois) + images en couleur (mode)
- Regle marque STRICT : "createurs"/"selection", jamais "marques" ; ton elegant + familial
- Anti-cannibalisation stricte vs lulli-sur-la-toile.com (guides/portraits/idees cadeaux/GEO, jamais requetes commerciales)
- Rubriques de header GENERIQUES (Conseils/Inspirations/Créateurs/Idées Cadeaux), pas des familles produits ; clusters de prompts = tags ; sous-categories par univers a venir (choix Charlie, 2026-07-22)
