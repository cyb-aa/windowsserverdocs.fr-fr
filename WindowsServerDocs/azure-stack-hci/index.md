---
title: Vue d’ensemble de pile HCI Azure
description: 'Azure Stack HCI est un cluster Windows Server 2019 hyperconvergé qui utilise validés matériel pour exécuter des charges de travail virtualisées local, si vous le souhaitez connexion aux services Azure pour la sauvegarde basée sur le cloud, restauration de site et bien plus encore. Azure solutions de pile HCI validé par Microsoft de matériel permettent d’assurer la fiabilité et des performances optimales et incluent la prise en charge des technologies telles que les lecteurs NVMe, la mémoire persistante et mise en réseau de (RDMA) un accès direct à distance à la mémoire.'
ms.technology: storage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 03/26/2019
---

# Vue d’ensemble de pile HCI Azure

>S’applique à: Windows Server 2019

Azure Stack HCI est un cluster Windows Server 2019 hyperconvergé qui utilise validés matériel pour exécuter des charges de travail virtualisées local, si vous le souhaitez connexion aux services Azure pour la sauvegarde basée sur le cloud, restauration de site et bien plus encore. Azure solutions de pile HCI validé par Microsoft de matériel permettent d’assurer la fiabilité et des performances optimales et incluent la prise en charge des technologies telles que les lecteurs NVMe, la mémoire persistante et mise en réseau de (RDMA) un accès direct à distance à la mémoire.

## La famille d’Azure Stack

Azure Stack HCI fait partie de l’Azure et famille Azure Stack, à l’aide de la même défini par logiciel de calcul, stockage et réseau logiciels en tant que Azure Stack. Voici un résumé rapide des différentes solutions:

- [Azure](https://azure.microsoft.com) - utiliser les services de cloud public
- [Azure Stack](https://azure.microsoft.com/overview/azure-stack) - opèrent sur site de services cloud
- [Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci) - exécuter virtualisé applications locaux, avec des connexions facultatives vers Azure

![Azure et Azure Stack exécuter des services cloud, pendant l’exécution d’Azure Stack HCI virtualisées applications locales](media/azure-and-azure-stack-family.png)

Pour en savoir plus:

- S’inscrire pour notre [événement virtuel de Cloud hybride](https://info.microsoft.com/ww-landing-building-a-successful-hybrid-cloud-strategy.html) 28 mars 2019.
- En savoir plus sur notre site Web solutions de [HCI de pile d’Azure](https://azure.microsoft.com/overview/azure-stack/hci) .
- Regardez des experts de Microsoft Jeff Woolsey et Vijay Tewari [Présentez les nouvelles solutions Azure Stack HCI](https://aka.ms/AzureStackOverviewVideo).

## Partenaires matériels

Vous pouvez acheter des solutions Azure Stack HCI validées qui s’exécutent Windows Server 2019 de 15 partenaires. Votre partenaire Microsoft préféré vous permet d’en cours d’exécution sans conception longue et le moment de la génération et offre un seul point de contact pour les services de support et d’implémentation.

Visitez le [site Web Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci) pour afficher plus de 70 Azure Stack HCI solutions actuellement disponibles à partir de ces partenaires de Microsoft: ASUS, Axellio, bluechip, DataON, Dell EMC, Fujitsu, HPE, Hitachi, Huawei, Lenovo, NEC, Solutions primeLine, QCT, SecureGUARD et Supermicro.

## Forum Aux Questions

### Ce que les solutions Azure Stack et Azure Stack HCI ont-ils en commun? 
Les solutions de pile HCI Azure fonctionnalité le même calcul de définition logicielle basé sur Hyper-V, le stockage et les technologies de réseau comme Azure Stack. Les deux offres répondent aux critères rigoureux de test et de validation pour assurer la fiabilité et la compatibilité avec la plateforme de matériel sous-jacent.

### Comment sont-elles différentes?
Avec la pile d’Azure, vous exécutez cloud services sur site. Vous pouvez exécuter Azure IaaS et PaaS services sur site pour constamment générer et exécuter des applications cloud n’importe où, gérées avec le portail Azure en local.

Avec Azure Stack HCI, vous exécutez des charges de travail virtualisées locaux, géré avec Windows Admin Center et familière Windows Server outils. Si vous le souhaitez, vous pouvez connecter à Azure pour des scénarios tels que la récupération de site basé sur le cloud, de surveillance et d’autres hybride.

### Pourquoi Microsoft générant le son HCI proposer à la famille d’Azure Stack? 
La technologie de Microsoft hyperconvergé est déjà le fondement d’Azure Stack. 

De nombreux clients Microsoft disposent d’environnements informatiques complexes et notre objectif est de fournir des solutions qui répondent à leur où ils se trouvent avec la technologie adéquate pour les entreprises droite ont besoin. Azure Stack HCI est une évolution des solutions Windows Server (WSSD) basée sur Windows Server 2016 auparavant disponibles à partir de nos partenaires de matériel. Nous introduits il la famille d’Azure Stack dans la mesure où nous avons commencé à l’offre de nouvelles options pour se connecter en toute transparence avec Azure pour les services de gestion d’infrastructure. 

### J’ai sera en mesure de mettre à niveau à partir d’Azure Stack HCI vers Azure Stack? 
Non, mais les clients peuvent migrer leurs charges de travail à partir d’Azure Stack HCI Azure Stack ou Azure.

### Les services Azure puis-je me connecter à Azure Stack HCI?

Pour une liste mise à jour des services Azure que vous pouvez connecter Azure Stack HCI à, voir [Connexion Windows Server aux services Azure hybrid](../azure-hybrid-services/index.md).

### Comment acheter des solutions Azure Stack HCI?
Procédez comme suit: 

1. Acheter un système matériel validé par Microsoft à partir de votre partenaire matériel préféré.
1. Installer Windows Server 2019 Datacenter edition et Windows Admin Center pour la gestion et la possibilité de se connecter à Azure pour les services de cloud
1. Si vous le souhaitez utiliser votre compte Azure pour lier les services de gestion et la sécurité basée sur le cloud à vos charges de travail.