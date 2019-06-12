---
title: Étape 2 configurer l’Infrastructure Multisite
description: Cette rubrique fait partie du guide de déploiement de plusieurs serveurs d’accès distant dans un déploiement Multisite dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: faec70ac-88c0-4b0a-85c7-f0fe21e28257
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f3b2eb55c11348c3abcb1ef9e234cd19ba727758
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446590"
---
# <a name="step-2-configure-the-multisite-infrastructure"></a>Étape 2 configurer l’Infrastructure Multisite

>S'applique à : Windows Server 2012 R2, Windows Server 2012

Pour configurer un déploiement multisite, il existe un nombre d’étapes requises pour modifier les paramètres d’infrastructure réseau, y compris : configuration des sites Active Directory supplémentaires et des contrôleurs de domaine, la configuration des groupes de sécurité supplémentaire et la configuration Objets de stratégie de groupe (GPO) si vous n’utilisez pas automatiquement configuré la stratégie de groupe.  
  
|Tâche|Description|  
|----|--------|  
|2.1. Configurez des sites Active Directory supplémentaires|Configurer des sites Active Directory supplémentaires pour le déploiement.|  
|2.2. Configurer les contrôleurs de domaine supplémentaires|Configurer les contrôleurs de domaine Active Directory supplémentaires en fonction des besoins.|  
|2.3. Configurer les groupes de sécurité|Configurer des groupes de sécurité pour tous les ordinateurs clients Windows 7.|  
|2.4. Configurer les objets de stratégie de groupe|Configurer des objets de stratégie de groupe supplémentaires en fonction des besoins.|  
  
> [!NOTE]  
> Cette rubrique inclut des exemples d'applets de commande Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, consultez [Utilisation des applets de commande](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_ConfigAD"></a>2.1. Configurez des sites Active Directory supplémentaires  
Tous les points d’entrée peuvent résider dans un seul site Active Directory. Par conséquent, au moins un site Active Directory est requis pour l’implémentation de serveurs d’accès à distance dans une configuration multisite. Utilisez cette procédure si vous avez besoin créer le premier site d’Active Directory, ou si vous souhaitez utiliser des sites Active Directory supplémentaires pour le déploiement multisite. Utilisez le composant logiciel enfichable Services et Sites Active Directory pour créer des sites de votre organisation « network s.  

L’appartenance à la **administrateurs de l’entreprise** groupe dans la forêt ou le **Admins du domaine** groupe dans le domaine racine de forêt, ou équivalent, au minimum est requis pour effectuer cette procédure. Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).  

Pour plus d’informations, consultez [Ajout d’un Site à la forêt](https://technet.microsoft.com/library/cc732761.aspx).  

### <a name="to-configure-additional-active-directory-sites"></a>Pour configurer des sites Active Directory supplémentaires  
  
1.  Sur le contrôleur de domaine principal, cliquez sur **Démarrer**, puis cliquez sur **Sites Active Directory et Services**.  
  
2.  Dans la console Services et Sites Active Directory, dans l’arborescence de la console, cliquez sur **Sites**, puis cliquez sur **nouveau Site**.  
  
3.  Sur le **nouvel objet - Site** boîte de dialogue le **nom** , entrez un nom pour le nouveau site.  
  
4.  Dans **nom du lien**, cliquez sur un objet de lien de site, puis cliquez sur **OK** à deux reprises.  
  
5.  Dans l’arborescence de la console, développez **Sites**, avec le bouton droit **sous-réseaux**, puis cliquez sur **nouveau sous-réseau**.  
  
6.  Sur le **nouvel objet - sous-réseau** boîte de dialogue **préfixe**, tapez le préfixe de sous-réseau IPv4 ou IPv6, dans le **sélectionnez un objet de site pour ce préfixe** , cliquez sur le site à associer avec ce sous-réseau, puis cliquez sur **OK**.  
  
7.  Répétez les étapes 5 et 6 jusqu'à ce que vous avez créé tous les sous-réseaux requis dans votre déploiement.  
  
8.  Fermez les Sites et Services Active Directory.  
  
![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
Pour installer la fonctionnalité de Windows « Du module Active Directory pour Windows PowerShell » :  
  
```  
Install-WindowsFeature "Name RSAT-AD-PowerShell  
```  
  
ou ajoutez le « Active Directory PowerShell enfichable » via OptionalFeatures.  
  
Si vous exécutez les applets de commande suivantes sur Windows 7" ou Windows Server 2008 R2, le module Active Directory PowerShell doit être importé :  
  
```  
Import-Module ActiveDirectory  
```  
  
Pour configurer un site Active Directory nommé « Seconde-Site », à l’aide de DEFAULTIPSITELINK intégré :  
  
```  
New-ADReplicationSite -Name "Second-Site"  
Set-ADReplicationSiteLink -Identity "DEFAULTIPSITELINK" -sitesIncluded @{Add="Second-Site"}  
```  
  
Pour configurer des sous-réseaux IPv4 et IPv6 pour le Site de seconde :  
  
```  
New-ADReplicationSubnet -Name "10.2.0.0/24" -Site "Second-Site"  
New-ADReplicationSubnet -Name "2001:db8:2::/64" -Site "Second-Site"  
```  
  
## <a name="BKMK_AddDC"></a>2.2. Configurer les contrôleurs de domaine supplémentaires  
Pour configurer un déploiement multisite dans un domaine unique, il est recommandé d’avoir au moins un contrôleur de domaine accessible en écriture pour chaque site dans votre déploiement.  
  
Pour effectuer cette procédure, au minimum, que vous devez être membre du groupe Admins du domaine dans le domaine dans lequel le contrôleur de domaine est en cours d’installation.  
  
Pour plus d’informations, consultez [installation d’un contrôleur de domaine supplémentaires](https://technet.microsoft.com/library/cc733027.aspx).
  
### <a name="to-configure-additional-domain-controllers"></a>Pour configurer les contrôleurs de domaine supplémentaires  
  
1.  Sur le serveur qui agira comme un contrôleur de domaine dans **le Gestionnaire de serveur**, dans le **tableau de bord**, cliquez sur **ajouter des rôles et fonctionnalités**.  
  
2.  Cliquez sur **suivant** trois fois pour accéder à l’écran de sélection de rôle de serveur  
  
3.  Sur le **sélectionner des rôles de serveur** , sélectionnez **Active Directory Domain Services**. Cliquez sur **ajouter des fonctionnalités** lorsque vous y êtes invité, puis cliquez sur **suivant** trois fois.  
  
4.  Dans la page **Confirmation**, cliquez sur **Installer**.  
  
5.  Une fois l’installation terminée, cliquez sur **promouvoir ce serveur en contrôleur de domaine**.  
  
6.  Dans l’Assistant de Configuration Active Directory domaine Services, sur le **Configuration de déploiement** , cliquez sur **ajouter un contrôleur de domaine à un domaine existant**.  
  
7.  Dans **domaine**, entrez le domaine nom ; par exemple, corp.contoso.com.  
  
8.  Sous **fournir les informations d’identification pour effectuer cette opération**, cliquez sur **modification**. Sur le **Windows Security** boîte de dialogue zone, fournissez le nom d’utilisateur et le mot de passe d’un compte qui peut installer le contrôleur de domaine supplémentaire. Pour installer un contrôleur de domaine supplémentaire, vous devez être membre du groupe Administrateurs de l’entreprise ou du groupe Admins du domaine. Une fois les informations d’identification fournies, cliquez sur **Suivant**.  
  
9. Sur le **Options du contrôleur de domaine** page, procédez comme suit :  
  
    1.  Effectuez les sélections suivantes :  
  
        -   **Serveur de noms (DNS) de domaine**« cette option est sélectionnée par défaut pour que votre contrôleur de domaine puisse fonctionner comme un serveur Domain Name System (DNS). Si vous ne souhaitez pas que le contrôleur de domaine soit un serveur DNS, désactivez cette case à cocher.  
  
            Si le rôle de serveur DNS n’est pas installé sur l’émulateur de contrôleur de domaine principal (PDC) dans le domaine racine de forêt, l’option d’installation du serveur DNS sur un contrôleur de domaine supplémentaire n’est pas disponible. Pour résoudre ce problème dans ce cas, vous pouvez installer le rôle serveur DNS avant ou après l’installation d’AD DS.  
  
            > [!NOTE]  
            > Si vous sélectionnez l’option d’installation du serveur DNS, vous pouvez recevoir un message indiquant qu’une délégation DNS pour le serveur DNS ne peut pas être créée et que vous devez créer manuellement une délégation DNS au serveur DNS pour garantir la résolution de noms fiable. Si vous installez un contrôleur de domaine supplémentaire dans le domaine racine de forêt ou un domaine racine d’arborescence, vous n’êtes pas obligé de créer la délégation DNS. Dans ce cas, cliquez sur **Oui** et ignorez le message.  
  
        -   **Catalogue global (GC)** « cette option est sélectionnée par défaut. Elle ajoute les partitions d’annuaire en lecture seule de catalogue global au contrôleur de domaine, et elle active la fonctionnalité de recherche dans le catalogue global.  
  
        -   **Le contrôleur de domaine en lecture seule (RODC)** « cette option n’est pas sélectionnée par défaut. Rend le contrôleur de domaine supplémentaires en lecture seule ; Autrement dit, il est le contrôleur de domaine un RODC.  
  
    2.  Dans **nom du Site**, sélectionnez un site dans la liste.  
  
    3.  Sous **tapez le mot de passe du Mode de restauration des Services annuaire (DSRM)** , dans **mot de passe** et **confirmer le mot de passe**, tapez un mot de passe fort à deux reprises, puis cliquez sur  **Suivant**. Ce mot de passe doit être utilisé pour démarrer les services AD DS en mode DSRM pour les tâches qui doivent être effectuées en mode hors connexion.  
  
10. Sur le **Options DNS** page, sélectionnez le **délégation DNS de mettre à jour** case à cocher si vous souhaitez mettre à jour la délégation DNS pendant l’installation du rôle, puis cliquez sur **suivant**.  
  
11. Sur le **des Options supplémentaires** page, tapez ou accédez aux emplacements de volume et de dossier pour le fichier de base de données, les fichiers journaux du service annuaire et les fichiers du volume (SYSVOL) système. Spécifiez les options de réplication si nécessaire, puis cliquez sur **suivant**.  
  
12. Sur le **passez en revue les Options** page, passez en revue les options d’installation, puis cliquez sur **suivant**.  
  
13. Sur le **vérification des prérequis** page, une fois les conditions préalables sont validés, cliquez sur **installer**.  
  
14. Attendez que l’Assistant a terminé la configuration, puis cliquez sur **fermer**.  
  
15. Redémarrez l’ordinateur si elle ne redémarre pas automatiquement.  
  
## <a name="BKMK_ConfigSG"></a>2.3. Configurer les groupes de sécurité  
Un déploiement multisite requiert un groupe de sécurité supplémentaires pour les ordinateurs clients Windows 7 pour chaque point d’entrée dans le déploiement qui autorise l’accès aux ordinateurs clients Windows 7. S’il existe plusieurs domaines contenant des ordinateurs clients Windows 7, il est recommandé pour créer un groupe de sécurité dans chaque domaine pour le même point d’entrée. Vous pouvez également, un groupe de sécurité universel contenant les ordinateurs clients à partir de ces deux domaines utilisable. Par exemple, dans un environnement comportant deux domaines, si vous souhaitez autoriser l’accès aux ordinateurs clients de Windows 7 dans les points d’entrée 1 et 3, mais pas dans l’entrée point 2, puis créez deux nouveaux groupes de sécurité pour qu’il contienne les ordinateurs clients de Windows 7 pour chaque point d’entrée dans chacun de la  domaines.  
  
### <a name="to-configure-additional-security-groups"></a>Pour configurer des groupes de sécurité supplémentaire  
  
1.  Sur le contrôleur de domaine principal, cliquez sur **Démarrer**, puis cliquez sur **Active Directory Users and Computers**.  
  
2.  Dans l’arborescence de la console, cliquez sur le dossier dans lequel vous souhaitez ajouter un nouveau groupe, par exemple, corp.contoso.com/Users. Pointez sur **Nouveau**, puis cliquez sur **Groupe**.  
  
3.  Sur le **nouvel objet - groupe** boîte de dialogue **nom_groupe**, tapez le nom du nouveau groupe, par exemple, Win7_Clients_Entrypoint1.  
  
4.  Sous **étendue du groupe**, cliquez sur **universelle**, sous **type de groupe**, cliquez sur **sécurité**, puis cliquez sur **OK**.  
  
5.  Pour ajouter des ordinateurs au nouveau groupe de sécurité, double-cliquez sur le groupe de sécurité, puis, dans le **< nom_groupe > Propriétés** boîte de dialogue, cliquez sur le **membres** onglet.  
  
6.  Sous l'onglet **Membres** , cliquez sur **Ajouter**.  
  
7.  Sélectionnez les ordinateurs Windows 7 à ajouter à ce groupe de sécurité, puis cliquez sur **OK**.  
  
8.  Répétez cette procédure pour créer un groupe de sécurité pour chaque point d’entrée en fonction des besoins.  
  
![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
Pour installer la fonctionnalité de Windows « Du module Active Directory pour Windows PowerShell » :  
  
```  
Install-WindowsFeature "Name RSAT-AD-PowerShell  
```  
  
ou ajoutez le « Active Directory PowerShell enfichable » via OptionalFeatures.  
  
Si vous exécutez les applets de commande suivantes sur Windows 7" ou Windows Server 2008 R2, le module Active Directory PowerShell doit être importé :  
  
```  
Import-Module ActiveDirectory  
```  
  
Pour configurer un groupe de sécurité nommé Win7_Clients_Entrypoint1 et pour ajouter un ordinateur client nommé CLIENT2 :  
  
```  
New-ADGroup -GroupScope universal -Name Win7_Clients_Entrypoint1  
Add-ADGroupMember -Identity Win7_Clients_Entrypoint1 -Members CLIENT2$  
```  
  
## <a name="ConfigGPOs"></a>2.4. Configurer les objets de stratégie de groupe  
Un déploiement de l’accès à distance multisite requiert les objets de stratégie de groupe suivants :  
  
-   Un objet stratégie de groupe pour chaque point d’entrée pour le serveur d’accès à distance.  
  
-   Un objet stratégie de groupe pour tous les ordinateurs clients Windows 8 pour chaque domaine.  
  
-   Un objet GPO dans chaque domaine contenant des ordinateurs clients Windows 7 pour chaque point d’entrée configuré pour prendre en charge Windows 7, les clients.  
  
    > [!NOTE]  
    > Si vous n’avez pas de tous les ordinateurs clients Windows 7, il est inutile de créer des ordinateurs de la stratégie de groupe pour Windows 7.  
  
Lorsque vous configurez l’accès à distance, l’Assistant crée automatiquement les objets de stratégie de groupe requis s’ils ne « t existe déjà. Si vous n’avez pas les autorisations requises pour créer des objets de stratégie de groupe, ceux-ci doivent être créés avant la configuration de l’accès à distance. L’administrateur de DirectAccess doit avoir des autorisations complètes sur les objets de stratégie de groupe (modifier + modification de la sécurité + SUPPR).  
  
> [!IMPORTANT]  
> Après avoir créé manuellement les objets de stratégie de groupe pour l’accès à distance, vous devez autoriser un délai suffisant pour Active Directory et la réplication DFS pour le contrôleur de domaine dans le site Active Directory qui est associé au serveur d’accès à distance. Si l’accès à distance créé automatiquement les objets de stratégie de groupe, aucun temps d’attente n’est requise.  
  
Pour créer des objets de stratégie de groupe, consultez [créer et modifier un objet de stratégie de groupe](https://technet.microsoft.com/library/cc754740.aspx).  
  
### <a name="DCMaintandDowntime"></a>Temps d’arrêt et de maintenance du contrôleur de domaine  
Lorsqu’un contrôleur de domaine en cours d’exécution en tant que l’émulateur de contrôleur de domaine principal, ou la gestion des GPO de serveur de contrôleurs de domaine rencontre des temps d’arrêt, il n’est pas possible de charger ou modifier la configuration de l’accès à distance. Cela n’affecte pas la connectivité du client si d’autres contrôleurs de domaine sont disponibles.  
  
Pour charger ou modifier la configuration de l’accès à distance, vous pouvez transférer le rôle d’émulateur PDC à un autre contrôleur de domaine pour le serveur de l’application cliente ou de stratégie de groupe ; pour le serveur de stratégie de groupe, modifiez les contrôleurs de domaine qui gèrent le serveur de stratégie de groupe.  
  
> [!IMPORTANT]  
> Cette opération peut être effectuée uniquement par un administrateur de domaine. L’impact de la modification du contrôleur de domaine principal n’est pas limité l’accès à distance ; Par conséquent, soyez prudent lorsque vous transférez le rôle d’émulateur PDC.  
  
> [!NOTE]  
> Avant de modifier l’association de contrôleur de domaine, assurez-vous que tous les GPO dans le déploiement de l’accès à distance ont été répliquées vers tous les contrôleurs de domaine dans le domaine. Si l’objet de stratégie de groupe n’est pas synchronisée, les modifications de configuration récentes peuvent être perdues après avoir modifié l’association de contrôleur de domaine, ce qui peut entraîner une configuration incorrecte. Pour vérifier la synchronisation de l’objet stratégie de groupe, consultez [vérification du statut d’Infrastructure de stratégie de groupe](https://technet.microsoft.com/library/jj134176.aspx).  
  
#### <a name="TransferPDC"></a>Pour transférer le rôle d’émulateur PDC  
  
1.  Sur le **Démarrer** , tapez**DSA.msc**, puis appuyez sur ENTRÉE.  
  
2.  Dans le volet gauche de la console Active Directory Users and Computers, cliquez sur **Active Directory Users and Computers**, puis cliquez sur **modifier le contrôleur de domaine**. Dans la boîte de dialogue Modifier le serveur de répertoire, cliquez sur **instance de ce contrôleur de domaine ou AD LDS**, dans la liste, cliquez sur le contrôleur de domaine qui sera le nouveau détenteur de rôle, puis cliquez sur **OK**.  
  
    > [!NOTE]  
    > Vous devez effectuer cette étape si vous n’êtes pas sur le contrôleur de domaine auquel vous souhaitez transférer le rôle. N’effectuez pas cette étape si vous êtes déjà connecté au contrôleur de domaine auquel vous souhaitez transférer le rôle.  
  
3.  Dans l’arborescence de la console, cliquez sur **Active Directory Users and Computers**, pointez sur **toutes les tâches**, puis cliquez sur **maîtres d’opérations**.  
  
4.  Dans la boîte de dialogue maîtres d’opérations, cliquez sur le **PDC** onglet, puis cliquez sur **modification**.  
  
5.  Cliquez sur **Oui** pour confirmer que vous souhaitez transférer le rôle, puis cliquez sur **fermer**.  
  
#### <a name="ChangeDC"></a>Pour modifier le contrôleur de domaine qui gère le serveur de stratégie de groupe  
  
-   Exécutez l’applet de commande Windows PowerShell `HYPERLINK "https://technet.microsoft.com/library/hh918412.aspx" Set-DAEntryPointDC` sur le serveur d’accès à distance et spécifiez le nom du contrôleur de domaine inaccessible pour le *ExistingDC* paramètre. Cette commande modifie l’association de contrôleur de domaine pour le serveur de stratégie de groupe des points d’entrée qui sont actuellement gérés par ce contrôleur de domaine.  
  
    -   Pour remplacer le contrôleur de domaine inaccessible « dc1.corp.contoso.com » par le contrôleur de domaine « dc2.corp.contoso.com », procédez comme suit :  
  
        ```  
        Set-DAEntryPointDC "ExistingDC 'dc1.corp.contoso.com' "NewDC 'dc2.corp.contoso.com' "ErrorAction Inquire  
        ```  
  
    -   Pour remplacer le contrôleur de domaine inaccessible « dc1.corp.contoso.com » avec un contrôleur de domaine dans le site Active Directory le plus proche pour le serveur d’accès à distance « DA1.corp.contoso.com », procédez comme suit :  
  
        ```  
        Set-DAEntryPointDC "ExistingDC 'dc1.corp.contoso.com' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
        ```  
  
### <a name="ChangeTwoDCs"></a>Modifier les deux ou plusieurs contrôleurs de domaine qui gèrent le serveur de stratégie de groupe  
Dans un nombre minimal de cas, deux ou plusieurs contrôleurs de domaine qui gèrent le serveur de stratégie de groupe ne sont pas disponibles. Si cela se produit, plusieurs étapes sont nécessaires pour modifier l’association de contrôleur de domaine pour le serveur de stratégie de groupe.  
  
Informations d’association de contrôleur de domaine sont stockées dans le Registre des serveurs d’accès à distance et dans tous les GPO de serveur. Dans l’exemple suivant, il existe deux points d’entrée avec deux serveurs d’accès distant, « DA1 » dans « point d’entrée 1 » et « DA2 » dans « Point d’entrée 2 ». Le serveur de stratégie de groupe du « Point d’entrée 1 » est géré dans le contrôleur de domaine « DC1 », tandis que l’objet stratégie de groupe server de « point d’entrée 2 » est géré dans le contrôleur de domaine « DC2 ». « DC1 » et « DC2 » ne sont pas disponibles. Un troisième contrôleur de domaine est toujours disponible dans le domaine, « DC3 », et les données à partir de « DC1 » et « DC2 » ont été déjà répliquées vers « DC3 ».  
  
![Configurer l’Infrastructure Multisite](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc1.png)  
  
##### <a name="to-change-two-or-more-domain-controllers-that-manage-server-gpos"></a>Pour modifier les deux ou plusieurs contrôleurs de domaine qui gèrent le serveur de stratégie de groupe  
  
1.  Pour remplacer le contrôleur de domaine non disponible « DC2 » par le contrôleur de domaine « DC3 », exécutez la commande suivante :  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC2' "NewDC 'DC3' "ComputerName 'DA2' "ErrorAction Continue  
    ```  
  
    Cette commande met à jour l’association contrôleur de domaine pour le serveur « Point d’entrée 2 » stratégie de groupe dans le Registre de DA2 et sur le serveur « Point d’entrée 2 » GPO lui-même ; Toutefois, il ne pas mettre à jour le GPO de serveur « Point d’entrée 1 », car le contrôleur de domaine qui gère l’il n’est pas disponible.  
  
    > [!TIP]  
    > Cette commande utilise la valeur de continuer pour le *ErrorAction* paramètre, qui met à jour le serveur « Point d’entrée 2 » GPO malgré l’échec de la mise à jour le GPO de serveur « Point d’entrée 1 ».  
  
    La configuration obtenue est illustrée dans le diagramme suivant.  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc2.png)  
  
2.  Pour remplacer le contrôleur de domaine non disponible « DC1 » par le contrôleur de domaine « DC3 », exécutez la commande suivante :  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC1' "NewDC 'DC3' "ComputerName 'DA2' "ErrorAction Continue  
    ```  
  
    Cette commande met à jour l’association de contrôleur de domaine pour le serveur de GPO de serveur dans le Registre de DA1 et dans le « Point d’entrée 1 » et « Point d’entrée 2 » de « Point d’entrée 1 » stratégie de groupe. La configuration obtenue est illustrée dans le diagramme suivant.  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc3.png)  
  
3.  Pour synchroniser l’association du contrôleur de domaine pour le serveur « Point d’entrée 2 » stratégie de groupe sur le serveur « Point d’entrée 1 » objet de stratégie de groupe, exécutez la commande pour remplacer « DC2 » avec « DC3 », spécifiez le serveur d’accès à distance dont le serveur GPO n’est pas synchronisé, dans ce cas « DA1 », le  *ComputerName* paramètre.  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC2' "NewDC 'DC3' "ComputerName 'DA1' "ErrorAction Continue  
    ```  
  
    La configuration finale est illustrée dans le diagramme suivant.  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssocFinal.png)  
  
### <a name="ConfigDistOptimization"></a>Optimisation de la distribution de configuration  
Pour apporter des modifications de configuration, les modifications sont appliquées uniquement une fois que le serveur de stratégie de groupe se propagent vers les serveurs d’accès à distance. Pour réduire le temps de distribution de configuration, l’accès à distance sélectionne automatiquement un contrôleur de domaine accessible en écriture qui est un lien hypertexte «<https://technet.microsoft.com/library/cc978016.aspx>» le plus proche pour le serveur d’accès à distance lors de la création de son serveur de stratégie de groupe.  
  
Dans certains scénarios, il peut être nécessaire de modifier manuellement le contrôleur de domaine qui gère un serveur de stratégie de groupe afin d’optimiser les temps de distribution de configuration :  
  
-   Il a été aucun contrôleur de domaine accessible en écriture dans le site Active Directory d’un serveur d’accès à distance au moment de l’ajouter en tant que point d’entrée. Un contrôleur de domaine accessible en écriture est désormais ajouté au site Active Directory du serveur d’accès à distance.  
  
-   Un changement d’adresse IP ou une modification de Sites Active Directory et les sous-réseaux avez déplacé le serveur d’accès à distance à un autre site Active Directory.  
  
-   L’association de contrôleur de domaine pour un point d’entrée a été modifiée manuellement en raison de travaux de maintenance sur un contrôleur de domaine, et le contrôleur de domaine est maintenant en ligne.  
  
Dans ces scénarios, exécutez l’applet de commande PowerShell `Set-DAEntryPointDC` sur le serveur d’accès à distance et spécifiez le nom du point d’entrée que vous souhaitez optimiser l’utilisation du paramètre *EntryPointName*. Vous devez le faire uniquement après les données de l’objet stratégie de groupe à partir du contrôleur de domaine stocke actuellement le serveur de stratégie de groupe ont été déjà entièrement répliquée vers le nouveau contrôleur de domaine souhaité.  
  
> [!NOTE]  
> Avant de modifier l’association de contrôleur de domaine, assurez-vous que tous les GPO dans le déploiement de l’accès à distance ont été répliquées vers tous les contrôleurs de domaine dans le domaine. Si l’objet de stratégie de groupe n’est pas synchronisée, les modifications de configuration récentes peuvent être perdues après avoir modifié l’association de contrôleur de domaine, ce qui peut entraîner une configuration incorrecte. Pour vérifier la synchronisation de l’objet stratégie de groupe, consultez [vérification du statut d’Infrastructure de stratégie de groupe](https://technet.microsoft.com/library/jj134176.aspx).  
  
Pour optimiser le temps de distribution de configuration, effectuez l’une des opérations suivantes :  
  
-   Pour gérer le serveur GPO d’entrée pointez « Point d’entrée 1 » sur un contrôleur de domaine dans le site Active Directory le plus proche sur le serveur d’accès à distance « DA1.corp.contoso.com », exécutez la commande suivante :  
  
    ```  
    Set-DAEntryPointDC "EntryPointName 'Entry point 1' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
    ```  
  
-   Pour gérer le serveur de GPO d’entrée point « Point d’entrée 1 » sur le domaine contrôleur « dc2.corp.contoso.com », exécutez la commande suivante :  
  
    ```  
    Set-DAEntryPointDC "EntryPointName 'Entry point 1' "NewDC 'dc2.corp.contoso.com' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
    ```  
  
    > [!NOTE]  
    > Lorsque vous modifiez le contrôleur de domaine associé à un point d’entrée spécifique, vous devez spécifier un serveur d’accès à distance est un membre de ce point d’entrée pour le *ComputerName* paramètre.  
  
## <a name="BKMK_Links"></a>Voir aussi  
  
-   [Étape 3 : Configurer le déploiement multisite](Step-3-Configure-the-Multisite-Deployment.md)  
-   [Étape 1 : Implémenter un seul serveur de déploiement de l’accès à distance](Step-1-Implement-a-Single-Server-Remote-Access-Deployment.md)  

