---
ms.assetid: 8be8c48d-790c-4199-b9d3-9f4a07d5ced2
title: Collecte d'informations réseau
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d88cc2fafd9aa4fc221efc901c48fa41ef007a21
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867040"
---
# <a name="collecting-network-information"></a>Collecte d'informations réseau

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La première étape de la conception d’une topologie de site efficace dans les Services de domaine Active Directory (AD DS) consiste à consulter les groupes de mise en réseau de votre organisation pour collecter des informations et communiquer avec eux régulièrement concernant votre topologie de réseau physique.  
  
## <a name="creating-a-location-map"></a>Création d’une carte d’emplacement

Créer une carte d’emplacement qui représente l’infrastructure réseau physique de votre organisation. Sur la carte d’emplacement, identifier les emplacements géographiques qui contiennent des groupes d’ordinateurs avec une connectivité interne de 10 mégabits par seconde (Mbits/s) ou supérieur (vitesse du réseau local (LAN) ou supérieur).  
  
## <a name="listing-communication-links-and-available-bandwidth"></a>Liste des liaisons de communication et la bande passante disponible

Après avoir créé une carte d’emplacement, le type de lien de communication, sa vitesse de liaison et la bande passante disponible entre chaque emplacement de document. Obtenir une topologie de réseau (étendu WAN) étendu à partir de votre groupe de mise en réseau. Pour obtenir la liste des types de circuit WAN courants et leurs bandes passantes, consultez la section « Détermination du coût » de [création d’un lien de Site](../../ad-ds/plan/Creating-a-Site-Link-Design.md). Vous devez ces informations pour créer des liens de sites plus loin dans le processus de conception de topologie de site.  
  
La bande passante fait référence à la quantité de données que vous pouvez transmettre sur un canal de communication dans un laps de temps donné. La bande passante disponible fait référence à la quantité de bande passante réellement disponible pour une utilisation par les services AD DS. Vous pouvez obtenir des informations de la bande passante disponible à partir de votre groupe de mise en réseau, ou vous pouvez analyser le trafic sur chaque lien à l’aide d’un analyseur de protocole, tel que moniteur réseau. Pour plus d’informations sur l’installation du Moniteur réseau, consultez l’article [surveillent le trafic réseau](https://go.microsoft.com/fwlink/?LinkId=107058).  
  
Documentez chaque emplacement et les autres emplacements qui lui sont liés. En outre, notez le type de lien de communication et de sa bande passante disponible. Pour une feuille de calcul pour vous aider à la liste des liaisons de communication et la bande passante disponible, consultez [travail aides pour Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip, et Ouvrez « Géographique emplacements et liaisons de Communication » (DSSTOPO_1.doc).  
  
## <a name="listing-ip-subnets-within-each-location"></a>Liste des sous-réseaux IP dans chaque emplacement

Une fois que vous documenter les liaisons de communication et la bande passante disponible entre chaque emplacement, enregistrez les sous-réseaux IP dans chaque emplacement. Si vous ne connaissez pas déjà le masque de sous-réseau et une adresse IP au sein de chaque emplacement, consultez votre groupe de mise en réseau.  
  
Les services AD DS associe une station de travail à un site en comparant l’adresse IP de la station de travail avec les sous-réseaux qui sont associés à chaque site. Lorsque vous ajoutez des contrôleurs de domaine à un domaine, les services AD DS également examine leurs adresses IP et les place dans le site le plus approprié.  
  
Pour une feuille de calcul pour vous aider à la liste des sous-réseaux IP dans chaque emplacement, consultez [travail aides pour Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip, puis ouvrez » Emplacements et sous-réseaux » (DSSTOPO_2.doc).  
  
> [!NOTE]  
> En plus du protocole IP version 4 adresses (IPv4), Windows Server prend également en charge le protocole IP version 6 (IPv6) des préfixes de sous-réseau. Pour une feuille de calcul pour vous aider à la liste les préfixes de sous-réseau IPv6, consultez [annexe a : Emplacements et préfixes de sous-réseau](../../ad-ds/plan/Appendix-A--Locations-and-Subnet-Prefixes.md).  

## <a name="listing-domains-and-number-of-users-for-each-location"></a>Liste des domaines et le nombre d’utilisateurs pour chaque emplacement

Le nombre d’utilisateurs pour chaque domaine régional est représentée dans un emplacement est un des facteurs qui déterminent le placement des contrôleurs de domaine régional et serveurs de catalogue global, qui est l’étape suivante dans le processus de conception de topologie de site. Par exemple, envisagez de placer un contrôleur de domaine régional dans un emplacement qui contient plus de 100 utilisateurs de domaine régional afin qu’ils peuvent toujours se connecter le domaine si le lien WAN échoue.  
  
Enregistrer les emplacements, les domaines qui sont représentés dans chaque emplacement et le nombre d’utilisateurs pour chaque domaine qui est représenté dans chaque emplacement. Pour une feuille de calcul pour vous aider à la liste des domaines et le nombre d’utilisateurs qui sont représentés dans chaque emplacement, consultez [travail aides pour Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), téléchargez Job_Aids_Designing_and_Deploying_Directory_and_ Security_Services.zip et ouvrez « domaines et les utilisateurs dans chaque emplacement » (DSSTOPO_3.doc).  
