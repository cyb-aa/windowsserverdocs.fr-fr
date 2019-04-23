---
ms.assetid: 5b2876ac-fe7d-4054-bfba-b692e57bc0d2
title: Annexe C - protégés des comptes et groupes dans Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 70e29ad42b57cf315c7179d6eea8220ad2bfab48
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834700"
---
# <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>Annexe c : Comptes protégés et groupes dans Active Directory

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

## <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>Annexe c : Comptes protégés et groupes dans Active Directory

Dans Active Directory, un ensemble de comptes à privilèges élevés et des groupes par défaut sont considérés comme des comptes protégés et des groupes. Avec la plupart des objets dans Active Directory, les administrateurs délégués (les utilisateurs qui ont été délégué des autorisations pour gérer des objets Active Directory) peuvent modifier les autorisations sur les objets, y compris la modification des autorisations pour permettre à eux-mêmes modifier les appartenances à des les groupes, par exemple.  

Toutefois, avec des comptes protégés et groupes, autorisations d’objets définies et appliquées via un processus automatique qui garantit que les autorisations sur les objets doivent rester cohérentes, même si les objets sont déplacés de l’annuaire. Même si quelqu'un modifie manuellement les autorisations d’un objet protégé, ce processus garantit que les autorisations sont renvoyées rapidement à leurs valeurs par défaut.  

### <a name="protected-groups"></a>Groupes protégés

Le tableau suivant contient les groupes protégés dans Active Directory sont répertoriés par système d’exploitation du contrôleur de domaine.  

#### <a name="protected-accounts-and-groups-in-active-directory-by-operating-system"></a>Comptes protégés et groupes dans Active Directory par le système d’exploitation

| Windows Server 2003 RTM | Windows Server 2003 SP1 + | Windows Server 2012, <br> Windows Server 2008 R2, <br> Windows Server 2008 | Windows Server 2016 |
| --- | --- | --- | --- |
|Opérateurs de compte|Opérateurs de compte|Opérateurs de compte|Opérateurs de compte|
|Administrateur|Administrateur|Administrateur|Administrateur|
|Administrateurs|Administrateurs|Administrateurs|Administrateurs|
|Opérateurs de sauvegarde|Opérateurs de sauvegarde|Opérateurs de sauvegarde|Opérateurs de sauvegarde|
|Éditeurs de certificats|||
|Administrateurs du domaine|Administrateurs du domaine|Administrateurs du domaine|Administrateurs du domaine|
|Contrôleurs de domaine|Contrôleurs de domaine|Contrôleurs de domaine|Contrôleurs de domaine|
|Administrateurs de l’entreprise|Administrateurs de l’entreprise|Administrateurs de l’entreprise|Administrateurs de l’entreprise|
||||Administrateurs de clé d’entreprise|
||||Administrateurs de clé|
|Krbtgt|Krbtgt|Krbtgt|Krbtgt|
|Opérateurs d'impression|Opérateurs d'impression|Opérateurs d'impression|Opérateurs d'impression|
|||Contrôleurs de domaine en lecture seule|Contrôleurs de domaine en lecture seule|
|Réplicateur|Réplicateur|Réplicateur|Réplicateur|
|Administrateurs du schéma|Administrateurs du schéma|Administrateurs du schéma|Administrateurs du schéma|
|Opérateurs de serveur|Opérateurs de serveur|Opérateurs de serveur|Opérateurs de serveur|

#### <a name="adminsdholder"></a>AdminSDHolder

L’objectif de l’objet AdminSDHolder consiste à fournir des autorisations de « modèle » pour les comptes protégés et les groupes dans le domaine. AdminSDHolder est automatiquement créé en tant qu’objet dans le conteneur système de chaque domaine Active Directory. Son chemin d’accès est : **CN=AdminSDHolder,CN=System,DC=<domain_component>,DC=<domain_component>?.**  

Contrairement à la plupart des objets dans le domaine Active Directory, qui sont détenus par le groupe d’administrateurs, AdminSDHolder est détenu par le groupe Admins du domaine. Par défaut, EAs peuvent apporter des modifications à l’objet AdminSDHolder de n’importe quel domaine, comme les groupes Admins du domaine et administrateurs du domaine. En outre, bien que le propriétaire de la valeur par défaut de AdminSDHolder est Admins du domaine du domaine, les membres du groupe Administrateurs ou administrateurs de l’entreprise peuvent prendre possession de l’objet.  

#### <a name="sdprop"></a>SDProp

SDProp est un processus qui s’exécute toutes les 60 minutes (par défaut) sur le contrôleur de domaine qui contient l’émulateur de contrôleur de domaine principal (PDCE du domaine). SDProp compare les autorisations sur l’objet AdminSDHolder du domaine avec les autorisations sur les comptes protégés et les groupes dans le domaine. Si les autorisations sur un des comptes protégés et des groupes ne correspondent pas les autorisations sur l’objet AdminSDHolder, les autorisations sur les comptes protégés et les groupes sont réinitialisées pour correspondre à celles de l’objet de AdminSDHolder du domaine.  

En outre, l’héritage des autorisations est désactivé sur les groupes protégés et les comptes, ce qui signifie que même si les comptes et groupes sont déplacés vers différents emplacements dans le répertoire, ils n’héritent pas des autorisations de leurs objets parents de nouveau. L’héritage est désactivé sur l’objet AdminSDHolder afin que les modifications de permission pour les objets parents ne modifiez pas les autorisations de AdminSDHolder.  

##### <a name="changing-sdprop-interval"></a>Modification SDProp intervalle

Normalement, vous ne devez pas modifier l’intervalle selon lequel s’exécute SDProp, à l’exception des fins de test. Si vous avez besoin modifier l’intervalle SDProp, sur l’ECDP pour le domaine, utilisez regedit pour ajouter ou modifier la valeur DWORD de AdminSDProtectFrequency dans HKLM\SYSTEM\CurrentControlSet\Services\NTDS\Parameters.  

La plage de valeurs est en secondes de 60 à 7200 (une minute à deux heures). Pour annuler les modifications, supprimez AdminSDProtectFrequency clé, ce qui provoque SDProp revenir à l’intervalle de 60 minutes. En règle générale ne réduisez pas cet intervalle dans les domaines de production car il peut augmenter LSASS une charge de traitement sur le contrôleur de domaine. L’impact de cette augmentation est dépend du nombre d’objets protégés dans le domaine.  

##### <a name="running-sdprop-manually"></a>Exécution manuelle de SDProp

Une meilleure approche de test des modifications de AdminSDHolder consiste à exécuter SDProp manuellement, ce qui amène la tâche à exécuter immédiatement, mais n’affecte pas l’exécution planifiée. Exécution manuelle de SDProp est légèrement différente sur les contrôleurs de domaine exécutant Windows Server 2008 et versions antérieures plutôt que sur les contrôleurs de domaine exécutant Windows Server 2012 ou Windows Server 2008 R2.  

Procédures pour l’exécution SDProp manuellement sur les systèmes d’exploitation plus anciens sont fournies dans [article du Support Microsoft 251343](https://support.microsoft.com/kb/251343), et Voici des instructions pas à pas pour les systèmes d’exploitation plus anciens et les versions ultérieures. Dans les deux cas, vous devez vous connecter à l’objet rootDSE dans Active Directory et effectuer une opération de modification avec un nom de domaine null pour l’objet rootDSE, en spécifiant le nom de l’opération en tant que l’attribut à modifier. Pour plus d’informations sur les opérations modifiables sur l’objet rootDSE, consultez [rootDSE modifier opérations](https://msdn.microsoft.com/library/cc223297.aspx) sur le site Web MSDN.  

###### <a name="running-sdprop-manually-in-windows-server-2008-or-earlier"></a>En cours d’exécution SDProp manuellement dans Windows Server 2008 ou version antérieure

Vous pouvez forcer SDProp à exécuter à l’aide de Ldp.exe ou en exécutant un script de modification LDAP. Pour exécuter SDProp à l’aide de Ldp.exe, procédez comme suit après avoir apporté des modifications à l’objet AdminSDHolder dans un domaine :  

1. Lancez **Ldp.exe**.  
2. Cliquez sur **connexion** dans la boîte de dialogue Ldp, cliquez sur **Connect**.  

   ![comptes protégés et groupes](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_9.gif)  

3. Dans le **Connect** boîte de dialogue, tapez le nom du contrôleur de domaine pour le domaine qui détient le rôle d’émulateur de contrôleur de domaine principal (PDCE) et cliquez sur **OK**.  

   ![comptes protégés et groupes](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_10.png)  

4. Vérifiez que vous êtes connecté avec succès, comme indiqué par **Dn : (RootDSE)**  dans la capture d’écran suivante, cliquez sur **connexion** et cliquez sur **lier**.  

   ![comptes protégés et groupes](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_11.png)  

5. Dans le **lier** boîte de dialogue, tapez les informations d’identification d’un compte d’utilisateur qui a l’autorisation de modifier l’objet rootDSE. (Si vous êtes connecté en tant que cet utilisateur, vous pouvez sélectionner **lier en tant que** utilisateur actuellement connecté.) Cliquez sur **OK**.  

   ![comptes protégés et groupes](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_12.png)  

6. Après avoir terminé l’opération de liaison, cliquez sur **Parcourir**, puis cliquez sur **modifier**.  

   ![comptes protégés et groupes](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_13.png)  

7. Dans le **modifier** boîte de dialogue, laissez le **DN** champ vide. Dans le **modifier l’entrée attribut** , tapez **FixUpInheritance**, puis, dans le **valeurs** , tapez **Oui**. Cliquez sur **entrée** pour remplir le **liste d’entrées** comme indiqué dans la capture d’écran suivante.  

   ![comptes protégés et groupes](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_14.gif)  

8. Dans la boîte de dialogue Modifier remplie, cliquez sur Exécuter et vérifiez que les modifications apportées à l’objet AdminSDHolder apparues sur cet objet.  

> [!NOTE]  
> Pour plus d’informations sur la modification AdminSDHolder pour autoriser des comptes non privilégiés désignés modifier l’appartenance à des groupes protégés, consultez [annexe i Création de comptes de gestion pour protégé des comptes et groupes dans Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  

Si vous préférez exécuter SDProp manuellement par le biais de LDIFDE ou d’un script, vous pouvez créer une entrée de modifier, comme illustré ici :  

![comptes protégés et groupes](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_15.gif)  

###### <a name="running-sdprop-manually-in-windows-server-2012-or-windows-server-2008-r2"></a>En cours d’exécution SDProp manuellement dans Windows Server 2012 ou Windows Server 2008 R2

Vous pouvez également forcer SDProp à exécuter à l’aide de Ldp.exe ou en exécutant un script de modification LDAP. Pour exécuter SDProp à l’aide de Ldp.exe, procédez comme suit après avoir apporté des modifications à l’objet AdminSDHolder dans un domaine :  

1. Lancez **Ldp.exe**.  

2. Dans le **Ldp** boîte de dialogue, cliquez sur **connexion**, puis cliquez sur **Connect**.  

   ![comptes protégés et groupes](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_16.gif)  

3. Dans le **Connect** boîte de dialogue, tapez le nom du contrôleur de domaine pour le domaine qui détient le rôle d’émulateur de contrôleur de domaine principal (PDCE) et cliquez sur **OK**.  

   ![comptes protégés et groupes](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_17.gif)  

4. Vérifiez que vous êtes connecté avec succès, comme indiqué par **Dn : (RootDSE)**  dans la capture d’écran suivante, cliquez sur **connexion** et cliquez sur **lier**.  

   ![comptes protégés et groupes](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_18.gif)  

5. Dans le **lier** boîte de dialogue, tapez les informations d’identification d’un compte d’utilisateur qui a l’autorisation de modifier l’objet rootDSE. (Si vous êtes connecté en tant que cet utilisateur, vous pouvez sélectionner **Bind utilisateur actuellement connecté**.) Cliquez sur **OK**.  

   ![comptes protégés et groupes](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_19.gif)  

6. Après avoir terminé l’opération de liaison, cliquez sur **Parcourir**, puis cliquez sur **modifier**.  

   ![comptes protégés et groupes](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_20.gif)  

7. Dans le **modifier** boîte de dialogue, laissez le **DN** champ vide. Dans le **modifier l’entrée attribut** , tapez **RunProtectAdminGroupsTask**, puis, dans le **valeurs** , tapez **1**. Cliquez sur **entrée** pour remplir la liste des entrées, comme illustré ici.  

   ![comptes protégés et groupes](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_21.gif)  

8. Dans rempli **modifier** boîte de dialogue, cliquez sur **exécuter**et vérifiez que les modifications apportées à l’objet AdminSDHolder apparues sur cet objet.  

Si vous préférez exécuter SDProp manuellement par le biais de LDIFDE ou d’un script, vous pouvez créer une entrée de modifier, comme illustré ici :  

![comptes protégés et groupes](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_22.gif)  
