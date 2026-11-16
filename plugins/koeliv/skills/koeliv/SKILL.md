---
name: koeliv
description: Travailler avec les données d'une boutique suivie par Koeliv. À utiliser quand la personne parle de ses ventes, de sa publicité Google, de son stock, de son site, de sa boutique ou de Koeliv, ou demande un point, une revue ou un bilan.
---

# Travailler avec Koeliv

Deux connecteurs, chacun son rôle :
- **Koeliv** lit et modifie les comptes Google de la boutique : Google Ads (`ads_*`), Google Analytics 4 (`ga4_*`),
  Search Console (`gsc_*`), Merchant Center (`merchant_*`), et donne la fiche Koeliv de la boutique (`koeliv_*`).
- **Shopify**, le connecteur officiel de Shopify, couvre toute la boutique : ventes, commandes, produits, stock,
  clients, modifications comprises. Koeliv n'a aucun outil Shopify.

Les comptes Google sont choisis par Koeliv d'après l'adresse e-mail connectée : la personne ne voit que sa boutique.

## En début de conversation

1. Lisez `koeliv_fiche` : objectifs, saisons, règles, feuille de route et recommandations validés avec Koeliv.
   `koeliv_contexte` détaille un sujet (saisons, seuils, google-ads…), `koeliv_memoire` donne les constats,
   décisions et actions récents. Tenez-en compte dans toutes les réponses.
2. Pour l'équipe Koeliv : chaque outil demande le client (argument `client`). Un client par conversation.

## Lire

- Répondez avec les outils plutôt que de mémoire, et donnez toujours la période des chiffres.
- Google Ads : en cas de doute sur un champ, ou après une erreur de requête, vérifiez-le avec
  `ads_get_resource_metadata` (réponse longue : une ressource à la fois) ; `cost_micros` est en millionièmes de la
  devise.
- Shopify : uniquement par le connecteur Shopify officiel ; s'il n'est pas connecté, dites-le et proposez de le
  connecter (onglet Connecteurs du plugin Koeliv). Préférez les totaux (requêtes d'analyse ShopifyQL) aux commandes
  une à une, et n'affichez ni nom, ni e-mail, ni adresse d'acheteur.
- Croisez les sources quand la question le demande (ventes Shopify, trafic GA4, coût Ads), en signalant les écarts
  de définition plutôt que de les masquer.

## Réconcilier les sources

- Mêmes jours et même fuseau partout (vérifiez celui du compte Ads et de la propriété GA4) ; dites si les montants
  sont TTC ou HT, avec ou sans port.
- **Ventes : Shopify fait foi.** Ventes nettes = brutes − remises − retours, hors port ; ventes totales = avec port
  et taxes.
- **Google Ads** : `metrics.conversions` et `metrics.conversions_value` (actions d'achat principales), jamais
  `all_conversions*`, qui mêlent vues de page et objectifs secondaires. Ce sont des ventes attribuées par Google,
  pas des ventes en plus, rangées à la date du clic : les derniers jours se complètent encore.
- **GA4** ne voit que les visiteurs qui acceptent les cookies : il sert au comportement sur le site et aux
  proportions, pas au chiffre d'affaires.
- Rentabilité d'ensemble : dépense Google Ads face aux ventes Shopify. Entre campagnes : conversions Google, en
  relatif et sur 14 jours au moins. Marges et seuils viennent de la fiche ; s'ils manquent, dites-le au lieu d'en
  supposer.

## Modifier

Les outils d'écriture modifient vraiment les comptes :
- Google Ads : `ads_campagne_statut` (pause, réactivation), `ads_budget_regler`, `ads_mots_cles_negatifs`, et
  `ads_mutate` pour tout le reste ; `ads_create_asset`, `ads_manage_labels`, `ads_manage_label_associations`.
- GA4 : `ga4_evenement_cle_creer` / `_supprimer` (conversions), `ga4_dimension_creer`, `ga4_metrique_creer`,
  `ga4_personnalise_archiver`, `ga4_conservation_regler`, `ga4_create_property_annotation`.
- Merchant Center : `merchant_produit_modifier`, `merchant_produit_ecrire`, `merchant_produit_retirer`,
  `merchant_create_data_source`, `merchant_fetch_data_source`. Pour un produit venu de Shopify, la source principale
  est réécrite par l'app Shopify : préférez une source supplémentaire, ou corrigez le produit dans Shopify.
- Search Console : `gsc_sitemap_envoyer`, `gsc_sitemap_retirer`.

- Les règles de la fiche passent avant ce skill : si elle dit qu'un mandat est suspendu, qu'une modification se fait
  à la main par l'équipe Koeliv, ou fixe un plafond ou une période sans changement, préparez et simulez, mais
  n'appliquez pas.
- Avant chaque modification : dites précisément ce qui va changer (avant, après) et attendez l'accord explicite de
  la personne. Une seule modification à la fois.
- Google Ads : chaque outil simule d'abord (`validate_only` à vrai, le défaut) ; montrez le résultat, puis
  appliquez avec `validate_only` à faux seulement après l'accord.
- Après une modification, relisez la donnée pour confirmer qu'elle a bien changé.

## Routines

Dans le menu du connecteur : « revue_hebdo_pub » (le lundi), « point_stock » (avant un réassort), « sante_site »
(chaque semaine), « point_mensuel » (début de mois). Si la personne demande l'un de ces points sans passer par le
menu, suivez la même démarche.

## Si le connecteur ne répond pas

- Connecteur non connecté : dans Claude, Personnaliser, puis Connecteurs, Koeliv, Connecter ; la connexion se fait
  avec l'adresse e-mail donnée à Koeliv et un code reçu par e-mail.
- Accès refusé ou outil refusé : cette adresse ou ce compte n'est pas ouvert chez Koeliv. Dites à la personne de
  contacter Koeliv ; ne cherchez pas d'autre moyen d'accéder aux données.
