---
title: Installer des licences d’accès client RDS
description: Découvrez comment installer des licences d’accès client pour les clients Bureau à distance.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 09/20/2016
manager: dongill
ms.openlocfilehash: 2f283b51acc869704a52f09bebc228660cdfbc38
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870570"
---
# <a name="install-rds-client-access-licenses-on-the-remote-desktop-license-server"></a>Installer des licences d’accès client services Bureau à distance sur le serveur de licences bureau à distance

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Utilisez les informations suivantes pour installer des licences d’accès client des Services Bureau à distance (CAL) sur le serveur de licences. Une fois que les licences d’accès client sont installés, le serveur de licences émettra les aux utilisateurs comme il convient.

Notez que vous avez besoin d’une connectivité Internet sur l’ordinateur exécutant le Gestionnaire de licences bureau à distance, mais ne pas sur l’ordinateur exécutant le serveur de licences.

1. Sur le serveur de licences (généralement la première RD Connection Broker), ouvrez le Gestionnaire de licences bureau à distance.
2. Cliquez sur le serveur de licences, puis cliquez sur **installer des licences**.
3. Cliquez sur **suivant** sur la page d’accueil.
4. Sélectionnez le programme que vous avez acheté vos CAL de services Bureau à distance à partir de, puis cliquez sur **suivant**. Si vous êtes un fournisseur de services, sélectionnez **contrat de licence de fournisseur de services**.
5. Entrez les informations de votre programme de licence. Dans la plupart des cas, il s’agit du code de licence ou un numéro de contrat, mais cela varie en fonction du programme de licences que vous utilisez.
6. Cliquez sur **Suivant**.
7. Sélectionnez la version du produit, le type de licence et le nombre de licences pour votre environnement, puis cliquez sur **suivant**. Le Gestionnaire de licences contacte Microsoft Clearinghouse pour valider et récupérer vos licences.
8.  Cliquez sur **Terminer** pour terminer le processus.