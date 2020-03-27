---
title: Étape 3 configurer le déploiement multisite
description: Cette rubrique fait partie du guide déployer plusieurs serveurs d’accès à distance dans un déploiement multisite dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ea7ecd52-4c12-4a49-92fd-b8c08cec42a9
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 0ac6e231ac797d1ba1e8dfb314c6aa3df99ff91d
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313963"
---
# <a name="step-3-configure-the-multisite-deployment"></a>Étape 3 configurer le déploiement multisite

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Après avoir configuré l’infrastructure multisite, procédez comme suit pour configurer le déploiement multisite de l’accès à distance.  
  
|Tâche|Description|  
|----|--------|  
|3.1. Configurer les serveurs d’accès à distance|Configurez des serveurs d’accès à distance supplémentaires en configurant des adresses IP, en les joignant au domaine et en installant le rôle accès à distance.|  
|3.2. Accorder un accès administrateur|Accordez des privilèges sur les autres serveurs d’accès à distance à l’administrateur DirectAccess.|  
|3.3. Configurer IP-HTTPs pour un déploiement multisite|Configurez le certificat IP-HTTPs utilisé dans un déploiement multisite.|  
|3.4. Configurer le serveur emplacement réseau pour un déploiement multisite|Configurez le certificat de serveur emplacement réseau utilisé dans un déploiement multisite.|  
|3.5. Configurer des clients DirectAccess pour un déploiement multisite|Supprimer les ordinateurs clients Windows 7 des groupes de sécurité Windows 8.|  
|3.6. Activer le déploiement multisite|Activez le déploiement multisite sur le premier serveur d’accès à distance.|  
|3,7. Ajouter des points d’entrée au déploiement multisite|Ajoutez des points d’entrée supplémentaires au déploiement multisite.|  
  
> [!NOTE]  
> Cette rubrique comprend des exemples d'applets de commande Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, consultez [Utilisation des applets de commande](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="31-configure-remote-access-servers"></a><a name="BKMK_ConfigServer"></a>3.1. Configurer les serveurs d’accès à distance  

  
### <a name="to-install-the-remote-access-role"></a>Pour installer le rôle Accès à distance  
  
1.  Assurez-vous que chaque serveur d’accès à distance est configuré avec la topologie de déploiement appropriée (Edge, derrière un NAT, une seule interface réseau) et les itinéraires correspondants.  
  
2.  Configurez les adresses IP sur chaque serveur d’accès à distance en fonction de la topologie de site et du schéma d’adressage IP de votre organisation.  
  
3.  Joignez chaque serveur d’accès à distance à un domaine Active Directory.  
  
4.  Dans la console Gestionnaire de serveur, dans le **tableau de bord**, cliquez sur **Ajouter des rôles et des fonctionnalités**.  
  
5.   Cliquez sur **Suivant** trois fois pour accéder à l’écran de sélection du rôle de serveur.  
  
6.  Dans la boîte de dialogue **Sélectionner des rôles de serveurs** , sélectionnez **accès à distance**, puis cliquez sur **suivant**.  
  
7.  Cliquez sur **suivant** trois fois.  
  
8.  Dans la boîte de dialogue **Sélectionner des services de rôle** , sélectionnez **DirectAccess et VPN (RAS)** , puis cliquez sur **Ajouter des fonctionnalités**.  
  
9.  Sélectionnez **routage**, sélectionnez **proxy d’application Web**, cliquez sur **Ajouter des fonctionnalités**, puis cliquez sur **suivant**.  
  
10. Cliquez sur **Suivant**, puis sur **Installer**.  
  
11.  Dans la boîte de dialogue **Progression de l'installation**, vérifiez que l'installation a réussi, puis cliquez sur **Fermer**.  
  
  
![les commandes Windows PowerShell](../../../../media/Step-3-Configure-the-Multisite-Deployment/PowerShellLogoSmall.gif)***<em>équivalentes</em> Windows PowerShell***  

  
Les étapes 1-3 doivent être effectuées manuellement et ne sont pas effectuées à l’aide de cette applet de commande Windows PowerShell.  
  
La ou les applets de commande Windows PowerShell suivantes ont la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles apparaissent ici sur plusieurs lignes en raison de contraintes de mise en forme.  
  
```  
Install-WindowsFeature RemoteAccess -IncludeManagementTools  
```  
  
## <a name="32-grant-administrator-access"></a><a name="BKMK_Admin"></a>3.2. Accorder un accès administrateur  
  
#### <a name="to-grant-administrator-permissions"></a>Pour accorder des autorisations d’administrateur  
  
1.  Sur le serveur d’accès à distance, dans le point d’entrée supplémentaire : dans l’écran d' **Accueil** , tapez gestion de l' **ordinateur**, puis appuyez sur entrée.  
  
2.  Dans le volet gauche, cliquez sur **utilisateurs et groupes locaux**.  
  
3.  Double-cliquez sur **groupes**, puis double-cliquez sur **administrateurs**.  
  
4.  Dans la boîte de dialogue **Propriétés des administrateurs** , cliquez sur **Ajouter**, puis dans la boîte de dialogue **Sélectionner les utilisateurs, les ordinateurs, les comptes de service ou les groupes** , cliquez sur **emplacements**.  
  
5.  Dans la boîte de dialogue **emplacements** , dans l’arborescence **emplacement** , cliquez sur l’emplacement qui contient le compte d’utilisateur de l’administrateur DirectAccess, puis cliquez sur **OK**.  
  
6.  Dans la **zone Entrez les noms des objets à sélectionner**, entrez le nom d’utilisateur de l’administrateur DirectAccess, puis cliquez deux fois sur **OK** .  
  
7.  Dans la boîte de dialogue **Propriétés des administrateurs** , cliquez sur **OK**.  
  
8.  Fermez la fenêtre Gestion de l'ordinateur.  
  
9. Répétez cette procédure sur tous les serveurs d’accès à distance qui feront partie du déploiement multisite.  
  
## <a name="33-configure-ip-https-for-a-multisite-deployment"></a><a name="BKMK_IPHTTPS"></a>3.3. Configurer IP-HTTPs pour un déploiement multisite  
Sur chaque serveur d’accès à distance qui sera ajouté au déploiement multisite, un certificat SSL est requis pour vérifier la connexion HTTPs au serveur Web IP-HTTPs. L'appartenance au groupe **Administrateurs** local, ou à un groupe équivalent, est la condition requise minimale pour effectuer cette procédure.  
  
#### <a name="to-obtain-an-ip-https-certificate"></a>Pour obtenir un certificat IP-HTTPs  
  
1.  Sur chaque serveur d’accès à distance : dans l’écran **Démarrer** , tapez **MMC**, puis appuyez sur entrée. Si la boîte de dialogue **Contrôle de compte d'utilisateur** apparaît, confirmez que l'action affichée est bien celle que vous souhaitez effectuer, puis cliquez sur **Oui**.  
  
2.  Cliquez sur **Fichier**, puis sur **Ajouter ou supprimer des composants logiciels enfichables**.  
  
3.  Cliquez sur **Certificats**, sur **Ajouter**, sur **Un compte d'ordinateur**, sur **Suivant**, sélectionnez **L'ordinateur local**, cliquez sur **Terminer**, puis sur **OK**.  
  
4.  Dans l'arborescence de la console du composant logiciel enfichable Certificats, ouvrez **Certificats (ordinateur local)\Personnel\Certificats**.  
  
5.  Cliquez avec le bouton droit sur **Certificats**, pointez sur **Toutes les Tâches**, puis cliquez sur **Demander un nouveau certificat**.  
  
6.  Cliquez deux fois sur **Suivant**.  
  
7.  Sur la page **demander des certificats** , cliquez sur le modèle de certificat de serveur Web, puis cliquez sur l' **inscription pour obtenir ce certificat nécessite des informations supplémentaires**.  
  
    Si le modèle de certificat de serveur Web n’apparaît pas, assurez-vous que le compte d’ordinateur du serveur d’accès à distance dispose des autorisations d’inscription pour le modèle de certificat de serveur Web. Pour plus d’informations, consultez [configurer des autorisations sur le modèle de certificat de serveur Web](https://technet.microsoft.com/library/ee649249(v=ws.10).aspx).  
  
8.  Sous l’onglet **objet** de la boîte de dialogue **Propriétés du certificat** , dans nom de l' **objet**, pour **type**, sélectionnez **nom commun**.  
  
9. Dans **valeur**, tapez le nom de domaine complet (FQDN) du nom Internet du serveur d’accès à distance (par exemple, Europe.contoso.com), puis cliquez sur **Ajouter**.  
  
10. Cliquez sur **OK**, sur **Inscrire**, puis sur **Terminer**.  
  
11. Dans le volet d'informations du composant logiciel enfichable Certificats, vérifiez qu'un nouveau certificat avec le nom de domaine complet a été inscrit avec **Rôles prévus** égal à **Authentification du serveur**.  
  
12. Cliquez avec le bouton droit sur le certificat, puis cliquez sur **Propriétés**.  
  
13. Dans **Nom convivial**, tapez **Certificat IP-HTTPS**, puis cliquez sur **OK**.  
  
    > [!TIP]  
    > Les étapes 12 et 13 sont facultatives, mais vous permettent de sélectionner plus facilement le certificat pour IP-HTTPs lors de la configuration de l’accès à distance.  
  
14. Répétez cette procédure sur tous les serveurs d’accès à distance de votre déploiement.  
  
## <a name="34-configure-the-network-location-server-for-a-multisite-deployment"></a><a name="BKMK_NLS"></a>3,4. Configurer le serveur emplacement réseau pour un déploiement multisite  
Si vous avez choisi de configurer le site Web du serveur emplacement réseau sur le serveur d’accès à distance lors de la configuration de votre premier serveur, chaque nouveau serveur d’accès à distance que vous ajoutez doit être configuré avec un certificat de serveur Web dont le nom d’objet est le même que celui sélectionné pour serveur d’emplacement réseau pour le premier serveur. Chaque serveur nécessite un certificat pour authentifier la connexion au serveur d’emplacement réseau, et les ordinateurs clients situés dans le réseau interne doivent être en mesure de résoudre le nom du site Web dans DNS.  
  
#### <a name="to-install-a-certificate-for-network-location"></a>Pour installer un certificat pour l’emplacement réseau  
  
1.  Sur le serveur d’accès à distance : dans l’écran **Démarrer** , tapez **MMC**, puis appuyez sur entrée. Si la boîte de dialogue **Contrôle de compte d'utilisateur** apparaît, confirmez que l'action affichée est bien celle que vous souhaitez effectuer, puis cliquez sur **Oui**.  
  
2.  Cliquez sur **Fichier**, puis sur **Ajouter ou supprimer des composants logiciels enfichables**.  
  
3.  Cliquez sur **Certificats**, sur **Ajouter**, sur **Un compte d'ordinateur**, sur **Suivant**, sélectionnez **L'ordinateur local**, cliquez sur **Terminer**, puis sur **OK**.  
  
4.  Dans l'arborescence de la console du composant logiciel enfichable Certificats, ouvrez **Certificats (ordinateur local)\Personnel\Certificats**.  
  
5.  Cliquez avec le bouton droit sur **Certificats**, pointez sur **Toutes les Tâches**, puis cliquez sur **Demander un nouveau certificat**.  
  
    > [!NOTE]  
    > Vous pouvez également importer le certificat utilisé pour le serveur emplacement réseau pour le premier serveur d’accès à distance.  
  
6.  Cliquez deux fois sur **Suivant**.  
  
7.  Sur la page **demander des certificats** , cliquez sur le modèle de certificat de serveur Web, puis cliquez sur l' **inscription pour obtenir ce certificat nécessite des informations supplémentaires**.  
  
    Si le modèle de certificat de serveur Web n’apparaît pas, assurez-vous que le compte d’ordinateur du serveur d’accès à distance dispose des autorisations d’inscription pour le modèle de certificat de serveur Web. Pour plus d’informations, consultez [configurer des autorisations sur le modèle de certificat de serveur Web](https://technet.microsoft.com/library/ee649249(v=ws.10).aspx).  
  
8.  Sous l’onglet **objet** de la boîte de dialogue **Propriétés du certificat** , dans nom de l' **objet**, pour **type**, sélectionnez **nom commun**.  
  
9. Dans **valeur**, tapez le nom de domaine complet (FQDN) qui a été configuré pour le certificat de serveur emplacement réseau du premier serveur d’accès à distance (par exemple, nls.Corp.contoso.com), puis cliquez sur **Ajouter**.  
  
10. Cliquez sur **OK**, sur **Inscrire**, puis sur **Terminer**.  
  
11. Dans le volet d'informations du composant logiciel enfichable Certificats, vérifiez qu'un nouveau certificat avec le nom de domaine complet a été inscrit avec **Rôles prévus** égal à **Authentification du serveur**.  
  
12. Cliquez avec le bouton droit sur le certificat, puis cliquez sur **Propriétés**.  
  
13. Dans **Nom convivial**, tapez **Certificat Emplacement réseau**, puis cliquez sur **OK**.  
  
    > [!TIP]  
    > Les étapes 12 et 13 sont facultatives, mais vous permettent de sélectionner plus facilement le certificat pour l’emplacement réseau lors de la configuration de l’accès à distance.  
  
14. Répétez cette procédure sur tous les serveurs d’accès à distance de votre déploiement.  
  
### <a name="to-create-the-network-location-server-dns-records"></a><a name="NLS"></a>Pour créer les enregistrements DNS du serveur d’emplacement réseau  
  
1.  Sur le serveur DNS : dans l’écran d' **Accueil** , tapez **dnsmgmt. msc**, puis appuyez sur entrée.  
  
2.  Dans le volet gauche de la console du **Gestionnaire DNS** , ouvrez la zone de recherche directe pour le réseau interne. Cliquez avec le bouton droit sur la zone appropriée, puis cliquez sur **nouvel hôte (A ou AAAA)** .  
  
3.  Dans la boîte de dialogue **nouvel hôte** , dans la zone **nom (utilise le nom de domaine parent si** ce champ est vide), entrez le nom utilisé pour le serveur d’emplacement réseau pour le premier serveur d’accès à distance. Dans la zone **adresse IP** , entrez l’adresse IPv4 intranet du serveur d’accès à distance, puis cliquez sur **Ajouter un ordinateur hôte**. Dans la boîte de dialogue **DNS**, cliquez sur **OK**.  
  
4.  Dans la boîte de dialogue **nouvel hôte** , dans la zone **nom (utilise le nom de domaine parent si** ce champ est vide), entrez le nom utilisé pour le serveur d’emplacement réseau pour le premier serveur d’accès à distance. Dans la zone **adresse IP** , entrez l’adresse IPv6 intranet du serveur d’accès à distance, puis cliquez sur **Ajouter un ordinateur hôte**. Dans la boîte de dialogue **DNS**, cliquez sur **OK**.  
  
5.  Répétez les étapes 3 et 4 pour chaque serveur d’accès à distance de votre déploiement.  
  
6.  Cliquez sur **Terminé**.  
  
7.  Répétez cette procédure avant d’ajouter des serveurs en tant que points d’entrée supplémentaires dans le déploiement.  
  
## <a name="35-configure-directaccess-clients-for-a-multisite-deployment"></a><a name="BKMK_Client"></a>3,5. Configurer des clients DirectAccess pour un déploiement multisite  
Les ordinateurs clients Windows DirectAccess doivent être membres d’un ou plusieurs groupes de sécurité qui définissent leur association DirectAccess. Avant l’activation de l’option multisite, ces groupes de sécurité peuvent contenir à la fois des clients Windows 8 et des clients Windows 7 (si le mode « de bas niveau » approprié a été sélectionné). Une fois que le multisite est activé, le ou les groupes de sécurité client existants, en mode serveur unique, sont convertis en groupes de sécurité pour Windows 8 uniquement. Une fois que le multisite est activé, les ordinateurs clients Windows 7 DirectAccess doivent être déplacés vers les groupes de sécurité client Windows 7 dédiés correspondants (qui sont associés à des points d’entrée spécifiques), ou ils ne pourront pas se connecter via DirectAccess. Les clients Windows 7 doivent d’abord être supprimés des groupes de sécurité existants qui sont désormais des groupes de sécurité Windows 8. ATTENTION : les ordinateurs clients Windows 7 qui sont membres des groupes de sécurité client Windows 7 et Windows 8 perdent la connectivité à distance et les clients Windows 7 sans SP1 perdent également la connectivité de l’entreprise. Par conséquent, tous les ordinateurs clients Windows 7 doivent être supprimés des groupes de sécurité Windows 8.  
  
#### <a name="remove--windows-7--clients-from-windows-8-security-groups"></a>Supprimer les clients Windows 7 des groupes de sécurité Windows 8  
  
1.  Sur le contrôleur de domaine principal, cliquez sur **Démarrer**, puis sur **Active Directory les utilisateurs et les ordinateurs**.  
  
2.  Pour supprimer des ordinateurs du groupe de sécurité, double-cliquez sur le groupe de sécurité, puis dans la boîte de dialogue **< Group_Name propriétés du >** , cliquez sur l’onglet **membres** .  
  
3.  Sélectionnez l’ordinateur client Windows 7, puis cliquez sur **supprimer**.  
  
4.  Répétez cette procédure pour supprimer les ordinateurs clients Windows 7 des groupes de sécurité Windows 8.  
  
> [!IMPORTANT]  
> Lorsque vous activez une configuration multisite d’accès à distance, tous les ordinateurs clients (Windows 7 et Windows 8) perdent la connectivité à distance jusqu’à ce qu’ils puissent se connecter au réseau d’entreprise directement ou par VPN pour mettre à jour leurs stratégies de groupe. Cela est vrai lors de l’activation de la fonctionnalité multisite pour la première fois, ainsi que lors de la désactivation de multisite.  
  
## <a name="36-enable-the-multisite-deployment"></a><a name="BKMK_Enable"></a>3,6. Activer le déploiement multisite  
Pour configurer un déploiement multisite, activez la fonctionnalité multisite sur votre serveur d’accès à distance existant. Avant d’activer la multisite dans votre déploiement, assurez-vous de disposer des informations suivantes :  
  
1.  Les paramètres globaux de l’équilibreur de charge et les adresses IP si vous souhaitez équilibrer la charge des connexions des clients DirectAccess sur tous les points d’entrée de votre déploiement.  
  
2.  Le ou les groupes de sécurité contenant les ordinateurs clients Windows 7 pour le premier point d’entrée de votre déploiement, si vous souhaitez activer l’accès à distance pour les ordinateurs clients Windows 7.  
  
3.  Stratégie de groupe des noms d’objets, si vous devez utiliser des objets de stratégie de groupe autres que ceux par défaut, qui sont appliqués sur les ordinateurs clients Windows 7 pour le premier point d’entrée de votre déploiement, si vous avez besoin de la prise en charge des ordinateurs clients Windows 7.  
  
### <a name="to-enable-a-multisite-configuration"></a><a name="EnabledMultisite"></a>Pour activer une configuration multisite  
  
1.  Sur votre serveur d’accès à distance existant : dans l’écran d' **Accueil** , tapez **RAMgmtUI. exe**, puis appuyez sur entrée. Si la boîte de dialogue **Contrôle de compte d'utilisateur** apparaît, confirmez que l'action affichée est bien celle que vous souhaitez effectuer, puis cliquez sur **Oui**.  
  
2.  Dans la console Gestion de l’accès à distance, cliquez sur **configuration**, puis dans le volet **tâches** , cliquez sur **activer multisite**.  
  
3.  Dans l’Assistant **activer le déploiement multisite** , dans la page **avant de commencer** , cliquez sur **suivant**.  
  
4.  Dans la page **nom du déploiement** , dans nom du **déploiement multisite**, entrez un nom pour votre déploiement. Dans **premier nom du point d’entrée**, entrez un nom pour identifier le premier point d’entrée qui est le serveur d’accès à distance actuel, puis cliquez sur **suivant**.  
  
5.  Sur la page **sélection du point d’entrée** , effectuez l’une des opérations suivantes :  
  
    -   Cliquez sur **affecter des points d’entrée automatiquement, et autorisez les clients à sélectionner manuellement** pour router automatiquement les ordinateurs clients vers le point d’entrée le plus approprié, tout en autorisant également les ordinateurs clients à sélectionner un point d’entrée manuellement. La sélection manuelle des points d’entrée est disponible uniquement pour les ordinateurs Windows 8. Cliquez sur **Suivant**.  
  
    -   Cliquez sur **affecter des points d’entrée automatiquement** pour router automatiquement les ordinateurs clients vers le point d’entrée le plus approprié, puis cliquez sur **suivant**.  
  
6.  Dans la page **équilibrage de charge global** , effectuez l’une des opérations suivantes :  
  
    -   Cliquez sur **non, ne pas utiliser l’équilibrage de charge global** si vous ne souhaitez pas utiliser un équilibrage de charge global, puis cliquez sur **suivant**.  
  
        > [!NOTE]  
        > Lorsque cette option est sélectionnée, les ordinateurs clients se connectent automatiquement à leur point d’entrée le plus proche.  
  
    -   Cliquez sur **Oui, utiliser l’équilibrage de charge global** si vous souhaitez équilibrer la charge du trafic globalement entre tous les points d’entrée. Dans **tapez le nom de domaine complet de l’équilibrage de charge global à utiliser par tous les points d’entrée**, entrez le nom de domaine complet de l’équilibrage de charge global, puis dans **tapez l’adresse IP d’équilibrage de charge globale pour ce point d’entrée** contenant le premier serveur d’accès à distance, entrez l’adresse IP d’équilibrage de charge globale pour ce point d’entrée, puis cliquez sur **suivant**  
  
7.  Sur la page **prise en charge du client** , effectuez l’une des opérations suivantes :  
  
    -   Pour limiter l’accès aux ordinateurs clients exécutant Windows 8 ou des systèmes d’exploitation ultérieurs, cliquez sur **limiter l’accès aux ordinateurs clients exécutant Windows 8 ou un système d’exploitation ultérieur**, puis cliquez sur **suivant**.  
  
    -   Pour permettre aux ordinateurs clients exécutant Windows 7 d’accéder à ce point d’entrée, cliquez sur **autoriser les ordinateurs clients exécutant Windows 7 à accéder à ce point d’entrée**, puis cliquez sur **Ajouter**. Dans la boîte de dialogue **Sélectionner des groupes** , sélectionnez le ou les groupes de sécurité qui contiennent les ordinateurs clients Windows 7, cliquez sur **OK**, puis sur **suivant**.  
  
8.  Dans la page Paramètres de l' **objet de stratégie de groupe du client** , acceptez l’objet de stratégie de groupe par défaut pour les ordinateurs clients Windows 7 pour ce point d’entrée, tapez le nom de l’objet de stratégie de groupe qui doit être créé automatiquement par l’accès à distance, ou cliquez sur **Parcourir** pour localiser le GPO pour les ordinateurs clients Windows 7, puis cliquez sur **suivant**.  
  
    > [!NOTE]  
    > -   La page des paramètres de l' **objet de stratégie de groupe du client** s’affiche uniquement lorsque vous configurez le point d’entrée pour autoriser les ordinateurs clients Windows 7 à accéder au point d’entrée.  
    > -   Vous pouvez éventuellement cliquer sur **valider les objets** de stratégie de groupe pour vous assurer que vous disposez des autorisations appropriées pour l’objet de stratégie de groupe ou les objets de stratégie de groupe sélectionnés pour ce point d’entrée. Si l’objet de stratégie de groupe n’existe pas et sera automatiquement créé, les autorisations créer et lier sont requises. Dans le cas où les objets de stratégie de groupe ont été créés manuellement, les autorisations modifier, modifier la sécurité et supprimer sont requises.  
  
9. Sur la page **Résumé** , cliquez sur **valider**.  
  
10. Dans la boîte de dialogue **activation du déploiement multisite** , cliquez sur **Fermer** , puis dans l’Assistant activer le déploiement multisite, cliquez sur **Fermer**.  
  
![les commandes Windows PowerShell](../../../../media/Step-3-Configure-the-Multisite-Deployment/PowerShellLogoSmall.gif)***<em>équivalentes</em> Windows PowerShell***  
  
La ou les applets de commande Windows PowerShell suivantes ont la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles apparaissent ici sur plusieurs lignes en raison de contraintes de mise en forme.  
  
Pour activer un déploiement multisite nommé « Contoso » sur le premier point d’entrée nommé « Edge1-US ». Le déploiement permet aux clients de sélectionner manuellement le point d’entrée et n’utilise pas d’équilibreur de charge global.  
  
```  
Enable-DAMultiSite -Name 'Contoso' -EntryPointName 'Edge1-US' -ManualEntryPointSelectionAllowed 'Enabled'  
```  
  
Pour autoriser l’accès des ordinateurs clients Windows 7 par le biais du premier point d’entrée via le groupe de sécurité DA_Clients_US et à l’aide de l’DA_W7_Clients_GPO_US de stratégie de groupe.  
  
```  
Add-DAClient -EntrypointName 'Edge1-US' -DownlevelSecurityGroupNameList @('corp.contoso.com\DA_Clients_US') -DownlevelGpoName @('corp.contoso.com\DA_W7_Clients_GPO_US)  
```  
  
## <a name="37-add-entry-points-to-the-multisite-deployment"></a><a name="BKMK_EntryPoint"></a>3,7. Ajouter des points d’entrée au déploiement multisite  
Après avoir activé la multisite dans votre déploiement, vous pouvez ajouter des points d’entrée supplémentaires à l’aide de l’Assistant Ajouter un point d’entrée. Avant d’ajouter des points d’entrée, vérifiez que vous disposez des informations suivantes :  
  
-   Adresses IP globales de l’équilibreur de charge pour chaque nouveau point d’entrée si vous utilisez l’équilibrage de charge global.  
  
-   Le ou les groupes de sécurité contenant des ordinateurs clients Windows 7 pour chaque point d’entrée qui sera ajouté si vous souhaitez activer l’accès à distance pour les ordinateurs clients Windows 7.  
  
-   Stratégie de groupe des noms d’objets, si vous devez utiliser des objets de stratégie de groupe non par défaut, qui sont appliqués sur les ordinateurs clients Windows 7 pour chaque point d’entrée qui sera ajouté, si vous avez besoin de la prise en charge des ordinateurs clients Windows 7.  
  
-   Dans le cas où IPv6 est déployé sur le réseau de l’organisation, vous devez préparer le préfixe IP-HTTPs pour le nouveau point d’entrée.  
  
### <a name="to-add-entry-points-to-your-multisite-deployment"></a><a name="AddEP"></a>Pour ajouter des points d’entrée à votre déploiement multisite  
  
1.  Sur votre serveur d’accès à distance existant : dans l’écran d' **Accueil** , tapez **RAMgmtUI. exe**, puis appuyez sur entrée. Si la boîte de dialogue **Contrôle de compte d'utilisateur** apparaît, confirmez que l'action affichée est bien celle que vous souhaitez effectuer, puis cliquez sur **Oui**.  
  
2.  Dans la console Gestion de l’accès à distance, cliquez sur **configuration**, puis dans le volet **tâches** , cliquez sur **Ajouter un point d’entrée**.  
  
3.  Dans l’Assistant Ajouter un point d’entrée, sur la page **Détails du point d’entrée** , dans serveur d' **accès à distance**, entrez le nom de domaine complet (FQDN) du serveur à ajouter. Dans **nom du point d’entrée**, entrez le nom du point d’entrée, puis cliquez sur **suivant**.  
  
4.  Dans la page **paramètres globaux** de l’équilibrage de charge, entrez l’adresse IP globale d’équilibrage de charge de ce point d’entrée, puis cliquez sur **suivant**.  
  
    > [!NOTE]  
    > La page **paramètres globaux** de l’équilibrage de charge s’affiche uniquement lorsque la configuration multisite utilise un équilibreur de charge global.  
  
5.  Dans la page **topologie du réseau** , cliquez sur la topologie qui correspond à la topologie réseau du serveur d’accès à distance que vous ajoutez, puis cliquez sur **suivant**.  
  
6.  Dans la page **nom du réseau ou adresse IP** , dans **tapez le nom public ou l’adresse IP utilisé par les clients pour se connecter au serveur d’accès à distance**, entrez le nom public ou l’adresse IP utilisé par les clients pour se connecter au serveur d’accès à distance. Le nom public correspond au nom d’objet du certificat IP-HTTPs. Dans le cas où la topologie du réseau de périmètre était implémentée, l’adresse IP est celle de la carte externe du serveur d’accès à distance. Cliquez sur **Suivant**.  
  
7.  Dans la page **cartes réseau** , effectuez l’une des opérations suivantes :  
  
    -   Si vous déployez une topologie avec deux cartes réseau, dans **carte externe**, sélectionnez la carte qui est connectée au réseau externe. Dans **adaptateur interne**, sélectionnez la carte qui est connectée au réseau interne.  
  
    -   Si vous déployez une topologie avec une carte réseau, dans **carte réseau**, sélectionnez la carte qui est connectée au réseau interne.  
  
8.  Dans la page **cartes réseau** , dans **Sélectionner le certificat utilisé pour authentifier les connexions IP-HTTPS**, cliquez sur **Parcourir** pour rechercher et sélectionner le certificat IP-HTTPS. Cliquez sur **Suivant**.  
  
9. Si IPv6 est configuré sur le réseau d’entreprise, dans la page **configuration du préfixe** , dans **préfixe IPv6 attribué aux ordinateurs clients**, entrez un PRÉfixe IP-HTTPS pour affecter des adresses IPv6 aux ordinateurs clients DirectAccess, puis cliquez sur **suivant**.  
  
10. Sur la page **prise en charge du client** , effectuez l’une des opérations suivantes :  
  
    -   Pour limiter l’accès aux ordinateurs clients exécutant Windows 8 ou des systèmes d’exploitation ultérieurs, cliquez sur **limiter l’accès aux ordinateurs clients exécutant Windows 8 ou un système d’exploitation ultérieur**, puis cliquez sur **suivant**.  
  
    -   Pour permettre aux ordinateurs clients exécutant Windows 7 d’accéder à ce point d’entrée, cliquez sur **autoriser les ordinateurs clients exécutant Windows 7 à accéder à ce point d’entrée**, puis cliquez sur **Ajouter**. Dans la boîte de dialogue **Sélectionner des groupes** , sélectionnez le ou les groupes de sécurité qui contiennent les ordinateurs clients Windows 7 qui se connecteront à ce point d’entrée, cliquez sur **OK**, puis sur **suivant**.  
  
11. Dans la page Paramètres de l' **objet de stratégie de groupe du client** , acceptez l’objet de stratégie de groupe par défaut pour les ordinateurs clients Windows 7 pour ce point d’entrée, tapez le nom de l’objet de stratégie de groupe que vous souhaitez que l’accès à distance crée automatiquement, ou cliquez sur **Parcourir** pour localiser le GPO pour les ordinateurs clients Windows 7, puis cliquez sur **suivant**.  
  
    > [!NOTE]  
    > -   La page des paramètres de l' **objet de stratégie de groupe du client** s’affiche uniquement lorsque vous configurez le point d’entrée pour autoriser les ordinateurs clients Windows 7 à accéder au point d’entrée.  
    > -   Vous pouvez éventuellement cliquer sur **valider les objets** de stratégie de groupe pour vous assurer que vous disposez des autorisations appropriées pour l’objet de stratégie de groupe ou les objets de stratégie de groupe sélectionnés pour ce point d’entrée. Si l’objet de stratégie de groupe n’existe pas et sera automatiquement créé, les autorisations créer et lier sont requises. Dans le cas où les objets de stratégie de groupe ont été créés manuellement, les autorisations modifier, modifier la sécurité et supprimer sont requises.  
  
12. Dans la page Paramètres de l' **objet de stratégie de groupe du serveur** , acceptez l’objet de stratégie de groupe par défaut pour ce serveur d’accès à distance, tapez le nom de l’objet de stratégie de groupe que vous souhaitez que l’accès à distance crée automatiquement, ou cliquez sur **Parcourir** pour localiser l’objet de stratégie de groupe pour ce serveur, puis cliquez sur **suivant**.  
  
13. Sur la page **serveur emplacement réseau** , cliquez sur **Parcourir** pour sélectionner le certificat pour le site Web du serveur emplacement réseau en cours d’exécution sur le serveur d’accès à distance, puis cliquez sur **suivant**.  
  
    > [!NOTE]  
    > La page **serveur emplacement réseau** s’affiche uniquement lorsque le site Web du serveur emplacement réseau est en cours d’exécution sur le serveur d’accès à distance.  
  
14. Sur la page **Résumé** , passez en revue les paramètres de point d’entrée, puis cliquez sur **valider**.  
  
15. Dans la boîte de dialogue Ajout d’un **point d’entrée** , cliquez sur **Fermer** , puis dans l’Assistant Ajouter un point d’entrée, cliquez sur **Fermer**.  
  
    > [!NOTE]  
    > Si le point d’entrée ajouté se trouve dans une forêt différente de celle des points d’entrée ou des ordinateurs clients existants, il est nécessaire de cliquer sur **Actualiser les serveurs d’administration** dans le volet **tâches** pour découvrir les contrôleurs de domaine et les Configuration Manager de la nouvelle forêt.  
  
16. Répétez cette procédure à partir de l’étape 2 pour chaque point d’entrée que vous souhaitez ajouter à votre déploiement multisite.  
  
![les commandes Windows PowerShell](../../../../media/Step-3-Configure-the-Multisite-Deployment/PowerShellLogoSmall.gif)***<em>équivalentes</em> Windows PowerShell***  
  
La ou les applets de commande Windows PowerShell suivantes ont la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles apparaissent ici sur plusieurs lignes en raison de contraintes de mise en forme.  
  
Pour ajouter l’ordinateur Edge2 à partir du domaine corp2 en tant que deuxième point d’entrée nommé Edge2-Europe. La configuration du point d’entrée est : un préfixe IPv6 client « 2001 : DB8:2 : 2000 ::/64 », une adresse de connexion (le certificat IP-HTTPs sur l’ordinateur Edge2) « edge2.contoso.com », un objet de stratégie de groupe de serveur nommé « paramètres de serveur DirectAccess-Edge2-Europe » et les interfaces internes et externes nommées Internet et Corpnet2 respectivement :  
  
```  
Add-DAEntryPoint -RemoteAccessServer 'edge2.corp2.corp.contoso.com' -Name 'Edge2-Europe' -ClientIPv6Prefix '2001:db8:2:2000::/64' -ConnectToAddress 'Europe.contoso.com' -ServerGpoName 'corp2.corp.contoso.com\DirectAccess Server Settings - Edge2-Europe' -InternetInterface 'Internet' -InternalInterface 'Corpnet2'  
```  
  
Pour autoriser l’accès des ordinateurs clients Windows 7 par le biais du deuxième point d’entrée via le groupe de sécurité DA_Clients_Europe et à l’aide de l’DA_W7_Clients_GPO_Europe de stratégie de groupe.  
  
```  
Add-DAClient -EntrypointName 'Edge2-Europe' -DownlevelGpoName @('corp.contoso.com\ DA_W7_Clients_GPO_Europe') -DownlevelSecurityGroupNameList @('corp.contoso.com\DA_Clients_Europe')  
```  
  
## <a name="see-also"></a><a name="BKMK_Links"></a>Voir aussi  
  
-   [Étape 2 : configurer l’infrastructure multisite](Step-2-Configure-the-Multisite-Infrastructure.md)
