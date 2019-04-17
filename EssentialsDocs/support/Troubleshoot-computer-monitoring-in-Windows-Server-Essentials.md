---
title: "Résoudre les problèmes d’analyse de l’ordinateur dans WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f1e6b377-4a24-4d28-9b25-05910914826b
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 72fe309e0e7ce6d7227cce8b7f2c5dbf018eb4a1
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-computer-monitoring-in-windows-server-essentials"></a>Résoudre les problèmes d’analyse de l’ordinateur dans WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Cette rubrique fournit des solutions aux problèmes rencontrés pendant la surveillance de l’état d’intégrité des ordinateurs dans l’Afficheur des alertes et via les notifications par courrier électronique dans WindowsServerEssentials.  
  
> [!NOTE]
>  Pour les informations de dépannage plus récentes à partir de la Communauté WindowsServerEssentials, nous vous suggérons de visiter le [Forum WindowsServerEssentials](https://social.technet.microsoft.com/Forums/winserveressentials/threads). Le Forum WindowsServerEssentials est idéale pour rechercher de l’aide ou pour poser une question.  
  
##  <a name="BKMK_TS"></a>Résolution des problèmes des notifications par courrier électronique pour les alertes  
 Cette section répertorie les différents problèmes que vous pouvez rencontrer lors de l’utilisation des notifications par courrier électronique pour les alertes.  
  
### <a name="cannot-send-the-test-email-for-the-alert"></a>Impossible d’envoyer le courrier électronique de test de l’alerte  
 **Problème** vous obtenez une erreur message indiquant que, ne peuvent pas envoyer le courrier électronique de test pour l’alerte.  
  
 **Cause** cette erreur peut se produire en raison d’un des problèmes suivants dans les paramètres de notifications d’alerte:  
  
-   Un nom de serveur SMTP incorrect ou le numéro de port.  
  
-   Spécification incorrecte que le serveur SMTP requiert une connexion unique SSL (Secure Sockets Layer).  
  
-   Le serveur SMTP requiert une authentification, et les informations d’identification incorrectes ont été entrées.  
  
 **Solutions** corriger d’éventuelles erreurs dans vos paramètres de notification par courrier électronique.  
  
##### <a name="to-identify-issues-in-your-email-notification-settings"></a>Pour identifier les problèmes dans vos paramètres de notification par courrier électronique  
  
-   Demandez à votre fournisseur de services Internet (ISP) le nom correct du serveur SMTP, numéro de port et d’utilisation SSL.  
  
-   Passez en revue les fichiers journaux pour les notifications par courrier électronique de l’alerte sur le serveur, dans cet emplacement:  
  
     %ProgramData%\Microsoft\Windows Server\Logs\SharedServiceHost-AlertServiceConfig.log  
  
    > [!TIP]
    >  Pour afficher le dossier ProgramData, vous devez masqué éléments affichés. Si vous ne pas voir le dossier ProgramData, sur le ruban s **affichage** onglet le **afficher/masquer** groupe, sélectionnez le **éléments masqués** zone de texte.  
  
##### <a name="to-update-your-email-notification-setup-for-alerts"></a>Pour mettre à jour vos paramètres de notification par courrier électronique pour les alertes  
  
1.  Dans le tableau de bord, cliquez sur une icône d’alerte dans le coin supérieur droit pour ouvrir l’Afficheur des alertes.  
  
2.  En bas de l’Afficheur des alertes, cliquez sur **configurer la notification par courrier électronique pour les alertes**.  
  
3.  Dans le **configurer la notification par courrier électronique pour les alertes** boîte de dialogue, cliquez sur **activer**.  
  
4.  Dans le **paramètres SMTP** boîte de dialogue, mettre à jour les paramètres SMTP, puis cliquez sur **OK**.  
  
5.  Pour tester vos paramètres mis à jour, cliquez sur **appliquer et envoyer par courrier électronique**.  
  
6.  Après avoir vérifié que le courrier électronique de test a réussi, cliquez sur **OK** pour enregistrer vos paramètres mis à jour.  
  
### <a name="test-email-notification-does-not-list-any-alerts"></a>Notification par courrier électronique de test ne répertorie pas toutes les alertes  
 **Problème** les notifications par courrier électronique de test pour les alertes n’affiche pas les alertes, même s’il existe des alertes répertoriées dans l’Afficheur des alertes.  
  
 **Solution** pas toutes les alertes qui sont signalées dans l’Afficheur des alertes génèrent une notification par courrier électronique. Seules les alertes qui sont configurées pour être transmises en tant qu’une notification par courrier électronique au sein de leurs fichiers de définition d’intégrité sont envoyées comme messages électroniques aux destinataires spécifiés.  
  
 Lorsque vous cliquez sur **appliquer et envoyer par courrier électronique**, en règle générale, vous recevrez une notification par courrier électronique d’exemple avec aucune liste des alertes d’intégrité. Toutefois, si une alerte d’intégrité qui est configurée pour envoyer des notifications par courrier électronique est identifiée durant ce processus de test, cette alerte est incluse dans le message électronique de test.  
  
### <a name="active-alerts-are-displayed-for-an-uninstalled-application"></a>Alertes actives sont affichées pour une application désinstallée  
 **Problème** les alertes actives pour une application sont affichés, même si l’application et son fichier de définition d’intégrité ont été désinstallés.  
  
 **Solution** vous devez supprimer manuellement les alertes actives de l’application désinstallée. Pour supprimer une alerte, procédez comme suit.  
  
##### <a name="to-delete-an-alert-from-the-server-by-using-the-dashboard"></a>Pour supprimer une alerte à partir du serveur à l’aide du tableau de bord  
  
1.  À partir du serveur, ouvrez le tableau de bord.  
  
2.  Dans le volet de navigation, cliquez sur une icône d’alerte affichée (critique, avertissement ou information). Cette opération lance l’Afficheur des alertes.  
  
3.  Dans l’Afficheur des alertes, l’alerte que vous souhaitez supprimer, puis cliquez avec le bouton droit **supprimer cette alerte**.
