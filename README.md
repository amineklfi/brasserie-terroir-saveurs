# 🍺 Brasserie Terroir & Saveurs — Site vitrine

> Page d'accueil artisanale pour une brasserie des Hauts-de-France, conçue sur **Lovable** puis intégrée dans **WordPress / Elementor** via un widget HTML autonome.

---

## 📋 Présentation du projet

**Brasserie Terroir & Saveurs** est un site vitrine fictif pour une brasserie artisanale fondée en 1923 dans les Hauts-de-France. Le projet a suivi deux phases :

1. **Conception UI sur Lovable** — prototype visuel complet avec React/Tailwind, hébergé sur `projet-guide-buddy.lovable.app`
2. **Portage Elementor** — le design a été retranscrit en un fichier HTML/CSS/JS autonome, à coller directement dans un widget *HTML personnalisé* d'Elementor, sans dépendance externe

---

## 🛠️ Stack technique

| Couche | Technologie |
|---|---|
| Prototypage | [Lovable](https://lovable.dev) (React + Tailwind CSS) |
| CMS cible | WordPress |
| Page builder | Elementor (widget HTML personnalisé) |
| Rendu final | HTML5 · CSS3 (custom properties) · JavaScript vanilla |
| Icônes | SVG inline (Lucide Icons) |
| Fonts | Georgia (serif) + Arial (sans-serif) — polices système, sans import |
| Images | Hébergées sur le CDN Lovable (`projet-guide-buddy.lovable.app/assets/`) |

---

## 📁 Structure du projet

```
brasserie-terroir-saveurs/
│
├── brasserie-elementor.html   # Fichier principal — à coller dans Elementor
└── README.md                  # Ce fichier
```

Tout le code (HTML + CSS + JS) est contenu dans **un seul fichier** pour faciliter le copier-coller dans Elementor.

---

## 🗂️ Architecture du fichier HTML

Le fichier est organisé en trois blocs distincts :

### 1. `<style>` — Styles CSS

Tous les styles sont **scopés** sous la classe `.bts-wrap` pour éviter tout conflit avec le thème WordPress ou les autres widgets Elementor.

**Variables CSS (design tokens) :**
```css
.bts-wrap {
  --copper:      #b5651d;   /* Couleur cuivre principale */
  --copper-deep: #7c3c0e;   /* Cuivre foncé (titres, logo) */
  --ocre:        #d4a042;   /* Ocre doré (accents, ornements) */
  --cream:       #faf6ef;   /* Crème (textes sur fond sombre) */
  --bg:          #fdf9f4;   /* Fond général */
  --card:        #fffdf9;   /* Fond des cartes produits */
  --border:      rgba(181,101,29,.18);
  --muted:       #7a6a5a;   /* Textes secondaires */
  --primary:     #8b4c12;   /* Boutons, liens actifs */
  --shadow-warm: 0 8px 32px rgba(124,60,14,.13);
}
```

**Responsive :** breakpoint unique à `768px` pour le menu burger, `720px` pour les grilles internes.

---

### 2. `<div class="bts-wrap">` — Structure HTML

La page est découpée en 7 sections, chacune avec un `id` servant d'ancre de navigation :

| Section | ID | Description |
|---|---|---|
| Navbar | `#bts-navbar` | Menu sticky avec burger mobile |
| Hero | `#bts-accueil` | Image plein écran avec overlay dégradé |
| À propos | `#bts-apropos` | Introduction + 3 piliers (icônes SVG) |
| Produits | `#bts-produits` | Grille de 5 cartes (3 visibles + 2 cachées) |
| Brasserie | `#bts-brasserie` | Section visite avec photo et CTA |
| Contact | `#bts-contact` | Infos de contact + lien mailto de réservation |
| Footer | — | Navigation, coordonnées, mentions légales |

---

### 3. `<script>` — JavaScript vanilla

Deux comportements interactifs, encapsulés dans une IIFE `(function() { ... })()` pour éviter les fuites de variables globales :

#### Menu burger
```js
btn.addEventListener('click', function() {
  var isOpen = menu.classList.toggle('open');
  btn.classList.toggle('open', isOpen);
  btn.setAttribute('aria-expanded', isOpen);
});
```
- Ouvre / ferme le drawer mobile
- Anime les 3 barres en croix via CSS (`transform: rotate`)
- Ferme automatiquement le menu au clic sur un lien

#### Affichage des produits supplémentaires
```js
toggleBtn.addEventListener('click', function() {
  expanded = !expanded;
  extras.forEach(function(card) {
    card.classList.toggle('visible', expanded);
  });
  toggleBtn.textContent = expanded ? 'Réduire la sélection ↑' : 'Voir tous les produits';
});
```
- Les 2 cartes supplémentaires ont la classe `.bts-card.extra` (cachées par défaut avec `display:none`)
- Au clic, la classe `.visible` les passe en `display:flex`
- Le texte du bouton bascule entre les deux états

---

## 🧩 Sections détaillées

### Navbar sticky
- Position `sticky top:0` avec `z-index: 999`
- Fond semi-transparent + `backdrop-filter: blur(10px)` pour l'effet verre
- Sur desktop (> 768px) : liens horizontaux
- Sur mobile (≤ 768px) : bouton burger + drawer vertical animé

### Hero
- Image de fond en `object-fit: cover` avec overlay `linear-gradient`
- Titre avec dégradé cuivre/ocre via `background-clip: text`
- 2 boutons CTA : *Découvrir nos produits* (`#bts-produits`) et *Visiter la brasserie* (`#bts-brasserie`)
- Hauteur : `min(88vh, 700px)`

### Grille produits
- CSS Grid avec `repeat(auto-fit, minmax(260px, 1fr))` — s'adapte automatiquement à la largeur
- 3 produits toujours visibles : Blonde, Brune, IPA (avec badge *Médaillée*)
- 2 produits masqués : Gin Terroir, Whisky Single Malt — révélés par le bouton

### Bouton "Réserver une visite"
```html
<button onclick="document.getElementById('bts-contact').scrollIntoView({behavior:'smooth'})">
  Réserver une visite
</button>
```
Scroll fluide vers la section `#bts-contact` via l'API native `scrollIntoView`.

### Navigation par ancres
Tous les liens du menu (desktop, mobile, footer) utilisent des ancres internes (`href="#bts-xxx"`). Le scroll fluide est activé globalement :
```css
html { scroll-behavior: smooth; }
```

---

## ⚙️ Installation dans Elementor

1. Ouvrir la page cible dans l'éditeur Elementor
2. Ajouter un widget **"HTML personnalisé"**
3. Ouvrir `brasserie-elementor.html`, copier **tout le contenu**
4. Coller dans le champ du widget
5. Cliquer sur **Mettre à jour / Publier**

> **Note :** Si le thème WordPress charge des styles globaux qui entrent en conflit (marges, typographie, couleurs de liens), ajouter `!important` sur les propriétés concernées dans le bloc `<style>`.

---

## 🖼️ Assets (images)

Les images sont servies directement depuis le CDN du projet Lovable :

| Fichier | Usage |
|---|---|
| `hero-brasserie-CYUneupn.jpg` | Fond du hero |
| `logo-D6oyY5SK.png` | Logo navbar + footer |
| `biere-blonde-BG9fQgaU.png` | Carte Bière Blonde |
| `biere-brune-DcybZNpg.png` | Carte Bière Brune |
| `biere-ipa-BzgZNmrN.png` | Carte Bière IPA |
| `brasserie-grains-Ok1KPV-r.jpg` | Section visite |

> Pour un usage en production, il est recommandé de télécharger ces images et de les héberger dans la médiathèque WordPress afin de ne pas dépendre du CDN Lovable.

---

## 🎨 Système de design

### Typographie
- **Titres / labels** : Arial, sans-serif — pour la lisibilité et le caractère moderne
- **Corps / italiques** : Georgia, serif — pour l'aspect artisanal et chaleureux
- Tailles fluides avec `clamp()` pour s'adapter à toutes les résolutions

### Palette
| Nom | Hex | Usage |
|---|---|---|
| Copper | `#b5651d` | Labels, accents |
| Copper Deep | `#7c3c0e` | Titres de marque |
| Ocre | `#d4a042` | Ornements, dégradés |
| Cream | `#faf6ef` | Textes sur fond sombre |
| Primary | `#8b4c12` | Boutons, CTA |
| Muted | `#7a6a5a` | Textes secondaires |

### Composants réutilisables
- `.btn-primary` — bouton plein cuivre
- `.btn-outline` — bouton contour transparent (sur fond sombre)
- `.btn-border` — bouton contour cuivre (sur fond clair)
- `.bts-card` — carte produit avec hover animé
- `.ornament` — séparateur décoratif avec lignes et icône centrale

---

## ✅ Points d'attention

- **Pas de framework JS** — JavaScript vanilla uniquement, aucun import externe
- **Pas de polices web** — Georgia et Arial sont des polices système, zéro requête réseau supplémentaire
- **Isolation CSS** — tout est préfixé `.bts-wrap` ou `.bts-` pour éviter les conflits Elementor/thème
- **Accessibilité de base** — `aria-label`, `aria-expanded`, `alt` sur toutes les images
- **Scroll smooth natif** — `scroll-behavior: smooth` + `scrollIntoView` sans librairie

---

## 📄 Licence

Projet réalisé à titre démonstratif. Les images et contenus sont fictifs.

## Lien

http://brasserie-akh.free.nf/?page_id=13
