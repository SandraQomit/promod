# Qomit — Core Checkout Library (composants)

Bibliothèque de composants codés depuis Figma « Core Checkout Library », pensée pour être
**re-thémable** : chaque composant ne référence que des **CSS custom properties** définies
dans une table unique. Changer la table → tous les composants changent d'aspect, sans
toucher au code des composants.

## Principe

```
css/
  tokens.css   ← LA TABLE (source unique de vérité). C'est le SEUL fichier à remplacer.
  reset.css
  header.css   ← composant Header (ne contient que des var(--…), zéro valeur en dur)
index.html     ← page de démo (tous les variants)
components/    ← (à venir : autres composants)
```

## La table de tokens actuelle

Extraite 1:1 des variables Figma (`get_variable_defs` sur le composant Header).
Correspondance nom Figma → variable CSS :

| Figma | CSS | Valeur |
|---|---|---|
| `bg/neutral/default` | `--bg-neutral-default` | `#ffffff` |
| `type/color/body` | `--type-color-body` | `#000000` |
| `type/color/mention` | `--type-color-mention` | `#666666` |
| `icon/color/primary` | `--icon-color-primary` | `#000000` |
| `border/color/default` | `--border-color-default` | `#929292` |
| `border/color/soft` | `--border-color-soft` | `#cccccc` |
| `border/color/ultra soft` | `--border-color-ultra-soft` | `#dfdfdf` |
| _(brand, DS primary/600)_ | `--color-primary` | `#ff6b4a` |
| `type/family/body` | `--type-family-body` | `Inter` |
| `type/size/body large` · `body 01` | `--type-size-body-large` · `-01` | `16px` |
| `type/size/body 02` · `body small` | `--type-size-body-02` · `-small` | `14px` |
| `type/weight/300` (regular) | `--type-weight-300` | `400` |
| `type/weight/500` (semi-bold) | `--type-weight-500` | `600` |
| `container/margins/XS·S·M·L` | `--container-margins-xs…l` | `12·16·20·24px` |
| `padding/600` | `--padding-600` | `24px` |
| `Theme/Block/block-padding-y` | `--block-padding-y` | `12px` |
| `container/height/5XL` | `--container-height-5xl` | `56px` |
| `border/width/default` | `--border-width-default` | `1px` |

## Comment re-thémer (quand tu me donnes une nouvelle table)

Je remplace uniquement les valeurs dans `css/tokens.css`. Aucun composant n'est modifié.
Exemple : passer `--color-primary` de `#ff6b4a` à `#3E63BE` re-colore le logo partout.

## Composants

- **Header** (`css/header.css`) — variants `Device = desktop | mobile` × `Type = stepper | accordion`,
  + bouton fermer (desktop) et flèche retour (mobile).
- **recap-order** (`css/recap.css`) — récapitulatif de commande. Panneau desktop (lignes produit
  avec badge quantité, remise, prix barré, récap + total) et panneau mobile repliable
  (`Open=on/off`, carte fidélité). Vignettes produit = placeholders (photos non fournies).
- **Delivery** (`css/delivery.css`) — choix du mode de livraison en cartes radio
  (`Type = pickup | store | home`, `Selected = off | on`). Carte sélectionnée dépliée
  avec encart adresse + formulaire. Définit aussi des **primitives réutilisables** :
  `.qc-radio`, `.qc-field` (input), `.qc-btn--tertiary`, drapeau `.qc-flag`. Interactif
  (clic = sélection/dépliage).
- **Payment Modules** (`css/payment.css`) — choix du moyen de paiement en cartes radio :
  CB (+ formulaire carte : adresse facturation, titulaire, n° carte, expiration/CVC,
  toggle « Enregistrer »), Apple/Google Pay, PayPal, Alma, Carte cadeau (input « filled »
  + `✓ Carte validée`), et bloc `secured_payment`. Nouvelles primitives :
  `.qc-field--filled`, `.qc-toggle`, `.qc-success`, `.qc-brand`. Interactif.

Voir tous les variants dans `index.html`.

## Page assemblée — `checkout.html`

Page checkout complète (maquette « Checkout Screens ») assemblée à partir des 4 composants :
header + **2 colonnes** (formulaire *Livraison* / *Paiement* / action à gauche, récap commande
à droite avec séparateur vertical). **Responsive** : sous 900px, une seule colonne avec le
récap repliable en haut. Ajoute 2 primitives : `.qc-btn--primary` (bouton noir) et
`.qc-checkbox`. Ouvre `checkout.html`.

### Tokens ajoutés par `recap-order`

`--foreground-primary/secondary/on-secondary`, `--type-body`, `--type-information`,
`--link-level1-default`, `--bg-neutral-muted/subtle/darkest`, `--border-color-inverted`,
`--feedback-information-bg/border`, `--feedback-error-bg`, `--feedback-error-text` (approx),
`--type-family-mention`, `--type-size-mention`, `--type-weight-600`,
`--container-margins-none/3xs/2xs/5xl`, `--container-padding-xl`, `--scale-12`,
`--gap-200/500`, `--border-weight-xs`, `--radius-100`, `--border-radius-default/full`.

## Voir le rendu

Ouvre `index.html` dans un navigateur (aucune build nécessaire).
