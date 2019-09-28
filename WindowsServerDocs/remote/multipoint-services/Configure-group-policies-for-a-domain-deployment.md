---
title: Configurer des stratégies de groupe pour un déploiement dans un domaine
description: Découvrez comment configurer des stratégies de groupe dans MultiPoint services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 13e5fa90-d330-4155-a6b8-78eb650cbbfa
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 5ac6524289d231d152e366d2ba750a59d27ce14f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395524"
---
# <a name="configure-group-policies-for-a-domain-deployment"></a>Configurer des stratégies de groupe pour un déploiement dans un domaine
Pour vous assurer que votre déploiement de domaine de MultiPoint Services fonctionne correctement, appliquez les paramètres de stratégie de groupe suivants au compte d’utilisateur WMSshell sur un système MultiPoint services.  
  
> [!IMPORTANT]  
> Certains paramètres de stratégie de groupe peuvent empêcher l’application des paramètres de configuration requis à MultiPoint services. Veillez à bien comprendre et définir vos paramètres de stratégie de groupe afin qu’ils fonctionnent correctement sur MultiPoint services. Par exemple, un paramètre de stratégie de groupe qui empêche l’ouverture de session automatique peut présenter des problèmes liés au comportement de connexion MultiPoint services.  
  
## <a name="update-group-policies-for-the-wmsshell-user-account"></a>Mettre à jour les stratégies de groupe pour le compte d’utilisateur WMSshell 
Le compte d’utilisateur WMSshell est un compte système que MultiPoint Services utilise pour se connecter à la console, où les stations réelles sont créées. Ce compte n’est pas destiné à être géré par le gestionnaire MultiPoint.
  
> [!NOTE]  
> Pour savoir comment mettre à jour les stratégies de groupe, consultez [éditeur de stratégie de groupe local](https://technet.microsoft.com/library/dn265982.aspx).  
  
**RENVOI** Configuration utilisateur > les modèles d’administration > le panneau de configuration > la **personnalisation**  
  
Affectez les valeurs suivantes:  
  
|Paramètre|Valeurs|  
|-----------|----------|  
|Activer l’économiseur d’écran|Désactivé|  
|Dépassement du délai d'expiration de l'écran de veille|Désactivé<br /><br />Secondes: xxx|  
|Un mot de passe protège l'écran de veille|Désactivé|  
  
**RENVOI** Configuration de l’ordinateur > Paramètres Windows > paramètres de sécurité > stratégies locales > attribution des droits utilisateur > **permettre l’ouverture d’une session locale**  
  
|Paramètre|Valeurs|  
|-----------|----------|  
|Permettre l’ouverture d’une session locale|Assurez-vous que la liste des comptes comprend le compte WMSshell.<br /><br />**Remarque :** Par défaut, le compte WMSshell est membre du groupe utilisateurs. Si le groupe utilisateurs figure dans la liste et que WMSshell est membre du groupe utilisateurs, vous n’avez pas besoin d’ajouter le compte WMSshell à la liste.|  
  
> [!IMPORTANT]  
> Lorsque vous définissez des stratégies de groupe, assurez-vous que les stratégies n’interfèrent pas avec les mises à jour automatiques et les erreurs de rapport d’erreurs Windows sur le serveur MultiPoint. Celles-ci sont définies par les paramètres **installer automatiquement les mises à jour** **rapport d’erreurs Windows et automatique** qui ont été sélectionnés pendant l’installation de Windows MultiPoint Server, configurés dans le gestionnaire multipoint à l’aide de l’option **modifier les paramètres du serveur**ou configuré dans les mises à jour planifiées pour la protection des disques.  
  
## <a name="update-the-registry"></a>Mettre à jour le registre  
Pour un déploiement de domaine de MultiPoint services, vous devez mettre à jour les sous-clés de Registre suivantes.  
  
> [!IMPORTANT]  
> Une modification incorrecte du Registre peut endommager gravement votre système. Avant toute modification du registre, il est conseillé de sauvegarder toutes les données importantes de votre ordinateur.  
  
#### <a name="to-update-registry-subkeys-for-a-domain-deployment-of-multipoint-services"></a>Pour mettre à jour les sous-clés de Registre pour un déploiement de domaine de MultiPoint services  
  
1.  Ouvrez l’éditeur du Registre. (À l’invite de commandes, tapez **regedit. exe**, puis appuyez sur entrée.)  
  
2.  Dans le volet gauche, recherchez la sous-clé de Registre suivante, puis sélectionnez-la:  
  
    HKEY_USERS @ no__t-0SIDofWMSshell > \Software\Policies\Microsoft\Windows\Control Panel\Desktop  
  
    où «<SIDofWMSshell>» est l’identificateur de sécurité (SID) pour le compte WMSshell. Pour savoir comment identifier le SID, consultez [comment associer un nom d’utilisateur à un identificateur de sécurité (SID)](https://support.microsoft.com/kb/154599).  
  
3.  Dans la liste à droite, mettez à jour les sous-clés suivantes.  
  
    |Sous-clé|Nom de valeur|Données de la valeur|  
    |----------|--------------|--------------|  
    |ScreenSaveActive|REG_SZ|0 (zéro)|  
    |ScreenSaveTimeout|REG_SZ|120|  
    |ScreenSaverIsSecure|REG_SZ|0 (zéro)|  
  
    Pour mettre à jour une sous-clé de Registre:  
  
    1.  Une fois la clé de Registre sélectionnée dans le volet gauche, cliquez avec le bouton droit sur la sous-clé dans le volet droit, puis cliquez sur **modifier**.  
  
    2.  Dans la boîte de dialogue modification de la chaîne, tapez une nouvelle valeur dans données de la **valeur**, puis cliquez sur **OK**.  
  
4.  Une fois la mise à jour des sous-clés du Registre terminée, redémarrez l’ordinateur pour activer les modifications. 
