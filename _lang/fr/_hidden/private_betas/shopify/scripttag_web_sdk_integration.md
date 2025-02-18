---
nav_title: Intégration SDK Web via Shopify ScriptTag
article_title: "Intégration SDK Web via Shopify ScriptTag"
description: "Cet article explique comment intégrer le SDK Web via Shopify ScriptTag. "
page_type: partner
search_tag: Partenaire
permalink: "/scripttag_web_sdk_integration/"
hidden: true

---

# Intégration SDK Web via Shopify ScriptTag

> [Shopify ScriptTag](https://shopify.dev/api/admin-rest/2021-10/resources/scripttag#top) est un code JavaScript distant chargé dans les pages de votre boutique ou sur la page de statut de la commande lors du paiement. Lorsqu’une page de boutique est chargée, Shopify vérifie si des balises de script doivent être chargées sur la page du site. Dans le cadre du processus, les scripts du SDK Web de Braze seront chargés directement sur le site de votre boutique Shopify.

## Conditions préalables

Vérifiez avec votre équipe de développement que les éléments suivants sont bien pris en charge lors de la configuration de votre boutique Shopify. Si l’un des éléments suivants n’est pas pris en charge dans votre boutique Shopify, le SDK Web de Braze via Shopify ScriptTag ne peut pas être pris en charge.

| Configuration requise | Description |
| ----------- | ----------- |
| [API Ajax Shopify](https://shopify.dev/api/ajax) | Voici plusieurs utilisations possibles de l’API Ajax :<br>- Ajouter des produits au panier et mettre à jour le compteur d’articles de celui-ci.<br>- Afficher les recommandations relatives aux produits.<br>- Suggérer des produits et des collections aux visiteurs lorsqu’ils écrivent dans un champ de recherche.<br><br>Braze nécessite l’API Ajax, car nous allons récupérer les informations produit pour vos événements de produits. |
| [Gestion des jetons de panier par Shopify](https://shopify.dev/api/examples/cart) | Un panier contient des marchandises qu’un client a l’intention d’acheter, ainsi que le coût total estimé du panier. Vous pouvez utiliser l’[API Storefront](https://shopify.dev/api/storefront) pour interagir avec le panier d’un client lorsqu’il parcourt le site. <br><br>Braze nécessite la gestion des jetons de panier via Shopify directement, et non un système tiers pour récupérer l’ID de jeton de panier en cas de panier abandonné. |
| Gestion des URL par Shopify | Votre boutique devra suivre le modèle de chemin structuré des URL Shopify, où chacun des chemins vers les collections/produits adopte la structure suivante : <br>- /collections/collectionA<br>- /collections/collectionA/produits/produitA<br>- /produits/produitB |
{: .reset-td-br-1 .reset-td-br-2}

## Qu’est-ce que le SDK Web ?
Le SDK Web est un outil puissant utilisé pour suivre le comportement des clients dans votre boutique Shopify. Avec le SDK Web, vous pouvez obtenir des données de session, identifier les utilisateurs et enregistrer les données de comportement des utilisateurs via un navigateur Web ou mobile. En outre, vous pouvez déverrouiller les canaux de messagerie natifs (sur navigateur, par exemple) pour vous assurer d’envoyer le bon message, à l’utilisateur approprié, sur le canal approprié.

Pour plus d’informations, consultez la [présentation du SDK Web]({{site.baseurl}}/user_guide/onboarding_with_braze/web_sdk/).

## Que va prendre en charge le SDK Web sur ma boutique Shopify ?

Le SDK Web de Braze sur votre boutique Shopify prendra en charge les éléments suivants :
- [Suivi anonyme des utilisateurs]({{site.baseurl}}/user_guide/data_and_analytics/user_data_collection/user_profile_lifecycle/#anonymous-user-profiles) pour suivre l’activité des clients dans votre magasin
- [Suivi des utilisateurs actifs par mois]({{site.baseurl}}/user_guide/data_and_analytics/your_analytics_dashboards/understanding_your_app_usage_data/#monthly-active-users) : le SDK Web est capable de suivre les données de session des visiteurs de votre boutique
- Option pour obtenir les [données utilisateur]({{site.baseurl}}/user_guide/data_and_analytics/user_data_collection) Shopify qui compteront dans votre consommation de [point de données]({{site.baseurl}}/user_guide/onboarding_with_braze/data_points#data-points).
- Option pour autoriser l’implémentation de la [la messagerie sur navigateur]({{site.baseurl}}/user_guide/message_building_by_channel/in-app_messages/about/) comme canal sur votre boutique Shopify.

## Intégration

Vérifiez les informations suivantes du SDK Web avec vos développeurs pour éviter des problèmes au cours du processus d’intégration.

### Initialisation du SDK Web de Braze

L’initialisation du SDK Web au démarrage de la session sera nécessaire. Braze devra obtenir le `device_id` pour suivre les données d’utilisateurs anonymes, ainsi que d’autres identifiants (tels que l’identifiant client Shopify, l’e-mail ou le numéro de téléphone) susceptibles de ne pas être facilement accessibles aux visiteurs de votre boutique Shopify.

Le `device_id` sera également utilisé pour rapprocher les données utilisateur du profil utilisateur anonyme comme client, et fournit plus informations permettant une identification simplifiée (e-mail ou numéro de téléphone, par exemple) pendant et après le processus de paiement.

### Version du SDK Web de Braze

La version actuelle du SDK Web de Braze avec intégration de Shopify ScriptTag est v4.0.

Si le SDK Web est déjà installé sur votre boutique Shopify, consultez la [section suivante](#existing) pour voir ce que cela implique pour vous.

### Utilisateur actif par mois

Le SDK Web suit les sessions de vos visiteurs et clients Shopify. Par conséquent, ils seront ajoutés en tant qu’utilisateurs actifs mensuels (MAU) dans votre tableau de bord de Braze, vers vos attributions MAU. Il est important de noter que les utilisateurs anonymes comptent également dans votre MAU. Pour les appareils mobiles, les utilisateurs anonymes dépendent de l’appareil. Pour les utilisateurs Web, les utilisateurs anonymes dépendent du cache de navigateur. 

### Données utilisateur
Vous avez la possibilité d’inclure les événements suivants qui nécessiteront le SDK Web :
- Produit affiché
- Produit cliqué
- Panier abandonné

À l’heure actuelle, vous ne pouvez pas personnaliser les scripts Shopify pour inclure plus d’événements et de suivi d’attributs.

{% alert note %}
Si vous souhaitez ajouter davantage de personnalisation à l’implémentation du SDK Web de Braze via Shopify ScriptTag, vous devez contacter votre équipe de développement pour intégrer directement l’API Shopify ScriptTag.
{% endalert %}

## Comment l’installation du SDK Web de Braze est-elle réalisée sur ma boutique Shopify ?

### Aucune implémentation du SDK Web pré-existante

[Shopify ScriptTag](https://shopify.dev/api/admin-rest/2021-10/resources/scripttag#top) est un code JavaScript distant chargé dans les pages de votre boutique ou sur la page de statut de la commande lors du paiement. Lorsqu’une page de boutique est chargée, Shopify vérifie si des balises de script doivent être chargées sur la page du site. Dans le cadre du processus, les scripts du SDK Web de Braze seront chargés directement sur le site de votre boutique Shopify.

Dans le sélecteur d’événements de l’assistant de configuration Shopify, les événements marqués d’un astérisque (&#42;) sont pris en charge par le SDK Web. Si vous sélectionnez ces événements ou que vous y ajoutez une messagerie sur navigateur, Braze déterminera que l’implémentation du SDK Web via Shopify ScriptTag sera ajoutée à votre boutique Shopify dans le cadre de la configuration.

Après avoir installé l’application Shopify de Braze, vous serez redirigé vers Braze. Une fois installé, vous verrez la page de configuration Shopify.

### Que se passe-t-il si j’ai déjà activé le SDK Web ou Google Tag Manager sur ma boutique Shopify ? {#existing}

Si le SDK Web est déjà installé sur votre boutique Shopify, vous pouvez continuer à configurer Shopify ScriptTag dans le processus d’onboarding. Au cours du processus d’installation, Braze vérifiera s’il existe des instances du SDK Web déjà disponibles dans votre boutique Shopify. 

Nous ajouterons ensuite les scripts nécessaires pour que vous soyez sûr de bien suivre les événements sélectionnés ou activer la messagerie sur navigateur. 

Il est important de vérifier que votre intégration SDK Web comporte ou non les éléments suivants :
- La version SDK Web doit être v4.0 ou ultérieure
- Le SDK Web initialise le démarrage de session

Si les éléments ci-dessus ne sont pas satisfaits, l’intégration SDK Web via Shopify ScriptTag ne pourra pas être prise en charge.

### Que se passe-t-il si j’utilise une plateforme de données client comme Segment ou mParticle ?

Assurez-vous de désactiver les événements Shopify que vous avez pu envoyer via votre plateforme de données client.
