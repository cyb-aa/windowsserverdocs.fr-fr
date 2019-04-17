---
title: "Résoudre les problèmes d’installation de WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ecf19216-7aac-4aca-839a-342ac28f5329
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 293b392203269a65efffcefb3744bedc659f71c9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-windows-server-essentials-installation"></a>Résoudre les problèmes d’installation de WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Cette rubrique fournit des solutions aux problèmes qui se produisent lors de l’installation de WindowsServerEssentials. Guide est fourni dans les domaines suivants:  
  

-   [Étapes de dépannage générales](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_GeneralTroubleshootingSteps)  
  
-   [Résoudre les problèmes liés aux pilotes](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_TroubleshootDrivers)  

-   [Étapes de dépannage générales](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_GeneralTroubleshootingSteps)  
  
-   [Résoudre les problèmes liés aux pilotes](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_TroubleshootDrivers)  

  
> [!NOTE]
>  Pour les informations de dépannage plus récentes à partir de la Communauté WindowsServerEssentials, nous vous suggérons de visiter le [Forum WindowsServerEssentials](https://social.technet.microsoft.com/Forums/winserveressentials/threads). Le Forum WindowsServerEssentials est idéale pour rechercher de l’aide ou pour poser une question.  
  
##  <a name="BKMK_GeneralTroubleshootingSteps"></a>Étapes de dépannage générales  
 En cas d’échec de l’installation de WindowsServerEssentials, effectuer ces étapes pour aider à identifier le problème qui a provoqué l’échec.  
  
> [!IMPORTANT]
>  Il est important que vous ne redémarrez pas manuellement de votre serveur lors de l’installation de WindowsServerEssentials. Le serveur redémarre automatiquement plusieurs fois pendant l’installation et la configuration initiale. Si vous avez redémarré le serveur manuellement avant que vous avez vu le **installation du serveur réussie** message, ce qui peut avoir interrompu l’installation et provoqué son échec.  
  
#### <a name="to-identify-issues-in-a-failed-installation-of-windows-server-essentials"></a>Pour identifier les problèmes dans un échec de l’installation de WindowsServerEssentials  
  
1.  Vérifiez que votre matériel serveur répond à la configuration minimale requise. Pour plus d’informations sur la configuration matérielle requise, consultez [System Configuration requise pour WindowsServerEssentials](../get-started/system-requirements.md).  
  
2.  Si vous avez reçu le DVD d’installation de WindowsServerEssentials à partir de MSDN, vérifiez que le DVD est valide en vérifiant la somme de SHA1. Pour plus d’informations, voir [disponibilité et description de l’utilitaire File Checksum Integrity Verifier](https://go.microsoft.com/fwlink/?LinkId=220495) (https://go.microsoft.com/fwlink/?LinkId=220495).  
  
3.  Vérifiez que la carte réseau sur le serveur est connectée à un routeur via un câble réseau.  
  
4.  Si le serveur possède plusieurs cartes réseau, ne vérifiez qu’une carte réseau est activée.  
  
    > [!IMPORTANT]
    >  Ne déconnectez le câble réseau ou redémarrer le service de routeur lors de l’installation de WindowsServerEssentials.  
  
5.  Consultez la section «installation du serveur et déploiement» dans [Release Documentation for WindowsServerEssentials](../get-started/release-notes.md)  
  
6.  Si vous recevez le message d’erreur une erreur s’est produite lors de la configuration de votre serveur pendant l’installation, utilisez le DVD de récupération de serveur et les instructions fournies par le fabricant de votre matériel pour restaurer le serveur de paramètres d’usine par défaut.  
  
##  <a name="BKMK_TroubleshootDrivers"></a>Résoudre les problèmes liés aux pilotes  
 Le problème le plus courant lors de l’installation de WindowsServerEssentials est contrôleurs de stockage qui nécessitent des pilotes installés manuellement. Windows inclut des pilotes pour de nombreux contrôleurs de stockage, mais il peut ne pas inclure des pilotes pour votre matériel spécifique.  
  
 Vous devrez peut-être également installer manuellement les pilotes de carte réseau pour votre matériel spécifique.  
  
###  <a name="BKMK_StorageDrivers"></a>Ajout de pilotes pour les contrôleurs de stockage  
 Si votre matériel nécessite des pilotes de stockage qui ne sont pas inclus dans WindowsServerEssentials, utilisez les informations suivantes pour terminer l’installation.  
  
 Si vous voyez le message suivant pendant l’installation, vous devez ajouter manuellement des pilotes pour votre contrôleur de stockage:  
  
 **Erreur d’installation de Windows Server**  
  
 Lecteur de disque dur capable d’héberger WindowsServerEssentials est introuvable. Souhaitez-vous charger des pilotes de stockage supplémentaires?  
  
 Utilisez la procédure suivante pour installer un pilote de contrôleur de stockage.  
  
##### <a name="to-manually-install-a-storage-controller-driver"></a>Pour installer manuellement un pilote de contrôleur de stockage  
  
1.  Rechercher les pilotes pour votre contrôleur de stockage. Ceux-ci sont fournis par le fabricant du matériel, et ils peuvent également être disponibles sur le site Web du fabricant.  
  
2.  Créez un dossier appelé pilotes sur une disquette ou une clé USB flash, puis copiez les pilotes dans le dossier.  
  
3.  Connectez le lecteur de disquette ou lecteur flash USB avec les pilotes à l’ordinateur  
  
4.  Démarrez l’ordinateur à partir du DVD WindowsServerEssentials.  
  
     Si les pilotes de contrôleur de stockage sont manquants, la boîte de dialogue Erreur d’installation de WindowsServerEssentials s’affiche.  
  
5.  Dans la boîte de dialogue Erreur d’installation de WindowsServerEssentials, cliquez sur **Oui** pour charger les pilotes de stockage supplémentaire.  
  
6.  À la **Veuillez sélectionner le fichier inf du pilote de votre** invite, accédez au fichier .inf dans le dossier pilotes sur votre disquette ou un disque mémoire flash USB, sélectionnez le fichier, cliquez sur le nom de fichier, puis cliquez sur **ouvrir**. Le pilote est chargé.  
  
    > [!NOTE]
    >  Avant de tenter de charger le fichier, vérifiez que le fichier nom de l’extension (.inf) est en minuscules. Cette opération respecte la casse et un fichier de pilote ne se charge pas si l’extension de nom de fichier est en majuscules.  
  
7.  À l’invite, cliquez sur **Oui** pour rendre le pilote de stockage disponible pendant la phase en mode texte du programme d’installation.  
  
 Le programme d’installation doit se poursuivre normalement.  
  
###  <a name="BKMK_AddingNICdrivers"></a>Ajout de pilotes pour les cartes réseau  
 Si une carte réseau sur l’ordinateur n’est pas pris en charge par WindowsServerEssentials, votre serveur n’aura pas de connectivité réseau une fois l’installation terminée, et vous ne serez pas en mesure de connecter des ordinateurs à votre serveur.  
  
 À la fin de l’installation de WindowsServerEssentials, vous êtes informé si un pilote de carte réseau n’a pas été installé automatiquement. Vous pouvez également utiliser **connexions réseau** dans le panneau de configuration pour rechercher un pilote de carte réseau manquant. Si vous ne voyez pas d’une connexion réseau associée à la carte réseau sur votre serveur, vous devez installer un pilote.  
  
 Si l’ordinateur ne dispose pas d’un pilote pris en charge pour les cartes réseau, vous devez installer manuellement le pilote de carte réseau approprié, puis redémarrez le serveur. Utilisez la procédure suivante.  
  
##### <a name="to-install-a-network-adapter-driver"></a>Pour installer un pilote de carte réseau  
  
1.  Obtenir le pilote manquant auprès du fabricant de la carte réseau.  
  
2.  Suivez les instructions d’installation du fabricant pour installer le pilote.  
  
3.  Redémarrez l’ordinateur.  
  
    > [!IMPORTANT]
    >  Si vous ne redémarrez pas le serveur après avoir installé le pilote de carte réseau manquant, l’installation du logiciel Connecteur WindowsServerEssentials sur vos ordinateurs clients peut échouer.
