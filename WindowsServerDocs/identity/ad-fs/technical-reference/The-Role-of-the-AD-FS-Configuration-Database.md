---
ms.assetid: 68db7f26-d6e3-4e67-859b-80f352e6ab6a
title: Rôle de la base de données de configuration AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 22047a93ab67d3f21b3e2318fcce497feab8f996
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385584"
---
# <a name="the-role-of-the-ad-fs-configuration-database"></a>Rôle de la base de données de configuration AD FS
La base de données de configuration AD FS stocke toutes les données de configuration qui représentent une seule instance de Services ADFS \(AD FS\) \(, service FS (Federation Service)\). La base de données de configuration AD FS définit l'ensemble des paramètres qui permettent à un service de fédération d'identifier les partenaires, les certificats, les magasins d'attributs, les revendications et différentes données sur ces entités associées. Vous pouvez stocker ces données de configuration dans une base de données Microsoft SQL Server® ou dans la fonctionnalité de\) de la base de données interne Windows \(WID incluse avec Windows Server® 2008, Windows Server 2008 R2 et Windows Server® 2012.  
  
> [!NOTE]  
> Tout le contenu de la base de données de configuration AD FS peut être stocké soit dans une instance de la base de données interne Windows, soit dans une instance de la base de données SQL, mais pas dans les deux. Cela signifie que vous ne pouvez pas avoir certains serveurs de fédération utilisant la base de données interne Windows et d'autres une base de données SQL Server pour la même instance de la base de données de configuration AD FS.  
  
À partir des informations fournies dans cette rubrique et du contenu de la rubrique [Considérations sur la topologie du déploiement d'AD FS](https://technet.microsoft.com/library/gg982489.aspx), vous pouvez avoir une idée des avantages et des inconvénients de la base de données interne Windows et de SQL Server pour stocker la base de données de configuration AD FS et effectuer votre choix en conséquence :  
  
WID utilise une banque de données relationnelle et n’a pas sa propre interface utilisateur de gestion \(\)de l’interface utilisateur. Au lieu de cela, les administrateurs peuvent modifier le contenu de la base de données de configuration AD FS à l’aide des applets de commande du composant logiciel enfichable de gestion AD FS\-dans, fsconfig. exe ou des applets de commande Windows PowerShell™.  
  
## <a name="using-wid-to-store-the-ad-fs-configuration-database"></a>Utilisation de la base de données interne Windows pour stocker la base de données de configuration AD FS  
Vous pouvez créer la base de données de configuration AD FS à l’aide de WID en tant que magasin à l’aide de la commande fsconfig. exe\-outil Line ou de l’Assistant Configuration du serveur de fédération AD FS. Quand vous utilisez l'un de ces outils, vous pouvez choisir l'une des options suivantes pour créer votre topologie de serveur de fédération. Chacune de ces options utilise la base de données interne Windows pour le stockage de la base de données de configuration AD FS :  
  
-   Créer un serveur de Fédération autonome\-  
  
-   Créer le premier serveur de fédération dans une batterie de serveurs de fédération  
  
-   Ajouter un serveur de fédération à une batterie de serveurs de fédération  
  
Si vous sélectionnez l’option stand\-seul, WID est utilisé pour stocker une seule instance de la base de données de configuration AD FS. Cette instance ne peut pas être partagée par plusieurs serveurs de fédération. Elle est uniquement destinée aux environnements lab de test. Pour plus d’informations sur l’option stand\-seul serveur de Fédération ou sur la façon d’en configurer un, consultez [serveur de Fédération autonome à l’aide de wid](https://technet.microsoft.com/library/gg982486.aspx) ou [créer un serveur de Fédération autonome](https://technet.microsoft.com/library/ee913579.aspx).  
  
Si vous choisissez l'option permettant de sélectionner le premier serveur de fédération dans une batterie de serveurs de fédération, la base de données interne Windows est configurée de manière à ce que la batterie puisse ultérieurement accueillir des serveurs de fédération supplémentaires. Pour plus d'informations sur le déploiement d'une batterie utilisant la base de données interne Windows ou sur la façon d'en configurer une, consultez [Batterie de serveurs de fédération utilisant la base de données interne Windows](https://technet.microsoft.com/library/gg982492.aspx) ou [Créer le premier serveur de fédération dans une batterie de serveurs de fédération](https://technet.microsoft.com/library/dd807070.aspx)  
  
Si vous choisissez l'option permettant d'ajouter un serveur de fédération, la base de données interne Windows est configurée de manière à pouvoir répliquer les modifications apportées à la base de données de configuration vers le nouveau serveur de fédération à des intervalles définis. Pour plus d'informations sur l'ajout d'un serveur de fédération à une batterie utilisant la base de données interne Windows, consultez [Batterie de serveurs de fédération utilisant la base de données interne Windows](https://technet.microsoft.com/library/gg982492.aspx) ou [Ajouter un serveur de fédération à une batterie de serveurs de fédération](https://technet.microsoft.com/library/ee913575.aspx).  
  
> [!NOTE]  
> Lorsque vous déployez une batterie de serveurs de Fédération à l’aide de WID, certaines fonctionnalités de AD FS peuvent ne pas être disponibles. Pour avoir accès à toutes les fonctionnalités quand vous configurez votre batterie de serveurs, envisagez plutôt d'utiliser Microsoft SQL Server pour stocker la base de données de configuration AD FS. Pour plus d'informations, consultez [Considérations sur la topologie du déploiement d'AD FS](https://technet.microsoft.com/library/gg982489(v=ws.11).aspx).  
  
### <a name="how-a-wid-federation-server-farm-works"></a>Fonctionnement d'une batterie de serveurs de fédération utilisant la base de données interne Windows  
Cette section décrit des concepts importants qui expliquent comment la batterie de serveurs de fédération utilisant la base de données interne Windows réplique des données entre un serveur de fédération principal et des serveurs de fédération secondaires. .  
  
#### <a name="primary-federation-server"></a>Serveur de fédération principal  
Un serveur de Fédération principal est un ordinateur exécutant Windows Server 2008, Windows Server 2008 R2 ou Windows Server® 2012 qui a été configuré dans le rôle de serveur de Fédération avec l’Assistant Configuration du serveur de fédération AD FS et qui dispose d’une copie en lecture/écriture de la base de données de configuration AD FS. Le serveur de Fédération principal est toujours créé lorsque vous utilisez l’Assistant Configuration du serveur de fédération AD FS et sélectionnez l’option permettant de créer un service FS (Federation Service) et de faire de cet ordinateur le premier serveur de Fédération de la batterie. Tous les autres serveurs de fédération de cette batterie, également appelés serveurs de fédération secondaires, doivent synchroniser les modifications apportées sur le serveur de fédération principal avec une copie de la base de données de configuration AD FS qui est stockée localement.  
  
#### <a name="secondary-federation-servers"></a>Serveurs de fédération secondaires  
Les serveurs de Fédération secondaires stockent une copie de la base de données de configuration AD FS à partir du serveur de Fédération principal, mais ces copies sont lues\-uniquement. À intervalles réguliers, les serveurs de fédération secondaires se connectent au serveur de fédération principal de la batterie et l'interrogent pour synchroniser les données si des modifications de données ont eu lieu. Les serveurs de Fédération secondaires existent pour fournir une tolérance de panne pour le serveur de Fédération principal tout en agissant pour charger\-équilibrer les demandes d’accès qui sont effectuées sur différents sites dans votre environnement réseau.  
  
> [!NOTE]  
> Si un serveur de fédération principal tombe en panne et se retrouve hors connexion, tous les serveurs de fédération secondaires continuent de traiter les demandes normalement. Toutefois, aucune nouvelle modification ne peut être apportée au service de fédération tant que le serveur de fédération principal n'est pas de nouveau en ligne. Vous pouvez également désigner un serveur de fédération secondaire comme serveur de fédération principal à l'aide de Windows PowerShell. Pour plus d’informations, consultez l' [Administration AD FS avec Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).  
  
#### <a name="how-the-adfs-configuration-database-is-synchronized"></a>Synchronisation de la base de données de configuration AD FS  
En raison du rôle important joué par la base de données de configuration AD FS, elle est mise à disposition sur tous les serveurs de Fédération du réseau pour fournir des fonctionnalités de tolérance de panne et d’équilibrage de charge\-lors du traitement des demandes \(lorsque les équilibrages de la charge réseau\-sont utilisés\). Toutefois, pour que les serveurs de fédération secondaires offrent ces fonctionnalités, la base de données de configuration AD FS stockée sur le serveur de fédération principal doit être synchronisée.  
  
Quand vous ajoutez un serveur de fédération à la batterie, le nouvel ordinateur qui devient un serveur de fédération secondaire se connecte au serveur de fédération principal pour répliquer la copie de la base de données de configuration AD FS. Dès cet instant, le nouveau serveur de fédération extrait les mises à jour du serveur de fédération principal à intervalles réguliers, comme le montre l'illustration suivante.  
  
![Rôles de AD FS](media/adfs2_WID.png)  
  
Chaque serveur de fédération secondaire interroge le serveur de fédération principal toutes les cinq minutes pour déterminer si des modifications ont été apportées. Vous pouvez ajuster cette valeur par défaut de cinq\-minutes ou forcer une synchronisation immédiate à tout moment à l’aide d’une applet de commande Windows PowerShell. Pour plus d’informations sur la procédure à suivre, consultez [AD FS administration avec Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).  
  
Le processus de synchronisation avec la base de données interne Windows prend également en charge les transferts incrémentiels, ce qui permet de transférer les modifications intermédiaires de façon plus efficace. Le processus de transfert incrémentiel génère sensiblement moins de trafic réseau, et les transferts sont effectués beaucoup plus rapidement.  
  
> [!NOTE]  
> La migration d'une base de données de configuration AD FS depuis la base de données interne Windows vers une instance de SQL Server est prise en charge. Pour plus d’informations sur la procédure à suivre, consultez [AD FS : migration de votre base de données de Configuration AD FS vers SQL Server](https://go.microsoft.com/fwlink/?LinkId=192232) sur le site wiki technet.  
  
## <a name="using-sql-server-to-store-the-ad-fs-configuration-database"></a>Utilisation de SQL Server pour stocker la base de données de configuration AD FS  
Vous pouvez créer la base de données de configuration AD FS à l’aide d’une seule instance de base de données SQL Server en tant que magasin à l’aide de la commande fsconfig. exe\-outil en ligne. Utiliser une base de données SQL Server comme base de données de configuration AD FS présente les avantages suivants par rapport à la base de données interne Windows :  
  
-   Les administrateurs peuvent tirer parti des fonctionnalités haute disponibilité de SQL Server  
  
-   Les performances sont accrues en cas de trafic élevé.  
  
-   Il fournit une prise en charge des fonctionnalités de la résolution d’artefacts SAML et de la détection de relecture de jetons de Fédération SAML/WS\-\(décrit ci-dessous\).  
  
Le terme « serveur de fédération principal » ne s'applique pas quand la base de données de configuration AD FS est stockée dans une instance de base de données SQL, car tous les serveurs de fédération bénéficient du même accès en lecture et écriture à la base de données de configuration AD FS qui utilise la même instance SQL Server en cluster, comme le montre l'illustration suivante.  
  
![Rôles de AD FS](media/adfs2_SQL.png)  
  
Vous pouvez utiliser SQL Server pour configurer deux serveurs ou plus afin qu’ils fonctionnent ensemble en tant que cluster de serveurs afin de garantir que AD FS est rendu hautement disponible pour traiter les demandes entrantes des clients. La haute disponibilité fournit une architecture de mise à l’échelle\-dans laquelle vous pouvez augmenter la capacité du serveur en ajoutant des serveurs supplémentaires. Les défaillances sont limitées grâce au basculement de cluster automatique.  
  
Vous pouvez obtenir une haute disponibilité en utilisant la charge réseau\-les services d’équilibrage et de basculement fournis par les technologies de clustering SQL. Pour plus d’informations sur la configuration de SQL Server pour la haute disponibilité, consultez [vue d’ensemble des solutions de haute disponibilité](https://go.microsoft.com/fwlink/?LinkId=179853).  
  
### <a name="saml-artifact-resolution"></a>Résolution d'artefacts SAML  
Security Assertion Markup Language \(\) la résolution d’artefacts SAML est un point de terminaison basé sur la partie du protocole SAML 2,0 qui décrit comment une partie de confiance peut récupérer un jeton directement à partir d’un fournisseur de revendications. Au cours de la première phase du processus de résolution, un client de navigateur contacte un serveur de fédération de ressources et lui fournit un artefact. Pendant la deuxième phase, les serveurs de fédération de ressources envoient l'artefact à une URL de point de terminaison d'artefact SAML hébergée dans une organisation partenaire de compte pour résoudre le message d'artefact. Au cours de la dernière phase, le serveur de fédération de comptes émet le jeton pour le serveur de fédération pour le compte du client de navigateur.  
  
> [!NOTE]  
> Si vous êtes administrateur dans une organisation partenaire de compte, assurez-vous d’affecter ou de lier un certificat SSL, qui est lié à un certificat racine d’un membre du programme de certificat racine Windows, au site Web passif de Fédération dans IIS \(<ComputerName>sites \\\\site Web par défaut\\ADFS\\LS\) sur tous les serveurs de Fédération de comptes de la batterie. Ainsi, les serveurs de fédération de ressources n'ont pas besoin d'ajouter eux-mêmes le certificat SSL au magasin de certificats Ordinateur Local/Personnes autorisées et peuvent résoudre l'artefact publié dans votre organisation.  
  
### <a name="samlws---federation-token-replay-detection"></a>Détection de relecture de jetons SAML/WS-Federation  
La *relecture de jetons* fait référence à la tentative par un client de navigateur dans une organisation partenaire de compte d'envoyer plusieurs fois le jeton qu'il a reçu d'un serveur de fédération de comptes pour s'authentifier auprès d'un serveur de fédération de ressources.  Cette action se produit lorsqu’un utilisateur clique sur le bouton **précédent** de son navigateur pour soumettre à nouveau la page d’authentification.  
  
AD FS fournit une fonctionnalité appelée *détection de relecture de jetons* qui permet de détecter et d'abandonner plusieurs demandes de jeton utilisant le même jeton. Lorsque cette fonctionnalité est activée, la détection de relecture de jetons protège l’intégrité des demandes d’authentification dans le profil WS\-Federation passif et le profil WebSSO SAML en s’assurant que le même jeton n’est jamais utilisé plusieurs fois. Cette fonctionnalité doit être activée dans les situations où la sécurité est très importante, par exemple quand des bornes sont utilisées.  
  
Dans l'exemple des bornes, un utilisateur peut se déconnecter de tous les sites web, puis un utilisateur malveillant peut essayer d'exploiter l'historique de navigation pour renvoyer la page d'authentification fédérée que l'utilisateur précédent avait chargée. Cette fonctionnalité atténue ce problème en stockant des informations supplémentaires sur chaque authentification réussie effectuée par une organisation partenaire de compte afin de détecter les relectures ultérieures du jeton et d’empêcher la réussite de plusieurs tentatives d’authentification.  
  

