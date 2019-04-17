---
title: "Préparer la migration du serveur ADFS 2.0 de fédération à ADFS dans Windows Server2012R2"
description: "Fournit des informations sur l’obtention de prêt à migrer le serveur ADFS vers Windows Server2012R2."
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4d0ff53b9118db1dd6ba5af94b3e627bf1597e0c
ms.sourcegitcommit: 03ce78a1624dbd7f4e6abf2ec1ef185b55de29a1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2017
---
# <a name="prepare-to-migrate-the-ad-fs-20-federation-server-to-ad-fs-on-windows-server-2012-r2"></a>Préparer la migration du serveur ADFS 2.0 de fédération à ADFS dans Windows Server2012R2

Ce document décrit comment migrer un ADFS 2.0 ou une batterie de serveurs de fédération Windows Server2012vers une batterie de serveurs ADFS de Windows Server2012R2.  Les étapes peuvent être utilisés avec des batteries de serveurs ADFS qui utilisent WID ou SQLServer en tant que la base de données sous-jacente.  
  
-   [Plan du processus de migration](prepare-migrate-ad-fs-server-r2.md#migrate-process-outline)  
  
-   [Nouvelles fonctionnalités ADFS dans Windows Server2012R2](prepare-migrate-ad-fs-server-r2.md#new-ad-fs-functionality-in-windows-server-2012-r2)  
  
-   [Configuration requise pour d’ADFS dans Windows Server2012R2](prepare-migrate-ad-fs-server-r2.md#ad-fs-requirements-in-windows-server-2012-r2)  
  
-   [Augmentation des limites de Windows PowerShell](prepare-migrate-ad-fs-server-r2.md#increasing-your-windows-powershell-limits)  
  
-   [Autres tâches de migration et les considérations](prepare-migrate-ad-fs-server-r2.md#other-migration-tasks-and-considerations)  
  
##  <a name="migration-process-outline"></a>Plan du processus de migration  
 Pour terminer la migration de votre batterie de serveurs de fédération ADFS vers Windows Server2012R2, vous devez effectuer les tâches suivantes:  
  
1.  Exportez, enregistrez et sauvegardez les données de configuration suivantes dans votre batterie ADFS existante. Pour obtenir des instructions détaillées sur la façon d’effectuer ces tâches, voir [migration du serveur de fédération ADFS](migrate-ad-fs-fed-server-r2.md).  
  
Les paramètres suivants sont migrés avec les scripts situés dans le dossier \support\adfs sur le CD d’installation de Windows Server2012R2:  
  
-   Revendications d’approbations de fournisseur, à l’exception des règles de revendication personnalisées sur l’approbation de fournisseur de revendications ActiveDirectory. Pour plus d’informations, voir [migration du serveur de fédération ADFS](migrate-ad-fs-fed-server-r2.md).  
  
-   Approbations de partie de confiance.  
  
-   ADFS généré en interne, auto-signé de signature de jetons et les certificats de déchiffrement de jeton.  
  
Les paramètres personnalisés suivants doivent être migrés manuellement:  
  
 -   Paramètres de service:  
  
     -   Signature de jetons non par défaut et les certificats de déchiffrement de jeton qui ont été émis par une entreprise ou une autorité de certification publique.  
  
     -   Le certificat d’authentification serveur SSL utilisé par ADFS.  
  
     -   Le certificat de communications de service utilisé par ADFS (par défaut, il s’agit du même certificat que le certificat SSL.  
  
      -   Non-par défaut pour toutes les propriétés du service de fédération, par exemple, la durée de vie AutoCertificateRollover ou l’authentification unique.  
  
      -   Paramètres de point de terminaison ADFS non par défaut et descriptions des revendications.  
  
-   Sur l’approbation de fournisseur de revendications ActiveDirectory, les règles de revendication personnalisées.  
  
    -   Personnalisations de page de connexion ADFS  
  
Pour plus d’informations, voir [migration du serveur de fédération ADFS](migrate-ad-fs-fed-server-r2.md).  
  
2.  Créer une batterie de serveurs de fédération Windows Server2012R2.  
  
3.  Importer les données de configuration d’origine dans cette nouvelle batterie ADFS de Windows Server2012R2.  
  
4.  Configurer et personnaliser les pages de connexion ADFS.  
  
##  <a name="new-ad-fs-functionality-in-windows-server-2012-r2"></a>Nouvelles fonctionnalités ADFS dans Windows Server2012R2  
 La fonctionnalité ADFS suivante modifie dans Windows Server2012R2 impact une migration depuis ADFS 2.0 ou ADFS dans Windows Server2012:  
  
**Dépendance d’IIS**  
   - ADFS dans Windows Server2012R2 sont auto-hébergés et ne nécessite pas l’installation d’IIS. Assurez-vous que vous notez les informations suivantes à la suite de cette modification:  
   -   Gestion des certificats SSL pour les serveurs de fédération et proxy dans votre batterie ADFS doit maintenant être effectuée via Windows PowerShell.  
  
**Modifications apportées aux personnalisations et paramètres de connexion des pages ADFS**  
-   Dans ADFS dans Windows Server2012R2, il existe plusieurs modifications destinées à améliorer l’expérience de connexion pour les administrateurs et utilisateurs. Les pages web hébergés par IIS qui existaient dans la version précédente d’ADFS sont maintenant supprimées. L’apparence des pages ADFS sign-in web sont auto-hébergés dans ADFS et peut désormais être personnalisé pour personnaliser l’expérience utilisateur. Les modifications sont les suivantes:  
    -   Personnalisation de l’ADFS expérience de connexion, y compris la personnalisation de la société, logo, illustration et description de la connexion.  
    -   Personnalisation des messages d’erreur.  
    -   Personnalisation de l’expérience de ADFS accueil la découverte de domaine, qui comprend les éléments suivants:  
        -   La configuration de votre fournisseur d’identité pour utiliser certains suffixes d’e-mail.  
        -   La configuration d’une liste de fournisseurs d’identité par partie de confiance tiers.  
        -   Contourner la découverte de domaine d’accueil pour l’intranet.  
        -   Création de thèmes web personnalisés.  
  
Pour obtenir des instructions détaillées sur la configuration de l’apparence des pages de connexion ADFS, voir [personnalisation des Pages ADFS Sign-in](../operations/AD-FS-Customization-in-Windows-Server-2016.md).  
  
Si vous disposez d’une personnalisation de page web dans votre batterie ADFS existante que vous souhaitez migrer vers Windows Server2012R2, vous pouvez les recréer dans le cadre du processus de migration à l’aide des nouvelles fonctionnalités de personnalisation dans Windows Server2012R2.  
  
-   **Autres modifications**  
  
    -   ADFS dans Windows Server2012R2 est basé sur WindowsIdentityFoundation (WIF) 3.5 et non WIF 4.5. Par conséquent, certaines fonctionnalités spécifiques de WIF 4.5 (par exemple, les revendications Kerberos et le contrôle d’accès dynamique) ne sont pas pris en charge dans ADFS dans Windows Server2012R2.  
  
    -   Service DRS (Device Registration Service) dans Windows Server2012R2 fonctionne sur le port 443; clientTLS pour l’authentification des certificats utilisateur fonctionne sur le port 49443  
  
        -   Pour les clients actives, sans navigateur à l’aide de l’authentification en mode de transport de certificats qui sont spécifiquement codés en dur pour pointer vers le port 443, une modification du code est nécessaire pour continuer à utiliser l’authentification des certificats utilisateur sur le port 49443.  
  
        -   Pour les applications passives, aucune modification n’est requise, car les services ADFS redirigent vers le port approprié pour l’authentification des certificats utilisateur.  
  
        -   Ports de pare-feu entre le client et le serveur proxy doivent activer le trafic du port 49443 de traverser pour l’authentification des certificats utilisateur.  
  
##  <a name="ad-fs-requirements-in-windows-server-2012-r2"></a>Configuration requise pour d’ADFS dans Windows Server2012R2  
 Pour pouvoir migrer votre batterie ADFS vers Windows Server2012R2, vous devez remplir les conditions suivantes:  
  
 Pour ADFS fonctionne, chaque ordinateur que vous souhaitez être une fédération doit être joint à un domaine.  
  
 Pour ADFS en cours d’exécution sur Windows Server2012R2 pour la fonction, votre domaine ActiveDirectory doit exécuter une des opérations suivantes:  
  
-   Windows Server2012R2  
  
-   Windows Server2012  
  
-   Windows Server2008R2  
  
-   Windows Server2008  
  
 Si vous prévoyez d’utiliser un groupe (gMSA) du compte de Service administré comme compte de service pour ADFS, vous devez disposer d’au moins un contrôleur de domaine dans votre environnement est en cours d’exécution sur le système d’exploitation Windows Server2012 ou Windows Server2012R2.  
  
 Si vous envisagez de déployer le Service DRS (Device Registration Service) pour la jonction d’espace de travail ActiveDirectory dans le cadre de votre déploiement d’ADFS, le schéma ADDS doit être mis à jour au niveau de Windows Server2012R2. Il existe trois façons de mettre à jour le schéma:  
  
1.  Dans une forêt ActiveDirectory existante, exécutez adprep /forestprep à partir du dossier \support\adprep du système d’exploitation de Windows Server2012R2 DVD sur tout serveur 64bits qui exécute Windows Server2008 ou version ultérieure. Dans ce cas, aucun contrôleur de domaine supplémentaire ne doit être installé, et aucun contrôleur de domaine existants à mettre à niveau.  
  
Pour exécuter adprep/forestprep, vous devez être membre du groupe Administrateurs du schéma, du groupe Administrateurs de l’entreprise et Admins du domaine du domaine qui héberge le contrôleur de schéma.  
  
2.  Dans une forêt ActiveDirectory existante, installez un contrôleur de domaine qui exécute Windows Server2012R2. Dans ce cas, adprep /forestprep s’exécute automatiquement dans le cadre de l’installation du contrôleur de domaine.  
  
Pendant l’installation du contrôleur de domaine, vous devrez peut-être spécifier des informations d’identification supplémentaires afin d’exécuter adprep /forestprep.  
  
3.  Créer une nouvelle forêt ActiveDirectory en installant les services ADDS sur un serveur qui exécute Windows Server2012R2. Dans ce cas, adprep /forestprep sans devoir être exécutée car le schéma sera initialement créé avec tous les conteneurs nécessaires et les objets à prendre en charge DRS.  
  
### <a name="sql-server-support-for-ad-fs-in-windows-server-2012-r2"></a>Prise en charge de SQLServer pour ADFS dans Windows Server2012R2  
 Si vous souhaitez créer une batterie ADFS et utiliser SQLServer pour stocker vos données de configuration, vous pouvez utiliser SQLServer2008 et versions plus récentes, notamment SQLServer2012.  
  
##  <a name="increasing-your-windows-powershell-limits"></a>Augmentation des limites de Windows PowerShell  
 Si vous avez plus de 1000approbations de fournisseur de revendications et approbations de partie de confiance dans votre batterie ADFS, ou si vous voyez l’erreur suivante lors de la tentative d’exécuter l’outil d’exportation/importation de migration ADFS, vous devez augmenter les limites de Windows PowerShell:  
  
```  
'Exception of type 'System.OutOfMemoryException' was thrown. At E:\dev\ds\security\ADFSv2\Product\Migration\Export-FederationConfiguration.ps1:176 char:21 + $configData = Invoke-Command -ScriptBlock $GetConfig -Argume ...  
```  
  
 Cette erreur est générée car la limite de mémoire par défaut session Windows PowerShell est trop faible. Dans WindowsPowerShell2.0, la mémoire par défaut de la session est 150Mo. Dans WindowsPowerShell3.0, la mémoire de la session par défaut est 1024Mo. Vous pouvez vérifier la limite de mémoire de session à distance de Windows PowerShell à l’aide de la commande suivante:`Get-Item wsman:localhost\Shell\MaxMemoryPerShellMB`. Vous pouvez augmenter la limite en exécutant la commande suivante:`Set-Item wsman:localhost\Shell\MaxMemoryPerShellMB 512`.  
  
## <a name="other-migration-tasks-and-considerations"></a>Autres tâches de migration et les considérations  
 Pour pouvoir migrer votre batterie ADFS vers Windows Server2012R2, assurez-vous que vous êtes informé des opérations suivantes:  
  
-   Les scripts de migration situés dans le dossier \support\adfs sur le CD d’installation de Windows Server2012R2 nécessitent de conserver le même nom de la batterie de serveur de fédération et le nom d’identité de compte de service que vous avez utilisé dans votre batterie ADFS héritée lors de sa migration vers Windows Server2012R2.  
  
-   Si vous souhaitez migrer une batterie ADFS SQLServer, notez que le processus de migration implique la création d’une nouvelle instance de base de données SQL dans laquelle vous devez importer les données de configuration d’origine.  
  
## <a name="next-steps"></a>Étapes suivantes
 [Migrer le rôle Services de fédération Active Directory pour Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Migration du serveur de fédération AD FS](migrate-ad-fs-fed-server-r2.md)   
 [Migration du serveur Proxy de fédération AD FS](migrate-fed-server-proxy-r2.md)   
 [Vérification de la Migration AD FS Windows Server 2012 R2](verify-ad-fs-migration.md)