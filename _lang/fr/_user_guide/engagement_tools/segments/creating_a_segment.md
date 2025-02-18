---
nav_title: Créer un segment
article_title: Créer un segment
page_order: 1
page_type: tutorial
description: "Cet article pratique vous explique comment configurer et créer un segment avec Braze."
tool: Segments

---

# [![Cours d’apprentissage Braze]({% image_buster /assets/img/bl_icon2.png %})](https://learning.braze.com/segmentation-course){: style="float:right;width:120px;border:0;" class="noimgborder"}Créer un segment

> Cet article vous guidera étape par étape à travers la création d’un segment, le filtrage de votre audience cible, la navigation dans les segments et l’archivage.

Vos développeurs ont intégré le SDK, et les données de vos utilisateurs commencent à affluer. Et maintenant ? Il est temps de commencer à segmenter vos utilisateurs.

La segmentation vous permet de cibler les utilisateurs en fonction de leurs actions et de leurs caractéristiques démographiques, comportementales, sociales ou techniques. La segmentation et les messages automatiques peuvent être utilisés de manière intelligente et créative pour transformer vos prospects en clients à long terme. Les segments sont mis à jour en temps réel en fonction des modifications de données, et vous pouvez créer autant de segments que nécessaire pour remplir vos objectifs de ciblage et de communication.

## Étape 1 : Accédez à la section Segments

![Section Engagement avec l’onglet Segments mis en surbrillance.][1]{: style="float:right;max-width:20%;"}

Accédez à la page **Segments** située dans **Engagements**.

## Étape 2 : Nommez votre segment

Cliquez sur <i class="fas fa-plus"></i> **Create Segment (Créer un segment)** pour concevoir votre segment. Nommez votre segment en décrivant le type d’utilisateur que vous souhaitez cibler. Cela garantira que ce segment pourra servir de cible pour plusieurs campagnes ou Canvas à venir. Les segments qui comportent des titres vagues peuvent prêter à confusion.

Vous pouvez aussi ajouter une description à votre segment pour fournir plus de détails sur l’intention de cette audience et laisser des notes pour les autres membres de votre équipe.

![Créez un modal de segment avec un segment nommé « Lapsed Users (Utilisateurs inactifs) » et pour lequel la description du segment est « This is our main Lapsed User segment to target non-actives within the past fourteen days (Il s’agit de notre principal segment d’utilisateurs inactifs pour cibler les utilisateurs non-actifs au cours des quatorze derniers jours) ». avec deux boutons : Cancel (Annuler) et Create Segment (Créer un segment).][2]{: style="max-width:70%;"}

## Étape 3 : Choisissez votre application ou plateforme

Choisissez les applications ou plateformes que vous souhaitez cibler en sélectionnant **Include users from all apps (Inclure les utilisateurs de toutes les applications)** (par défaut) ou en décochant la case. Désélectionnez cette option si vous souhaitez choisir les applications ou plateformes à inclure dans votre segment. Par exemple, si vous souhaitez envoyer un message dans l’application uniquement aux appareils iOS, sélectionnez votre application iOS. Cela permettra aux utilisateurs qui peuvent utiliser à la fois un appareil iOS et Android de recevoir uniquement le message sur leur appareil iOS.

Pour plus d’informations sur cette option, reportez-vous à la section [Calcul de l’appartenance à un segment](#segment-membership-calculation).

![Panneau Segment Details (Informations relatives au segment) avec la case « Include users from all apps (Inclure les utilisateurs de toutes les applications) » non cochée dans la section Apps Used (Applications utilisées).][5]

## Étape 4 : Ajoutez des filtres à votre segment

Ajoutez au moins un filtre à votre segment comme illustré dans l’image ci-dessous. Vous pouvez combiner autant de filtres que vous le souhaitez pour que votre segmentation soit plus spécifique.

{% alert note %}
Braze ne crée pas de profils pour les utilisateurs tant qu’ils n’ont pas utilisé l’application une première fois, ce qui signifie que vous ne pouvez pas cibler des utilisateurs qui n’ont pas encore ouvert votre application.
{% endalert %}

![Filtres du segment avec la condition « OR (OU) » sélectionnée.][3]

Si vous choisissez « OR (OU) » pour vos filtres, votre segment contiendra des utilisateurs qui remplissent un filtre, certains filtres ou tous ces filtres, tandis que la condition « AND (ET) » signifie que les utilisateurs qui ne remplissent pas ce filtre ne seront pas inclus dans votre segment. Cette logique peut être combinée afin de segmenter les utilisateurs qui remplissent un filtre « ET » un ou deux autres filtres.

Notez que les statistiques de votre segment s’actualisent en temps réel lorsque vous ajoutez ou enlevez des filtres. Gardez à l’esprit que ces statistiques sont des estimations (+/- 1 %) et que la base utilisateur exacte du segment est toujours calculée avant qu’un segment ne reçoive un message envoyé dans le cadre d’une campagne ou d’un Canvas. Notez qu’une erreur s’affichera si le segment que vous indiquez dans l’un de vos segments imbriqués est déjà archivé. 

{% alert important %}
Les segments qui utilisent déjà le filtre Segment Membership (Appartenance à un segment) ne peuvent pas être inclus ou imbriqués dans d’autres segments.
{% endalert %}

### Segments mono-utilisateur

Vous pouvez créer des segments comportant un seul utilisateur (ou une poignée d’utilisateurs) en utilisant des attributs uniques qui identifient les utilisateurs, comme un nom d’utilisateur ou un ID utilisateur.

Cependant, il se peut que cet utilisateur individuel ne soit pas reflété dans les statistiques ou l’aperçu de segmentation, car les statistiques des segments sont calculées sur la base d’un échantillon aléatoire avec un degré de confiance de 95 % que le résultat est compris entre +/- 1 %. Plus votre base d’utilisateurs est grande, plus il est probable que la taille de votre segment soit une estimation approximative. Pour vous assurer que votre segment contient le seul utilisateur que vous souhaitez cibler, cliquez sur **Calculate Exact Statistics (Calculer les statistiques exactes)** sur la page **Segment Details (Informations relatives au segment)**. Cela calculera le nombre exact d’utilisateurs dans votre segment, sans arrondi.

Braze propose des filtres de test pour cibler des utilisateurs spécifiques en fonction de leur ID utilisateur ou de leur adresse e-mail.

## Étape 5 : Enregistrez votre segment

Cliquez sur **Save (Enregistrer)** et vous serez prêt(e) à envoyer des messages à vos utilisateurs !

## Calcul de l’appartenance à un segment {#segment-membership-calculation}

Braze met à jour l’appartenance des utilisateurs à un segment au fur et à mesure que nos serveurs reçoivent et traitent les données, ce qui se produit généralement de manière instantanée. L’appartenance d’un utilisateur à un segment donné ne changera pas tant que cette session n’a pas été traitée. Par exemple, un utilisateur faisant partie d’un segment d’utilisateurs inactifs au début d’une session sera immédiatement sorti du segment d’utilisateurs inactifs une fois la session traitée.

De plus, l’appartenance au segment est calculée différemment lorsque la case **Include users from all apps (Inclure les utilisateurs de toutes les applications)** est cochée ou lorsqu’un groupe d’apps ne comporte qu’une seule application. Dans ce type de scénarios, la base utilisateur du segment inclut tous les utilisateurs : ceux qui ont des sessions enregistrées, ainsi que les utilisateurs qui n’ont aucune session ni aucune donnée d’application (généralement créée via User Import ou l’API REST).

Si la case **Include users from all apps (Inclure les utilisateurs de toutes les applications)** est décochée et que vous avez plus d’une application dans votre groupe d’apps, la l’appartenance à un segment inclura uniquement les utilisateurs qui ont des sessions enregistrées dans les applications sélectionnées, et exclura tous les utilisateurs sans sessions ni données d’application. Par conséquent, le total des segments individuels d’une seule application sélectionnée ne sera pas égal à un segment dont la case **Include users from all apps (Inclure les utilisateurs de toutes les applications)** a été cochée.

## Archivage des segments

Si vous n’avez plus besoin d’un segment ou que vous souhaitez l’abandonner, vous pouvez l’archiver en allant sur la page **Segments**, en cliquant sur l’icône en forme d’engrenage, puis en sélectionnant « Archive (Archiver) » dans la liste déroulante qui apparaît.

{% alert warning %}
Lorsque vous archivez un segment, toutes les campagnes et tous les Canvas qui utilisent ce segment seront également archivés (même si le segment est uniquement utilisé dans une seule Canvas Step). Vous recevrez un avertissement indiquant quels Canvas ou campagnes sont sur le point d’être archivés avec le segment associé.
{% endalert %}

Vous pouvez décompresser le segment en y accédant via la page Segments, puis en sélectionnant **Décompresser**.

[1]: {% image_buster /assets/img_archive/Segment1.png %}
[2]: {% image_buster /assets/img_archive/Segment2.png %}
[3]: {% image_buster /assets/img_archive/segment_step4.png %}
[5]: {% image_buster /assets/img_archive/segment_app_selection.png %}
