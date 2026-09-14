# Etat d'avancement — Le Magazine Lulli

Derniere mise a jour : 2026-09-14

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


## Audit d'eligibilite au run GEO (2026-09-14)

Controle fait en direct pour trancher si le magazine entre dans le run GEO Lulli.

**Ce qui est bon**
- Domaine custom `magazine.lulli-sur-la-toile.com` actif et stable, 200 OK.
- Les 5 crawlers testes recoivent le contenu complet en 200 : GPTBot, PerplexityBot, ClaudeBot, OAI-SearchBot, Googlebot. Rien ne bloque cote acces IA.
- Bilingue FR + EN reellement servi, sitemap index qui pointe les deux langues.
- `llms.txt` present et servi.
- Couvert par la GSC via la propriete `sc-domain:lulli-sur-la-toile.com` (compte datashake), pas besoin de creer une propriete dediee.
- Qualite editoriale au niveau attendu. Article "choisir-site-multimarque-createurs" verifie : 1 242 mots, 4 blocs JSON-LD (Organization, BlogPosting, BreadcrumbList, FAQPage), 5 liens vers le site marchand, une seule occurrence du mot "marque" (regle STRICT respectee).
- 4 articles FR en ligne au 14/09 : garde-robe-capsule-elegante, colliers-tendance-createurs, chaussures-createur-formes-createurs, choisir-site-multimarque-createurs.

**Ce qui bloque une entree immediate**
- [ ] **Toujours pas indexe.** `inspect_url` renvoie "URL is unknown to Google". Le sitemap est soumis, la decouverte suit son cours. L'Indexing API n'est pas activable ici (elle ne couvre officiellement que JobPosting et BroadcastEvent), donc pas de raccourci par la.
- [ ] **Aucun lien entrant depuis le site principal.** Verifie le 2026-09-14, `www.lulli-sur-la-toile.com` ne pointe vers le magazine nulle part, alors que le magazine envoie 3 liens vers la boutique depuis sa seule home. C'est le vrai frein a la decouverte. Demander a l'agence Magento un lien dans le footer du site marchand.
- [x] **Sitemap soumis en GSC le 2026-09-14** sur `sc-domain:lulli-sur-la-toile.com` via l'API (PUT sitemaps, 204). Confirme cote GSC, 0 erreur et 0 warning.
- [x] **`robots.txt` corrige le 2026-09-14.** La cause n'etait pas le fichier mais le build. `enableRobotsTXT = true` + Hugo 0.139 cote CI faisait generer a Hugo son robots.txt par defaut, qui ecrasait `static/robots.txt`. Le local en 0.160 ne reproduisait pas le bug. Flag passe a `false`, le fichier complet est desormais servi en ligne (Allow, 6 crawlers IA, ligne Sitemap).
- [x] **`llms.txt` recale le 2026-09-14.** Rubriques remises sur Conseils / Inspirations / Createurs / Idees Cadeaux, ajout des 4 articles FR et EN avec leur resume, de la navigation, du lien boutiques et du Wikidata Q140656973. Les 11 URLs citees repondent toutes en 200.
- [ ] **Volume insuffisant.** 4 articles ne pesent pas en GEO. La `roadmap.yaml` est toujours vide, donc aucune production planifiee.
- [ ] **DA toujours pas validee par le client** avant mise en avant publique.

**Arbitrage pose**
Le magazine est un PBN de type A, sous-domaine du client. Deux consequences. Il n'est pas soumis au secret des PBN, il peut donc etre nomme dans les livrables client, contrairement a comparatif-mode ou secretdestyle. Mais il n'a pas non plus la valeur de citation d'un media tiers aux yeux des moteurs, un sous-domaine de marque restant percu comme de la parole de marque. Il complete les medias tiers dans le run GEO, il ne les remplace pas.


### Propriete GSC dediee au sous-domaine (2026-09-14)

- Token fourni par Charlie : `i0PrJItMbxaflVx9y-1SUIq2BWaMZ6G05rR2toaVYks`.
- Pas de TXT DNS pose sur `magazine.lulli-sur-la-toile.com` (verifie, seul le CNAME vers analytics-ds.github.io existe), et le domaine racine porte 3 autres tokens sans rapport. Methode retenue : **balise meta**, qu'on maitrise cote depot, plutot que le DNS qui aurait demande une action du client.
- Meta posee dans `themes/magazine-lulli/layouts/partials/seo-head.html`, pilotee par `params.googleSiteVerification` de `hugo.toml`. Servie sur les 63 pages, FR et EN, verifie en ligne.
- Propriete `https://magazine.lulli-sur-la-toile.com/` **creee** via l'API (PUT /webmasters/v3/sites, 204). Elle ressort en `siteUnverifiedUser`.
- **Deux preuves de propriete en ligne et verifiees le 2026-09-14**, les deux methodes restent ouvertes cote interface, l'une ou l'autre suffit :
  - balise meta, token `i0PrJItMbxaflVx9y-1SUIq2BWaMZ6G05rR2toaVYks`, sur les 63 pages ;
  - fichier `static/google65f408abb8918f8a.html`, servi en 200 a la racine.
- [ ] **Validation a finir a la main.** L'API Site Verification renvoie 403, le refresh token GSC datashake ne porte que les scopes `webmasters`, `webmasters.readonly` et `indexing`, pas `siteverification`. Charlie clique Valider dans l'interface GSC. Statut relu apres mise en ligne des deux preuves : toujours `siteUnverifiedUser`, le clic est bien la seule etape manquante.
- [ ] Une fois validee, soumettre le sitemap sur cette propriete aussi. Il est deja soumis sur `sc-domain:lulli-sur-la-toile.com`.

## Decisions cles

- Nom : "Le Magazine Lulli" sur `magazine.lulli-sur-la-toile.com` (choix Charlie, 2026-07-22)
- Type A (site client exclusif Lulli), comme jitrois-atelier
- Serif Fraunces + grotesque Inter = substituts libres des polices de marque Atacama + Antarctica
- Mode CLAIR par defaut (l'inverse de Jitrois) + images en couleur (mode)
- Regle marque STRICT : "createurs"/"selection", jamais "marques" ; ton elegant + familial
- Anti-cannibalisation stricte vs lulli-sur-la-toile.com (guides/portraits/idees cadeaux/GEO, jamais requetes commerciales)
- Rubriques de header GENERIQUES (Conseils/Inspirations/Créateurs/Idées Cadeaux), pas des familles produits ; clusters de prompts = tags ; sous-categories par univers a venir (choix Charlie, 2026-07-22)
- Entree dans le run GEO conditionnee a l'indexation + sitemap soumis + llms.txt recale + roadmap remplie (arbitrage Charlie, 2026-09-14)
