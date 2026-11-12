# Marchés publics – crédits et dossiers d'achat

Site de gestion des crédits budgétaires et des dossiers d'achat : un dossier encodé se lie automatiquement au crédit de son exercice et de son catalogue.
Hébergement gratuit : **Supabase** (base de données + connexion) et **Vercel** (le site).

## Fichiers

| Fichier | Rôle |
|---|---|
| `index.html` | Le site complet |
| `config.js` | Adresse et clé publique de ton projet Supabase (à remplir) |
| `supabase.sql` | Crée les tables, la sécurité et le « qui a encodé » |
| `donnees-initiales.sql` | Reprend les 2 crédits et 2 dossiers déjà encodés |

## 1. Supabase

1. Ouvre ton projet Supabase (un existant convient : toutes les tables commencent par `mp_`), ou crée-en un nouveau.
2. **SQL Editor → New query** : colle le contenu de `supabase.sql`.
   Tout en bas, remplace `ton.adresse@exemple.be` par **ton adresse e-mail**, puis **Run**.
3. Nouvelle requête : colle `donnees-initiales.sql`, puis **Run**.
4. **Project Settings → API** : copie la **Project URL** et la clé **anon public** dans `config.js`.
   Ne mets jamais la clé `service_role` dans ce fichier.

## 2. GitHub

1. Crée un dépôt (privé ou public), par exemple `marches-publics`.
2. **Add file → Upload files** : dépose `index.html`, `config.js`, `README.md` (les fichiers `.sql` peuvent aussi y aller, ils ne contiennent aucun secret).
3. **Commit changes**.

## 3. Vercel

1. Sur vercel.com : **Add New → Project**, importe le dépôt GitHub.
2. Framework : **Other**. Aucune commande de build. **Deploy**.
3. Vercel te donne une adresse du type `https://marches-publics.vercel.app`.

## 4. Relier les deux

Dans Supabase, **Authentication → URL Configuration** :
- **Site URL** : l'adresse Vercel ;
- **Redirect URLs** : ajoute aussi l'adresse Vercel.

Sans cela, les liens de confirmation et de « mot de passe oublié » renverraient vers la mauvaise adresse.

## 5. Première connexion

1. Ouvre l'adresse Vercel → **Créer un compte** avec l'adresse e-mail mise dans `supabase.sql`.
2. Confirme via l'e-mail reçu (ou désactive la confirmation : **Authentication → Sign In / Providers → Email → Confirm email**).
3. Connecte-toi.

## Ajouter un collègue

Dans Supabase, **Table Editor → mp_membres → Insert row** : son adresse e-mail.
Il crée ensuite son compte sur le site avec cette adresse. Une personne qui crée un compte sans être dans `mp_membres` ne voit **aucune** donnée.

## À savoir (plan gratuit)

- Un projet sans activité pendant 7 jours est **mis en pause** : il se relance depuis le tableau de bord Supabase.
- Le plan gratuit n'a **pas de sauvegarde automatique** : exporte de temps en temps `mp_credits` et `mp_dossiers` (Table Editor → Export to CSV).
- Pour modifier le site : change `index.html` sur GitHub, Vercel republie tout seul.
