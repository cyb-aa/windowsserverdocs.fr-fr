---
title: Copiez le certificat d’autorité de certification et la liste CRL dans le répertoire virtuel
description: Cette rubrique fait partie du guide de certificats de serveur de déploiement pour 802.1 X câblé et sans fil déploiements
manager: brianlic
ms.topic: article
ms.assetid: a1b5fa23-9cb1-4c32-916f-2d75f48b42c7
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e37bfce7f8cf33fd7fcb5e6227d783c28bd29d35
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="copy-the-ca-certificate-and-crl-to-the-virtual-directory"></a>Copiez le certificat d’autorité de certification et la liste CRL dans le répertoire virtuel

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette procédure pour copier le certificat d’autorité de certification racine liste de révocation de certificats et d’entreprise à partir de votre autorité de certification vers un répertoire virtuel sur votre serveur Web et pour vous assurer que les services ADCS est correctement configuré. Avant d’exécuter les commandes ci-dessous, assurez-vous que vous remplacez active et noms de serveur par celles qui sont appropriées pour votre déploiement.  
  
Pour effectuer cette procédure, vous devez être membre du **Admins du domaine**.  
  
#### <a name="to-copy-the-certificate-revocation-list-from-ca1-to-web1"></a>Pour copier la liste de révocation de certificats d’autorité de certification 1vers WEB1  
  
1.  Sur CA1, exécutez Windows PowerShell en tant qu’administrateur et puis publier la liste de révocation avec la commande suivante:  
  
    - Type `certutil -crl`, puis appuyez sur ENTRÉE.  
  
    - Pour copier le certificat d’autorité de certification sur le partage de fichiers sur votre serveur Web, tapez `copy C:\Windows\system32\certsrv\certenroll\*.crt \\WEB1\pki`, puis appuyez sur ENTRÉE.  
    - Pour copier les listes de révocation de certificats sur le partage de fichiers sur votre serveur Web, tapez `copy C:\Windows\system32\certsrv\certenroll\*.crl \\WEB1\pki`, puis appuyez sur ENTRÉE.  
  
2. Pour redémarrer les services ADCS, tapez `Restart-Service certsvc`, puis appuyez sur ENTRÉE.  
  
2.  Pour vérifier que vos emplacements d’extension CDP et AIA sont correctement configurés, tapez `pkiview.msc`, puis appuyez sur ENTRÉE. Pkiview MMC de PKI d’entreprise s’ouvre.  
  
3.  Cliquez sur le nom de votre autorité de certification. Par exemple, si votre nom de l’autorité de certification est corp-CA1-autorité de certification, cliquez sur **corp-CA1-CA**. Dans le volet d’informations, vérifiez que le **état** valeur pour le **certificat d’autorité de certification**, **AIA emplacement #1**, et **CDP emplacement #1** sont tous **OK**.  
  
L’illustration suivante décrit le volet des résultats pkiview avec l’état OK pour tous les éléments.  
  
! [adcs_pkiviewmedia/adcs_pkiview.png)  
  
> [!IMPORTANT]  
> Si **état** pour n’importe quel élément n’est pas **OK**, procédez comme suit:  
> -   Ouvrez le partage sur votre serveur Web pour vérifier que le certificat et les fichiers de liste de révocation de certificats ont été correctement copiés sur le partage. Si elles n’étaient pas correctement copiées sur le partage, modifier vos commandes de copie avec la source de fichier correct et partager de destination et exécutez les commandes à nouveau.  
> -   Vérifiez que vous avez entré les emplacements CDP et AIA corrects sur l’onglet Extensions de l’autorité de certification Vérifiez qu’il y a pas d’espace supplémentaire ou d’autres caractères dans les emplacements que vous avez fournies.  
> -   Vérifiez que vous avez copié le certificat d’autorité de certification et de révocation de certificats à l’emplacement correct sur votre serveur Web, et que l’emplacement correspond à l’emplacement que vous avez fournie pour les emplacements CDP et AIA sur l’autorité de certification.  
> -   Vérifiez que vous avez correctement configuré les autorisations du dossier virtuel dans lequel le certificat d’autorité de certification et la révocation de certificats sont stockés.  
  


