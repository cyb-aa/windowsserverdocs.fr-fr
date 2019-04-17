---
title: Activation de MicrosoftWindowsServer
description: Comment activer WindowsServer2016.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 09/19/2018
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a45d5cc7a54
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 79f15c4c9c635138ae74a61fe1c259b97717155e
ms.sourcegitcommit: d622f7af181ed0063d716b30278d41887a57db19
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "9150849"
---
# Activation de WindowsServer2016

Les informations suivantes exposent les considérations de planification initiales à prendre en compte dans le cadre d’une activation KMS (Key Management Services, service de gestion de clés) impliquant WindowsServer2016. Pour plus d’informations sur les activations KMS impliquant des systèmes d’exploitation plus anciens que ceux répertoriés ici, voir [Étape 1: passer en revue et sélectionner les méthodes d’activation](https://technet.microsoft.com/library/jj134256(WS.11).aspx).

Le service KMS utilise un modèle client-serveur pour les clients actifs. Les clients KMS se connectent à un serveur KMS, nommé hôte KMS, pour l’activation. L’hôte KMS doit résider sur votre réseau local.

Les hôtes KMS n’ont pas besoin d’être des serveurs dédiés et le service KMS peut être co-hébergé avec d’autres services. Vous pouvez exécuter un hôte KMS sur n’importe quel système physique ou virtuel exécutant Windows10, WindowsServer2016, WindowsServer2012R2, Windows8.1 ou WindowsServer2012.

Un hôte KMS qui s’exécute sur Windows10 or Windows8.1 peut uniquement activer des ordinateurs qui exécutent des systèmes d’exploitation clients.
Le tableau suivant récapitule la configuration requise par le client et l’hôte KMS pour les réseaux qui incluent des clients WindowsServer2016 ou Windows10.

>[!NOTE]
>**Remarque:**  Mises à jour peuvent être requises sur le serveur KMS pour prendre en charge l’activation d’un de ces clients plus récents. Si vous recevez des erreurs d’activation, vérifiez que vous disposez des mises à jour appropriées répertoriées sous ce tableau.

|Groupe de clés de produit|Service KMS peut être hébergé sur|Éditions de Windows activées par cet hôte KMS|  
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|  
|Licence en volume pour WindowsServer2016|WindowsServer2012<br /><br />WindowsServer2012R2<br /><br />Windows Server2016<br /><br />|Canal semi-annuel WindowsServer <br><br>WindowsServer2016 (toutes éditions)<br /><br />Windows10 LTSB (2015 et 2016)<br /><br />Windows10Professionnel<br /><br />Windows10 Entreprise<br /><br />Windows 10 Professionnel pour les stations de travail<br><br>Windows10 Éducation<br><br>WindowsServer2012R2 (toutes éditions)<br /><br />Windows8.1Professionnel<br /><br />Windows8.1Entreprise<br /><br />WindowsServer2012 (toutes éditions)<br /><br />Windows Server 2008 R2 (toutes éditions)<br /><br />Windows Server 2008 (toutes éditions)<br /><br />Windows7Professionnel<br /><br />Windows7Entreprise| 
|Licence en volume pour Windows10|Windows7<br /><br />Windows8.1<br /><br /> Windows10|Windows10Professionnel<br /><br /> Windows10ProfessionnelN<br /><br /> Windows10Entreprise<br /><br /> Windows10EntrepriseN<br /><br /> Windows10Éducation<br /><br /> Windows10ÉducationN<br /><br /> Windows 10 entreprise LTSB (2015)<br /><br /> Windows 10 entreprise Ltsb (2015)<br /><br /> Windows 10 Professionnel pour les stations de travail<br><br>Windows8.1Professionnel<br /><br /> Windows8.1Entreprise<br /><br /> Windows7Professionnel<br /><br /> Windows7Entreprise<br /><br />|  
|Licence en volume pour «WindowsServer2012R2 pour Windows10»|WindowsServer2008R2<br /><br /> WindowsServer2012Standard<br /><br /> WindowsServer2012Datacenter<br /><br /> WindowsServer2012R2Standard<br /><br />WindowsServer2012R2Datacenter|Windows10Professionnel<br /><br /> Windows10 Entreprise<br /><br />Windows 10 entreprise LTSB (2015)<br><br>Windows 10 Professionnel pour les stations de travail<br><br>Windows10 Éducation<br><br> WindowsServer2012R2 (toutes éditions)<br /><br /> Windows8.1Professionnel<br /><br /> Windows8.1Entreprise<br /><br /> WindowsServer2012 (toutes éditions)<br /><br /> Windows Server 2008 R2 (toutes éditions)<br /><br />Windows Server 2008 (toutes éditions)<br /><br /> Windows7Professionnel<br /><br /> Windows7Entreprise|

> [!NOTE]  
>En fonction du système d’exploitation que votre serveur KMS exécute et des systèmes d’exploitation que vous souhaitez activer, il se peut que vous deviez installer une ou plusieurs de ces mises à jour :
>- Les installations du service de gestion de clés (KMS) sur Windows7 ou WindowsServer2008R2 doivent être mises à jour pour prendre en charge l’activation des clients qui exécutent Windows10. Pour plus d’informations, voir la[mise à jour qui permet à Windows 7 et les hôtes KMS de Windows Server 2008 R2 d’activer Windows 10](https://support.microsoft.com/help/3079821/update-that-enables-windows-7-and-windows-server-2008-r2-kms-hosts-to-activate-windows-10).  
>- Les installations du service de gestion de clés (KMS) sur WindowsServer2012 doivent être mises à jour pour prendre en charge l’activation des clients exécutant Windows10, WindowsServer2016 ou des systèmes d’exploitation client ou serveur plus récents. Pour plus d’informations, voir[juillet 2016 correctif cumulatif pour Windows Server 2012](https://support.microsoft.com/help/3172615/july-2016-update-rollup-for-windows-server-2012). 
>- Les installations du service de gestion de clés (KMS) sur Windows8.1 ou WindowsServer2012R2 doivent être mises à jour pour prendre en charge l’activation des clients exécutant Windows10, WindowsServer2016 ou des systèmes d’exploitation client ou serveur plus récents. Pour plus d’informations, voir[juillet 2016 correctif cumulatif pour Windows 8.1 et Windows Server 2012 R2](https://support.microsoft.com/help/3172614/july-2016-update-rollup-for-windows-8.1-and-windows-server-2012-r2).  
>- WindowsServer2008R2 ne peut pas être mis à jour pour prendre en charge l’activation de clients exécutant WindowsServer2016 ou un système d’exploitation plus récent. 

Un hôte KMS individuel peut prendre en charge un nombre illimité de clients KMS. Si vous possédez plus de 50clients, nous vous recommandons d’utiliser au moins deux hôtes KMS au cas où le premier deviendrait indisponible. La plupart des organisations peuvent fonctionner avec pas plus de deux hôtes KMS pour leur infrastructure complète.

# Prise en charge des conditions opérationnelles KMS
Le service KMS peut activer des ordinateurs physiques et virtuels, mais pour autoriser l’activation basée sur le service KMS, un réseau doit comporter un nombre minimal d’ordinateurs physiques (nommé seuil d’activation). Les clients KMS effectuent l’activation une fois seulement que ce seuil est atteint. Pour vérifier que ce seuil d’activation est atteint, un hôte KMS compte le nombre d’ordinateurs physiques qui demandent une activation sur le réseau.

Les hôtes KMS comptent les connexions les plus récentes. Lorsqu’un client ou un serveur contacte l’hôte KMS, ce dernier ajoute l’ID de l’ordinateur à son comptage, puis renvoie cette valeur dans sa réponse. Le client ou le serveur s’active si ce nombre est assez élevé. Les clients s’activent si ce nombre est supérieur ou égal à 25. Les serveurs et les éditions en volume de produits MicrosoftOffice s’activent si ce nombre est supérieur ou égal à5. Le service KMS ne compte que les connexions uniques dans les 30derniers jours et ne stocke que les 50contacts les plus récents.

Les activations KMS sont valides pendant 180jours, une période appelée intervalle de validité de l’activation. Les clients KMS doivent renouveler leur activation en se connectant à l’hôte KMS au moins une fois tous les 180jours pour rester activés. Par défaut, les ordinateurs clients KMS tentent de renouveler leur activation tous les sept jours. Une fois que l’activation d’un client a été renouvelée, l’intervalle de validité de l’activation est réinitialisé.

# Prise en charge des exigences fonctionnelles de KMS

Les activations basées sur le service KMS requièrent une connectivité TCP/IP. Les clients et les hôtes KMS sont configurés par défaut pour utiliser le système DNS (Domain Name System). Par défaut, les hôtes KMS utilisent la mise à jour dynamique DNS pour publier automatiquement les informations que les clients KMS doivent trouver et s’y connecter. Vous pouvez accepter ces paramètres par défaut ou, si vous avez une configuration requise spéciale de sécurité et de réseau, vous pouvez configurer manuellement les clients et les hôtes KMS.

Une fois que le premier hôte KMS a été activé, la clé KMS utilisée sur le premier hôte peut être utilisée pour activer jusqu’à cinq hôtes KMS supplémentaires sur votre réseau. Une fois qu’un hôte KMS a été activé, les administrateurs peuvent réactiver le même hôte jusqu’à neuf fois avec la même clé.

Si votre organisation a besoin de plus de six hôtes KMS, vous devez demander des activations supplémentaires pour la clé KMS de votre organisation; par exemple, si vous avez dix emplacements physiques sous un seul contrat de licence en volume et que vous voulez que chaque emplacement possède un hôte KMS local.

>[!NOTE] 
>Pour demander une telle exception, contactez votre Centre d’activation. Pour plus d’informations, voir la page [Licences en volume Microsoft]( https://www.microsoft.com/licensing).

Les ordinateurs qui exécutent des éditions de licence en volume de Windows10, WindowsServer2016, Windows8.1, WindowsServer2012R2, WindowsServer2012, Windows7 ou WindowsServer2008R2 sont, par défaut, des clients KMS et aucune configuration supplémentaire n’est nécessaire.

Si vous convertissez un ordinateur depuis un hôte KMS, une clé d’activation multiple (MAK) ou une version commerciale de Windows en un client KMS, installez la clé de configuration de client KMS applicable. Pour plus d’informations, voir les[Clés d’installation du Client KMS](KMSclientkeys.md). 
