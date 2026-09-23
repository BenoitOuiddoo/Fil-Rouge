# Fil Rouge — blind test à thème

Blind test « à l'envers » : toutes les chansons d'un thème cachent le même fil conducteur
(une couleur, un prénom, un animal…). On ne cherche pas le titre, mais ce qui relie la chanson au thème.

Une partie = **3 thèmes tirés au hasard**, **6 chansons chacun** (18 titres), score cumulé par équipe.
La musique est jouée via **Spotify** (morceaux entiers), le titre n'est jamais affiché.

Page unique, sans dépendance à installer : `index.html`.

## Héberger (GitHub Pages)

1. Mettre `index.html` (et ce `README.md`) à la racine du dépôt.
2. Repo → **Settings → Pages** → *Build and deployment* → Source : **Deploy from a branch**,
   branche `main`, dossier `/ (root)` → **Save**.
3. Au bout d'une minute, le site est en ligne sur `https://<pseudo>.github.io/<repo>/`.

Le fichier ne contient **aucun secret** (le Client ID Spotify n'est pas confidentiel, pas de mot de passe) :
un dépôt **public** convient.

## Configurer Spotify (une fois)

Prérequis : un compte **Spotify Premium** et un navigateur **desktop ou mobile** (Chrome, Edge, Firefox).

1. `https://developer.spotify.com/dashboard` → **Create app**.
   - APIs : cocher **Web API** + **Web Playback SDK**.
   - **Redirect URI** : l'URL exacte de la page (ex. `https://<pseudo>.github.io/<repo>/`).
     À l'ouverture, le jeu affiche le Redirect URI exact à copier — c'est la valeur à déclarer.
2. Copier le **Client ID** (Settings de l'app).
3. Ouvrir la page, coller le Client ID, **Se connecter à Spotify**.

Le Client ID et la session sont mémorisés par navigateur (localStorage).

## Jouer

- Régler les équipes, **Lancer la partie** (3 thèmes surprise).
- Durée d'écoute réglable : **15 s / 30 s / Tout** (15 s et 30 s démarrent au premier tiers du morceau).
- Révéler le fil rouge manuellement, ou automatiquement en fin de morceau (mode « Tout »).
- Attribuer le point à l'équipe qui a trouvé, puis chanson / thème suivant.

## Ajouter ou modifier des thèmes

Tout est dans la constante `THEMES` en haut du `<script>` de `index.html` :

```js
{ name:"Couleurs", songs:[
  { t:"Purple Rain", a:"Prince", r:"Violet" },
  ...
]}
```

- `t` = titre, `a` = artiste, `r` = la réponse (le lien au thème).
- Le morceau est retrouvé automatiquement sur Spotify via titre + artiste (filtré par artiste).
- Pour forcer une version précise (éviter un live / remix), ajouter `q:"recherche exacte"` à la chanson.

## Limites connues

- **Premium complet** requis (le lecteur ne joue rien sans).
- Le morceau joué peut être une version live/remix du bon artiste : verrouiller avec `q` si besoin.
- La **notification média** du téléphone affiche le titre en cours (spoiler potentiel si l'écran est visible).
- L'historique anti-répétition est **par appareil** (localStorage), pas partagé entre appareils.

## Technique

- HTML/CSS/JS **vanilla**, fichier unique, aucune dépendance à builder.
- Spotify **Web Playback SDK** (lecture) + **Web API** (recherche, contrôle).
- Authentification **Authorization Code + PKCE** (front-end pur, sans secret).
- Logo : image PNG détourée, embarquée en base64 dans `index.html`.
- Polices Google Fonts : Syne, Inter.
