---
title: Inscrire un serveur NPS dans un domaine ActiveDirectory
description: Vous pouvez utiliser cette rubrique pour inscrire un serveur exécutant le serveur NPS dans Windows Server2016dans le domaine par défaut du serveur NPS ou dans un autre domaine.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2de954fd-a7d8-4cc6-85b1-b0c3c06f788f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7ba18f6c994e8b15da3a07a3e37550d5dbff24af
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="register-an-nps-server-in-an-active-directory-domain"></a>Inscrire un serveur NPS dans un domaine ActiveDirectory

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour inscrire un serveur exécutant le serveur NPS dans Windows Server2016dans le domaine par défaut du serveur NPS ou dans un autre domaine.

## <a name="register-an-nps-server-in-its-default-domain"></a>Inscrire un serveur NPS dans son domaine par défaut

Vous pouvez utiliser cette procédure pour inscrire un serveur NPS dans le domaine où le serveur est membre d’un domaine. 

Les serveurs NPS doivent être enregistrés dans ActiveDirectory afin qu’ils sont autorisés à lire les propriétés de numérotation des comptes d’utilisateur au cours du processus d’autorisation. L’inscription d’un serveur NPS ajoute le serveur à le **serveurs RAS et IAS** groupe dans ActiveDirectory.

L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer ces procédures.

### <a name="to-register-an-nps-server-in-its-default-domain"></a>Pour inscrire un serveur NPS dans son domaine par défaut


1. Sur le serveur NPS, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **serveur NPS**. La console du serveur NPS s’ouvre.

2. Avec le bouton droit **NPS (Local)**, puis cliquez sur **inscrire le serveur dans ActiveDirectory**. Le **serveur NPS** boîte de dialogue s’ouvre.

3. Dans **serveur NPS**, cliquez sur **OK**, puis cliquez sur **OK** à nouveau.

## <a name="register-an-nps-server-in-another-domain"></a>Inscrire un serveur NPS dans un autre domaine

Pour fournir un serveur NPS avec l’autorisation de lire les propriétés de numérotation des comptes d’utilisateur dans ActiveDirectory, le serveur NPS doit être enregistré dans le domaine où résident les comptes.

Vous pouvez utiliser cette procédure pour inscrire un serveur NPS dans un domaine où le serveur NPS n’est pas membre d’un domaine.

L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer ces procédures.

### <a name="to-register-an-nps-server-in-another-domain"></a>Pour inscrire un serveur NPS dans un autre domaine

1. Sur le contrôleur de domaine dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **ActiveDirectory Users and Computers**. La console ActiveDirectory Users and Computers s’ouvre.

2. Dans l’arborescence de la console, accédez au domaine dans lequel vous souhaitez que le serveur NPS lire les informations de compte d’utilisateur, puis cliquez sur le **utilisateurs** dossier. 

3. Dans le volet d’informations, cliquez sur **serveurs RAS et IAS**, puis cliquez sur **propriétés**. Le **propriétés RAS et IAS serveurs** boîte de dialogue s’ouvre.

4. Dans le **propriétés RAS et IAS serveurs** boîte de dialogue, cliquez sur le **membres** onglet, chacun des serveurs NPS que vous souhaitez inscrire dans le domaine, puis cliquez sur Ajouter **OK**.


### <a name="to-register-an-nps-server-in-another-domain-by-using-netsh-commands-for-nps"></a>Pour inscrire un serveur NPS dans un autre domaine à l’aide des commandes Netsh pour NPS

1. Ouvrez une invite de commandes ou windows PowerShell. 

2. Tapez la commande suivante à l’invite de commandes: **netsh nps ajouter registeredserver**&nbsp;*domaine*&nbsp;*server*, puis appuyez sur ENTRÉE.

>[!NOTE]
>Dans la commande précédente, *domaine* est le nom de domaine DNS du domaine dans lequel vous souhaitez inscrire le serveur NPS, et *server* est le nom de l’ordinateur du serveur NPS.

