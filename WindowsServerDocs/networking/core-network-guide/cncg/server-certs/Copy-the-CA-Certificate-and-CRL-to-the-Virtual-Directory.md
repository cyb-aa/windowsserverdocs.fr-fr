---
title: Copiez le certificat d’autorité de certification et la révocation de certificats dans le répertoire virtuel
description: Cette rubrique fait partie du guide des certificats de serveur de déploiement pour les déploiements de sans fil et câblé à 802.1 X
manager: dougkim
ms.topic: article
ms.assetid: a1b5fa23-9cb1-4c32-916f-2d75f48b42c7
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.date: 07/19/2018
ms.openlocfilehash: 15cc807db805e1be0349ea51119663e515c7e551
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874030"
---
# <a name="copy-the-ca-certificate-and-crl-to-the-virtual-directory"></a>Copiez le certificat d’autorité de certification et la révocation de certificats dans le répertoire virtuel

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour copier le certificat d’autorité de certification racine liste de révocation de certificats et d’entreprise à partir de votre autorité de certification vers un répertoire virtuel sur votre serveur Web et pour vous assurer que les services AD CS est correctement configuré. Avant d’exécuter les commandes ci-dessous, veillez à remplacer les noms de répertoire et le serveur à celles qui sont appropriées pour votre déploiement.  
  
Pour effectuer cette procédure, vous devez être membre du **Admins du domaine**.  
  
#### <a name="to-copy-the-certificate-revocation-list-from-ca1-to-web1"></a>Pour copier la liste de révocation de certificats à partir de l’autorité de certification 1 à WEB1  
  
1.  Sur CA1, exécutez Windows PowerShell en tant qu’administrateur et puis publier la liste de révocation avec la commande suivante :  
  
    - Tapez `certutil -crl`, puis appuyez sur ENTRÉE.  

    - Pour copier les listes de révocation de certificats sur le partage de fichiers sur votre serveur Web, tapez `copy C:\Windows\system32\certsrv\certenroll\*.crl \\WEB1\pki`, puis appuyez sur ENTRÉE.  
  
2.  Pour vérifier que vos emplacements d’extension CDP et AIA sont correctement configurés, tapez `pkiview.msc`, puis appuyez sur ENTRÉE. Pkiview Enterprise PKI MMC s’ouvre.  
  
3.  Dans le volet gauche, cliquez sur le nom de votre autorité de certification.<p>Par exemple, si le nom de votre autorité de certification est corp-CA1-CA, cliquez sur **corp-CA1-CA**. 

4. Dans la colonne d’état du volet des résultats, vérifiez que les valeurs pour les éléments suivants s’affiche **OK**:

    - **Certificat d’autorité de certification**
    - **Emplacement AIA #1**
    - **Emplacement du CDP #1**   
  
  
> [!TIP]  
> Si **état** pour n’importe quel élément n’est pas **OK**, procédez comme suit :  
> -   Ouvrez le partage sur votre serveur Web pour vérifier que le certificat et les fichiers de liste de révocation de certificats ont été copiés avec succès au partage. Si elles n’étaient pas correctement copiées sur le partage, modifiez vos commandes de copie avec la source de fichier correct et partager la destination et réexécutez les commandes.  
> -   Vérifiez que vous avez entré les emplacements corrects de la CDP et AIA dans l’onglet Extensions de l’autorité de certification. Assurez-vous qu’il n’y a pas d’espaces supplémentaires ou d’autres caractères dans les emplacements que vous avez fournies.  
> -   Vérifiez que vous avez copié le certificat d’autorité de certification et de révocation de certificats à l’emplacement approprié sur votre serveur Web, et que l’emplacement correspond à l’emplacement que vous avez fourni pour les emplacements CDP et AIA sur l’autorité de certification.  
> -   Vérifiez que vous configuré correctement les autorisations pour le dossier virtuel dans lequel le certificat d’autorité de certification et la révocation de certificats sont stockés.  
  


