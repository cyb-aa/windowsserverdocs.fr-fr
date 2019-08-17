---
title: Copier le certificat et la liste de révocation des certificats de l’autorité de certification dans le répertoire virtuel
description: Cette rubrique fait partie du guide déployer des certificats de serveur pour les déploiements sans fil et câblés 802.1 X.
manager: dougkim
ms.topic: article
ms.assetid: a1b5fa23-9cb1-4c32-916f-2d75f48b42c7
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.date: 07/19/2018
ms.openlocfilehash: 9dbe14bec1c39ab5b967276c4faf3e9fc5a9aae3
ms.sourcegitcommit: 0467b8e69de66e3184a42440dd55cccca584ba95
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69546536"
---
# <a name="copy-the-ca-certificate-and-crl-to-the-virtual-directory"></a>Copier le certificat et la liste de révocation des certificats de l’autorité de certification dans le répertoire virtuel

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour copier la liste de révocation de certificats et le certificat d’autorité de certification racine d’entreprise de votre autorité de certification vers un répertoire virtuel sur votre serveur Web, et pour vous assurer que les services AD CS sont correctement configurés. Avant d’exécuter les commandes ci-dessous, veillez à remplacer les noms de répertoires et de serveurs par ceux qui conviennent à votre déploiement.  
  
Pour effectuer cette procédure, vous devez être membre du **groupe Admins du domaine**.  
  
#### <a name="to-copy-the-certificate-revocation-list-from-ca1-to-web1"></a>Pour copier la liste de révocation de certificats de CA1 vers WEB1  
  
1.  Sur CA1, exécutez Windows PowerShell en tant qu’administrateur, puis publiez la liste de révocation de certificats à l’aide de la commande suivante:  
  
    - Tapez `certutil -crl`, puis appuyez sur ENTRÉE.  

    - Pour copier le certificat CA1 dans le partage de fichiers sur votre serveur Web, `copy C:\Windows\system32\certsrv\certenroll\*.crt \\WEB1\pki`tapez, puis appuyez sur entrée.  
    
    - Pour copier les listes de révocation de certificats dans le partage de fichiers sur votre serveur `copy C:\Windows\system32\certsrv\certenroll\*.crl \\WEB1\pki`Web, tapez, puis appuyez sur entrée.  
  
2.  Pour vérifier que vos emplacements d’extension CDP et AIA sont correctement configurés `pkiview.msc`, tapez, puis appuyez sur entrée. La console MMC d’entreprise à clé publique PKIView s’ouvre.  
  
3.  Dans le volet gauche, cliquez sur le nom de votre autorité de certification.<p>Par exemple, si votre nom d’autorité de certification est Corp-CA1-CA, cliquez sur **Corp-CA1-ca**. 

4. Dans la colonne État du volet de résultats, vérifiez que les valeurs de l’exemple suivantsont correctes:

    - **Certificat d’autorité de certification**
    - **Emplacement AIA #1**
    - **Emplacement CDP #1**   
  
  
> [!TIP]  
> Si l' **État** d’un élément n’est pas **OK**, procédez comme suit:  
> -   Ouvrez le partage sur votre serveur Web pour vérifier que le certificat et les fichiers de liste de révocation de certificats ont été correctement copiés dans le partage. S’ils n’ont pas été correctement copiés dans le partage, modifiez vos commandes de copie avec la source de fichier et la destination de partage appropriées, puis réexécutez les commandes.  
> -   Vérifiez que vous avez entré les emplacements appropriés pour le CDP et l’AIA sous l’onglet Extensions de l’autorité de certification. Assurez-vous qu’il n’y a pas d’espaces ou d’autres caractères supplémentaires dans les emplacements que vous avez fournis.  
> -   Vérifiez que vous avez copié la liste de révocation de certificats et le certificat d’autorité de certification vers l’emplacement approprié sur votre serveur Web, et que l’emplacement correspond à l’emplacement que vous avez fourni pour les emplacements CDP et AIA sur l’autorité de certification.  
> -   Vérifiez que vous avez correctement configuré les autorisations pour le dossier virtuel dans lequel le certificat d’autorité de certification et la liste de révocation de certificats sont stockés.  
  


