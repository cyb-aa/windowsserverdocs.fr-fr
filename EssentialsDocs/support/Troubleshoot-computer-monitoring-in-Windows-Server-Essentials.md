---
title: Résoudre les problèmes d’analyse de l’ordinateur dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f1e6b377-4a24-4d28-9b25-05910914826b
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 968c8c82bfde350e631f1f6ae4830a4fd920aa4b
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318606"
---
# <a name="troubleshoot-computer-monitoring-in-windows-server-essentials"></a>Résoudre les problèmes d’analyse de l’ordinateur dans Windows Server Essentials

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Cette rubrique fournit des conseils pour résoudre les problèmes rencontrés lors de la surveillance de l’état d’intégrité des ordinateurs dans l’afficheur des alertes et via les notifications par courrier électronique dans Windows Server Essentials.  
  
> [!NOTE]
>  Pour obtenir les informations les plus récentes sur la résolution des problèmes de la communauté Windows Server Essentials, nous vous suggérons de visiter le [Forum Windows Server Essentials](https://social.technet.microsoft.com/Forums/winserveressentials/threads). Le forum Windows Server Essentials est le lieu idéal pour rechercher de l'aide ou pour poser une question.  
  
##  <a name="troubleshooting-email-notifications-for-alerts"></a><a name="BKMK_TS"></a>Dépannage des notifications par courrier électronique pour les alertes  
 Cette section répertorie les différents problèmes que vous pouvez rencontrer en utilisant les notifications par courrier électronique pour les alertes.  
  
### <a name="cannot-send-the-test-email-for-the-alert"></a>Impossible d'envoyer le message électronique de test pour l'alerte  
 **Problème** Vous recevez un message d’erreur indiquant que ne peut pas envoyer le message électronique de test pour l’alerte.  
  
 **Cause** Cette erreur peut se produire en raison de l'un des problèmes suivants dans les paramètres de notifications d'alerte :  
  
- nom de serveur SMTP ou un numéro de port incorrect ;  
  
- spécification incorrecte que le serveur SMTP nécessite une connexion unique SSL (Secure Sockets Layer) ;  
  
- entrée d'informations d'identification erronées sur le serveur SMTP qui nécessite une authentification.  
  
  **Solutions** Corrigez les erreurs dans vos paramètres de notification de courrier électronique.  
  
##### <a name="to-identify-issues-in-your-email-notification-settings"></a>Pour identifier les problèmes dans vos paramètres de notification de messagerie  
  
-   Demandez à votre fournisseur de services Internet (ISP) le nom du serveur SMTP, le numéro de port et l'utilisation SSL corrects.  
  
-   Passez en revue les fichiers journaux de notifications par courrier électronique pour l'alerte sur le serveur, à cet emplacement :  
  
     %ProgramData%\Microsoft\Windows Server\Logs\SharedServiceHost-AlertServiceConfig.log  
  
    > [!TIP]
    >  Pour afficher le dossier ProgramData, les éléments masqués doivent être affichés. Si vous ne voyez pas le dossier ProgramData, sous l’onglet **affichage** du ruban, dans le groupe **Afficher/masquer** , sélectionnez la zone de texte **éléments masqués** .  
  
##### <a name="to-update-your-email-notification-setup-for-alerts"></a>Pour mettre à jour vos paramètres de notification par courrier électronique pour les alertes  
  
1.  Dans le tableau de bord, cliquez sur une icône en haut à droite pour ouvrir l'Afficheur des alertes.  
  
2.  Au bas de l'Afficheur des alertes, cliquez sur **Configurer les notifications par courrier électronique pour les alertes**.  
  
3.  Dans la boîte de dialogue **Configurer la notification par courrier électronique pour les alertes**, cliquez sur **Activer**.  
  
4.  Dans la boîte de dialogue **Paramètres SMTP**, mettez à jour les paramètres SMTP, puis cliquez sur **OK**.  
  
5.  Pour tester vos paramètres mis à jour, cliquez sur **Appliquer et envoyer un courrier électronique**.  
  
6.  Si le test d'envoi du courrier électronique est concluant, cliquez sur **OK** pour enregistrer vos paramètres à jour.  
  
### <a name="test-email-notification-does-not-list-any-alerts"></a>La notification par courrier électronique test ne répertorie pas les alertes  
 **Problème** La notification par courrier électronique test pour les alertes n'affiche pas les alertes, même si l'Afficheur des alertes en contient.  
  
 **Solution** Toutes les alertes qui sont signalées dans l'Afficheur des alertes ne génèrent pas de notification par courrier électronique. Seules les alertes qui sont configurées pour être transmises en tant que notifications par courrier électronique dans leurs fichiers de définition d'intégrité sont envoyées comme messages électroniques aux destinataires spécifiés.  
  
 En cliquant sur **Appliquer et envoyer un courrier électronique**, vous recevrez généralement un exemple de notification par courrier électronique sans aucune alerte d'intégrité répertoriée. Cependant, si une alerte d'intégrité qui est configurée pour envoyer des notifications par courrier électronique est identifiée durant ce processus de test, cette alerte figure dans le courrier électronique de test.  
  
### <a name="active-alerts-are-displayed-for-an-uninstalled-application"></a>Affichage d'alertes actives pour une application désinstallée  
 **Problème** Les alertes actives pour une application s'affichent, même si l'application et son fichier de définition d'intégrité ont été désinstallés.  
  
 **Solution** Vous devez supprimer manuellement les alertes actives liées à l'application désinstallée. Pour cela, procédez comme suit.  
  
##### <a name="to-delete-an-alert-from-the-server-by-using-the-dashboard"></a>Pour supprimer une alerte à partir du serveur à l'aide du tableau de bord  
  
1.  À partir du serveur, ouvrez le tableau de bord.  
  
2.  Dans le volet de navigation, cliquez sur une icône d'alerte affichée (critique, avertissement ou informatif). L'Afficheur des alertes s'ouvre.  
  
3.  Dans l'Afficheur des alertes, cliquez avec le bouton droit sur l'alerte que vous souhaitez supprimer, puis cliquez sur **Supprimer l'alerte**.
