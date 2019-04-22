---
title: Étape 3 configurer le déploiement Multisite
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
ms.assetid: ea7ecd52-4c12-4a49-92fd-b8c08cec42a9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5c74a9277af3853d709a8ecd58c1e53e1bccc719
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812510"
---
# <a name="step-3-configure-the-multisite-deployment"></a>Étape 3 configurer le déploiement Multisite

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Après avoir configuré l’infrastructure multisite, procédez comme suit pour configurer le déploiement multisite de l’accès à distance.  
  
|Tâche|Description|  
|----|--------|  
|3.1. Configurer les serveurs d’accès à distance|Configurer des serveurs d’accès à distance supplémentaires en définissant des adresses IP, les joindre au domaine et l’installation du rôle de l’accès à distance.|  
|3.2. Accorder l’accès administrateur|Accorder des privilèges sur les serveurs d’accès à distance supplémentaires à l’administrateur DirectAccess.|  
|3.3. Configurer IP-HTTPS pour un déploiement multisite|Configurez le certificat IP-HTTPS utilisé dans un déploiement multisite.|  
|3.4. Configurer le serveur d’emplacement réseau pour un déploiement multisite|Configurer le certificat de serveur d’emplacement réseau utilisé dans un déploiement multisite.|  
|3.5. Configurer des clients DirectAccess pour un déploiement multisite|Supprimer les ordinateurs clients Windows 7 à partir de groupes de sécurité de Windows 8.|  
|3.6. Activer le déploiement multisite|Activer le déploiement multisite sur le premier serveur d’accès à distance.|  
|3.7. Ajouter des points d’entrée au déploiement multisite|Ajouter des points d’entrée supplémentaires au déploiement multisite.|  
  
> [!NOTE]  
> Cette rubrique inclut des exemples d'applets de commande Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, consultez [Utilisation des applets de commande](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_ConfigServer"></a>3.1. Configurer les serveurs d’accès à distance  

  
### <a name="to-install-the-remote-access-role"></a>Pour installer le rôle Accès à distance  
  
1.  Assurez-vous que chaque serveur d’accès à distance est configuré avec la topologie de déploiement appropriée (Edge, derrière un NAT, une seule interface réseau) et les itinéraires correspondants.  
  
2.  Configurer les adresses IP sur chaque serveur d’accès à distance en fonction de la topologie de site et le schéma d’adressage IP de votre organisation.  
  
3.  Joindre chaque serveur d’accès à distance à un domaine Active Directory.  
  
4.  Dans la console Gestionnaire de serveur, dans le **tableau de bord**, cliquez sur **ajouter des rôles et fonctionnalités**.  
  
5.   Cliquez sur **Suivant** trois fois pour accéder à l’écran de sélection du rôle de serveur.  
  
6.  Sur le **sélectionner des rôles de serveur** boîte de dialogue, sélectionnez **accès à distance**, puis cliquez sur **suivant**.  
  
7.  Cliquez sur **suivant** trois fois.  
  
8.  Sur le **sélectionnez services de rôle** boîte de dialogue, sélectionnez **DirectAccess et VPN (RAS)** puis cliquez sur **ajouter des fonctionnalités**.  
  
9.  Sélectionnez **routage**, sélectionnez **Proxy d’Application Web**, cliquez sur **ajouter des fonctionnalités**, puis cliquez sur **suivant**.  
  
10. Cliquez sur **Suivant**, puis cliquez sur **Installer**.  
  
11.  Dans la boîte de dialogue **Progression de l’installation** , vérifiez que l’installation s’est correctement déroulée et cliquez sur **Fermer**.  
  
  
![Windows PowerShell](../../../../media/Step-3-Configure-the-Multisite-Deployment/PowerShellLogoSmall.gif)Windows PowerShell équivalente commandes ***  

  
Étapes 1 à 3 doivent être effectuées manuellement et ne sont pas réalisées à l’aide de cette applet de commande Windows PowerShell.  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
Install-WindowsFeature RemoteAccess -IncludeManagementTools  
```  
  
## <a name="BKMK_Admin"></a>3.2. Accorder l’accès administrateur  
  
#### <a name="to-grant-administrator-permissions"></a>Pour accorder des autorisations d’administrateur  
  
1.  Sur le serveur d’accès à distance dans le point d’entrée supplémentaires : Sur le **Démarrer** , tapez **gestion de l’ordinateur**, puis appuyez sur ENTRÉE.  
  
2.  Dans le volet gauche, cliquez sur **utilisateurs et groupes locaux**.  
  
3.  Double-cliquez sur **groupes**, puis double-cliquez sur **administrateurs**.  
  
4.  Sur le **propriétés du groupe Administrateurs** boîte de dialogue, cliquez sur **ajouter**, puis, dans le **sélectionner des utilisateurs, les ordinateurs, les comptes de Service ou les groupes** boîte de dialogue, cliquez sur  **Emplacements**.  
  
5.  Sur le **emplacements** boîte de dialogue le **emplacement** arborescence et cliquez sur l’emplacement qui contient le compte d’utilisateur de l’administrateur DirectAccess, puis cliquez sur **OK**.  
  
6.  Dans le **Entrez les noms des objets à sélectionner**, entrez le nom d’utilisateur de l’administrateur DirectAccess, puis cliquez sur **OK** à deux reprises.  
  
7.  Sur le **propriétés du groupe Administrateurs** boîte de dialogue, cliquez sur **OK**.  
  
8.  Fermez la fenêtre de gestion de l’ordinateur.  
  
9. Répétez cette procédure sur tous les serveurs d’accès à distance qui fera partie du déploiement multisite.  
  
## <a name="BKMK_IPHTTPS"></a>3.3. Configurer IP-HTTPS pour un déploiement multisite  
Sur chaque serveur d’accès à distance qui sera ajouté au déploiement multisite, un certificat SSL est nécessaire pour vérifier la connexion HTTPS au serveur web IP-HTTPS. L'appartenance au groupe local **Administrateurs**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.  
  
#### <a name="to-obtain-an-ip-https-certificate"></a>Pour obtenir un certificat IP-HTTPS  
  
1.  Sur chaque serveur d’accès à distance : Sur le **Démarrer** , tapez **mmc**, puis appuyez sur ENTRÉE. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
2.  Cliquez sur **Fichier**, puis sur **Ajouter ou supprimer des composants logiciels enfichables**.  
  
3.  Cliquez sur **Certificats**, sur **Ajouter**, sur **Un compte d'ordinateur**, sur **Suivant**, sélectionnez **L'ordinateur local**, cliquez sur **Terminer**, puis sur **OK**.  
  
4.  Dans l'arborescence de la console du composant logiciel enfichable Certificats, ouvrez **Certificats (ordinateur local)\Personnel\Certificats**.  
  
5.  Cliquez avec le bouton droit sur **Certificats**, pointez sur **Toutes les Tâches**, puis cliquez sur **Demander un nouveau certificat**.  
  
6.  Cliquez sur **Suivant** deux fois.  
  
7.  Sur le **demander des certificats** page et cliquez sur le modèle de certificat de serveur Web, puis cliquez sur **inscription pour obtenir ce certificat nécessite des informations plus**.  
  
    Si le modèle de certificat de serveur Web n’apparaît pas, vérifiez que le compte d’ordinateur de serveur accès à distance a droits d’inscription pour le modèle de certificat de serveur Web. Pour plus d’informations, consultez [configurer les autorisations sur le modèle de certificat de serveur Web](https://technet.microsoft.com/library/ee649249(v=ws.10).aspx).  
  
8.  Sur le **sujet** onglet de la **propriétés du certificat** boîte de dialogue **nom de l’objet**, pour **Type**, sélectionnez **courantes nom**.  
  
9. Dans **valeur**, tapez le nom de domaine complet (FQDN) du nom Internet du serveur d’accès à distance (par exemple, Europe.contoso.com), puis cliquez sur **ajouter**.  
  
10. Cliquez sur **OK**, sur **Inscrire**, puis sur **Terminer**.  
  
11. Dans le volet d'informations du composant logiciel enfichable Certificats, vérifiez qu'un nouveau certificat avec le nom de domaine complet a été inscrit avec **Rôles prévus** égal à **Authentification du serveur**.  
  
12. Cliquez avec le bouton droit sur le certificat, puis cliquez sur **Propriétés**.  
  
13. Dans **Nom convivial**, tapez **Certificat IP-HTTPS**, puis cliquez sur **OK**.  
  
    > [!TIP]  
    > Les étapes 12 et 13 sont facultatifs, mais est plus facile pour vous permet de sélectionner le certificat pour IP-HTTPS lors de la configuration d’accès à distance.  
  
14. Répétez cette procédure sur tous les serveurs d’accès à distance dans votre déploiement.  
  
## <a name="BKMK_NLS"></a>3.4. Configurer le serveur d’emplacement réseau pour un déploiement multisite  
Si vous avez sélectionné pour configurer le site de serveur d’emplacement réseau sur le serveur d’accès à distance lorsque vous configurez votre premier serveur, chaque nouveau serveur d’accès à distance que vous ajoutez doit être configuré avec un certificat de serveur Web qui porte le même nom de sujet qui a été sélectionné pour t serveur d’emplacement réseau he pour le premier serveur. Chaque serveur nécessite un certificat pour authentifier la connexion au serveur d’emplacement réseau, et les ordinateurs clients situés dans le réseau interne doivent être en mesure de résoudre le nom du site Web dans le système DNS.  
  
#### <a name="to-install-a-certificate-for-network-location"></a>Pour installer un certificat pour l’emplacement réseau  
  
1.  Sur le serveur d'accès à distance : Sur le **Démarrer** , tapez **mmc**, puis appuyez sur ENTRÉE. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
2.  Cliquez sur **Fichier**, puis sur **Ajouter ou supprimer des composants logiciels enfichables**.  
  
3.  Cliquez sur **Certificats**, sur **Ajouter**, sur **Un compte d'ordinateur**, sur **Suivant**, sélectionnez **L'ordinateur local**, cliquez sur **Terminer**, puis sur **OK**.  
  
4.  Dans l'arborescence de la console du composant logiciel enfichable Certificats, ouvrez **Certificats (ordinateur local)\Personnel\Certificats**.  
  
5.  Cliquez avec le bouton droit sur **Certificats**, pointez sur **Toutes les Tâches**, puis cliquez sur **Demander un nouveau certificat**.  
  
    > [!NOTE]  
    > Vous pouvez également importer le même certificat qui a été utilisé pour le serveur d’emplacement réseau pour le premier serveur d’accès à distance.  
  
6.  Cliquez sur **Suivant** deux fois.  
  
7.  Sur le **demander des certificats** page et cliquez sur le modèle de certificat de serveur Web, puis cliquez sur **inscription pour obtenir ce certificat nécessite des informations plus**.  
  
    Si le modèle de certificat de serveur Web n’apparaît pas, vérifiez que le compte d’ordinateur de serveur accès à distance a droits d’inscription pour le modèle de certificat de serveur Web. Pour plus d’informations, consultez [configurer les autorisations sur le modèle de certificat de serveur Web](https://technet.microsoft.com/library/ee649249(v=ws.10).aspx).  
  
8.  Sur le **sujet** onglet de la **propriétés du certificat** boîte de dialogue **nom de l’objet**, pour **Type**, sélectionnez **courantes nom**.  
  
9. Dans **valeur**, tapez le nom de domaine complet (FQDN) qui a été configuré pour le certificat de serveur d’emplacement réseau du premier serveur d’accès à distance (par exemple, nls.corp.contoso.com), puis cliquez sur **ajouter**.  
  
10. Cliquez sur **OK**, sur **Inscrire**, puis sur **Terminer**.  
  
11. Dans le volet d'informations du composant logiciel enfichable Certificats, vérifiez qu'un nouveau certificat avec le nom de domaine complet a été inscrit avec **Rôles prévus** égal à **Authentification du serveur**.  
  
12. Cliquez avec le bouton droit sur le certificat, puis cliquez sur **Propriétés**.  
  
13. Dans **Nom convivial**, tapez **Certificat Emplacement réseau**, puis cliquez sur **OK**.  
  
    > [!TIP]  
    > Les étapes 12 et 13 sont facultatifs, mais est plus facile pour vous permet de sélectionner le certificat pour l’emplacement réseau lors de la configuration d’accès à distance.  
  
14. Répétez cette procédure sur tous les serveurs d’accès à distance dans votre déploiement.  
  
### <a name="NLS"></a>Pour créer des enregistrements DNS de serveur d’emplacement réseau  
  
1.  Sur le serveur DNS : Sur le **Démarrer** , tapez **dnsmgmt.msc**, puis appuyez sur ENTRÉE.  
  
2.  Dans le volet gauche de la **Gestionnaire DNS** de la console, ouvrez la zone de recherche directe pour le réseau interne. Cliquez avec le bouton droit sur la zone concernée et cliquez sur **nouvel hôte (A ou AAAA)**.  
  
3.  Sur le **nouvel hôte** boîte de dialogue le **nom (utilise nom du domaine parent si vide)** , entrez le nom qui a été utilisé pour le serveur d’emplacement réseau pour le premier serveur d’accès à distance. Dans le **adresse IP** zone, entrez l’adresse IPv4 intranet du serveur d’accès à distance, puis cliquez sur **ajouter un hôte**. Dans la boîte de dialogue **DNS**, cliquez sur **OK**.  
  
4.  Sur le **nouvel hôte** boîte de dialogue le **nom (utilise nom du domaine parent si vide)** , entrez le nom qui a été utilisé pour le serveur d’emplacement réseau pour le premier serveur d’accès à distance. Dans le **adresse IP** zone, entrez l’adresse IPv6 intranet du serveur d’accès à distance, puis cliquez sur **ajouter un hôte**. Dans la boîte de dialogue **DNS**, cliquez sur **OK**.  
  
5.  Répétez les étapes 3 et 4 pour chaque serveur d’accès à distance dans votre déploiement.  
  
6.  Cliquez sur **Terminé**.  
  
7.  Répétez cette procédure avant d’ajouter des serveurs en tant que points d’entrée supplémentaires dans le déploiement.  
  
## <a name="BKMK_Client"></a>3.5. Configurer des clients DirectAccess pour un déploiement multisite  
Les ordinateurs clients DirectAccess Windows doivent être membres du ou des groupes de sécurité qui définissent leur association de DirectAccess. Avant le déploiement multisite est activé, ces groupes de sécurité peuvent contenir les clients de Windows 8 et Windows 7 (si le mode approprié « niveau inférieur » a été sélectionné). Une fois le déploiement multisite est activé, les groupes de sécurité client existants, en mode de serveur unique, sont convertis en groupes de sécurité pour Windows 8 uniquement. Une fois le déploiement multisite est activé, les ordinateurs clients DirectAccess Windows 7 doivent être déplacés vers dédié Windows 7 client groupes de sécurité correspondants (qui sont associés à des points d’entrée spécifiques), ou qu’ils ne seront pas en mesure de se connecter via DirectAccess. Les clients Windows 7 doivent d’abord être supprimés des groupes de sécurité existants qui sont maintenant des groupes de sécurité de Windows 8. Avertissement :  Ordinateurs clients Windows 7 qui sont membres de groupes de sécurité des clients Windows 7 et Windows 8 perd la connectivité à distance, et les clients Windows 7 sans SP1 installé n’auront plus également une connectivité d’entreprise. Par conséquent, tous les ordinateurs clients de Windows 7 doivent être supprimés à partir de groupes de sécurité de Windows 8.  
  
#### <a name="remove--windows-7--clients-from-windows-8-security-groups"></a>Supprimer les clients Windows 7 à partir de groupes de sécurité de Windows 8  
  
1.  Sur le contrôleur de domaine principal, cliquez sur **Démarrer**, puis cliquez sur **Active Directory Users and Computers**.  
  
2.  Pour supprimer les ordinateurs du groupe de sécurité, double-cliquez sur le groupe de sécurité, puis, dans le **< nom_groupe > Propriétés** boîte de dialogue, cliquez sur le **membres** onglet.  
  
3.  Sélectionnez l’ordinateur client Windows 7, puis cliquez sur **supprimer**.  
  
4.  Répétez cette procédure pour supprimer les ordinateurs clients de Windows 7 à partir de groupes de sécurité de Windows 8.  
  
> [!IMPORTANT]  
> Lorsque vous activez une configuration multisite de l’accès à distance tous les ordinateurs clients (Windows 7 et Windows 8) seront perdre la connectivité à distance jusqu'à ce qu’ils soient en mesure de se connecter directement au réseau d’entreprise ou par VPN pour mettre à jour leurs stratégies de groupe. Cela est vrai lors de l’activation de la fonctionnalité multisite pour la première fois et également lors de la désactivation multisite.  
  
## <a name="BKMK_Enable"></a>3.6. Activer le déploiement multisite  
Pour configurer un déploiement multisite, activer la fonctionnalité multisite sur votre serveur d’accès à distance existant. Avant d’activer le multisite dans votre déploiement, vérifiez que vous disposez des informations suivantes :  
  
1.  Paramètres de l’équilibreur de charge global et les adresses IP si vous souhaitez charger équilibrent les connexions de client DirectAccess sur tous les points de saisie dans votre déploiement.  
  
2.  La sécurité de groupe (s) contenant les ordinateurs clients Windows 7 pour le premier point d’entrée dans votre déploiement, si vous souhaitez permettre aux ordinateurs de clients d’accès distant pour Windows 7.  
  
3.  Noms d’objets de stratégie de groupe si vous devez utiliser des objets de stratégie de groupe par défaut qui sont appliqués sur les ordinateurs clients Windows 7 pour le premier point d’entrée dans votre déploiement, si vous avez besoin de prise en charge pour les ordinateurs clients Windows 7.  
  
### <a name="EnabledMultisite"></a>Pour activer une configuration multisite  
  
1.  Sur votre serveur d’accès à distance existant : Sur le **Démarrer** , tapez **RAMgmtUI.exe**, puis appuyez sur ENTRÉE. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
2.  Dans la Console de gestion de l’accès à distance, cliquez sur **Configuration**, puis, dans le **tâches** volet, cliquez sur **Multisite activer**.  
  
3.  Dans le **activer le déploiement Multisite** Assistant, sur le **avant de commencer** , cliquez sur **suivant**.  
  
4.  Sur le **nom du déploiement** page **nom du déploiement Multisite**, entrez un nom pour votre déploiement. Dans **premier point d’entrée nom**, entrez un nom pour identifier le premier point d’entrée qui est le serveur d’accès à distance en cours, puis cliquez sur **suivant**.  
  
5.  Sur le **sélection de Point d’entrée** page, effectuez l’une des opérations suivantes :  
  
    -   Cliquez sur **affecter automatiquement des points d’entrée, et permettent aux clients de sélectionner manuellement** achemine automatiquement les ordinateurs clients vers le point d’entrée plus approprié, tout en permettant aux clients ordinateurs Sélectionner un point d’entrée manuellement. Sélection de point d’entrée manuelle est disponible uniquement pour les ordinateurs Windows 8. Cliquez sur **Suivant**.  
  
    -   Cliquez sur **affecter automatiquement des points d’entrée** à automatiquement acheminer des ordinateurs clients au point d’entrée plus approprié, puis cliquez sur **suivant**.  
  
6.  Sur le **l’équilibrage de charge Global** page, effectuez l’une des opérations suivantes :  
  
    -   Cliquez sur **non, n’utilisez pas l’équilibrage de charge global** si vous procédez au paramétrage pas à utiliser un équilibrage de charge global, puis cliquez sur **suivant**.  
  
        > [!NOTE]  
        > Lorsque vous sélectionnez ce client option ordinateurs se connectent à leur point d’entrée le plus proche.  
  
    -   Cliquez sur **Oui, utilisez l’équilibrage de charge global** si vous souhaitez charger équilibrer le trafic dans le monde entier entre tous les points d’entrée. Dans **tapez la nom de domaine complet à utiliser par tous les points d’entrée d’équilibrage de charge globale**, entrez la nom de domaine complet, d’équilibrage de charge globale et dans **tapez l’adresse IP pour ce point d’entrée d’équilibrage de charge globale** qui contient le premier Serveur d’accès à distance, entrez l’adresse IP pour ce point d’entrée d’équilibrage de charge globale, puis cliquez sur **suivant**.  
  
7.  Sur le **prise en charge Client** page, effectuez l’une des opérations suivantes :  
  
    -   Pour limiter l’accès aux ordinateurs clients exécutant Windows 8 ou des systèmes d’exploitation ultérieurs, cliquez sur **limiter l’accès aux ordinateurs clients exécutant Windows 8 ou un système d’exploitation ultérieur**, puis cliquez sur **suivant**.  
  
    -   Pour permettre aux clients ordinateurs 7 de Windows pour accéder à ce point d’entrée en cours d’exécution, cliquez sur **les ordinateurs qui exécutent 7 Windows pour accéder à ce point d’entrée clients**, puis cliquez sur **ajouter**. Sur le **sélectionner des groupes** boîte de dialogue, sélectionnez les ou les groupes de sécurité qui contient les ordinateurs clients Windows 7, cliquez sur **OK**, puis cliquez sur **suivant**.  
  
8.  Sur le **paramètres de stratégie de groupe Client** page, acceptez les ordinateurs du client de stratégie de groupe pour Windows 7 par défaut pour ce point d’entrée, tapez le nom de l’objet de stratégie de groupe désireuses d’accès à distance pour créer automatiquement, ou cliquez sur **Parcourir** à localiser les ordinateurs client de stratégie de groupe pour Windows 7, puis cliquez sur **suivant**.  
  
    > [!NOTE]  
    > -   Le **paramètres de stratégie de groupe Client** page apparaît uniquement lorsque vous configurez le point d’entrée pour les ordinateurs clients de Windows 7 pourront accéder au point d’entrée.  
    > -   Vous pouvez éventuellement cliquer sur **valider la stratégie de groupe** pour vous assurer que vous disposez des autorisations appropriées pour le GPO sélectionné ou de la stratégie de groupe pour ce point d’entrée. Si l’objet de stratégie de groupe n’existe pas et sera automatiquement créé, puis crée et lie les autorisations sont requises. Dans le cas où les objets de stratégie de groupe créés manuellement, puis le modifier, modifier la sécurité et les autorisations delete sont requises.  
  
9. Sur le **Résumé** , cliquez sur **valider**.  
  
10. Sur le **activer le déploiement Multisite** boîte de dialogue, cliquez sur **fermer** puis cliquez sur l’Assistant Activer le déploiement Multisite, **fermer**.  
  
![Windows PowerShell](../../../../media/Step-3-Configure-the-Multisite-Deployment/PowerShellLogoSmall.gif)Windows PowerShell équivalente commandes ***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
Pour activer un déploiement multisite nommé « Contoso » sur le premier point d’entrée nommés « Edge1-US ». Le déploiement permet aux clients de sélectionner manuellement le point d’entrée et n’utilise pas un équilibreur de charge globale.  
  
```  
Enable-DAMultiSite -Name 'Contoso' -EntryPointName 'Edge1-US' -ManualEntryPointSelectionAllowed 'Enabled'  
```  
  
Pour autoriser l’accès des ordinateurs clients Windows 7 via le premier point d’entrée via le groupe de sécurité DA_Clients_US et à l’aide de l’objet stratégie de groupe DA_W7_Clients_GPO_US.  
  
```  
Add-DAClient -EntrypointName 'Edge1-US' -DownlevelSecurityGroupNameList @('corp.contoso.com\DA_Clients_US') -DownlevelGpoName @('corp.contoso.com\DA_W7_Clients_GPO_US)  
```  
  
## <a name="BKMK_EntryPoint"></a>3.7. Ajouter des points d’entrée au déploiement multisite  
Après l’activation multisite dans votre déploiement, vous pouvez ajouter des points d’entrée supplémentaires à l’aide de l’Assistant Ajouter une entrée Point. Avant d’ajouter des points d’entrée, vérifiez que vous disposez des informations suivantes :  
  
-   Adresses d’IP équilibreur de charge global pour chaque nouvelle entrée point si vous utilisez l’équilibrage de charge global.  
  
-   Les groupes de sécurité contenant des ordinateurs clients de Windows 7 pour chaque point d’entrée qui seront ajoutés si vous souhaitez permettre aux ordinateurs de clients d’accès distant pour Windows 7.  
  
-   Noms d’objets de stratégie de groupe si vous êtes amené à utiliser des objets de stratégie de groupe par défaut qui sont appliqués sur les ordinateurs clients Windows 7 pour chaque point d’entrée qui doivent être ajoutées, si vous avez besoin de prise en charge pour les ordinateurs clients Windows 7.  
  
-   Dans le cas où le protocole IPv6 est déployé dans le réseau de l’organisation, vous devez préparer le préfixe IP-HTTPS pour le nouveau point d’entrée.  
  
### <a name="AddEP"></a>Pour ajouter des points d’entrée à votre déploiement multisite  
  
1.  Sur votre serveur d’accès à distance existant : Sur le **Démarrer** , tapez **RAMgmtUI.exe**, puis appuyez sur ENTRÉE. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
2.  Dans la Console de gestion de l’accès à distance, cliquez sur **Configuration**, puis, dans le **tâches** volet, cliquez sur **ajouter un Point d’entrée**.  
  
3.  Dans la zone Ajouter un Point d’entrée Assistant, sur le **détails de Point d’entrée** page **serveur d’accès à distance**, entrez le nom de domaine complet (FQDN) du serveur à ajouter. Dans **nom de point d’entrée**, entrez le nom du point d’entrée, puis cliquez sur **suivant**.  
  
4.  Sur le **paramètres d’équilibrage de charge Global** page, entrez l’adresse IP de ce point d’entrée d’équilibrage de charge globale, puis cliquez sur **suivant**.  
  
    > [!NOTE]  
    > Le **paramètres d’équilibrage de charge Global** page apparaît uniquement lorsque la configuration multisite utilise un équilibreur de charge globale.  
  
5.  Sur le **topologie de réseau** , cliquez sur la topologie qui correspond à la topologie de réseau du serveur d’accès à distance que vous ajoutez, puis cliquez sur **suivant**.  
  
6.  Sur le **nom réseau ou l’adresse IP** page **Type dans le nom public ou une adresse IP utilisée par les clients pour se connecter au serveur d’accès à distance**, entrez le nom public ou l’adresse IP utilisée par les clients pour se connecter à la Serveur d’accès à distance. Le nom public correspond avec le nom du sujet du certificat IP-HTTPS. Dans le cas où la topologie de réseau de périmètre a été implémentée, l’adresse IP est celle de la carte externe du serveur d’accès à distance. Cliquez sur **Suivant**.  
  
7.  Sur le **cartes réseau** page, effectuez l’une les éléments suivants :  
  
    -   Si vous déployez une topologie avec deux cartes réseau, dans **adaptateur externe**, sélectionnez la carte est connectée au réseau externe. Dans **carte réseau interne**, sélectionnez la carte est connectée au réseau interne.  
  
    -   Si vous déployez une topologie avec une carte réseau, dans **carte réseau**, sélectionnez la carte est connectée au réseau interne.  
  
8.  Sur le **cartes réseau** page **sélectionner le certificat utilisé pour authentifier les connexions IP-HTTPS**, cliquez sur **Parcourir** pour rechercher et sélectionner le certificat IP-HTTPS. Cliquez sur **Suivant**.  
  
9. Si IPv6 est configuré sur le réseau d’entreprise, sur le **Configuration du préfixe** page **préfixe IPv6 attribué aux ordinateurs clients**, entrez un préfixe IP-HTTPS pour affecter des adresses IPv6 pour le client DirectAccess ordinateurs, puis cliquez sur **suivant**.  
  
10. Sur le **prise en charge Client** page, effectuez l’une des opérations suivantes :  
  
    -   Pour limiter l’accès aux ordinateurs clients exécutant Windows 8 ou des systèmes d’exploitation ultérieurs, cliquez sur **limiter l’accès aux ordinateurs clients exécutant Windows 8 ou un système d’exploitation ultérieur**, puis cliquez sur **suivant**.  
  
    -   Pour permettre aux clients ordinateurs 7 de Windows pour accéder à ce point d’entrée en cours d’exécution, cliquez sur **les ordinateurs qui exécutent 7 Windows pour accéder à ce point d’entrée clients**, puis cliquez sur **ajouter**. Sur le **sélectionner des groupes** boîte de dialogue, sélectionnez les ou les groupes de sécurité qui contiennent les ordinateurs clients de Windows 7 qui permettront de vous connecter à ce point d’entrée, cliquez sur **OK**, puis cliquez sur **suivant**.  
  
11. Sur le **paramètres de stratégie de groupe Client** page, acceptez les ordinateurs du client de stratégie de groupe pour Windows 7 par défaut pour ce point d’entrée, tapez le nom de l’objet stratégie de groupe l’accès à distance pour créer automatiquement, ou cliquez sur **Parcourir** à localiser les ordinateurs client de stratégie de groupe pour Windows 7, puis cliquez sur **suivant**.  
  
    > [!NOTE]  
    > -   Le **paramètres de stratégie de groupe Client** page apparaît uniquement lorsque vous configurez le point d’entrée pour les ordinateurs clients de Windows 7 pourront accéder au point d’entrée.  
    > -   Vous pouvez éventuellement cliquer sur **valider la stratégie de groupe** pour vous assurer que vous disposez des autorisations appropriées pour le GPO sélectionné ou de la stratégie de groupe pour ce point d’entrée. Si l’objet de stratégie de groupe n’existe pas et sera automatiquement créé, puis crée et lie les autorisations sont requises. Dans le cas où les objets de stratégie de groupe créés manuellement, puis le modifier, modifier la sécurité et les autorisations delete sont requises.  
  
12. Sur le **paramètres de stratégie de groupe serveur** page, acceptez l’objet de stratégie de groupe par défaut pour ce serveur d’accès à distance, tapez le nom de l’objet stratégie de groupe l’accès à distance pour créer automatiquement, ou cliquez sur **Parcourir** pour localiser l’objet de stratégie de groupe pour ce serveur, puis cliquez sur **suivant**.  
  
13. Sur le **serveur emplacement réseau** , cliquez sur **Parcourir** à sélectionner le certificat pour le site de serveur d’emplacement réseau en cours d’exécution sur le serveur d’accès à distance, puis cliquez sur **suivant**.  
  
    > [!NOTE]  
    > Le **serveur emplacement réseau** page apparaît uniquement lorsque le site de serveur d’emplacement réseau est en cours d’exécution sur le serveur d’accès à distance.  
  
14. Sur le **Résumé** page, passez en revue les paramètres de point d’entrée, puis cliquez sur **valider**.  
  
15. Sur le **Point d’entrée Ajout** boîte de dialogue, cliquez sur **fermer** puis cliquez sur l’Assistant Ajouter une entrée Point, **fermer**.  
  
    > [!NOTE]  
    > Si le point d’entrée qui a été ajouté est dans une forêt différente de celle des points d’entrée existants ou des ordinateurs clients, il est nécessaire de cliquer sur **actualiser les serveurs d’administration** dans le **tâches** volet pour connaître le contrôleurs de domaine et System Center Configuration Manager dans la nouvelle forêt.  
  
16. Répétez cette procédure à l’étape 2 pour chaque point d’entrée que vous souhaitez ajouter à votre déploiement multisite.  
  
![Windows PowerShell](../../../../media/Step-3-Configure-the-Multisite-Deployment/PowerShellLogoSmall.gif)Windows PowerShell équivalente commandes ***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
Pour ajouter l’ordinateur edge2 à partir du domaine corp2 qu’un deuxième point d’entrée nommé Edge2 en Europe. La configuration de point d’entrée est : un préfixe IPv6 du client ' 2001:db8:2:2000 :: / 64 », une connexion à l’adresse (le certificat IP-HTTPS sur l’ordinateur edge2) 'edge2.contoso.com', un objet stratégie de groupe serveur nommé « DirectAccess Server paramètres - Edge2-Europe » et le texte interne et interfaces externes appelées respectivement Internet et Corpnet2 :  
  
```  
Add-DAEntryPoint -RemoteAccessServer 'edge2.corp2.corp.contoso.com' -Name 'Edge2-Europe' -ClientIPv6Prefix '2001:db8:2:2000::/64' -ConnectToAddress 'Europe.contoso.com' -ServerGpoName 'corp2.corp.contoso.com\DirectAccess Server Settings - Edge2-Europe' -InternetInterface 'Internet' -InternalInterface 'Corpnet2'  
```  
  
Pour autoriser l’accès des ordinateurs clients Windows 7 via le deuxième point d’entrée via le groupe de sécurité DA_Clients_Europe et à l’aide de l’objet stratégie de groupe DA_W7_Clients_GPO_Europe.  
  
```  
Add-DAClient -EntrypointName 'Edge2-Europe' -DownlevelGpoName @('corp.contoso.com\ DA_W7_Clients_GPO_Europe') -DownlevelSecurityGroupNameList @('corp.contoso.com\DA_Clients_Europe')  
```  
  
## <a name="BKMK_Links"></a>Voir aussi  
  
-   [Étape 2 : Configurer l’infrastructure multisite](Step-2-Configure-the-Multisite-Infrastructure.md)
