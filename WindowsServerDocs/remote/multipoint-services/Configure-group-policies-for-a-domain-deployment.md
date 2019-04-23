---
title: Configurer des stratégies de groupe pour un déploiement dans un domaine
description: Découvrez comment configurer des stratégies de groupe dans MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 13e5fa90-d330-4155-a6b8-78eb650cbbfa
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: f661fbdc40fd7dd2562d51756bc7642c8e9a4a82
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888040"
---
# <a name="configure-group-policies-for-a-domain-deployment"></a>Configurer des stratégies de groupe pour un déploiement dans un domaine
Pour vous assurer que votre déploiement de MultiPoint Services de domaine fonctionne correctement, appliquer les paramètres de stratégie de groupe suivants pour le compte d’utilisateur WMSshell sur un système MultiPoint Services.  
  
> [!IMPORTANT]  
> Certains paramètres de stratégie de groupe peuvent empêcher d’être appliquée à MultiPoint Services, les paramètres de configuration requis. N’oubliez pas que vous comprenez et définissez vos paramètres de stratégie de groupe afin qu’ils fonctionnent correctement sur MultiPoint Services. Par exemple, un paramètre de stratégie de groupe qui empêche l’ouverture de session automatique peut présenter des problèmes avec un comportement d’ouverture de session MultiPoint Services.  
  
## <a name="update-group-policies-for-the-wmsshell-user-account"></a>Mettre à jour des stratégies de groupe pour le compte d’utilisateur WMSshell 
Le compte d’utilisateur WMSshell est un compte système quels services MultiPoint utilise pour se connecter à la console où les stations actuall sont créées. Ce compte n’est pas ment être gérés par le gestionnaire MultiPoint.
  
> [!NOTE]  
> Pour savoir comment mettre à jour des stratégies de groupe, consultez [éditeur de stratégie de groupe locale](https://technet.microsoft.com/library/dn265982.aspx).  
  
**STRATÉGIE :** Configuration de l’utilisateur > modèles d’administration > Panneau de configuration > **personnalisation**  
  
Attribuer les valeurs suivantes :  
  
|Paramètre|Valeurs|  
|-----------|----------|  
|Activer l’écran de veille|Désactivée|  
|Dépassement du délai d'expiration de l'écran de veille|Désactivée<br /><br />Secondes : xxx|  
|Un mot de passe protège l'écran de veille|Désactivée|  
  
**STRATÉGIE :** Configuration ordinateur > Paramètres de Windows > Paramètres de sécurité > Stratégies locales > Attribution des droits utilisateur > **autoriser la connexion localement**  
  
|Paramètre|Valeurs|  
|-----------|----------|  
|Permettre l’ouverture d’une session locale|Vérifiez que la liste des comptes comprend le compte WMSshell.<br /><br />**Remarque :** Par défaut, le compte de WMSshell est un membre du groupe utilisateurs. Si le groupe d’utilisateurs est dans la liste et WMSshell est un membre du groupe d’utilisateurs, il est inutile d’ajouter le compte de WMSshell à la liste.|  
  
> [!IMPORTANT]  
> Lorsque vous définissez des stratégies de groupe, assurez-vous que les stratégies n’interfèrent pas avec les mises à jour automatiques et d’erreur Windows rapport d’erreurs sur le serveur MultiPoint. Ces propriétés sont définies le **installation met automatiquement à jour** et **rapport d’erreurs Windows automatique** paramètres qui ont été sélectionnés pendant l’installation de Windows MultiPoint Server, configurée dans MultiPoint Gestionnaire à l’aide **modifier les paramètres serveur**, ou configurés dans les mises à jour planifiées pour la Protection des disques.  
  
## <a name="update-the-registry"></a>Mettre à jour le Registre  
Pour un déploiement de domaine de MultiPoint Services, vous devez mettre à jour les sous-clés de Registre suivantes.  
  
> [!IMPORTANT]  
> Une modification incorrecte du Registre peut endommager gravement votre système. Avant toute modification du registre, il est conseillé de sauvegarder toutes les données importantes de votre ordinateur.  
  
#### <a name="to-update-registry-subkeys-for-a-domain-deployment-of-multipoint-services"></a>Pour mettre à jour les sous-clés de Registre pour un déploiement de domaine de MultiPoint Services  
  
1.  Ouvrez l’Éditeur du Registre. (À l’invite de commandes, tapez **regedit.exe**, puis appuyez sur ENTRÉE.)  
  
2.  Dans le volet gauche, recherchez et sélectionnez la sous-clé de Registre suivante :  
  
    HKEY_USERS\<SIDofWMSshell>\Software\Policies\Microsoft\Windows\Control Panel\Desktop  
  
    où «<SIDofWMSshell>» est l’identificateur de sécurité (SID) pour le compte WMSshell. Pour savoir comment identifier le SID, consultez [comment associer un nom d’utilisateur avec un identificateur de sécurité (SID)](https://support.microsoft.com/kb/154599).  
  
3.  Dans la liste sur la droite, mettez à jour les sous-clés suivantes.  
  
    |Sous-clé|Nom de valeur|Données de la valeur|  
    |----------|--------------|--------------|  
    |ScreenSaveActive|REG_SZ|0 (zéro)|  
    |ScreenSaveTimeout|REG_SZ|120|  
    |ScreenSaverIsSecure|REG_SZ|0 (zéro)|  
  
    Pour mettre à jour une sous-clé de Registre :  
  
    1.  Avec la clé de Registre sélectionnée dans le volet gauche, cliquez sur la sous-clé dans le volet droit, puis cliquez sur **modifier**.  
  
    2.  Dans la boîte de dialogue Modifier la chaîne, tapez une nouvelle valeur dans **données de la valeur**, puis cliquez sur **OK**.  
  
4.  Une fois que vous avez terminé la mise à jour des sous-clés de Registre, redémarrez l’ordinateur pour activer les modifications. 