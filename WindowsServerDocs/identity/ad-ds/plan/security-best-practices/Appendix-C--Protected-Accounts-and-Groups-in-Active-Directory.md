---
ms.assetid: 5b2876ac-fe7d-4054-bfba-b692e57bc0d2
title: 'Annexe C : comptes et groupes protégés dans Active Directory'
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 606b3a42d70ee5c2a3479f9c9df2f95a495d6afd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408727"
---
# <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>Annexe C : Comptes et groupes protégés dans Active Directory

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

## <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>Annexe C : Comptes et groupes protégés dans Active Directory

Dans Active Directory, un ensemble par défaut de comptes et de groupes à privilèges élevés est considéré comme des groupes et des comptes protégés. Avec la plupart des objets dans Active Directory, les administrateurs délégués (les utilisateurs qui ont été délégués des autorisations pour gérer des objets Active Directory) peuvent modifier les autorisations sur les objets, y compris modifier les autorisations pour autoriser la modification des appartenances de les groupes, par exemple.  

Toutefois, avec des comptes et des groupes protégés, les autorisations des objets sont définies et appliquées via un processus automatique qui garantit que les autorisations sur les objets restent cohérentes même si les objets sont déplacés dans le répertoire. Même si une personne modifie manuellement les autorisations d’un objet protégé, ce processus garantit que les autorisations sont retournées rapidement à leurs valeurs par défaut.  

### <a name="protected-groups"></a>Groupes protégés

Le tableau suivant contient les groupes protégés dans Active Directory listés par le système d’exploitation du contrôleur de domaine.  

#### <a name="protected-accounts-and-groups-in-active-directory-by-operating-system"></a>Comptes et groupes protégés dans Active Directory par système d’exploitation

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

L’objectif de l’objet AdminSDHolder est de fournir des autorisations « template » pour les comptes et les groupes protégés dans le domaine. AdminSDHolder est automatiquement créé en tant qu’objet dans le conteneur système de chaque Active Directory domaine. Son chemin d’accès est le suivant : **CN = AdminSDHolder, CN = System, DC = < domain_component >, DC = < domain_component >?.**  

Contrairement à la plupart des objets du domaine Active Directory, qui sont détenus par le groupe administrateurs, AdminSDHolder appartient au groupe Admins du domaine. Par défaut, EAs peut apporter des modifications à l’objet AdminSDHolder d’un domaine, tout comme les groupes Admins du domaine et administrateurs du domaine. En outre, bien que le propriétaire par défaut de AdminSDHolder soit le groupe Admins du domaine du domaine, les membres des administrateurs ou administrateurs de l’entreprise peuvent prendre possession de l’objet.  

#### <a name="sdprop"></a>SDProp

SDProp est un processus qui s’exécute toutes les 60 minutes (par défaut) sur le contrôleur de domaine qui détient l’émulateur PDC du domaine (émulateur). SDProp compare les autorisations sur l’objet AdminSDHolder du domaine avec les autorisations sur les comptes et les groupes protégés dans le domaine. Si les autorisations sur l’un des comptes et groupes protégés ne correspondent pas aux autorisations sur l’objet AdminSDHolder, les autorisations sur les comptes et les groupes protégés sont réinitialisées pour correspondre à celles de l’objet AdminSDHolder du domaine.  

En outre, l’héritage des autorisations est désactivé sur les groupes et les comptes protégés, ce qui signifie que même si les comptes et les groupes sont déplacés vers des emplacements différents dans l’annuaire, ils n’héritent pas des autorisations de leurs nouveaux objets parents. L’héritage est désactivé sur l’objet AdminSDHolder afin que les modifications apportées aux autorisations des objets parents ne modifient pas les autorisations de AdminSDHolder.  

##### <a name="changing-sdprop-interval"></a>Modification de l’intervalle de SDProp

Normalement, vous ne devez pas modifier l’intervalle auquel SDProp s’exécute, sauf à des fins de test. Si vous devez modifier l’intervalle SDProp, sur le émulateur du domaine, utilisez regedit pour ajouter ou modifier la valeur DWORD AdminSDProtectFrequency dans HKLM\SYSTEM\CurrentControlSet\Services\NTDS\Parameters.  

La plage de valeurs est en secondes comprise entre 60 et 7200 (une minute à deux heures). Pour annuler les modifications, supprimez la clé AdminSDProtectFrequency, ce qui entraîne la restauration de SDProp à l’intervalle de 60 minutes. En général, vous ne devez pas réduire cet intervalle dans les domaines de production, car cela peut augmenter la charge de traitement LSASS sur le contrôleur de domaine. L’impact de cette augmentation dépend du nombre d’objets protégés dans le domaine.  

##### <a name="running-sdprop-manually"></a>Exécution manuelle de SDProp

Une meilleure approche pour tester les modifications de AdminSDHolder consiste à exécuter SDProp manuellement, ce qui entraîne l’exécution immédiate de la tâche, mais n’affecte pas l’exécution planifiée. L’exécution manuelle de SDProp s’effectue légèrement différemment sur les contrôleurs de domaine exécutant Windows Server 2008 et antérieurs sur des contrôleurs de domaine exécutant Windows Server 2012 ou Windows Server 2008 R2.  

Les procédures d’exécution manuelle de SDProp sur des systèmes d’exploitation plus anciens sont fournies dans [support Microsoft article 251343](https://support.microsoft.com/kb/251343), et les instructions suivantes sont des instructions pas à pas pour les systèmes d’exploitation plus anciens et plus récents. Dans les deux cas, vous devez vous connecter à l’objet rootDSE dans Active Directory et effectuer une opération de modification avec un DN null pour l’objet rootDSE, en spécifiant le nom de l’opération comme attribut à modifier. Pour plus d’informations sur les opérations modifiables sur l’objet rootDSE, consultez [opérations de modification RootDSE](https://msdn.microsoft.com/library/cc223297.aspx) sur le site Web MSDN.  

###### <a name="running-sdprop-manually-in-windows-server-2008-or-earlier"></a>Exécution manuelle de SDProp dans Windows Server 2008 ou version antérieure

Vous pouvez forcer l’exécution de SDProp à l’aide de Ldp. exe ou en exécutant un script de modification LDAP. Pour exécuter SDProp à l’aide de Ldp. exe, procédez comme suit après avoir apporté des modifications à l’objet AdminSDHolder dans un domaine :  

1. Lancez **LDP. exe**.  
2. Cliquez sur **connexion** dans la boîte de dialogue Ldp, puis cliquez sur **se connecter**.  

   ![comptes et groupes protégés](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_9.gif)  

3. Dans la boîte de dialogue **connexion** , tapez le nom du contrôleur de domaine pour le domaine qui détient le rôle d’émulateur de contrôleur de domaine principal (émulateur), puis cliquez sur **OK**.  

   ![comptes et groupes protégés](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_10.png)  

4. Vérifiez que vous vous êtes correctement connecté, comme indiqué par **Dn : (RootDSE)**  dans la capture d’écran suivante, cliquez sur **connexion** , puis sur **lier**.  

   ![comptes et groupes protégés](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_11.png)  

5. Dans la boîte de dialogue **lier** , tapez les informations d’identification d’un compte d’utilisateur qui a l’autorisation de modifier l’objet rootDSE. (Si vous êtes connecté en tant qu’utilisateur, vous pouvez sélectionner **lier comme** utilisateur actuellement connecté.) Cliquez sur **OK**.  

   ![comptes et groupes protégés](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_12.png)  

6. Une fois l’opération de liaison terminée, cliquez sur **Parcourir**, puis sur **modifier**.  

   ![comptes et groupes protégés](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_13.png)  

7. Dans la boîte de dialogue **modifier** , laissez le champ **DN** vide. Dans le champ **modifier l’attribut d’entrée** , tapez **FixUpInheritance**, puis, dans le champ **valeurs** , tapez **Oui**. Cliquez sur **entrée** pour remplir la **liste d’entrées** comme indiqué dans la capture d’écran suivante.  

   ![comptes et groupes protégés](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_14.gif)  

8. Dans la boîte de dialogue modification remplie, cliquez sur Exécuter, puis vérifiez que les modifications que vous avez apportées à l’objet AdminSDHolder sont apparues sur cet objet.  

> [!NOTE]  
> Pour plus d’informations sur la modification de AdminSDHolder pour permettre aux comptes non privilégiés désignés de modifier l’appartenance aux groupes protégés, consultez [Appendix I : Création de comptes de gestion pour les comptes et les groupes protégés dans Active Directory @ no__t-0.  

Si vous préférez exécuter SDProp manuellement via LDIFDE ou un script, vous pouvez créer une entrée de modification comme indiqué ici :  

![comptes et groupes protégés](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_15.gif)  

###### <a name="running-sdprop-manually-in-windows-server-2012-or-windows-server-2008-r2"></a>Exécution manuelle de SDProp dans Windows Server 2012 ou Windows Server 2008 R2

Vous pouvez également forcer l’exécution de SDProp à l’aide de Ldp. exe ou en exécutant un script de modification LDAP. Pour exécuter SDProp à l’aide de Ldp. exe, procédez comme suit après avoir apporté des modifications à l’objet AdminSDHolder dans un domaine :  

1. Lancez **LDP. exe**.  

2. Dans la boîte de dialogue **LDP** , cliquez sur **connexion**, puis sur **se connecter**.  

   ![comptes et groupes protégés](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_16.gif)  

3. Dans la boîte de dialogue **connexion** , tapez le nom du contrôleur de domaine pour le domaine qui détient le rôle d’émulateur de contrôleur de domaine principal (émulateur), puis cliquez sur **OK**.  

   ![comptes et groupes protégés](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_17.gif)  

4. Vérifiez que vous vous êtes correctement connecté, comme indiqué par **Dn : (RootDSE)**  dans la capture d’écran suivante, cliquez sur **connexion** , puis sur **lier**.  

   ![comptes et groupes protégés](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_18.gif)  

5. Dans la boîte de dialogue **lier** , tapez les informations d’identification d’un compte d’utilisateur qui a l’autorisation de modifier l’objet rootDSE. (Si vous êtes connecté en tant qu’utilisateur, vous pouvez sélectionner **lier comme utilisateur actuellement connecté**.) Cliquez sur **OK**.  

   ![comptes et groupes protégés](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_19.gif)  

6. Une fois l’opération de liaison terminée, cliquez sur **Parcourir**, puis sur **modifier**.  

   ![comptes et groupes protégés](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_20.gif)  

7. Dans la boîte de dialogue **modifier** , laissez le champ **DN** vide. Dans le champ **modifier l’attribut d’entrée** , tapez **RunProtectAdminGroupsTask**, puis dans le champ **valeurs** , tapez **1**. Cliquez sur **entrée** pour remplir la liste d’entrées comme indiqué ici.  

   ![comptes et groupes protégés](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_21.gif)  

8. Dans la boîte de dialogue **modification** remplie, cliquez sur **exécuter**, puis vérifiez que les modifications que vous avez apportées à l’objet AdminSDHolder sont apparues sur cet objet.  

Si vous préférez exécuter SDProp manuellement via LDIFDE ou un script, vous pouvez créer une entrée de modification comme indiqué ici :  

![comptes et groupes protégés](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_22.gif)  
