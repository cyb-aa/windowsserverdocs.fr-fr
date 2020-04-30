---
ms.assetid: 8be8c48d-790c-4199-b9d3-9f4a07d5ced2
title: Collecte d'informations réseau
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 6792d565e08a188e1957c67ce419676d4a044d82
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81624387"
---
# <a name="collecting-network-information"></a>Collecte d'informations réseau

> S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La première étape de la conception d’une topologie de site efficace dans Active Directory Domain Services (AD DS) consiste à consulter le groupe réseau de votre organisation pour collecter des informations et communiquer régulièrement avec eux sur votre topologie de réseau physique.

## <a name="creating-a-location-map"></a>Création d’une carte d’emplacements

Créez une carte d’emplacement qui représente l’infrastructure réseau physique de votre organisation. Sur la carte d’emplacement, identifiez les emplacements géographiques qui contiennent des groupes d’ordinateurs avec une connectivité interne de 10 mégabits par seconde (Mbits/s) ou plus (vitesse du réseau local).

## <a name="listing-communication-links-and-available-bandwidth"></a>Liste des liens de communication et de la bande passante disponible

Après avoir créé un mappage d’emplacement, documentez le type de lien de communication, sa vitesse de liaison et la bande passante disponible entre chaque emplacement. Obtenez une topologie de réseau étendu (WAN) à partir de votre groupe de mise en réseau. Pour obtenir la liste des types de circuits WAN courants et leurs bandes passantes, consultez la section « détermination du coût » dans [création d’une conception de lien de sites](../../ad-ds/plan/Creating-a-Site-Link-Design.md). Vous aurez besoin de ces informations pour créer des liens de sites ultérieurement dans le processus de conception de la topologie de site.

La bande passante fait référence à la quantité de données que vous pouvez transmettre sur un canal de communication dans un laps de temps donné. La bande passante disponible fait référence à la quantité de bande passante réellement disponible pour une utilisation par AD DS. Vous pouvez obtenir des informations de bande passante disponibles à partir de votre groupe de mise en réseau, ou vous pouvez analyser le trafic sur chaque lien à l’aide d’un analyseur de protocole tel que Moniteur réseau. Pour plus d’informations sur l’installation de Moniteur réseau, consultez l’article [surveillance du trafic réseau](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc783075(v=ws.10)).

Documentez chaque emplacement et les autres emplacements qui y sont liés. En outre, enregistrez le type de lien de communication et sa bande passante disponible. Pour obtenir une feuille de calcul qui vous aide à répertorier les liens de communication et la bande passante disponible, consultez [Outils d’aide pour le kit de déploiement Windows Server 2003](https://microsoft.com/download/details.aspx?id=9608), Télécharger Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip et ouvrez « emplacements géographiques et liens de communication » (DSSTOPO_1. doc).

## <a name="listing-ip-subnets-within-each-location"></a>Liste des sous-réseaux IP dans chaque emplacement

Une fois que vous avez documenté les liaisons de communication et la bande passante disponible entre chaque emplacement, enregistrez les sous-réseaux IP dans chaque emplacement. Si vous ne connaissez pas déjà le masque de sous-réseau et l’adresse IP dans chaque emplacement, consultez votre groupe de mise en réseau.

AD DS associe une station de travail à un site en comparant l’adresse IP de la station de travail aux sous-réseaux associés à chaque site. Lorsque vous ajoutez des contrôleurs de domaine à un domaine, AD DS examine également leurs adresses IP et les place dans le site le plus approprié.

Pour obtenir une feuille de calcul qui vous aide à répertorier les sous-réseaux IP dans chaque emplacement, consultez [Outils d’aide pour le kit de déploiement Windows Server 2003](https://microsoft.com/download/details.aspx?id=9608), Télécharger Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip et ouvrez « emplacements et sous-réseaux » (DSSTOPO_2. doc).

> [!NOTE]
> Outre les adresses IP version 4 (IPv4), Windows Server prend également en charge les préfixes de sous-réseau IP version 6 (IPv6). Pour obtenir une feuille de calcul qui vous aide à répertorier les préfixes de sous-réseau IPv6, consultez [annexe a : emplacements et préfixes de sous-réseau](../../ad-ds/plan/Appendix-A--Locations-and-Subnet-Prefixes.md).

## <a name="listing-domains-and-number-of-users-for-each-location"></a>Répertorier les domaines et le nombre d’utilisateurs pour chaque emplacement

Le nombre d’utilisateurs pour chaque domaine régional représenté dans un emplacement est l’un des facteurs qui déterminent l’emplacement des contrôleurs de domaine régionaux et des serveurs de catalogue global, qui est l’étape suivante du processus de conception de la topologie de site. Par exemple, envisagez de placer un contrôleur de domaine régional dans un emplacement contenant plus de 100 utilisateurs de domaine régionaux, afin qu’ils puissent toujours se connecter au domaine en cas d’échec de la liaison de réseau étendu.

Enregistrez les emplacements, les domaines représentés à chaque emplacement et le nombre d’utilisateurs pour chaque domaine représenté dans chaque emplacement. Pour obtenir une feuille de calcul qui vous aide à répertorier les domaines et le nombre d’utilisateurs représentés dans chaque emplacement, consultez [Outils d’aide pour le kit de déploiement Windows Server 2003](https://microsoft.com/download/details.aspx?id=9608), Télécharger Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip et ouvrez « domaines et utilisateurs dans chaque emplacement » (DSSTOPO_3. doc).
