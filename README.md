# NOVA Chat — prototype v0.1

## Contenu
- `index.html` : interface interactive de démonstration (salons, messages locaux, création de salons, signalements, console de modération).
- `manifest.webmanifest` : métadonnées PWA.
- `sw.js` : cache de base pour le fonctionnement PWA après déploiement HTTPS.

## Important
Ce n'est PAS encore une vraie messagerie en ligne :
- Aucun compte ni authentification réelle.
- Les messages, salons et signalements restent uniquement dans la mémoire de la page.
- Les salons privés et la console de modération sont simulés.
- Ne publiez pas de vrais messages personnels dans cette version.

## Tester
Pour un simple aperçu, ouvrez `index.html` dans un navigateur. Les fonctions de service worker et l'installation PWA nécessitent un hébergement HTTPS ou un environnement local compatible.

## Architecture cible
- Frontend PWA : HTML/CSS/JS pour le prototype, puis éventuellement React/TypeScript.
- Authentification : Supabase Auth.
- Données : PostgreSQL via Supabase.
- Temps réel : Supabase Realtime, avec politiques d'accès.
- Hébergement : service statique HTTPS.
- Application native future : Capacitor ou développement natif dédié.

## Modèle de données à mettre en place côté serveur
- `profiles`: id lié à l'identité authentifiée, nom public, rôle non modifiable par l'utilisateur.
- `rooms`: nom, visibilité, créateur, date de création.
- `room_members`: salon, utilisateur, rôle dans le salon.
- `messages`: salon, auteur, texte, date, état de modération.
- `reports`: auteur du signalement, message/salon concerné, motif, statut.
- `moderation_audit`: acteur administratif, action, cible, date et motif.

## Règles de sécurité obligatoires avant la mise en ligne
1. Activer RLS sur toutes les tables et écrire des politiques testées pour chaque action.
2. Les utilisateurs ne peuvent lire les messages que des salons publics ou des salons privés dont ils sont membres.
3. Un utilisateur ne peut écrire qu'en son propre nom et seulement dans un salon où il a le droit de participer.
4. Le rôle administrateur ne doit jamais être contrôlable depuis le navigateur.
5. Les actions de modération doivent être vérifiées côté serveur et consignées dans un journal non modifiable par les utilisateurs ordinaires.
6. Ne jamais intégrer de clé secrète/service-role dans le frontend.
7. Prévoir signalement, blocage, limitation anti-spam, suppression de compte et politique de confidentialité avant ouverture publique.
8. Les administrateurs ne doivent accéder au contenu que pour des motifs de modération légitimes, avec accès restreint et journalisé.

## Prochaine étape
Créer un projet Supabase, définir le schéma SQL et les politiques RLS, puis connecter l'authentification et les messages temps réel. Ne pas inviter de vrais utilisateurs avant les tests de sécurité.
