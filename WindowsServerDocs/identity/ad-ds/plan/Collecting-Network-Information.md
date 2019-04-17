---
ms.assetid: 8be8c48d-790c-4199-b9d3-9f4a07d5ced2
title: "Collecte des informations de réseau"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 174f11ca85a659f9d0c52e220d5ce37b1804f39b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="collecting-network-information"></a>Collecte des informations de réseau

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

La première étape de la conception d’une topologie de site efficace dans les Services de domaine ActiveDirectory (ADDS) consiste à consulter le groupe de mise en réseau de votre organisation pour collecter des informations et communiquer avec eux régulièrement sur la topologie de votre réseau physique.  
  
## <a name="creating-a-location-map"></a>Création d’un mappage d’emplacement  
Créer un mappage de l’emplacement qui représente l’infrastructure réseau physique de votre organisation. Sur la carte d’emplacement, identifier les emplacements géographiques qui contiennent des groupes d’ordinateurs avec une connectivité interne de 10mégabits par seconde (Mbits/s) ou une version ultérieure (vitesse du réseau local (LAN) ou supérieure).  
  
## <a name="listing-communication-links-and-available-bandwidth"></a>Liste des liaisons de communication et la bande passante disponible  
Après avoir créé une carte d’emplacements, documentez le type de lien de communication, sa vitesse de liaison et la bande passante disponible entre chaque emplacement. Obtenir une topologie de réseau (étendu WAN) étendu à partir de votre groupe de mise en réseau. Pour obtenir la liste des types courants de circuit WAN et leur bande passante, voir la section «Détermination du coût» de [création d’une conception de lien de Site](../../ad-ds/plan/Creating-a-Site-Link-Design.md). Vous devez ces informations pour créer des liens de sites plus loin dans le processus de conception de topologie de site.  
  
La bande passante fait référence à la quantité de données que vous pouvez transmettre sur un canal de communication dans un laps de temps donné. La bande passante disponible fait référence à la quantité de bande passante réellement disponible pour une utilisation par les services ADDS. Vous pouvez obtenir des informations de la bande passante disponible à partir de votre groupe de mise en réseau, ou vous pouvez analyser le trafic sur chaque lien en utilisant un analyseur de protocole, tel que moniteur réseau, qui est un composant qui est fourni avec Windows Server2008. Pour plus d’informations sur l’installation du Moniteur réseau, consultez l’analyse du trafic réseau ([https://go.microsoft.com/fwlink/?LinkId=107058](https://go.microsoft.com/fwlink/?LinkId=107058)).  
  
Documentez chaque emplacement et les autres emplacements qui y sont liés. En outre, notez le type de lien de communication et de sa bande passante disponible. Pour une feuille de calcul pour vous aider à la liste des liaisons de communication et la bande passante disponible, voir le travail d’identificateurs d’applet pour Windows Server2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), télécharger Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip, et Ouvrez (DSSTOPO_1.doc) "Géographique emplacements et des liaisons de Communication".  
  
## <a name="listing-ip-subnets-within-each-location"></a>Liste des sous-réseaux IP dans chaque emplacement  
Une fois que vous documenter les liaisons de communication et la bande passante disponible entre chaque emplacement, enregistrez les sous-réseaux IP dans chaque emplacement. Si vous ne connaissez pas déjà le masque de sous-réseau et l’adresse IP au sein de chaque emplacement, consultez votre groupe de mise en réseau.  
  
Les services ADDS associe une station de travail à un site en comparant l’adresse IP de la station de travail avec les sous-réseaux qui sont associés à chaque site. Lorsque vous ajoutez des contrôleurs de domaine à un domaine, les services ADDS également examine leurs adresses IP et les place dans le site le plus approprié.  
  
Pour une feuille de calcul pour vous aider à la liste des sous-réseaux IP dans chaque emplacement, voir tâche identificateurs d’applet pour Windows Server2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip et ouvrir" emplacements et sous-réseaux «(DSSTOPO_2.doc).  
  
> [!NOTE]  
> En plus de IP version4 (IPv4) adresses, Windows Server2008 prend également en charge IP version6 (IPv6) des préfixes de sous-réseau. Pour plus d’une feuille de calcul pour vous aider à la liste des préfixes de sous-réseau IPv6, consultez [annexe a: emplacements et préfixes de sous-réseau](../../ad-ds/plan/Appendix-A--Locations-and-Subnet-Prefixes.md).  
  
## <a name="listing-domains-and-number-of-users-for-each-location"></a>Liste des domaines et le nombre d’utilisateurs pour chaque emplacement  
Le nombre d’utilisateurs pour chaque domaine régional est représenté dans un emplacement est un des facteurs qui déterminent le placement de contrôleurs de domaine régional et serveurs de catalogue global, qui est l’étape suivante du processus de conception de topologie de site. Par exemple, envisagez de placer un contrôleur de domaine régional dans un emplacement qui contient plus de 100utilisateurs de domaine régional afin qu’ils peuvent toujours se connecter le domaine en cas d’échec de la liaison WAN.  
  
Enregistrez les emplacements, les domaines qui sont représentées dans chaque emplacement et le nombre d’utilisateurs pour chaque domaine qui est représenté dans chaque emplacement. Pour une feuille de calcul pour vous aider à la liste des domaines et le nombre d’utilisateurs qui sont représentés dans chaque emplacement, voir tâche identificateurs d’applet pour Windows Server2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), téléchargez < DICT__Job_Aids_Designing_and_Deploying_ Directory_and_Security_Services.zip>Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip</DICT__Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip >, ouvrez "Domaines et les utilisateurs dans chaque emplacement" (DSSTOPO_3.doc).  
  


