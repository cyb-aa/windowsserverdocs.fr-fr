---
title: Installer des licences d’accès client RDS
description: Découvrez comment installer des licences d’accès client pour les clients Bureau à distance.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 09/20/2016
manager: dongill
ms.openlocfilehash: 3a9f73418bd4da67c97db30a3272588afc287b95
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80860442"
---
# <a name="install-rds-client-access-licenses-on-the-remote-desktop-license-server"></a>Installer les licences d’accès client RDS sur le serveur de licences des services Bureau à distance

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Utilisez les informations suivantes pour installer les licences d’accès client (CAL) aux services Bureau à distance sur le serveur de licences. Une fois les licences d’accès client installées, le serveur de licences les émettra aux utilisateurs comme il convient.

Notez que vous avez besoin d’une connectivité internet sur l’ordinateur exécutant le gestionnaire de licences Bureau à distance, mais pas sur l’ordinateur exécutant le serveur de licences.

1. Sur le serveur de licences (généralement le premier serveur Service Broker pour les connexions Bureau à distance), ouvrez le gestionnaire de licences Bureau à distance.
2. Cliquez avec le bouton droit sur le serveur de licences, puis cliquez sur **Installer les licences**.
3. Dans la page de bienvenue, cliquez sur **Suivant**.
4. Sélectionnez le programme à partir duquel vous avez acheté vos licences d’accès client RDS, puis cliquez sur **Suivant**. Si vous êtes un fournisseur de services, sélectionnez **Contrat de licence de fournisseur de services**.
5. Saisissez les informations de votre programme de licence. Dans la plupart des cas, il s’agit du code de licence ou d’un numéro de contrat, mais cela varie en fonction du programme de licences que vous utilisez.
6. Cliquez sur **Suivant**.
7. Sélectionnez la version du produit, le type de licence et le nombre de licences pour votre environnement, puis cliquez sur **Suivant**. Le gestionnaire de licences contacte le serveur Microsoft Clearinghouse pour valider et récupérer vos licences.
8.  Cliquez sur **Terminer** pour terminer le processus.