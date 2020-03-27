---
title: Déplacer les paramètres et données vers le serveur de destination pour la migration vers Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2b882e87-347a-4010-b7fd-9599d61198dd
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 4f4ba08c17429f70ef754b0861553e38ba116e5d
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318835"
---
# <a name="move-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Déplacer les paramètres et données vers le serveur de destination pour la migration vers Windows Server Essentials

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Déplacez les paramètres et les données vers le serveur de destination comme suit :

1. [Copier les données sur le serveur de destination](#copy-data-to-the-destination-server)

2. [Configurer le réseau](#configure-the-network) 

3. [Mapper les ordinateurs autorisés aux comptes d’utilisateur](#map-permitted-computers-to-user-accounts)
 
## <a name="copy-data-to-the-destination-server"></a>Copier les données vers le serveur de destination
 Avant de copier des données du serveur source vers le serveur de destination, effectuez les tâches suivantes : 
 
- Passez en revue la liste des dossiers partagés sur le serveur source, y compris les autorisations de chaque dossier. Créez ou personnalisez les dossiers sur le serveur de destination afin qu'ils correspondent à la structure de dossiers que vous migrez depuis le serveur source. 
 
- Passez en revue la taille de chaque dossier et vérifiez que le serveur de destination dispose de suffisamment d'espace de stockage. 
 
- Mettez en lecture seule les dossiers partagés sur le serveur source pour tous les utilisateurs afin qu'aucune écriture ne puisse être réalisée sur le lecteur pendant que vous copiez les fichiers vers le serveur de destination. 
 
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Pour copier les données du serveur source vers le serveur de destination 
 
1. Ouvrez une session sur le serveur de destination en tant qu'administrateur de domaine, puis ouvrez une fenêtre de commande. 
 
2. À l'invite de commandes, tapez la commande suivante, puis appuyez sur ENTRÉE : 
 
 `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt` 
 
 Où :
 - \<SourceServerName\> est le nom du serveur source
 - \<Nomdossiersourcepartagé\> est le nom du dossier partagé sur le serveur source
 - \<NomServeurDestination\> est le nom du serveur de destination,
 - \<Nomdossierdestinationpartagé\> est le dossier partagé sur le serveur de destination vers lequel les données seront copiées. 
 
3. Répétez l'étape précédente pour chaque dossier partagé que vous migrez depuis le serveur source. 
 
## <a name="configure-the-network"></a>Configurer le réseau
 Après avoir déplacé le rôle DHCP vers le routeur, configurez les paramètres réseau sur le serveur de destination. 
 
#### <a name="to-configure-the-network"></a>Pour configurer le réseau 
 
1. Sur le serveur de destination, ouvrez le tableau de bord. 
 
2. Dans la page d'**accueil** du tableau de bord, cliquez sur **CONFIGURATION**, sur **Configurer l'Accès en tout lieu**, puis choisissez l'option **Cliquez pour configurer l'Accès en tout lieu**. 
 
3. Suivez les instructions de l'Assistant pour configurer votre routeur et les noms de domaine. 
 
 Si votre routeur ne prend pas en charge l'infrastructure UPnP, ou si l'infrastructure UPnP est désactivée, une icône d'avertissement jaune peut apparaître en regard du nom du routeur. Vérifiez que les ports suivants sont ouverts et qu'ils sont dirigés vers l'adresse IP du serveur de destination : 
 
- Port 80 : trafic web HTTP 
 
- Port 443 : trafic web HTTPS 
 
## <a name="map-permitted-computers-to-user-accounts"></a>Mapper les ordinateurs autorisés aux comptes d'utilisateur
 Chaque compte d'utilisateur qui est migré à partir du serveur source doit être mappé à un ou plusieurs ordinateurs. 
 
#### <a name="to-map-user-accounts-to-computers"></a>Pour mapper les comptes d'utilisateur aux ordinateurs
 
1. Ouvrez le tableau de bord Windows Server Essentials. 
 
2. Dans la barre de navigation, cliquez sur **Utilisateurs**. 
 
3. Dans la liste des comptes d'utilisateur, cliquez avec le bouton droit sur un compte d'utilisateur, puis cliquez sur **Afficher les propriétés du compte**. 
 
4. Cliquez sur l'onglet **Accès en tout lieu**, puis sur **Autoriser l'accès web à distance et l'accès aux applications de services Web**. 
 
5. Sélectionnez **Dossiers partagés**, **Ordinateurs**, puis **Liens vers la page d'accueil** et cliquez sur **Appliquer**. 
 
6. Cliquez sur l'onglet **Accès à l'ordinateur**, puis sur le nom de l'ordinateur auquel vous souhaitez autoriser l'accès. 
 
7. Répétez les étapes 3, 4, 5 et 6 pour chaque compte d'utilisateur. 
 
> [!NOTE]
> Vous n'avez pas besoin de modifier la configuration de l'ordinateur client. Il est configuré automatiquement. 
>
> Après avoir effectué la migration, si vous rencontrez un problème quand vous créez le premier compte d'utilisateur sur le serveur de destination, supprimez le compte d'utilisateur que vous avez ajouté, puis recréez-le.
