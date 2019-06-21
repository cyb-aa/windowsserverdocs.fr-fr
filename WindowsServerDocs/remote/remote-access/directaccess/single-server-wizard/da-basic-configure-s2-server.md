---
title: Étape 2 configurer le serveur DirectAccess de base
description: Cette rubrique fait partie du guide de déployer un serveur DirectAccess unique à l’aide de la prise en main Assistant pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82bf5fed-93b3-4fa6-8e71-522146eccdb1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5bd248e36c316b11ea5e272707b75624d73dc49a
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283425"
---
# <a name="step-2-configure-the-basic-directaccess-server"></a>Étape 2 configurer le serveur DirectAccess de base

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit comment configurer les paramètres de client et de serveur requis pour un déploiement DirectAccess de base. Avant de commencer les étapes de déploiement, vérifiez que vous avez effectué les étapes de planification décrites dans [planifier un déploiement DirectAccess de base](Plan-a-Basic-DirectAccess-Deployment.md).  
  
|Tâche|Description|  
|----|--------|  
|Installer le rôle Accès à distance|Installez le rôle Accès à distance.|  
|Configurer DirectAccess à l’aide de l’Assistant Mise en route|Le nouvel Assistant Mise en route présente une expérience de configuration grandement simplifiée. L’Assistant masque la complexité de DirectAccess et permet une configuration automatisée en quelques étapes simples. Il fournit une expérience en toute transparence pour l’administrateur en configurant automatiquement le proxy Kerberos pour éliminer la nécessité d’effectuer le déploiement d’une infrastructure PKI interne.|  
|Mettre à jour les clients avec la configuration DirectAccess|Pour recevoir les paramètres DirectAccess, les clients doivent mettre à jour la stratégie de groupe lorsqu’ils sont connectés à l’intranet.|  
  
> [!NOTE]  
> Cette rubrique inclut des exemples d'applets de commande Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, consultez [Utilisation des applets de commande](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_Role"></a>Installer le rôle accès à distance  
Pour déployer l'accès à distance, vous devez installer le rôle Accès à distance sur un serveur de votre organisation qui agira en tant que serveur d'accès à distance.  
  
#### <a name="to-install-the-remote-access-role"></a>Pour installer le rôle Accès à distance  
  
1.  Sur le serveur d’accès à distance, dans la console Gestionnaire de serveur, dans le **tableau de bord**, cliquez sur **ajouter des rôles et fonctionnalités**.  
  
2.  Cliquez sur **Suivant** trois fois pour accéder à l’écran de sélection du rôle de serveur.  
  
3.  Dans la boîte de dialogue **Sélectionner des rôles de serveurs** , sélectionnez **Accès à distance**, puis cliquez sur **Suivant**.  
  
4.  Dans la boîte de dialogue **Sélectionner des fonctionnalités**, cliquez sur **Suivant**.  
  
5.  Cliquez sur **suivant**, puis, dans le **sélectionnez services de rôle** boîte de dialogue, cliquez sur le **DirectAccess et VPN (RAS)** case à cocher.  
  
6.  Cliquez sur **ajouter des fonctionnalités**, cliquez sur **suivant**, puis cliquez sur **installer**.  
  
7.  Dans la boîte de dialogue **Progression de l’installation** , vérifiez que l’installation s’est correctement déroulée et cliquez sur **Fermer**.  
  
![Windows PowerShell](../../../media/Step-2-Configure-the-DirectAccess-Server/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  
  
L’applet de commande Windows PowerShell suivante ou les applets de commande installer le rôle accès à distance : 

1. Ouvrez PowerShell en tant qu’administrateur.

2. Installer la fonctionnalité d’accès à distance :

   ```  
   Install-WindowsFeature RemoteAccess   
   ```  

3. Redémarrez l’ordinateur :

   ```
   Restart-Computer
   ```
   
4. Installer PowerShell pour l’accès à distance :

   ```
   Install-WindowsFeature RSAT-RemoteAccess-PowerShell
   ```



  
## <a name="configure-directaccess-with-the-getting-started-wizard"></a>Configurer DirectAccess à l’aide de l’Assistant Mise en route  
  
#### <a name="to-configure-directaccess-using-the-getting-started-wizard"></a>Pour configurer DirectAccess à l’aide de l’Assistant Mise en route  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **Outils**, puis sur **Gestion de l’accès à distance**.  
  
2.  Dans la console de gestion de l’accès à distance, sélectionnez le service de rôle à configurer dans le volet de navigation gauche, puis cliquez sur **exécuter l’Assistant Mise en route**.  
  
3.  Cliquez sur **Déployer DirectAccess uniquement**.  
  
4.  Sélectionnez la topologie de votre configuration réseau et tapez le nom public auquel les clients d’accès à distance se connecteront. Cliquez sur **Suivant**.  
  
    > [!NOTE]  
    > Par défaut, l’Assistant Mise en route déploie DirectAccess sur tous les ordinateurs portables figurant dans le domaine en appliquant un filtre WMI à l’objet de stratégie de groupe des paramètres client.  
  
5.  Cliquez sur **Terminer**.  
  
6.  Comme il n’existe aucune infrastructure à clé publique (PKI) dans ce déploiement, si aucun certificat n’est trouvé, l’Assistant fournit automatiquement des certificats auto-signés pour IP-HTTPS et le serveur Emplacement réseau, et active automatiquement le proxy Kerberos. L'Assistant active également NAT64 et DNS64 pour la traduction de protocole dans l'environnement IPv4 uniquement. Une fois que l’Assistant a fini d’appliquer la configuration, cliquez sur **Fermer**.  
  
7.  Dans l’arborescence de la console de gestion de l’accès à distance, sélectionnez **État des opérations**. Attendez que l’état de tous les moniteurs corresponde à « Travail en cours ». Dans le volet Tâches, sous Analyse, cliquez régulièrement sur **Actualiser** pour mettre à jour l’affichage.  
  
## <a name="update-clients-with-the-directaccess-configuration"></a>Mettre à jour les clients avec la configuration DirectAccess  
  
#### <a name="to-update-directaccess-clients"></a>Pour mettre à jour les clients DirectAccess  
  
1.  Ouvrez PowerShell en tant qu’administrateur.  
  
2.  Dans la fenêtre PowerShell, tapez **gpupdate** et appuyez sur **Entrée**.  
  
3.  Attendez que la mise à jour de la stratégie d’ordinateur se termine correctement.  
  
4.  Tapez **Get-DnsClientNrptPolicy**, puis appuyez sur **ENTRÉE**  
  
    Les entrées de la table de stratégie de résolution de noms (NRPT) pour DirectAccess sont affichées. Notez que l’exemption de serveur NLS est affichée. L’Assistant Mise en route a créé automatiquement cette entrée DNS pour le serveur DirectAccess et a fourni un certificat auto-signé associé afin que le serveur DirectAccess puisse fonctionner comme serveur Emplacement réseau.  
  
5.  Tapez **Get-NCSIPolicyConfiguration**, puis appuyez sur **ENTRÉE**. Les paramètres de l’indicateur d’état de la connectivité réseau déployés par l’Assistant sont affichés. Notez la valeur de DomainLocationDeterminationURL. Lorsque cette URL de serveur Emplacement réseau est accessible, le client déterminera qu’il est dans le réseau d’entreprise et les paramètres NRPT ne seront pas appliqués.  
  
6.  Tapez **Get-DAConnectionStatus**, puis appuyez sur **ENTRÉE**. Comme le client peut atteindre l’URL du serveur Emplacement réseau, l’état s’affiche sous la forme **ConnectedLocally**.  
  
## <a name="BKMK_Links"></a>Étape précédente  
  
-   [Étape 1 : Configurer l’infrastructure DirectAccess](Step-1-Configure-the-DirectAccess-Infrastructure.md)  
  
## <a name="next-step"></a>Étape suivante  
  
-   [Étape 3 vérifier les déploiements de DirectAccess de base](da-basic-configure-s3-verify.md)  
  


