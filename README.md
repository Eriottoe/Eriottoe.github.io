# Relais Orbital

Une application qui tourne sur ton ordinateur et envoie une même vidéo verticale en **Short YouTube**, **Reel Instagram** et **Reel Facebook**, tout de suite ou à l'heure que tu choisis.

Rien n'est hébergé ailleurs : tes vidéos, tes identifiants et ton planning restent dans ce dossier.

---

## 1. Lancer l'application

1. Installe **Node.js** (version « LTS ») depuis <https://nodejs.org/fr>. C'est le seul prérequis, et il n'y a rien d'autre à installer ensuite.
2. Double-clique sur le lanceur de ton système :
   - Windows : `Demarrer-Windows.bat`
   - Mac : `Demarrer-Mac.command` (au premier lancement, fais clic droit → Ouvrir, car macOS bloque les scripts téléchargés)
   - Linux : `./demarrer-linux.sh`
3. Ton navigateur s'ouvre sur <http://localhost:4280>. Si ce n'est pas le cas, ouvre cette adresse à la main.

Une fenêtre de terminal reste ouverte : c'est l'application. **Tant qu'elle est ouverte, les publications programmées partent.** Ferme-la pour arrêter.

Pour essayer sans rien publier, va dans **Comptes → Réglages** et coche **Mode simulation**.

---

## 2. Relier YouTube (une dizaine de minutes, une seule fois)

Google demande que chaque application ait son propre « projet ». Il est gratuit.

1. Va sur <https://console.cloud.google.com> et crée un projet (le nom est libre).
2. Menu **API et services → Bibliothèque** : cherche **YouTube Data API v3** et clique sur **Activer**.
3. Menu **Google Auth Platform** (anciennement « Écran de consentement OAuth ») :
   - **Branding** : donne un nom à l'application et ton adresse e-mail. N'ajoute pas de logo (Google exigerait alors une validation).
   - **Audience** : choisis **Externe**. (« Interne » n'existe que pour les comptes Google Workspace d'entreprise.)
   - Choisis ensuite l'une des deux voies :

   **Voie rapide : rester en mode « Test ».** Dans **Audience → Utilisateurs tests**, ajoute le compte Google de ta chaîne. Tout fonctionne, mais Google coupe la connexion tous les 7 jours : il faut alors cliquer sur « Enregistrer et relier à nouveau » dans Comptes.

   **Voie durable : publier l'application.** Google ne débloque le bouton **Publier l'application** que si la page **Branding** contient deux adresses publiques (pas `localhost`) :
   - **Page d'accueil de l'application** : une page qui décrit l'application ;
   - **Règles de confidentialité** : une page qui explique ce que l'application fait des données.

   Ajoute le domaine de ces pages dans **Domaines autorisés**, enregistre, puis clique sur **Audience → Publier l'application**. La connexion ne tombe plus chaque semaine. La même page de confidentialité servira pour Meta (étape 3.5).
4. **Clients → Créer un client** :
   - Type : **Application Web**.
   - **URI de redirection autorisés** : colle l'adresse affichée dans Relais Orbital, page **Comptes** (bouton Copier). Par défaut : `http://localhost:4280/oauth/google/callback`.
   - Google affiche un **ID client** et un **code secret**. Copie-les tout de suite : le code secret n'est montré qu'à la création.
5. Dans Relais Orbital, page **Comptes**, colle l'ID client et le code secret, puis clique sur **Enregistrer et relier ma chaîne**.
6. Google affiche « Google n'a pas validé cette application ». C'est normal pour une application personnelle : clique sur **Paramètres avancés**, puis **Accéder à … (non sécurisé)**, et coche toutes les autorisations demandées.

Quota : un projet Google a droit à environ 100 envois de vidéos par jour.

---

## 3. Relier Facebook et Instagram (un quart d'heure, une seule fois)

Ce qu'il te faut avant de commencer :

- une **Page Facebook** (Facebook ne laisse pas les applications publier sur un profil personnel) ;
- un compte Instagram **professionnel** (Créateur ou Entreprise), **rattaché à cette Page**. Le rattachement se fait dans les paramètres de la Page, rubrique « Comptes associés ».

Les intitulés ci-dessous changent parfois d'une saison à l'autre chez Meta ; l'ordre des étapes, lui, reste le même.

1. Va sur <https://developers.facebook.com/apps> et clique sur **Créer une application**.
2. Parmi les cas d'usage proposés, choisis **Tout gérer sur votre Page** et **Gérer les messages et le contenu sur Instagram**.
3. Dans le tableau de bord de l'application, ouvre chaque cas d'usage (**Personnaliser**) et ajoute les autorisations suivantes :
   `pages_show_list`, `pages_read_engagement`, `pages_manage_posts`, `instagram_basic`, `instagram_content_publish`, `business_management`.
4. **Paramètres de l'application → Général** : note l'**ID de l'application** et la **clé secrète**.
5. **Passe l'application en mode « En ligne »** (interrupteur en haut du tableau de bord). Meta demande pour cela l'adresse d'une page de politique de confidentialité ; n'importe quelle page publique à toi convient.
   En mode « Développement », tes publications Facebook ne seraient visibles que par toi.
6. Ouvre le **Graph API Explorer** : <https://developers.facebook.com/tools/explorer>.
   - **Application Meta** : choisis la tienne.
   - **Utilisateur ou Page** : « Jeton utilisateur ».
   - **Autorisations** : ajoute les six autorisations de l'étape 3.
   - Clique sur **Generate Access Token**. Dans la fenêtre Facebook, coche bien ta Page **et** ton compte Instagram.
   - Copie le jeton affiché en haut.
7. Dans Relais Orbital, page **Comptes**, colle l'ID de l'application, la clé secrète et le jeton, puis clique sur **Enregistrer et relier**.

L'application transforme ce jeton en jeton longue durée et garde celui de ta Page, qui n'expire pas en temps normal. Si un jour Meta le refuse (changement de mot de passe, par exemple), l'application te le dira : il suffira de coller un nouveau jeton.

Tu n'as pas besoin de l'« App Review » de Meta : elle ne concerne que les applications utilisées par d'autres personnes que toi.

---

## 4. Publier et programmer

**Publier** : dépose une vidéo, écris la légende, coche les destinations, puis **Publier maintenant** ou **Plus tard**.

- La légende sert pour Instagram et Facebook, et devient la description sur YouTube. YouTube demande en plus un titre.
- Formats : MP4 de préférence (H.264, son AAC), vertical 9:16, 1080 × 1920 conseillé.
- Durées : 3 minutes au plus pour qu'une vidéo soit un Short ; 3 à 90 secondes pour un Reel Facebook envoyé par une application.

**Plus tard** : la publication part à l'heure dite **si l'application tourne à ce moment-là**. Si l'ordinateur était éteint ou en veille, et que le retard dépasse 30 minutes, la publication n'est pas envoyée : elle apparaît dans **À reprendre** et tu choisis de la publier tout de suite ou de la reprogrammer.

**Confier l'heure de sortie à YouTube et Facebook** : avec cette case, la vidéo leur est envoyée immédiatement et ce sont eux qui la sortent à l'heure prévue, même ordinateur éteint. Instagram ne propose pas cela : son Reel part toujours d'ici. Conditions : YouTube en visibilité « Publique » ; Facebook entre 20 minutes et 28 jours à l'avance.

**Planning** : la semaine en cours, case par case. Clique sur une publication pour voir où elle en est, changer l'heure, la relancer ou la supprimer. Supprimer ici ne retire pas une vidéo déjà reçue par une plateforme.

Limites imposées par les plateformes : environ 100 envois par jour sur YouTube, 100 publications par 24 h sur Instagram, 30 Reels par 24 h sur Facebook.

---

## 5. Tes données

Tout est dans le sous-dossier `data/` :

- `data/db.json` : réglages, planning, et **jetons d'accès à tes comptes**. Ne partage pas ce fichier et n'envoie pas le dossier `data/` à quelqu'un.
- `data/media/` : les vidéos en attente. Une vidéo est effacée d'ici dès que toutes ses destinations l'ont reçue (ton fichier d'origine n'est jamais touché).

L'application n'écoute que sur ton ordinateur (`localhost`) ; elle n'est pas joignable depuis le réseau.

Pour repartir de zéro, ferme l'application et supprime le dossier `data/`.

---

## 6. En cas de souci

| Ce que tu vois | Ce qu'il faut faire |
| --- | --- |
| « La connexion YouTube a expiré » | Relie la chaîne à nouveau. Si cela revient chaque semaine, l'application Google est restée en mode « Test » (étape 2.3). |
| `redirect_uri_mismatch` chez Google | L'adresse de redirection déclarée chez Google n'est pas exactement celle affichée dans Comptes. |
| « Le jeton Meta n'est plus valide » | Génère un nouveau jeton dans le Graph API Explorer et colle-le dans Comptes. |
| « Autorisations manquantes dans ce jeton » | Coche les autorisations indiquées dans le Graph API Explorer avant de générer le jeton. |
| Le Reel Facebook n'est visible que par toi | L'application Meta est restée en mode « Développement » (étape 3.5). |
| « Aucun compte Instagram professionnel n'est rattaché à cette Page » | Passe le compte Instagram en professionnel et rattache-le à la Page, puis colle un nouveau jeton. |
| Instagram ou Facebook « a refusé la vidéo » | Le message donne la raison ; le plus souvent le format (durée, définition, codec). Réexporte en MP4 H.264, 1080 × 1920. |
| Meta annonce qu'une version d'API n'existe plus | Comptes → Réglages → Version de l'API Meta : indique la version en cours (par exemple `v27.0`). |
| Le port 4280 est déjà pris | Lance avec un autre port : `PORT=4300 node server.js` (et déclare la nouvelle adresse de redirection chez Google). |

---

Polices : Russo One (Jovanny Lemonad) et Barlow (The Barlow Project Authors), sous licence SIL Open Font License 1.1.
