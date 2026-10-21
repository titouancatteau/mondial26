@AGENTS.md

# Mondial 26

> Fiche lue par la Centrale Projets (`~/.claude/centrale/PROJETS.md`). Une session neuve doit pouvoir
> reprendre le projet avec cette seule fiche : ce qui est durable va ici, pas dans l'historique d'une conversation.
> Le fichier `AGENTS.md` (importé ci-dessus) rappelle que ce Next.js a des changements incompatibles :
> lire le guide concerné dans `node_modules/next/dist/docs/` avant d'écrire du code.

## En bref
- **Pitch** : site de la Coupe du Monde 2026 entre amis — actualités, stats en direct, classements des poules, et un quiz (avec classement des joueurs, boutique de points et succès) pour se défier.
- **Statut** : en développement, **à l'arrêt depuis le 26/05/2026** (dernier commit). Le projet était lié à la Coupe du Monde 2026 (juin-juillet 2026) : à reclasser en sommeil si Titouan ne le reprend pas.
- **Plateforme et stack** : web · Next.js 16 (App Router) · React 19 · TypeScript · Tailwind 4 · Supabase (Postgres, connexion Google, temps réel) · Vercel (hébergement et tâches planifiées).
- **Dépôt** : `titouancatteau/mondial26` (dépôt **personnel**, pas dans TCDevOrg ; public) — branche de travail : `main`. Le dépôt local pointe encore vers l'ancien nom de compte `titouanc-ui/mondial26`, que GitHub redirige : à corriger avec `git remote set-url` si cela gêne.
- **Dossier** : `~/Developer/mondial26` (anciennement `~/Downloads/Projet CDM26/mondial26`).
- **Alias** : CDM26, Projet CDM26, Mondial 26.

## Commandes
- Installer : `npm install` (le dossier `node_modules` n'est pas là : à faire avant tout).
- Lancer : `npm run dev` puis http://localhost:3000 (la configuration `.claude/launch.json` le fait aussi).
- Vérifier : `npm run lint` et `npm run build`. Il n'y a pas de tests automatiques.
- Publier : poussée sur GitHub, Vercel reconstruit. Les tâches planifiées (`/api/cron/refresh-news` toutes les 15 minutes, `/api/cron/refresh-matches` toutes les 10) sont décrites dans le README.
- Base de données : les scripts SQL sont dans `supabase/migrations/` (0001 à 0010, à passer dans l'ordre dans l'éditeur SQL de Supabase). Le README ne cite que la 0001 : il est en retard.

## Règles propres au projet
- Sans clés, le site tourne en mode démonstration (`FOOTBALL_PROVIDER=mock`, quiz sans sauvegarde des scores). C'est le mode à utiliser pour développer.
- Les sources de matchs passent toutes par l'interface de `lib/data-providers/` : changer de fournisseur ne doit toucher que ce dossier. Le fournisseur API-Football décrit dans le README n'est **pas écrit** (il n'y a que `mock-provider.ts` et `football-data-org.ts`).
- Le score du quiz est calculé **côté serveur** depuis un jeton signé : ne jamais envoyer au navigateur la bonne réponse avant la soumission finale.
- Les routes d'administration ne répondent qu'aux adresses listées dans la variable `ADMIN_EMAILS`.

## Accès utilisés
Les valeurs ne sont jamais écrites ici. Voir `~/.claude/centrale/ACCES.md`.
- App Store Connect : non — Cloudflare : non — autre : Supabase, Vercel, Google (connexion OAuth et Search Console), football-data.org. Les clés sont dans `.env.local` (jamais lu ici, non versionné) ; les noms des variables sont dans `.env.example`.

## Pièges connus
- Le README décrit l'architecture du début : le dossier `app/` compte aussi `boutique`, `profil`, `conditions`, `confidentialite`, et `lib/` contient `achievements`, `shop`, `admin.ts`, `rate-limit.ts`. Se fier à l'arborescence réelle.
- `proxy.ts` remplace l'ancien `middleware` (Next.js 16) : il rafraîchit la session Supabase.
- La connexion Google exige les adresses de retour (`/auth/callback`) déclarées à la fois dans Supabase et dans la console Google.
- L'adresse de contact des pages légales est `titouan.c@tteau.com` (commit `d8f45c8`).

## État actuel — mis à jour le 21/10/2026
- Où on en est : `main` = `origin/main`, arbre propre, dernier commit le 26/05/2026 (« bouton "Se connecter" pour les visiteurs anonymes »). Le quiz, les classements, la boutique de points, les succès et les pages légales existent. Je n'ai pas relancé le site ni vérifié ce qui tourne sur Vercel.
- Prochaines étapes : décider si le projet continue après la Coupe du Monde ; sinon le ranger dans `~/Developer/_sommeil/`. Mettre le README à jour si le projet reprend.
- En attente de Titouan : cette décision.
