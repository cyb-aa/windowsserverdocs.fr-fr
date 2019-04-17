---
ms.assetid: 5b2876ac-fe7d-4054-bfba-b692e57bc0d2
title: "Annexe C - protégé des comptes et groupes dans Active Directory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c1d29b61844428115f8b2d1f5d935f225a249659
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>Annexe c: comptes protégés et groupes dans ActiveDirectory

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012


## <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>Annexe c: comptes protégés et groupes dans ActiveDirectory  
Dans Active Directory, un ensemble de comptes dotés de privilèges élevés et les groupes par défaut sont considérés comme des comptes protégés et des groupes. Avec la plupart des objets dans Active Directory, les administrateurs délégués (les utilisateurs qui ont les autorisations nécessaires pour gérer les objets Active Directory) peuvent modifier les autorisations sur les objets, y compris la modification des autorisations pour permettre à eux-mêmes à modifier les appartenances aux groupes, par exemple.  

Toutefois, avec des comptes protégés et groupes, les autorisations des objets sont définir et appliquées via un processus automatique qui garantit que les autorisations sur le reste des objets cohérent même si les objets sont déplacement le répertoire. Même si quelqu'un change manuellement les autorisations d’un objet protégé, ce processus permet de s’assurer que les autorisations sont renvoyées rapidement à leurs valeurs par défaut.  

### <a name="protected-groups"></a>Groupes protégés  
Le tableau suivant contient les groupes protégés dans Active Directory répertoriés par le système d’exploitation de contrôleur de domaine.  

**Comptes protégés et groupes dans Active Directory par le système d’exploitation**  

|||||  
|-|-|-|-|  
|**Windows 2000 < SP4**|**Windows 2000 SP4 - Windows Server 2003 RTM**|**Windows Server 2003 SP1 +**|**Windows Server 2012, Windows Server 2008 R2, Windows Server 2008**|  
|Administrateurs|Opérateurs de compte|Opérateurs de compte|Opérateurs de compte|  
||Administrateur|Administrateur|Administrateur|  
||Administrateurs|Administrateurs|Administrateurs|  
||Opérateurs de sauvegarde|Opérateurs de sauvegarde|Opérateurs de sauvegarde|  
||Éditeurs de certificats|||  
|Admins du domaine|Admins du domaine|Admins du domaine|Admins du domaine|  
||Contrôleurs de domaine|Contrôleurs de domaine|Contrôleurs de domaine|  
|Administrateurs de l’entreprise|Administrateurs de l’entreprise|Administrateurs de l’entreprise|Administrateurs de l’entreprise|  
||Krbtgt|Krbtgt|Krbtgt|  
||Opérateurs d’impression|Opérateurs d’impression|Opérateurs d’impression|  
||||Contrôleurs de domaine en lecture seule|  
||Duplicateurs|Duplicateurs|Duplicateurs|  
|Administrateurs du schéma|Administrateurs du schéma|Administrateurs du schéma|Administrateurs du schéma|  
||Opérateurs de serveur|Opérateurs de serveur|Opérateurs de serveur|  

#### <a name="adminsdholder"></a>AdminSDHolder  
L’objectif de l’objet AdminSDHolder consiste à fournir des autorisations «modèle» pour les comptes protégés et les groupes dans le domaine. AdminSDHolder est automatiquement créé en tant qu’objet dans le conteneur système de chaque domaine Active Directory. Son chemin d’accès: **CN = AdminSDHolder, CN = System, DC = < domain_component >, DC = < domain_component >?.**  

Contrairement à la plupart des objets dans le domaine Active Directory, qui sont détenus par le groupe Administrateurs, AdminSDHolder est détenu par le groupe Admins du domaine. Par défaut, EAs peuvent apporter des modifications à l’objet AdminSDHolder de n’importe quel domaine comme des groupes d’administrateurs du domaine et administrateurs du domaine. En outre, bien que le propriétaire par défaut de AdminSDHolder Admins du domaine du domaine, les membres du groupe Administrateurs ou administrateurs de l’entreprise peuvent prendre possession de l’objet.  

#### <a name="sdprop"></a>SDProp  
SDProp est un processus qui s’exécute toutes les 60 minutes (par défaut) sur le contrôleur de domaine qui détient l’émulateur de contrôleur de domaine principal (PDCE du domaine). SDProp compare les autorisations sur l’objet AdminSDHolder du domaine avec les autorisations sur les comptes protégés et les groupes dans le domaine. Si les autorisations sur un des comptes protégés et des groupes ne correspondent pas les autorisations sur l’objet AdminSDHolder, les autorisations sur les comptes protégés et les groupes sont rétablies pour correspondre à celles de l’objet AdminSDHolder du domaine.  

En outre, l’héritage des autorisations est désactivé sur protégé groupes et comptes, ce qui signifie que même si les comptes et les groupes sont déplacés vers différents emplacements dans le répertoire, ils n’héritent pas des autorisations de leurs nouveaux objets parents. L’héritage est désactivé sur l’objet AdminSDHolder afin que les modifications des autorisations pour les objets parents ne modifiez pas les autorisations de AdminSDHolder.  

##### <a name="changing-sdprop-interval"></a>Modification de SDProp intervalle  
En règle générale, vous ne devez pas modifier l’intervalle sur lequel s’exécute SDProp, à l’exception des fins de test. Si vous avez besoin modifier l’intervalle SDProp, sur l’émulateur pour le domaine, utiliser regedit pour ajouter ou modifier la valeur DWORD AdminSDProtectFrequency HKLM\SYSTEM\CurrentControlSet\Services\NTDS\Parameters.  

La plage de valeurs est exprimée en secondes de 60 à 7200 (une minute à deux heures). Pour annuler les modifications, supprimez la clé AdminSDProtectFrequency, ce qui entraînera SDProp revenir à l’intervalle de 60 minutes. En règle générale ne réduisez pas cet intervalle dans les domaines de production comme il peut augmenter LSASS une charge de traitement sur le contrôleur de domaine. L’impact de cette augmentation est dépend du nombre d’objets protégés dans le domaine.  

##### <a name="running-sdprop-manually"></a>Exécution manuelle de SDProp  
Une meilleure approche pour tester les modifications AdminSDHolder consiste à exécuter SDProp manuellement, ce qui entraîne la tâche à exécuter immédiatement, mais n’affecte pas l’exécution planifiée. Exécution manuelle de SDProp est effectué légèrement différemment sur les contrôleurs de domaine exécutant Windows Server 2008 et versions antérieures plutôt que sur les contrôleurs de domaine exécutant Windows Server 2012 ou Windows Server 2008 R2.  

Procédures pour l’exécution SDProp manuellement sur des systèmes d’exploitation plus anciens sont fournies dans [article du Support Microsoft 251343](https://support.microsoft.com/kb/251343), et Voici des instructions détaillées sur les systèmes d’exploitation plus anciens et les plus récentes. Dans les deux cas, vous devez vous connecter à l’objet rootDSE dans Active Directory et effectuer une opération de modification avec un nom unique null pour l’objet rootDSE, en spécifiant le nom de l’opération en tant que l’attribut à modifier. Pour plus d’informations sur les opérations modifiables sur l’objet rootDSE, voir [rootDSE modifier Operations](https://msdn.microsoft.com/library/cc223297.aspx) sur le site Web MSDN.  

###### <a name="running-sdprop-manually-in-windows-server-2008-or-earlier"></a>En cours d’exécution SDProp manuellement dans Windows Server 2008 ou une version antérieure  
Vous pouvez forcer SDProp à exécuter à l’aide de Ldp.exe ou en exécutant un script de modification LDAP. Pour exécuter SDProp à l’aide de Ldp.exe, effectuez les étapes suivantes une fois que vous avez apporté des modifications à l’objet AdminSDHolder dans un domaine:  

1.  Lancer **Ldp.exe**.  

2.  Cliquez sur **connexion** dans la boîte de dialogue Ldp, cliquez sur **Connect**.  

    ![comptes protégés et groupes](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_9.gif)  

3.  Dans le **Connect** boîte de dialogue, tapez le nom du contrôleur de domaine pour le domaine qui détient le rôle d’émulateur de contrôleur de domaine principal (PDCE) et cliquez sur **OK**.  

    ![comptes protégés et groupes](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_10.png)  

4.  Vérifiez que vous êtes connecté correctement, comme indiqué par **Dn: (RootDSE)** dans la capture d’écran suivante, cliquez sur **connexion** et cliquez sur **liaison**.  

    ![comptes protégés et groupes](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_11.png)  

5.  Dans le **liaison** boîte de dialogue, tapez les informations d’identification d’un compte d’utilisateur qui a l’autorisation de modifier l’objet rootDSE. (Si vous êtes connecté en tant qu’utilisateur, vous pouvez sélectionner **liaison en tant que** utilisateur actuellement connecté.) Cliquez sur **OK**.  

    ![comptes protégés et groupes](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_12.png)  

6.  Après avoir terminé l’opération de liaison, cliquez sur **Parcourir**, puis cliquez sur **modifier**.  

    ![comptes protégés et groupes](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_13.png)  

7.  Dans le **modifier** boîte de dialogue, laissez le **DN** champ vide. Dans le **modifier l’entrée attribut** , tapez **FixUpInheritance**et dans le **valeurs** , tapez **Oui**. Cliquez sur **entrée** pour remplir la **liste d’entrées** comme illustré dans la capture d’écran suivante.  

    ![comptes protégés et groupes](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_14.gif)  

8.  Dans la boîte de dialogue Modifier remplie, cliquez sur Exécuter et vérifiez que les modifications apportées à l’objet AdminSDHolder apparus sur cet objet.  

> [!NOTE]  
> Pour plus d’informations sur la modification AdminSDHolder pour autoriser les comptes non privilégiés désignés modifier l’appartenance aux groupes protégés, consultez [annexe i: création de gestion des comptes pour des comptes protégés et les groupes dans Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  

Si vous préférez exécuter SDProp manuellement via LDIFDE ou un script, vous pouvez créer une entrée de modifier, comme illustré ici:  

![comptes protégés et groupes](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_15.gif)  

###### <a name="running-sdprop-manually-in-windows-server-2012-or-windows-server-2008-r2"></a>En cours d’exécution SDProp manuellement dans Windows Server 2012 ou Windows Server 2008 R2  
Vous pouvez également forcer SDProp à exécuter à l’aide de Ldp.exe ou en exécutant un script de modification LDAP. Pour exécuter SDProp à l’aide de Ldp.exe, effectuez les étapes suivantes une fois que vous avez apporté des modifications à l’objet AdminSDHolder dans un domaine:  

1.  Lancer **Ldp.exe**.  

2.  Dans le **Ldp** boîte de dialogue, cliquez sur **connexion**, puis cliquez sur **Connect**.  

    ![comptes protégés et groupes](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_16.gif)  

3.  Dans le **Connect** boîte de dialogue, tapez le nom du contrôleur de domaine pour le domaine qui détient le rôle d’émulateur de contrôleur de domaine principal (PDCE) et cliquez sur **OK**.  

    ![comptes protégés et groupes](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_17.gif)  

4.  Vérifiez que vous êtes connecté correctement, comme indiqué par **Dn: (RootDSE)** dans la capture d’écran suivante, cliquez sur **connexion** et cliquez sur **liaison**.  

    ![comptes protégés et groupes](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_18.gif)  

5.  Dans le **liaison** boîte de dialogue, tapez les informations d’identification d’un compte d’utilisateur qui a l’autorisation de modifier l’objet rootDSE. (Si vous êtes connecté en tant qu’utilisateur, vous pouvez sélectionner **liaison utilisateur actuellement connecté**.) Cliquez sur **OK**.  

    ![comptes protégés et groupes](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_19.gif)  

6.  Après avoir terminé l’opération de liaison, cliquez sur **Parcourir**, puis cliquez sur **modifier**.  

    ![comptes protégés et groupes](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_20.gif)  

7.  Dans le **modifier** boîte de dialogue, laissez le **DN** champ vide. Dans le **modifier l’entrée attribut** , tapez **RunProtectAdminGroupsTask**et dans le **valeurs** , tapez **1**. Cliquez sur **entrée** pour remplir la liste d’entrées, comme illustré ici.  

    ![comptes protégés et groupes](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_21.gif)  

8.  Dans la liste remplie **modifier** boîte de dialogue, cliquez sur **exécuter**et vérifiez que les modifications apportées à l’objet AdminSDHolder apparus sur cet objet.  

Si vous préférez exécuter SDProp manuellement via LDIFDE ou un script, vous pouvez créer une entrée de modifier, comme illustré ici:  

![comptes protégés et groupes](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_22.gif)  
