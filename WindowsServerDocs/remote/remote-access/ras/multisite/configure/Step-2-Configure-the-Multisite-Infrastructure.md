---
title: Étape 2 configurer l’infrastructure multisite
description: Cette rubrique fait partie du guide déployer plusieurs serveurs d’accès à distance dans un déploiement multisite dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: faec70ac-88c0-4b0a-85c7-f0fe21e28257
ms.author: lizross
author: eross-msft
ms.openlocfilehash: b636396ac7102d52ca5700dd2a4a4f81357b1685
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858397"
---
# <a name="step-2-configure-the-multisite-infrastructure"></a>Étape 2 configurer l’infrastructure multisite

>S’applique à : Windows Server 2012 R2, Windows Server 2012

Pour configurer un déploiement multisite, vous devez effectuer un certain nombre d’étapes pour modifier les paramètres de l’infrastructure réseau, notamment : configurer des sites Active Directory supplémentaires et des contrôleurs de domaine, configurer des groupes de sécurité supplémentaires et configurer Stratégie de groupe les objets (GPO) si vous n’utilisez pas les objets de stratégie de groupe configurés automatiquement.  
  
|Tâche|Description|  
|----|--------|  
|2.1. Configurer des sites Active Directory supplémentaires|Configurez des sites de Active Directory supplémentaires pour le déploiement.|  
|2.2. Configurer des contrôleurs de domaine supplémentaires|Configurez des contrôleurs de domaine Active Directory supplémentaires selon les besoins.|  
|2.3. Configurer les groupes de sécurité|Configurez des groupes de sécurité pour tous les ordinateurs clients Windows 7.|  
|2.4. Configurer les objets de stratégie de groupe|Configurez des objets stratégie de groupe supplémentaires selon les besoins.|  
  
> [!NOTE]  
> Cette rubrique comprend des exemples d'applets de commande Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, consultez [Utilisation des applets de commande](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="21-configure-additional-active-directory-sites"></a><a name="BKMK_ConfigAD"></a>2,1. Configurer des sites Active Directory supplémentaires  
Tous les points d’entrée peuvent résider sur un seul site Active Directory. Par conséquent, au moins un site Active Directory est requis pour l’implémentation des serveurs d’accès à distance dans une configuration multisite. Utilisez cette procédure si vous avez besoin de créer le premier site Active Directory ou si vous souhaitez utiliser des sites Active Directory supplémentaires pour le déploiement multisite. Utilisez le composant logiciel enfichable sites et services Active Directory pour créer des sites dans le réseau « s » de votre organisation.  

Pour effectuer cette procédure, vous devez appartenir au minimum au groupe administrateurs de l' **entreprise** dans la forêt ou au groupe **Admins du domaine** dans le domaine racine de la forêt, ou à un groupe équivalent. Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).  

Pour plus d’informations, consultez [Ajout d’un site à la forêt](https://technet.microsoft.com/library/cc732761.aspx).  

### <a name="to-configure-additional-active-directory-sites"></a>Pour configurer des sites Active Directory supplémentaires  
  
1.  Sur le contrôleur de domaine principal, cliquez sur **Démarrer**, puis sur **Sites et services Active Directory**.  
  
2.  Dans la console sites et services Active Directory, dans l’arborescence de la console, cliquez avec le bouton droit sur **sites**, puis cliquez sur **nouveau site**.  
  
3.  Dans la boîte de dialogue **nouvel objet-site** , dans la zone **nom** , entrez un nom pour le nouveau site.  
  
4.  Dans **nom du lien**, cliquez sur un objet lien de sites, puis cliquez deux fois sur **OK** .  
  
5.  Dans l’arborescence de la console, développez **sites**, cliquez avec le bouton droit sur **sous-réseaux**, puis cliquez sur **nouveau sous-réseau**.  
  
6.  Dans la boîte de dialogue **nouvel objet-sous-réseau** , sous **préfixe**, tapez le préfixe de sous-réseau IPv4 ou IPv6 dans la liste **Sélectionnez un objet de site pour ce préfixe** , cliquez sur le site à associer à ce sous-réseau, puis cliquez sur **OK**.  
  
7.  Répétez les étapes 5 et 6 jusqu’à ce que vous ayez créé tous les sous-réseaux requis dans votre déploiement.  
  
8.  Fermez Active Directory sites et services.  
  
![les commandes Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/PowerShellLogoSmall.gif)***<em>équivalentes</em> Windows PowerShell***  
  
La ou les applets de commande Windows PowerShell suivantes ont la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles apparaissent ici sur plusieurs lignes en raison de contraintes de mise en forme.  
  
Pour installer la fonctionnalité Windows « Active Directory module pour Windows PowerShell » :  
  
```  
Install-WindowsFeature "Name RSAT-AD-PowerShell  
```  
  
ou ajoutez le « composant logiciel enfichable PowerShell Active Directory » via OptionalFeatures.  
  
Si vous exécutez les applets de commande suivantes sur Windows 7 ou Windows Server 2008 R2, le module PowerShell Active Directory doit être importé :  
  
```  
Import-Module ActiveDirectory  
```  
  
Pour configurer un site Active Directory nommé « second-site » à l’aide de l’intégration DEFAULTIPSITELINK :  
  
```  
New-ADReplicationSite -Name "Second-Site"  
Set-ADReplicationSiteLink -Identity "DEFAULTIPSITELINK" -sitesIncluded @{Add="Second-Site"}  
```  
  
Pour configurer des sous-réseaux IPv4 et IPv6 pour le deuxième site :  
  
```  
New-ADReplicationSubnet -Name "10.2.0.0/24" -Site "Second-Site"  
New-ADReplicationSubnet -Name "2001:db8:2::/64" -Site "Second-Site"  
```  
  
## <a name="22-configure-additional-domain-controllers"></a><a name="BKMK_AddDC"></a>2,2. Configurer des contrôleurs de domaine supplémentaires  
Pour configurer un déploiement multisite dans un domaine unique, il est recommandé de disposer d’au moins un contrôleur de domaine accessible en écriture pour chaque site de votre déploiement.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe Admins du domaine dans le domaine dans lequel le contrôleur de domaine est installé.  
  
Pour plus d’informations, consultez [installation d’un contrôleur de domaine supplémentaire](https://technet.microsoft.com/library/cc733027.aspx).
  
### <a name="to-configure-additional-domain-controllers"></a>Pour configurer des contrôleurs de domaine supplémentaires  
  
1.  Sur le serveur qui jouera le rôle de contrôleur de domaine, dans **Gestionnaire de serveur**, dans le **tableau de bord**, cliquez sur **Ajouter des rôles et des fonctionnalités**.  
  
2.  Cliquez sur **suivant** trois fois pour accéder à l’écran de sélection du rôle de serveur  
  
3.  Dans la page **Sélectionner des rôles de serveurs** , sélectionnez **Active Directory Domain Services**. Cliquez sur **Ajouter des fonctionnalités** lorsque vous y êtes invité, puis cliquez sur **suivant** trois fois.  
  
4.  Dans la page **Confirmation**, cliquez sur **Installer**.  
  
5.  Une fois l’installation terminée, cliquez sur **promouvoir ce serveur en contrôleur de domaine**.  
  
6.  Dans l’Assistant Configuration de Active Directory Domain Services, dans la page **configuration du déploiement** , cliquez sur **Ajouter un contrôleur de domaine à un domaine existant**.  
  
7.  Dans **domaine**, entrez le nom de domaine ; par exemple, corp.contoso.com.  
  
8.  Sous **fournir les informations d’identification pour effectuer cette opération**, cliquez sur **modifier**. Dans la boîte de dialogue **sécurité Windows** , indiquez le nom d’utilisateur et le mot de passe d’un compte pouvant installer le contrôleur de domaine supplémentaire. Pour installer un contrôleur de domaine supplémentaire, vous devez être membre du groupe Administrateurs de l’entreprise ou du groupe Admins du domaine. Une fois les informations d’identification fournies, cliquez sur **Suivant**.  
  
9. Dans la page **Options du contrôleur de domaine** , procédez comme suit :  
  
    1.  Effectuez les sélections suivantes :  
  
        -   **Serveur DNS (Domain Name System)** "cette option est sélectionnée par défaut afin que votre contrôleur de domaine puisse fonctionner en tant que serveur DNS (Domain Name System). Si vous ne souhaitez pas que le contrôleur de domaine soit un serveur DNS, désactivez cette case à cocher.  
  
            Si le rôle de serveur DNS n’est pas installé sur l’émulateur de contrôleur de domaine principal (PDC) dans le domaine racine de forêt, l’option d’installation du serveur DNS sur un contrôleur de domaine supplémentaire n’est pas disponible. En guise de solution de contournement dans ce cas, vous pouvez installer le rôle serveur DNS avant ou après l’installation de l’AD DS.  
  
            > [!NOTE]  
            > Si vous sélectionnez l’option d’installation du serveur DNS, vous pouvez recevoir un message indiquant qu’une délégation DNS pour le serveur DNS n’a pas pu être créée et que vous devez créer manuellement une délégation DNS au serveur DNS pour garantir une résolution de noms fiable. Si vous installez un contrôleur de domaine supplémentaire dans le domaine racine de la forêt ou dans un domaine racine de l’arborescence, il n’est pas nécessaire de créer la délégation DNS. Dans ce cas, cliquez sur **Oui** et ignorez le message.  
  
        -   **Catalogue global (GC)** "cette option est sélectionnée par défaut. Elle ajoute les partitions d’annuaire en lecture seule de catalogue global au contrôleur de domaine, et elle active la fonctionnalité de recherche dans le catalogue global.  
  
        -   **Contrôleur de domaine en lecture seule (RODC)** "cette option n’est pas sélectionnée par défaut. Il rend le contrôleur de domaine supplémentaire en lecture seule ; autrement dit, il transforme le contrôleur de domaine en RODC.  
  
    2.  Dans **nom du site**, sélectionnez un site dans la liste.  
  
    3.  Sous **tapez le mot de passe du mode de restauration des services d’annuaire (DSRM)** , dans **mot** de passe et **confirmer le mot de passe**, tapez un mot de passe fort à deux reprises, puis cliquez sur **suivant**. Ce mot de passe doit être utilisé pour démarrer AD DS en mode DSRM pour les tâches qui doivent être effectuées hors connexion.  
  
10. Dans la page **options DNS** , activez la case à cocher **mettre à jour la délégation DNS** si vous souhaitez mettre à jour la délégation DNS lors de l’installation du rôle, puis cliquez sur **suivant**.  
  
11. Dans la page **options supplémentaires** , tapez ou accédez à l’emplacement du volume et du dossier du fichier de base de données, des fichiers journaux du service d’annuaire et des fichiers du volume système (SYSVOL). Spécifiez les options de réplication requises, puis cliquez sur **suivant**.  
  
12. Dans la page **examiner les options** , passez en revue les options d’installation, puis cliquez sur **suivant**.  
  
13. Dans la page vérification de la **Configuration requise** , une fois les conditions préalables validées, cliquez sur **installer**.  
  
14. Attendez que l’Assistant ait terminé la configuration, puis cliquez sur **Fermer**.  
  
15. Redémarrez l’ordinateur s’il n’a pas été redémarré automatiquement.  
  
## <a name="23-configure-security-groups"></a><a name="BKMK_ConfigSG"></a>2,3. Configurer les groupes de sécurité  
Un déploiement multisite requiert un groupe de sécurité supplémentaire pour les ordinateurs clients Windows 7 pour chaque point d’entrée du déploiement qui autorise l’accès aux ordinateurs clients Windows 7. S’il existe plusieurs domaines contenant des ordinateurs clients Windows 7, il est recommandé de créer un groupe de sécurité dans chaque domaine pour le même point d’entrée. Vous pouvez également utiliser un groupe de sécurité universel contenant les ordinateurs clients des deux domaines. Par exemple, dans un environnement avec deux domaines, si vous souhaitez autoriser l’accès aux ordinateurs clients Windows 7 dans les points d’entrée 1 et 3, mais pas dans le point d’entrée 2, créez deux groupes de sécurité qui contiennent les ordinateurs clients Windows 7 pour chaque point d’entrée de chacun des domaines.  
  
### <a name="to-configure-additional-security-groups"></a>Pour configurer des groupes de sécurité supplémentaires  
  
1.  Sur le contrôleur de domaine principal, cliquez sur **Démarrer**, puis sur **Active Directory les utilisateurs et les ordinateurs**.  
  
2.  Dans l’arborescence de la console, cliquez avec le bouton droit sur le dossier dans lequel vous souhaitez ajouter un nouveau groupe, par exemple, corp.contoso.com/Users. Pointez sur **Nouveau**, puis cliquez sur **Groupe**.  
  
3.  Dans la boîte de dialogue **nouvel objet-groupe** , sous **nom du groupe**, tapez le nom du nouveau groupe, par exemple, Win7_Clients_Entrypoint1.  
  
4.  Sous **étendue du groupe**, cliquez sur **universel**, sous **type de groupe**, cliquez sur **sécurité**, puis sur **OK**.  
  
5.  Pour ajouter des ordinateurs au nouveau groupe de sécurité, double-cliquez sur le groupe de sécurité, puis dans la boîte de dialogue **< Group_Name propriétés du >** , cliquez sur l’onglet **membres** .  
  
6.  Sous l'onglet **Membres**, cliquez sur **Ajouter**.  
  
7.  Sélectionnez les ordinateurs Windows 7 à ajouter à ce groupe de sécurité, puis cliquez sur **OK**.  
  
8.  Répétez cette procédure pour créer un groupe de sécurité pour chaque point d’entrée en fonction des besoins.  
  
![les commandes Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/PowerShellLogoSmall.gif)***<em>équivalentes</em> Windows PowerShell***  
  
La ou les applets de commande Windows PowerShell suivantes ont la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles apparaissent ici sur plusieurs lignes en raison de contraintes de mise en forme.  
  
Pour installer la fonctionnalité Windows « Active Directory module pour Windows PowerShell » :  
  
```  
Install-WindowsFeature "Name RSAT-AD-PowerShell  
```  
  
ou ajoutez le « composant logiciel enfichable PowerShell Active Directory » via OptionalFeatures.  
  
Si vous exécutez les applets de commande suivantes sur Windows 7 ou Windows Server 2008 R2, le module PowerShell Active Directory doit être importé :  
  
```  
Import-Module ActiveDirectory  
```  
  
Pour configurer un groupe de sécurité nommé Win7_Clients_Entrypoint1 et ajouter un ordinateur client nommé CLIENT2 :  
  
```  
New-ADGroup -GroupScope universal -Name Win7_Clients_Entrypoint1  
Add-ADGroupMember -Identity Win7_Clients_Entrypoint1 -Members CLIENT2$  
```  
  
## <a name="24-configure-gpos"></a><a name="ConfigGPOs"></a>2,4. Configurer les objets de stratégie de groupe  
Un déploiement de l’accès à distance multisite requiert les objets stratégie de groupe suivants :  
  
-   Un objet de stratégie de groupe pour chaque point d’entrée pour le serveur d’accès à distance.  
  
-   Un objet de stratégie de groupe pour tous les ordinateurs clients Windows 8 pour chaque domaine.  
  
-   Un objet de stratégie de groupe dans chaque domaine contenant des ordinateurs clients Windows 7 pour chaque point d’entrée configuré pour prendre en charge les clients Windows 7.  
  
    > [!NOTE]  
    > Si vous n’avez pas d’ordinateurs clients Windows 7, vous n’avez pas besoin de créer des objets de stratégie de groupe pour les ordinateurs Windows 7.  
  
Quand vous configurez l’accès à distance, l’Assistant crée automatiquement les objets de stratégie de groupe requis s’ils n’existent pas déjà. Si vous ne disposez pas des autorisations nécessaires pour créer des objets stratégie de groupe, vous devez les créer avant de configurer l’accès à distance. L’administrateur DirectAccess doit disposer des autorisations complètes sur les objets de stratégie de groupe (modifier + modifier sécurité + supprimer).  
  
> [!IMPORTANT]  
> Après avoir créé manuellement les objets de stratégie de groupe pour l’accès à distance, vous devez accorder suffisamment de temps pour Active Directory et la réplication DFS au contrôleur de domaine dans le site Active Directory qui est associé au serveur d’accès à distance. Si l’accès à distance a créé automatiquement les objets stratégie de groupe, aucun délai d’attente n’est nécessaire.  
  
Pour créer des objets stratégie de groupe, consultez [créer et modifier un objet stratégie de groupe](https://technet.microsoft.com/library/cc754740.aspx).  
  
### <a name="domain-controller-maintenance-and-downtime"></a><a name="DCMaintandDowntime"></a>Maintenance et temps d’arrêt du contrôleur de domaine  
Lorsqu’un contrôleur de domaine qui exécute en tant qu’émulateur de contrôleur de domaine ou que les contrôleurs de domaine gèrent des objets de stratégie de groupe de serveur connaissent un temps d’arrêt, il n’est pas possible de charger ou modifier la configuration de l’accès Cela n’affecte pas la connectivité du client si d’autres contrôleurs de domaine sont disponibles.  
  
Pour charger ou modifier la configuration de l’accès à distance, vous pouvez transférer le rôle d’émulateur de contrôleur de domaine principal vers un autre contrôleur de domaine pour les objets de stratégie de groupe du client ou du serveur d’applications ; pour les objets de stratégie de groupe de serveur, modifiez les contrôleurs de domaine qui gèrent les objets de stratégie de groupe serveur.  
  
> [!IMPORTANT]  
> Cette opération ne peut être effectuée que par un administrateur de domaine. L’impact de la modification du contrôleur de domaine principal n’est pas limité à l’accès à distance ; par conséquent, soyez prudent lors du transfert du rôle d’émulateur de contrôleur de domaine principal.  
  
> [!NOTE]  
> Avant de modifier l’Association du contrôleur de domaine, assurez-vous que tous les objets de stratégie de groupe du déploiement de l’accès à distance ont été répliqués sur tous les contrôleurs de domaine du domaine. Si l’objet de stratégie de groupe n’est pas synchronisé, les modifications récentes de la configuration peuvent être perdues après la modification de l’Association du contrôleur de domaine, ce qui peut entraîner une configuration endommagée. Pour vérifier la synchronisation des objets de stratégie de groupe, consultez [vérifier stratégie de groupe État](https://technet.microsoft.com/library/jj134176.aspx)de l’infrastructure.  
  
#### <a name="to-transfer-the-pdc-emulator-role"></a><a name="TransferPDC"></a>Pour transférer le rôle d’émulateur de contrôleur de domaine principal  
  
1.  Dans l’écran d' **Accueil** , tapez**DSA. msc**, puis appuyez sur entrée.  
  
2.  Dans le volet gauche de la console Active Directory utilisateurs et ordinateurs, cliquez avec le bouton droit sur **Active Directory utilisateurs et ordinateurs**, puis cliquez sur **modifier le contrôleur de domaine**. Dans la boîte de dialogue Modifier le serveur d’annuaire, cliquez sur **ce contrôleur de domaine ou AD LDS instance**, dans la liste, cliquez sur le contrôleur de domaine qui sera le nouveau détenteur du rôle, puis cliquez sur **OK**.  
  
    > [!NOTE]  
    > Vous devez effectuer cette étape si vous n’êtes pas sur le contrôleur de domaine vers lequel vous souhaitez transférer le rôle. N’effectuez pas cette étape si vous êtes déjà connecté au contrôleur de domaine vers lequel vous souhaitez transférer le rôle.  
  
3.  Dans l’arborescence de la console, cliquez avec le bouton droit sur **Active Directory utilisateurs et ordinateurs**, pointez sur **toutes les tâches**, puis cliquez sur **maîtres d’opérations**.  
  
4.  Dans la boîte de dialogue maîtres d’opérations, cliquez sur l’onglet **PDC** , puis sur **modifier**.  
  
5.  Cliquez sur **Oui** pour confirmer que vous souhaitez transférer le rôle, puis cliquez sur **Fermer**.  
  
#### <a name="to-change-the-domain-controller-that-manages-server-gpos"></a><a name="ChangeDC"></a>Pour modifier le contrôleur de domaine qui gère les objets de stratégie de groupe de serveur  
  
-   Exécutez l’applet de commande Windows PowerShell [Set-DAEntryPointDC](https://docs.microsoft.com/powershell/module/remoteaccess/set-daentrypointdc) sur le serveur d’accès à distance et spécifiez le nom du contrôleur de domaine inaccessible pour le paramètre *ExistingDC* . Cette commande modifie l’Association du contrôleur de domaine pour les objets de stratégie de groupe de serveur des points d’entrée actuellement gérés par ce contrôleur de domaine.
  
    -   Pour remplacer le contrôleur de domaine inaccessible « dc1.corp.contoso.com » par le contrôleur de domaine « dc2.corp.contoso.com », procédez comme suit :  
  
        ```  
        Set-DAEntryPointDC "ExistingDC 'dc1.corp.contoso.com' "NewDC 'dc2.corp.contoso.com' "ErrorAction Inquire  
        ```  
  
    -   Pour remplacer le contrôleur de domaine inaccessible « dc1.corp.contoso.com » par un contrôleur de domaine dans le site de Active Directory le plus proche du serveur d’accès à distance « DA1.corp.contoso.com », procédez comme suit :  
  
        ```  
        Set-DAEntryPointDC "ExistingDC 'dc1.corp.contoso.com' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
        ```  
  
### <a name="change-two-or-more-domain-controllers-that-manage-server-gpos"></a><a name="ChangeTwoDCs"></a>Modifier deux ou plusieurs contrôleurs de domaine qui gèrent les objets de stratégie de groupe de serveur  
Dans un nombre minimal de cas, deux ou plusieurs contrôleurs de domaine qui gèrent les objets de stratégie de groupe de serveur ne sont pas disponibles. Si cela se produit, des étapes supplémentaires sont nécessaires pour modifier l’Association du contrôleur de domaine pour les objets de stratégie de groupe de serveur.  
  
Les informations d’association du contrôleur de domaine sont stockées dans le registre des serveurs d’accès à distance et dans tous les objets de stratégie de groupe de serveur. Dans l’exemple suivant, il existe deux points d’entrée avec deux serveurs d’accès à distance, « DA1 » dans « point d’entrée 1 » et « DA2 » dans « point d’entrée 2 ». L’objet de stratégie de groupe du serveur « point d’entrée 1 » est géré dans le contrôleur de domaine « DC1 », tandis que l’objet de stratégie de groupe du serveur « point d’entrée 2 » est géré dans le contrôleur de domaine « DC2 ». « DC1 » et « DC2 » ne sont pas disponibles. Un troisième contrôleur de domaine est toujours disponible dans le domaine, « DC3 », et les données de « DC1 » et « DC2 » ont déjà été répliquées sur « DC3 ».  
  
![Configurer l’infrastructure multisite](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc1.png)  
  
##### <a name="to-change-two-or-more-domain-controllers-that-manage-server-gpos"></a>Pour modifier deux ou plusieurs contrôleurs de domaine qui gèrent les objets de stratégie de groupe de serveur  
  
1.  Pour remplacer le contrôleur de domaine non disponible « DC2 » par le contrôleur de domaine « DC3 », exécutez la commande suivante :  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC2' "NewDC 'DC3' "ComputerName 'DA2' "ErrorAction Continue  
    ```  
  
    Cette commande met à jour l’Association du contrôleur de domaine pour l’objet de stratégie de groupe serveur « point d’entrée 2 » dans le registre de DA2 et dans l’objet de stratégie de groupe serveur « point d’entrée 2 ». Toutefois, il ne met pas à jour l’objet de stratégie de groupe du serveur « point d’entrée 1 », car le contrôleur de domaine qui le gère n’est pas disponible.  
  
    > [!TIP]  
    > Cette commande utilise la valeur continue pour le paramètre *ErrorAction* , qui met à jour l’objet de stratégie de groupe du serveur « point d’entrée 2 » Malgré l’échec de la mise à jour de l’objet de stratégie de groupe du serveur « point d’entrée 1 ».  
  
    La configuration obtenue est illustrée dans le diagramme suivant.  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc2.png)  
  
2.  Pour remplacer le contrôleur de domaine non disponible « DC1 » par le contrôleur de domaine « DC3 », exécutez la commande suivante :  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC1' "NewDC 'DC3' "ComputerName 'DA2' "ErrorAction Continue  
    ```  
  
    Cette commande met à jour l’Association du contrôleur de domaine pour l’objet de stratégie de groupe serveur « point d’entrée 1 » dans le registre de DA1 et dans les objets de stratégie de groupe de serveur « point d’entrée 1 » et « point d’entrée 2 ». La configuration obtenue est illustrée dans le diagramme suivant.  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc3.png)  
  
3.  Pour synchroniser l’Association de contrôleur de domaine pour l’objet de stratégie de groupe de serveur « point d’entrée 2 » dans l’objet de stratégie de groupe de serveur « point d’entrée 1 », exécutez la commande pour remplacer « DC2 » par « DC3 » et spécifiez le serveur d’accès à distance *dont l’objet* de stratégie de groupe de serveur n’est pas synchronisé  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC2' "NewDC 'DC3' "ComputerName 'DA1' "ErrorAction Continue  
    ```  
  
    La configuration finale est présentée dans le diagramme suivant.  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssocFinal.png)  
  
### <a name="optimization-of-configuration-distribution"></a><a name="ConfigDistOptimization"></a>Optimisation de la distribution de la configuration  
Lorsque vous apportez des modifications de configuration, les modifications sont appliquées uniquement une fois que les objets de stratégie de groupe du serveur se propagent aux serveurs d’accès à distance. Pour réduire le temps de distribution de la configuration, l’accès à distance sélectionne automatiquement un contrôleur de domaine accessible [en écriture le plus proche du serveur d’accès à distance lors de la création de](https://technet.microsoft.com/library/cc978016.aspx) son objet de stratégie de groupe de serveur.  
  
Dans certains scénarios, il peut être nécessaire de modifier manuellement le contrôleur de domaine qui gère un objet de stratégie de groupe de serveur afin d’optimiser la durée de distribution de la configuration :  
  
-   Il n’existait aucun contrôleur de domaine accessible en écriture dans le site Active Directory d’un serveur d’accès à distance au moment de l’ajout en tant que point d’entrée. Un contrôleur de domaine accessible en écriture est maintenant ajouté au site Active Directory du serveur d’accès à distance.  
  
-   Une modification d’adresse IP ou un Active Directory sites et sous-réseaux modifiés peut avoir déplacé le serveur d’accès à distance vers un autre site de Active Directory.  
  
-   L’Association du contrôleur de domaine pour un point d’entrée a été modifiée manuellement en raison d’un travail de maintenance sur un contrôleur de domaine et le contrôleur de domaine est à nouveau en ligne.  
  
Dans ces scénarios, exécutez l’applet de commande PowerShell `Set-DAEntryPointDC` sur le serveur d’accès à distance et spécifiez le nom du point d’entrée que vous souhaitez optimiser à l’aide du paramètre *EntryPointName*. Vous ne devez effectuer cette opération qu’une fois que les données de l’objet de stratégie de groupe du contrôleur de domaine qui stockent actuellement l’objet de stratégie de groupe du serveur ont déjà été entièrement répliquées sur le nouveau contrôleur de domaine souhaité.  
  
> [!NOTE]  
> Avant de modifier l’Association du contrôleur de domaine, assurez-vous que tous les objets de stratégie de groupe du déploiement de l’accès à distance ont été répliqués sur tous les contrôleurs de domaine du domaine. Si l’objet de stratégie de groupe n’est pas synchronisé, les modifications récentes de la configuration peuvent être perdues après la modification de l’Association du contrôleur de domaine, ce qui peut entraîner une configuration endommagée. Pour vérifier la synchronisation des objets de stratégie de groupe, consultez [vérifier stratégie de groupe État](https://technet.microsoft.com/library/jj134176.aspx)de l’infrastructure.  
  
Pour optimiser l’heure de la distribution de la configuration, effectuez l’une des opérations suivantes :  
  
-   Pour gérer l’objet de stratégie de groupe de serveur du point d’entrée « point d’entrée 1 » sur un contrôleur de domaine dans le site de Active Directory le plus proche du serveur d’accès à distance « DA1.corp.contoso.com », exécutez la commande suivante :  
  
    ```  
    Set-DAEntryPointDC "EntryPointName 'Entry point 1' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
    ```  
  
-   Pour gérer l’objet de stratégie de groupe de serveur du point d’entrée « point d’entrée 1 » sur le contrôleur de domaine « dc2.corp.contoso.com », exécutez la commande suivante :  
  
    ```  
    Set-DAEntryPointDC "EntryPointName 'Entry point 1' "NewDC 'dc2.corp.contoso.com' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
    ```  
  
    > [!NOTE]  
    > Lorsque vous modifiez le contrôleur de domaine associé à un point d’entrée spécifique, vous devez spécifier un serveur d’accès à distance qui est membre de ce point d’entrée pour le paramètre *ComputerName* .  
  
## <a name="see-also"></a><a name="BKMK_Links"></a>Voir aussi  
  
-   [Étape 3 : configurer le déploiement multisite](Step-3-Configure-the-Multisite-Deployment.md)  
-   [Étape 1 : implémenter un déploiement de l’accès à distance à un serveur unique](Step-1-Implement-a-Single-Server-Remote-Access-Deployment.md)  
