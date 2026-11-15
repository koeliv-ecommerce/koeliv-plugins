---
name: koeliv
description: Travailler avec les données d'une boutique suivie par Koeliv. À utiliser quand la personne parle de ses ventes, de sa publicité Google, de son stock, de son site, de sa boutique ou de Koeliv, ou demande un point, une revue ou un bilan.
---

# Travailler avec Koeliv

Le connecteur Koeliv lit à l'instant les comptes de la boutique : Shopify (`shopify_*`), Google Ads (`ads_*`),
Google Analytics 4 (`ga4_*`), Search Console (`gsc_*`) et Merchant Center (`merchant_*`). Les comptes sont choisis
par Koeliv d'après l'adresse e-mail connectée : la personne ne voit que sa boutique. Tout est en lecture seule.

## Comment s'y prendre

- Répondez avec les outils plutôt que de mémoire, et donnez toujours la période des chiffres.
- Un client par conversation. Pour l'équipe Koeliv, chaque outil demande le client (argument `client`).
- Google Ads : avant une requête `ads_search`, vérifiez les champs avec `ads_get_resource_metadata` ; les montants
  `cost_micros` sont en millionièmes de la devise.
- GA4 : `ga4_run_report` pour le trafic et les ventes du site ; `ga4_get_custom_dimensions_and_metrics` pour les
  noms de champs.
- Shopify : `shopify_commandes`, `shopify_produits`, `shopify_clients`, et `shopify_shopifyql` pour les rapports
  (ventes, sessions). Les moyens de paiement et les coordonnées bancaires ne sont jamais accessibles.
- Croisez les sources quand la question le demande (ventes Shopify, trafic GA4, coût Ads), en signalant les écarts
  de définition plutôt que de les masquer.

## Si le connecteur ne répond pas

- Connecteur non connecté : dans Claude, Personnaliser, puis Connecteurs, Koeliv, Connecter ; la connexion se fait
  avec l'adresse e-mail donnée à Koeliv et un code reçu par e-mail.
- Accès refusé ou outil refusé : cette adresse ou ce compte n'est pas ouvert chez Koeliv. Dites à la personne de
  contacter Koeliv ; ne cherchez pas d'autre moyen d'accéder aux données.
