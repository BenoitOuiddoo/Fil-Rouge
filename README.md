# Fil Rouge — blind test à l'envers

Inspiré du jeu de société *DJ Set*. On ne devine pas le titre : on cherche la **réponse** cachée
dans chaque chanson, qui la relie au **fil rouge** (le thème de la manche). La réponse peut être
dans le **titre**, les **paroles** ou le **nom de l'artiste**.

> Fil rouge « Couleurs » : *Purple Rain* (Prince) → **Violet** ; *Yellow Submarine* (The Beatles) → **Jaune**.

Page unique, sans dépendance à installer : `index.html` (logo et favicon embarqués).
En ligne : https://benoitouiddoo.github.io/Fil-Rouge/

## Jouer

- **Partie** : 3 fils rouges tirés au hasard × 6 musiques, en équipes, score cumulé.
- **Solo** : un seul fil rouge (6 musiques), sans équipe ni points — tuile 🎲 *Aléatoire* ou choix du fil rouge.
- **Durée d'écoute** : 15 s / 30 s / Tout. 15 s et 30 s démarrent au premier tiers du morceau.
- **Révéler la réponse** : bouton ; automatique en fin de morceau en mode *Tout*.
- **💡 Indice** (2 niveaux) : 1) où se cache la réponse (titre / paroles / artiste) ; 2) catégorie, si renseignée.
- **Pas de réponse en double** dans un même fil rouge (ex. une seule « pluie » par manche).
- **Anti-répétition** : les fils rouges et musiques déjà joués ne reviennent pas tant qu'un fil rouge
  n'est pas épuisé (historique par appareil, réinitialisable depuis l'accueil).
- **Réglages** (mémorisés par appareil) : **niveau** Facile / Moyen / Difficile et **genres** (Pop, Rock, Variété française, Rap, Soul/Funk/R&B, Disco/Électro, Jazz & crooners, Folk/country/reggae/BO). Ce sont des **priorités** : les titres correspondants sortent d'abord, le reste complète la manche.
- Noms d'équipe mémorisés par appareil. Lien *Quitter la partie* en cours de jeu.

## Héberger (GitHub Pages)

Repo → **Settings → Pages** → *Deploy from a branch*, branche `main`, dossier `/ (root)`.
Le fichier ne contient **aucun secret** (le Client ID Spotify est public par nature) : dépôt public OK.

## Configurer Spotify (une fois)

Le **Client ID est intégré** dans `index.html` : il suffit de cliquer « Se connecter à Spotify ».

Dans l'app Spotify (`https://developer.spotify.com/dashboard`, compte propriétaire) :
- APIs : **Web API** + **Web Playback SDK**.
- **Redirect URI** : l'URL exacte de la page (`https://benoitouiddoo.github.io/Fil-Rouge/`), au caractère près.
  À redéclarer à chaque changement d'URL.
- **User Management** : ajouter chaque compte joueur (Development mode, 5 comptes max).

Prérequis joueur : compte **Spotify Premium**, navigateur desktop ou mobile.

## Ajouter ou modifier des fils rouges

Tout est dans la constante `THEMES` en haut du `<script>` de `index.html` :

```js
{ name:"Couleurs", songs:[
  { t:"Purple Rain", a:"Prince", r:"Violet" },
  { t:"Amsterdam", a:"Jacques Brel", r:"Amsterdam", c:"une ville" },
  ...
]}
```

| Champ | Rôle | Obligatoire |
|---|---|---|
| `t` | titre | oui |
| `a` | artiste (doit être contenu dans le nom Spotify, ou l'inverse) | oui |
| `r` | réponse | oui |
| `w` | où se cache la réponse : `titre` / `paroles` / `artiste` (défaut `titre`) | non |
| `c` | catégorie affichée au 2ᵉ indice (ex. « une ville ») | non |
| `q` | recherche Spotify exacte, pour forcer une version (éviter live / remix) | non |
| `d` | niveau : 1 facile · 2 moyen · 3 difficile (popularité Deezer : ≥ 650 000 / ≥ 400 000 / en dessous) | non (défaut 2) |
| `g` | genre : `pop` `rock` `variete` `rap` `soul` `electro` `jazz` `autres` (attribué par artiste) | non (défaut `pop`) |

Règles de contenu : **au moins 6 réponses différentes** par fil rouge et **chaque chanson vérifiée**
(existence + bon artiste) avant ajout. Une même réponse peut apparaître plusieurs fois dans le pool :
le tirage (`pickDistinct`) garantit qu'elle ne sort **qu'une fois par fil rouge joué**.

## Limites connues

- **Premium** requis (le lecteur ne joue rien sans) ; seuls les comptes déclarés dans User Management peuvent lire.
- iOS/Safari : parfois 2 appuis sur « Lancer » au premier morceau.
- La **notification média** du téléphone affiche le titre en cours (spoiler si l'écran est visible).
- L'historique anti-répétition est **par appareil** (localStorage).

## Technique

- HTML/CSS/JS **vanilla**, fichier unique.
- Spotify **Web Playback SDK** (lecture) + **Web API** (recherche, contrôle, `position_ms` pour le départ au 1/3).
- Authentification **Authorization Code + PKCE** (front-end pur, sans secret).
- Persistance localStorage : partie en cours, historique, noms d'équipe, session Spotify.
- Polices Google Fonts : Syne, Inter.
