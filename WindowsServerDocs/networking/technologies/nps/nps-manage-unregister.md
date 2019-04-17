---
title: Annuler l’inscription d’un serveur NPS à partir d’un domaine ActiveDirectory
description: Vous pouvez utiliser cette rubrique pour inscrire un serveur exécutant le serveur NPS dans Windows Server2016dans le domaine par défaut du serveur NPS ou dans un autre domaine.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 68a94616-3c29-45bd-bd33-e4c578f119e1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 55c3b00146706831351ce63d1e5b74f45d7b9be1
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="unregister-an-nps-server-from-an-active-directory-domain"></a>Annuler l’inscription d’un serveur NPS à partir d’un domaine ActiveDirectory

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

En cours de la gestion de votre déploiement de serveur NPS, vous posez s’avérer utile pour déplacer un serveur NPS vers un autre domaine, pour remplacer un serveur NPS, ou pour mettre hors service un serveur NPS. 

Lorsque vous déplacez ou retirez un serveur NPS, vous pouvez annuler l’inscription du serveur NPS dans les domaines ActiveDirectory où le serveur NPS est autorisé à lire les propriétés des comptes d’utilisateur dans ActiveDirectory.

L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer ces procédures.

## <a name="to-unregister-an-nps-server"></a>Pour annuler l’inscription d’un serveur NPS

1. Sur le contrôleur de domaine dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **ActiveDirectory Users and Computers**. La console ActiveDirectory Users and Computers s’ouvre.

2. Cliquez sur **utilisateurs**, puis double-cliquez sur **serveurs RAS et IAS**.

3. Cliquez sur le **membres** onglet, puis sélectionnez le serveur NPS que vous souhaitez annuler l’inscription.

4. Cliquez sur **supprimer**, cliquez sur **Oui**, puis cliquez sur **OK**.

