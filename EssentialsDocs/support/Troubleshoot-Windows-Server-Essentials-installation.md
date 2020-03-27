---
title: Résoudre les problèmes d’installation de Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ecf19216-7aac-4aca-839a-342ac28f5329
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f8f8adf97011629dbdd68622b5d6ad1ba40f7bc2
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318612"
---
# <a name="troubleshoot-windows-server-essentials-installation"></a>Résoudre les problèmes d’installation de Windows Server Essentials

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Cette rubrique explique comment résoudre les problèmes qui se produisent lors de l’installation de Windows Server Essentials. Des conseils sont fournis dans les domaines suivants :  
  

-   [Étapes de dépannage générales](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_GeneralTroubleshootingSteps)  
  
-   [Résoudre les problèmes liés aux pilotes](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_TroubleshootDrivers)  

-   [Étapes de dépannage générales](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_GeneralTroubleshootingSteps)  
  
-   [Résoudre les problèmes liés aux pilotes](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_TroubleshootDrivers)  

  
> [!NOTE]
>  Pour obtenir les informations les plus récentes sur la résolution des problèmes de la communauté Windows Server Essentials, nous vous suggérons de visiter le [Forum Windows Server Essentials](https://social.technet.microsoft.com/Forums/winserveressentials/threads). Le forum Windows Server Essentials est le lieu idéal pour rechercher de l'aide ou pour poser une question.  
  
##  <a name="general-troubleshooting-steps"></a><a name="BKMK_GeneralTroubleshootingSteps"></a>Étapes de dépannage générales  
 Si l’installation de Windows Server Essentials échoue, suivez ces étapes pour aider à identifier le problème à l’origine de l’échec.  
  
> [!IMPORTANT]
>  Il est important que vous ne redémarriez pas manuellement votre serveur lors de l’installation de Windows Server Essentials. Le serveur redémarre automatiquement plusieurs fois pendant l’installation et la configuration initiale. Si vous avez redémarré le serveur manuellement avant que le message **Installation du serveur réussie** ne s’affiche, cela peut avoir interrompu l’installation et provoqué son échec.  
  
#### <a name="to-identify-issues-in-a-failed-installation-of-windows-server-essentials"></a>Pour identifier les problèmes liés à l’échec d’une installation de Windows Server Essentials  
  
1.  Vérifiez que le matériel du serveur respecte la configuration minimale requise. Pour plus d’informations sur la configuration matérielle requise, consultez [Configuration système requise pour Windows Server Essentials](../get-started/system-requirements.md).  
  
2.  Si vous avez reçu le DVD d’installation de Windows Server Essentials à partir de MSDN, vérifiez que le DVD est valide en vérifiant la somme de SHA1. Pour plus d’informations, consultez [disponibilité et description de l’utilitaire File Checksum Integrity Verifier](https://go.microsoft.com/fwlink/?LinkId=220495) (https://go.microsoft.com/fwlink/?LinkId=220495).  
  
3.  Vérifiez que la carte réseau du serveur est connectée à un routeur via un câble réseau.  
  
4.  Si le serveur possède plusieurs cartes réseau, vérifiez qu’une seule carte réseau est activée.  
  
    > [!IMPORTANT]
    >  Ne déconnectez pas le câble réseau ou ne redémarrez pas le routeur lors de l’installation de Windows Server Essentials.  
  
5.  Consultez « Installation et déploiement du serveur » dans [la documentation de version de Windows Server Essentials](../get-started/release-notes.md)  
  
6.  Si vous recevez le message d’erreur une erreur s’est produite lors de la configuration de votre serveur lors de l’installation, utilisez le DVD de récupération du serveur et les instructions fournies par le fabricant de votre matériel pour restaurer les paramètres par défaut du serveur.  
  
##  <a name="troubleshoot-driver-issues"></a><a name="BKMK_TroubleshootDrivers"></a>Résoudre les problèmes liés aux pilotes  
 Le problème le plus courant lors de l’installation de Windows Server Essentials est les contrôleurs de stockage qui nécessitent l’installation manuelle des pilotes. Windows inclut des pilotes pour de nombreux contrôleurs de stockage, mais il peut ne pas inclure les pilotes de votre matériel spécifique.  
  
 Vous devrez peut-être également installer manuellement les pilotes de carte réseau pour votre matériel spécifique.  
  
###  <a name="adding-drivers-for-storage-controllers"></a><a name="BKMK_StorageDrivers"></a>Ajout de pilotes pour les contrôleurs de stockage  
 Si votre matériel nécessite des pilotes de stockage qui ne sont pas inclus dans Windows Server Essentials, utilisez les informations suivantes pour terminer l’installation.  
  
 Si le message suivant s’affiche lors de l’installation, vous devez ajouter manuellement des pilotes pour votre contrôleur de stockage :  
  
 **Erreur du programme d’installation de Windows Server**  
  
 Le lecteur de disque dur qui ne prend pas en charge l’hébergement de Windows Server Essentials est introuvable. Souhaitez-vous charger des pilotes de stockage supplémentaires ?  
  
 Procédez comme suit pour installer un pilote de contrôleur de stockage.  
  
##### <a name="to-manually-install-a-storage-controller-driver"></a>Pour installer manuellement un pilote de contrôleur de stockage  
  
1. Recherchez les pilotes pour votre contrôleur de stockage. Ceux-ci sont fournis par le fabricant du matériel, et ils peuvent également être disponibles sur le site web du fabricant.  
  
2. Créez un dossier appelé PILOTES sur une disquette ou un lecteur flash USB, puis copiez les pilotes dans le dossier.  
  
3. Connectez la disquette ou le lecteur flash USB comportant les pilotes à l’ordinateur.  
  
4. Démarrez l’ordinateur à partir du DVD Windows Server Essentials.  
  
    Si des pilotes de contrôleur de stockage sont manquants, la boîte de dialogue erreur d’installation de Windows Server Essentials s’affiche.  
  
5. Dans la boîte de dialogue erreur d’installation de Windows Server Essentials, cliquez sur **Oui** pour charger les pilotes de stockage supplémentaires.  
  
6. À l’invite **Veuillez sélectionner le fichier .inf de votre pilote**, accédez au fichier .inf du dossier PILOTES sur votre disquette ou lecteur flash USB, sélectionnez le fichier, cliquez avec le bouton droit sur le nom de fichier, puis cliquez sur **Ouvrir**. Cette opération charge le pilote.  
  
   > [!NOTE]
   >  Avant de tenter de charger le fichier, vérifiez que l’extension du nom du fichier (.inf) est en minuscules. Cette opération respecte la casse et un fichier de pilote ne se charge pas si l’extension du nom du fichier est en majuscules.  
  
7. À l’invite, cliquez sur **Oui** pour rendre le pilote de stockage disponible pendant la phase en mode texte du programme d’installation.  
  
   Le programme d’installation doit se poursuivre normalement.  
  
###  <a name="adding-drivers-for-network-adapters"></a><a name="BKMK_AddingNICdrivers"></a>Ajout de pilotes pour les cartes réseau  
 Si une carte réseau de l’ordinateur n’est pas prise en charge par Windows Server Essentials, votre serveur n’aura pas de connectivité réseau une fois l’installation terminée, et vous ne pourrez pas connecter les ordinateurs à votre serveur.  
  
 À la fin de l’installation de Windows Server Essentials, vous êtes informé si aucun pilote de carte réseau n’a été installé automatiquement. Vous pouvez également utiliser l’option **Connexions réseau** du panneau de configuration pour vérifier si des pilotes de carte réseau sont manquants. Si vous ne voyez pas de connexion réseau associée à la carte réseau sur votre serveur, vous devez installer un pilote.  
  
 Si l’ordinateur ne dispose pas d’un pilote pris en charge pour les cartes réseau, installez manuellement le pilote de carte réseau approprié, puis redémarrez le serveur. Procédez comme suit :  
  
##### <a name="to-install-a-network-adapter-driver"></a>Pour installer un pilote de carte réseau  
  
1.  Obtenez le pilote manquant auprès du fabricant de la carte réseau.  
  
2.  Suivez les instructions d’installation du fabricant pour installer le pilote.  
  
3.  Redémarrez l'ordinateur.  
  
    > [!IMPORTANT]
    >  Si vous ne redémarrez pas le serveur après avoir installé le pilote de carte réseau manquant, l’installation du logiciel connecteur Windows Server Essentials sur vos ordinateurs clients peut échouer.
