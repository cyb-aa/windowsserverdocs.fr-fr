---
ms.assetid: 68db7f26-d6e3-4e67-859b-80f352e6ab6a
title: "Le rôle de la base de données de Configuration ADFS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3372e1f051ba7f900753a4961d948ddabdef6f4d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

# <a name="the-role-of-the-ad-fs-configuration-database"></a>Le rôle de la base de données de Configuration ADFS
La base de données de configuration ADFS stocke toutes les données de configuration qui représente une instance unique de \(ADFS\) \(that is, the Federation Service\) ActiveDirectory Federation Services. La base de données de configuration ADFS définit l’ensemble des paramètres requis par un Service de fédération pour identifier les partenaires, les certificats, les magasins d’attributs, les revendications et différentes données sur ces entités associées. Vous pouvez stocker ces données de configuration dans une base de données MicrosoftSQLServer® ou la fonctionnalité de base de données interne Windows \(WID\) qui est incluse avec Windows Server® 2008, Windows Server2008R2 et Windows Server® 2012.  
  
> [!NOTE]  
> Tout le contenu de la base de données de configuration ADFS peut être stockée dans une instance de WID ou dans une instance de la base de données SQL, mais pas les deux. Cela signifie que vous ne peut pas avoir certains serveurs de fédération à l’aide de WID et autres à l’aide d’une base de données SQLServer pour la même instance de la base de données de configuration ADFS.  
  
Vous pouvez utiliser les informations suivantes dans cette rubrique, ainsi que le contenu de [considérations sur la topologie ADFS déploiement](https://technet.microsoft.com/library/gg982489.aspx) pour en savoir plus sur les avantages et inconvénients de choisir WID ou SQLServer pour stocker la base de données de configuration ADFS:  
  
Utilise WID une base de données relationnelle stocker et ne possède pas sa propre gestion de l’interface \(UI\). Au lieu de cela, les administrateurs peuvent modifier le contenu de la base de données de configuration ADFS à l’aide de la gestion ADFS dans composants, Fsconfig.exe ou les applets de commande Windows PowerShell™.  
  
## <a name="using-wid-to-store-the-ad-fs-configuration-database"></a>À l’aide de WID pour stocker la base de données de configuration ADFS  
Vous pouvez créer la base de données de configuration ADFS à l’aide de WID que le Windows store à l’aide de l’outil de ligne de commande Fsconfig.exe ou de l’Assistant Configuration du serveur de fédération ADFS. Lorsque vous utilisez un de ces outils, vous pouvez choisir une des options suivantes pour créer votre topologie de serveur de fédération. Chacune de ces options utilise WID pour stocker la base de données de configuration ADFS:  
  
-   Créer un serveur de fédération autonome  
  
-   Créer le premier serveur de fédération dans une batterie de serveurs de fédération  
  
-   Ajouter un serveur de fédération à une batterie de serveurs de fédération  
  
Si vous sélectionnez l’option autonome, WID est utilisé pour stocker une instance unique de la base de données de configuration ADFS. Cette instance ne peut pas être partagée entre plusieurs serveurs de fédération. Il est destiné aux environnements de laboratoire de test uniquement. Pour plus d’informations sur l’option de serveur de fédération autonome ou de la procédure à suivre, voir [autonome Federation Server à l’aide de WID](https://technet.microsoft.com/library/gg982486.aspx) ou [créer un serveur de fédération autonome](https://technet.microsoft.com/library/ee913579.aspx).  
  
Si vous sélectionnez le premier serveur de fédération dans une option de la batterie de serveur de fédération, WID est configuré pour une évolutivité qui autorise les serveurs de fédération supplémentaires à ajouter à la batterie de serveurs à un moment ultérieur. Pour plus d’informations sur le déploiement d’une batterie WID ou la procédure à suivre, voir [Federation Server batterie de serveurs à l’aide de WID](https://technet.microsoft.com/library/gg982492.aspx) ou [créer le premier serveur de fédération dans une batterie de serveurs de fédération](https://technet.microsoft.com/library/dd807070.aspx)  
  
Si vous sélectionnez l’ajout d’une option de serveur de fédération, WID est configuré pour répliquer les modifications de base de données de configuration sur le nouveau serveur de fédération à intervalles réguliers. Pour plus d’informations sur l’ajout d’un serveur de fédération à une batterie WID, consultez [Federation Server batterie de serveurs à l’aide de WID](https://technet.microsoft.com/library/gg982492.aspx) ou [ajouter un serveur de fédération à une batterie de serveurs de fédération](https://technet.microsoft.com/library/ee913575.aspx).  
  
> [!NOTE]  
> Lorsque vous déployez une batterie de serveurs de fédération à l’aide de WID, certaines fonctionnalités d’ADFS n’est peut-être pas disponibles. Pour accéder à toutes les fonctionnalités lorsque vous configurez votre batterie de serveurs, envisagez d’utiliser MicrosoftSQLServer pour stocker la base de données de configuration ADFS à la place. Pour plus d’informations, voir [considérations sur la topologie ADFS déploiement](https://technet.microsoft.com/library/gg982489(v=ws.11).aspx).  
  
### <a name="how-a-wid-federation-server-farm-works"></a>Fonctionne d’une batterie de serveurs de fédération WID  
Cette section décrit les concepts importants qui expliquent comment la batterie de serveurs de fédération WID réplique les données entre un serveur de fédération principal et les serveurs de fédération secondaires. .  
  
#### <a name="primary-federation-server"></a>Serveur de fédération principal  
Un serveur de fédération principal est un ordinateur exécutant Windows Server2008, Windows Server2008R2 ou Windows Server® 2012 qui a été configuré dans le rôle de serveur de fédération avec l’Assistant Configuration du serveur de fédération ADFS et qui possède une copie en lecture/écriture de la base de données de configuration ADFS. Le serveur de fédération principal est toujours créé lorsque vous utilisez l’Assistant Configuration du serveur de fédération ADFS et que vous sélectionnez l’option pour créer un nouveau Service de fédération et de faire de cet ordinateur le premier serveur de fédération dans la batterie de serveurs. Tous les autres serveurs de fédération de cette batterie, également appelés serveurs de fédération secondaires, doivent synchroniser les modifications apportées sur le serveur de fédération principal vers une copie de la base de données de configuration ADFS qui est stockée localement.  
  
#### <a name="secondary-federation-servers"></a>Serveurs de fédération secondaires  
Serveurs de fédération secondaires stockent une copie de la base de données de configuration ADFS à partir du serveur de fédération principal, mais ces copies sont en lecture seule. Serveurs de fédération secondaires se connecteront à et synchroniser les données avec le serveur de fédération principal dans la batterie de serveurs par l’interrogation à intervalles réguliers pour vérifier si les données ont été modifiées. Les serveurs de fédération secondaire existent pour fournir une tolérance de panne pour le serveur de fédération principal tout en équilibrant la load\-demandes d’accès qui sont effectuées dans différents sites de votre environnement réseau.  
  
> [!NOTE]  
> Si un serveur de fédération principal tombe en panne et est hors connexion, tous les serveurs de fédération secondaires continuent de traiter les demandes normalement. Toutefois, aucune nouvelle modification ne permettre être apportées au Service de fédération jusqu'à ce que le serveur de fédération principal a été remis en ligne. Vous pouvez également désigner un serveur de fédération secondaires à devenir le serveur de fédération principal à l’aide de Windows PowerShell. Pour plus d’informations, voir la [Administration d’ADFS avec Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).  
  
#### <a name="how-the-ad-fs-configuration-database-is-synchronized"></a>Procédure de synchronisation de la base de données de configuration ADFS  
En raison d’un rôle important joue de la base de données de configuration ADFS, elle est rendue disponible sur tous les serveurs de fédération dans le réseau pour fournir une tolérance de pannes et l’équilibrage de charge load\ lors du traitement des demandes de fonctionnalités \ (lorsque load\-équilibreurs de réseau sont used\). Toutefois, pour les serveurs de fédération secondaires offrent ces fonctionnalités, la base de données de configuration ADFS qui est stocké sur le serveur de fédération principal doit être synchronisé.  
  
Lorsque vous ajoutez un serveur de fédération à la batterie de serveurs, le nouvel ordinateur qui deviendra serveur de fédération secondaire se connecte au serveur de fédération principal pour répliquer la copie de la base de données de configuration ADFS. À ce stade, le nouveau serveur de fédération continue à extraire les mises à jour à partir du serveur de fédération principal de façon régulière, comme illustré dans l’illustration suivante.  
  
![Rôles ADFS](media/adfs2_WID.png)  
  
Chaque serveur de fédération secondaire interroge le serveur de fédération principal toutes les cinq minutes pour les modifications. Vous pouvez ajuster cette valeur de five\ minutes par défaut ou forcer une synchronisation immédiate à tout moment à l’aide d’une applet de commande Windows PowerShell. Pour plus d’informations sur la procédure à suivre, voir [Administration d’ADFS avec Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).  
  
Le processus de synchronisation WID prend également en charge les transferts incrémentiels pour les transferts plus efficaces de modifications intermédiaires. Le processus de transfert incrémentiel nécessite beaucoup moins de trafic réseau et les transferts sont effectués beaucoup plus rapidement.  
  
> [!NOTE]  
> La migration d’une base de configuration ADFS à partir de WID à une instance de SQLServer est prise en charge. Pour plus d’informations sur la procédure à suivre, voir [ADFS: migrer votre base de données de Configuration ADFS vers SQLServer](https://go.microsoft.com/fwlink/?LinkId=192232) sur le site TechNet Wiki.  
  
## <a name="using-sql-server-to-store-the-ad-fs-configuration-database"></a>À l’aide de SQLServer pour stocker la base de données de configuration ADFS  
Vous pouvez créer la base de données de configuration ADFS à l’aide d’une seule instance de base de données SQLServer que le Windows store à l’aide de l’outil de ligne de commande Fsconfig.exe. À l’aide d’une base de données SQLServer en tant que la base de données de configuration ADFS offre les avantages suivants sur WID:  
  
-   Les administrateurs peuvent tirer parti des fonctionnalités haute disponibilité de SQLServer  
  
-   Il performances sont accrues de trafic élevé.  
  
-   Il fournit la prise en charge de la fonctionnalité de résolution d’artefacts SAML et SAML/WS-Federation relecture de jetons détection \(described below\).  
  
Le terme «serveur de fédération principal» ne s’applique pas quand la base de données de configuration ADFS est stockée dans une instance de base de données SQL, car tous les serveurs de fédération bénéficient du même lire et écrire dans la base de données de configuration ADFS qui est à l’aide de la même instance SQLServer en cluster, comme illustré dans l’illustration suivante.  
  
![Rôles ADFS](media/adfs2_SQL.png)  
  
Vous pouvez utiliser SQLServer pour configurer deux ou plusieurs serveurs de travailler ensemble en un cluster de serveur pour vous assurer que ADFS soient hautement disponibles pour traiter les demandes client entrantes. Haute disponibilité offre une architecture de scale\ arrière dans laquelle vous pouvez augmenter la capacité des serveurs en ajoutant des serveurs. Points de défaillance uniques sont traitées par le basculement de cluster automatique.  
  
Vous pouvez obtenir une haute disponibilité à l’aide de la load\ équilibrage et les services de basculement qui fournissent des technologies de clustering SQL. Pour plus d’informations sur la configuration de SQLServer pour la haute disponibilité, voir [vue d’ensemble des Solutions haute disponibilité](https://go.microsoft.com/fwlink/?LinkId=179853).  
  
### <a name="saml-artifact-resolution"></a>Résolution d’artefacts SAML  
Résolution d’artefacts Security Assertion Markup Language \(SAML\) est un point de terminaison en fonction de la part du protocole SAML 2.0 qui décrit comment une partie de confiance peut récupérer un jeton directement à partir d’un fournisseur de revendications. Dans la première étape du processus de résolution, un client de navigateur contacte un serveur de fédération de ressources et lui fournit un artefact. Dans la deuxième phase, les serveurs de fédération de ressources envoient l’artefact à une URL de point de terminaison d’artefact SAML hébergée dans une organisation partenaire de compte afin de résoudre le message d’artefact. Dans la phase finale, le serveur de fédération de comptes émet le jeton pour le serveur de fédération pour le compte du client de navigateur.  
  
> [!NOTE]  
> Si vous êtes administrateur dans une organisation partenaire de compte, veillez à affecter ou de lier un certificat SSL, qui est lié à un certificat racine d’un membre du programme de certificat racine Windows, pour le site Web passif de fédération dans IIS \ (<ComputerName>\\Sites\\Default WebSite\\adfs\\ls\) sur tous les serveurs de fédération de compte de la batterie de serveurs. Cela est important empêcher les serveurs de fédération de ressources d’avoir à ajouter manuellement le certificat SSL au magasin de certificats personnes de confiance des ordinateurs locaux ou à partir de l’impossibilité de résoudre l’artefact publié dans votre organisation.  
  
### <a name="samlws---federation-token-replay-detection"></a>SAML/WS - détection de relecture de jetons de fédération  
Le terme *relecture de jetons* fait référence à la loi par lequel un client de navigateur dans une organisation partenaire de compte tente d’envoyer des même jeton qu’il a reçu à partir d’un serveur de fédération de comptes plusieurs fois pour s’authentifier sur un serveur de fédération de ressources.  Cette action se produit lorsqu’un utilisateur clique sur le **précédent** bouton de son navigateur pour renvoyer la page d’authentification.  
  
ADFS fournit une fonctionnalité appelée *la détection de relecture de jetons* par les plusieurs demandes de jeton à l’aide de la même jeton peut être détecté et ensuite ignoré. Lorsque cette fonctionnalité est activée, la détection de relecture de jetons protège l’intégrité des demandes d’authentification dans le profil passif WS-Federation et le profil WebSSO SAML en s’assurant que le même jeton n’est jamais utilisé plusieurs fois. Cette fonctionnalité doit être activée dans les situations où la sécurité est très importante tel que lors de l’utilisation des bornes.  
  
Dans l’exemple des bornes, un utilisateur peut se déconnecter de tous les sites Web et ultérieurement un utilisateur malveillant peut essayer d’utiliser l’historique du navigateur afin de renvoyer la page d’authentification fédérée qui a été chargée par l’utilisateur précédent. Cette fonctionnalité limite ce risque en stockant des informations supplémentaires sur chaque authentification réussie effectuée par une organisation partenaire de compte afin de détecter les relectures ultérieures du jeton et empêcher plusieurs tentatives d’authentification de réussir.  
  

