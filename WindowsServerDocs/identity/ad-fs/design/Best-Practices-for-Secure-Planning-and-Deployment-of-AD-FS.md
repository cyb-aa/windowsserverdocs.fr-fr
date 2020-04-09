---
ms.assetid: 963a3d37-d5f1-4153-b8d5-2537038863cb
title: Meilleures pratiques pour sécuriser la planification et le déploiement d'AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: bcddb3cc7534f45f0a84e25a6174648f1e3b82af
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858412"
---
# <a name="best-practices-for-secure-planning-and-deployment-of-ad-fs"></a>Meilleures pratiques pour sécuriser la planification et le déploiement d'AD FS


Cette rubrique fournit des informations sur les meilleures pratiques pour vous aider à planifier et à évaluer la sécurité lors de la conception de votre déploiement Services ADFS (AD FS). Cette rubrique est un point de départ pour examiner et évaluer les considérations qui affectent la sécurité globale de votre utilisation de AD FS. Les informations qu'elle contient complètent et étendent vos meilleures pratiques en matière de planification de la sécurité et de conception.  
  
## <a name="core-security-best-practices-for-ad-fs"></a>Meilleures pratiques principales en matière de sécurité pour AD FS  
Les meilleures pratiques principales suivantes sont communes à toutes les installations de AD FS où vous souhaitez améliorer ou étendre la sécurité de votre conception ou déploiement :  

-   **Sécuriser les AD FS en tant que système de « niveau 0 »** 

    AD FS est fondamentalement un système d’authentification.  Par conséquent, il doit être traité comme un système de « niveau 0 » comme un autre système d’identité sur votre réseau.  [Microsoft docs](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material) contient plus d’informations sur le modèle de niveau d’administration Active Directory. 


-   **Utilisez l’Assistant Configuration de la sécurité pour appliquer les meilleures pratiques de sécurité propres à AD FS aux serveurs de Fédération et aux ordinateurs proxy de serveur de Fédération.**  
  
    L’Assistant Configuration de la sécurité (SCW) est un outil qui est préinstallé sur tous les ordinateurs Windows Server 2008, Windows Server 2008 R2 et Windows Server 2012. Vous pouvez l'utiliser pour appliquer des meilleures pratiques de sécurité qui permettent de réduire la surface d'attaque d'un serveur, en fonction des rôles serveurs que vous installez.  
  
    Quand vous installez AD FS, le programme d'installation crée des fichiers d'extension de rôle, que vous pouvez utiliser avec l'Assistant Configuration de la sécurité pour créer une stratégie de sécurité à appliquer au rôle serveur AD FS (serveur de fédération ou serveur proxy de fédération) que vous choisissez pendant l'installation.  
  
    Chaque fichier d'extension de rôle installé représente le type de rôle et de sous-rôle pour lequel chaque ordinateur est configuré. Les fichiers d’extension de rôle suivants sont installés dans le répertoire C :WindowsADFSScw :  
  
    -   Farm.xml  
  
    -   SQLFarm.xml  
  
    -   StandAlone.xml  
  
    -   Proxy.xml (ce fichier n'est présent que si vous avez configuré l'ordinateur dans le rôle de serveur proxy de fédération.)  
  
    Pour appliquer les extensions de rôle AD FS dans l'Assistant Configuration de la sécurité, effectuez les étapes suivantes dans l'ordre indiqué :  
  
    1.  Installez AD FS et choisissez le rôle serveur approprié pour cet ordinateur. Pour plus d’informations, consultez [installer le service de rôle proxy FSP (Federation Service Proxy)](../../ad-fs/deployment/Install-the-Federation-Service-Proxy-Role-Service.md) dans le Guide de déploiement AD FS.  
  
    2.  Inscrivez le fichier d'extension de rôle approprié à l'aide de l'outil en ligne de commande Scwcmd. Consultez le tableau suivant pour plus d'informations sur l'utilisation de cet outil dans le rôle pour lequel votre ordinateur est configuré.  
  
    3.  Vérifiez que la commande s’est correctement déroulée en examinant le fichier SCWRegister_log. xml, qui se trouve dans le répertoire WindowssecurityMsscwLogs.  
  
    Vous devez effectuer toutes ces étapes sur chaque ordinateur serveur de fédération ou serveur proxy de fédération auquel vous souhaitez appliquer des stratégies de sécurité AD FS dans l'Assistant Configuration de la sécurité.  
  
    Le tableau suivant explique comment inscrire l'extension de rôle appropriée dans l'Assistant Configuration de la sécurité, en fonction du rôle serveur AD FS que vous avez choisi sur l'ordinateur sur lequel vous avez installé AD FS.  
  
    |Rôle serveur AD FS|Base de données de configuration AD FS utilisée|Tapez la commande suivante depuis une invite de commandes :|  
    |---------------------|-------------------------------------|---------------------------------------------------|  
    |Serveur de fédération autonome|Base de données interne Windows|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwStandAlone.xml"`|  
    |Serveur de fédération appartenant à une batterie|Base de données interne Windows|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwFarm.xml"`|  
    |Serveur de fédération appartenant à une batterie|SQL Server|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwSQLFarm.xml"`|  
    |Serveur proxy de fédération|N/A|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwProxy.xml"`|  
  
    Pour plus d'informations sur les bases de données que vous pouvez utiliser avec AD FS, consultez [Rôle de la base de données de configuration AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
-   **Utilisez la détection de relecture de jetons dans les situations où la sécurité est très importante, par exemple quand des bornes sont utilisées.**  
    La détection de relecture de jetons est une fonctionnalité de AD FS qui garantit que toute tentative de relecture d’une demande de jeton effectuée sur le service FS (Federation Service) est détectée et que la demande est ignorée. La détection des relectures de jetons est activée par défaut. Elle fonctionne à la fois pour le profil passif WS-Federation et pour le profil WebSSO SAML (Security Assertion Markup Language) et s'assure qu'un même jeton n'est jamais réutilisé.  
  
    Quand le service de fédération démarre, il commence par créer un cache de toutes les demandes de jeton qu'il traite. À mesure que des demandes de jeton sont ajoutées au cache, la possibilité de détecter des tentatives de relecture d'une demande de jeton augmente pour le service de fédération. Si vous désactivez la détection de relectures de jetons, puis que vous décidez de la réactiver, gardez à l'esprit que le service de fédération acceptera toujours des jetons qui ont peut-être déjà été utilisés, ce jusqu'à ce que le système alloue au cache de relectures suffisamment de temps pour qu'il régénère son contenu. Pour plus d'informations, consultez [Rôle de la base de données de configuration AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
-   **Utilisez le chiffrement de jeton, en particulier si vous utilisez la prise en charge de la résolution d’artefacts SAML.**  
  
    Le chiffrement des jetons est fortement recommandé pour renforcer la sécurité et la protection contre les attaques intercepteur (Man-in-the-Middle) potentielles qui peuvent être tentées contre votre déploiement AD FS. L'utilisation du chiffrement peut avoir une légère incidence sur le débit, mais en général, elle s'effectue de manière transparente et, dans de nombreux déploiements, les avantages procurés par une sécurité renforcée compensent largement les pertes de performances éventuellement accusées par les serveurs.  
  
    Pour activer le chiffrement de jetons, définissez d'abord un certificat de chiffrement pour vos approbations de partie de confiance. Vous pouvez configurer un certificat de chiffrement quand vous créez une approbation de partie de confiance ou ultérieurement. Pour ajouter un certificat de chiffrement ultérieurement à une approbation de partie de confiance existante, vous pouvez définir un certificat à utiliser sous l’onglet **chiffrement** dans les propriétés d’approbation lors de l’utilisation du composant logiciel enfichable AD FS. Pour spécifier un certificat pour une approbation existante à l’aide des applets de commande AD FS, utilisez le paramètre EncryptionCertificate des applets de commande **Set-ClaimsProviderTrust** ou **Set-RelyingPartyTrust** . Pour définir un certificat pour l’service FS (Federation Service) à utiliser lors du déchiffrement de jetons, utilisez l’applet de commande **Set-ADFSCertificate** et spécifiez «`Token-Encryption`» pour le paramètre *CertificateType* . Vous pouvez activer et désactiver le chiffrement pour une approbation de partie de confiance spécifique à l'aide du paramètre *EncryptClaims* de l'applet de commande **Set-RelyingPartyTrust**.  
  
-   **Utiliser la protection étendue pour l’authentification**  
  
    Pour vous aider à sécuriser vos déploiements, vous pouvez définir et utiliser la fonctionnalité de protection étendue de l’authentification avec AD FS. Ce paramètre spécifie le niveau de protection étendue pour l’authentification prise en charge par un serveur de Fédération.  
  
    La protection étendue de l'authentification assure une protection contre les attaques de l'intercepteur, dans lesquelles une personne malveillante intercepte les informations d'identification d'un client et les transmet à un serveur. La protection contre les attaques de ce type s'effectue par le biais d'un jeton de liaison de canal qui peut être autorisé, imposé ou non imposé par le serveur quand il établit des communications avec des clients.  
  
    Pour activer la protection étendue, utilisez le paramètre **ExtendedProtectionTokenCheck** de l'applet de commande **Set-ADFSProperties**. Les valeurs possibles de ce paramètre et le niveau de sécurité qu'elles fournissent sont décrits dans le tableau suivant.  
  
    |Valeur du paramètre|Niveau de sécurité|Paramétrage de la protection|  
    |-------------------|------------------|----------------------|  
    |Exiger|Le serveur est entièrement sécurisé.|La protection étendue est appliquée et toujours obligatoire.|  
    |Autoriser|Le serveur est partiellement sécurisé.|La protection étendue est appliquée sur les systèmes concernés qui ont fait l'objet d'un correctif pour prendre en charge cette fonctionnalité.|  
    |Aucune|Le serveur est vulnérable.|La protection étendue n'est pas appliquée.|  
  
-   **Si vous utilisez la journalisation et le suivi, garantissez la confidentialité de toutes les informations sensibles.**  
  
    Par défaut, AD FS n’exposent pas ou ne suivent pas directement les informations d’identification personnelle (PII) dans le cadre du service FS (Federation Service) ou des opérations normales. Lorsque la journalisation des événements et la journalisation du suivi de débogage sont activées dans AD FS, en fonction de la stratégie de revendications que vous configurez, les types de revendications et les valeurs qui leur sont associées peuvent contenir des PII qui peuvent être enregistrés dans les journaux d’événements ou de suivi AD FS.  
  
    Par conséquent, il est vivement recommandé d’appliquer le contrôle d’accès sur la configuration AD FS et ses fichiers journaux. Si vous souhaitez que ce type d'informations demeure invisible, vous devez désactiver la journalisation ou retirer toutes les informations d'identification personnelles ou données sensibles des journaux avant de les partager.  
  
    Les conseils suivants vous permettent d'éviter l'exposition involontaire du contenu d'un fichier journal :  
  
    -   Assurez-vous que le journal des événements AD FS et les fichiers journaux des traces sont protégés par des listes de contrôle d’accès (ACL) qui limitent l’accès aux seuls administrateurs de confiance qui doivent y accéder.  
  
    -   Ne copiez pas ou n'archivez pas les fichiers journaux en utilisant des extensions de fichier ou des chemins d'accès qui peuvent être facilement fournis à l'aide d'une demande web. Par exemple, l'extension de nom de fichier .xml n'est pas un choix judicieux. Consultez le guide d'administration IIS (Internet Information Services) pour obtenir la liste des extensions qui peuvent être fournies.  
  
    -   Si vous modifiez le chemin d'accès du fichier journal, veillez à spécifier un chemin d'accès absolu pour l'emplacement de ce fichier, qui doit se trouver en dehors du répertoire public de la racine virtuelle de l'hôte web afin d'empêcher tout intervenant externe d'y accéder à l'aide d'un navigateur web.  

-   **AD FS le verrouillage à chaud de l’extranet et AD FS la protection du verrouillage intelligent extranet**  
    
    En cas d’attaque sous la forme de demandes d’authentification avec des mots de passe non valides qui transitent par le proxy d’application Web, AD FS le verrouillage extranet vous permet de protéger vos utilisateurs contre le verrouillage d’un compte AD FS. Outre la protection de vos utilisateurs contre le verrouillage de compte AD FS, AD FS le verrouillage extranet protège également contre les attaques par déduction de mot de passe en force brute.  
    
    Pour le verrouillage logiciel extranet pour AD FS sur Windows Server 2012 R2, consultez [AD FS la protection par verrouillage à chaud](../../ad-fs/operations/Configure-AD-FS-Extranet-Soft-Lockout-Protection.md)de l’extranet.  

     Pour le verrouillage intelligent extranet pour AD FS sur Windows Server 2016, consultez [AD FS protection intelligente du verrouillage extranet](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md).  
  
## <a name="sql-serverspecific-security-best-practices-for-ad-fs"></a>Meilleures pratiques propres à SQL Server en matière de sécurité pour AD FS  
Les meilleures pratiques de sécurité suivantes sont spécifiques à l’utilisation de Microsoft SQL Server&reg; ou de la base de données interne Windows (WID) lorsque ces technologies de base de données sont utilisées pour gérer les données dans AD FS la conception et le déploiement.  
  
> [!NOTE]  
> Ces recommandations étendent, mais ne remplacent pas, les conseils de sécurité pour SQL Server. Pour plus d’informations sur la planification d’une installation SQL Server sécurisée, consultez [Considérations sur la sécurité pour une installation SQL sécurisée](https://go.microsoft.com/fwlink/?LinkID=139831) (https://go.microsoft.com/fwlink/?LinkID=139831).  
  
-   **Déployez toujours SQL Server derrière un pare-feu dans un environnement réseau physiquement sécurisé.**  
  
    Une installation SQL Server ne doit jamais être directement exposée à Internet. Seuls les ordinateurs qui se trouvent à l’intérieur de votre centre de contenu doivent être en mesure d’accéder à votre installation SQL Server qui prend en charge AD FS. Pour plus d’informations, consultez [liste de vérification des meilleures pratiques](https://go.microsoft.com/fwlink/?LinkID=189229) pour la sécurité (https://go.microsoft.com/fwlink/?LinkID=189229).  
  
-   **Exécutez SQL Server sous un compte de service au lieu d’utiliser les comptes de service système par défaut intégrés.**  
  
    Par défaut, SQL Server est souvent installé et configuré pour utiliser l'un des comptes système intégrés pris en charge, tels que les comptes LocalSystem ou NetworkService. Pour améliorer la sécurité de votre installation SQL Server pour AD FS, dans la mesure du possible, utilisez un compte de service distinct pour accéder à votre service SQL Server et activez l’authentification Kerberos en inscrivant le nom de principal de sécurité (SPN) de ce compte dans votre déploiement Active Directory. Cela permet une authentification mutuelle entre le client et le serveur. Si vous n'inscrivez pas le nom de principal de sécurité d'un compte de service distinct, seul le client est authentifié, car SQL Server utilise alors l'authentification NTLM ou Windows.  
  
-   **Réduisez la surface d’exposition de SQL Server.**  
  
    Activez uniquement les points de terminaison SQL Server qui sont nécessaires. Par défaut, SQL Server fournit un point de terminaison TCP intégré unique qui ne peut pas être supprimé. Pour AD FS, vous devez activer ce point de terminaison TCP pour l’authentification Kerberos. Pour passer en revue les points de terminaison TCP actuels afin de déterminer si des ports TCP définis par l'utilisateur sont ajoutés à une installation SQL, vous pouvez utiliser l'instruction de requête « SELECT * FROM sys.tcp_endpoints » dans une session Transact-SQL (T-SQL). Pour plus d’informations sur la configuration d’un point de terminaison SQL Server, consultez [procédure : configurer le moteur de base de données pour écouter sur plusieurs ports TCP](https://go.microsoft.com/fwlink/?LinkID=189231) (https://go.microsoft.com/fwlink/?LinkID=189231).  
  
-   **Évitez d’utiliser l’authentification basée sur SQL.**  
  
    Pour que les mots de passe ne circulent pas sur votre réseau sous la forme de texte en clair ou qu'ils ne soient pas stockés dans les paramètres de configuration, n'utilisez l'authentification Windows qu'avec votre installation SQL Server. L'authentification SQL Server est un mode d'authentification hérité. Nous vous déconseillons de stocker les informations d'identification SQL (noms d'utilisateur et mots de passe SQL) quand vous utilisez l'authentification SQL Server. Pour plus d’informations, consultez [modes d’authentification](https://go.microsoft.com/fwlink/?LinkID=189232) (https://go.microsoft.com/fwlink/?LinkID=189232).  
  
-   **Évaluez avec soin la nécessité d’une sécurité de canal supplémentaire dans votre installation SQL.**  
  
    Même si l'authentification Kerberos est appliquée, l'interface SQL Server SSPI (Security Support Provider Interface) ne fournit pas de sécurité de niveau canal. Toutefois, pour les installations dans lesquelles les serveurs sont reliés de manière sécurisée à un réseau protégé par un pare-feu, le chiffrement des communications SQL n'est pas forcément nécessaire.  
  
    Bien que le chiffrement soit un outil de sécurisation précieux, évitez d'y recourir pour la totalité des données ou des connexions. Pour déterminer si le chiffrement est justifié, analysez la façon dont les utilisateurs accèdent aux données. Si ces derniers y accèdent via un réseau public, le chiffrement des données peut s'avérer nécessaire pour renforcer la sécurité. Toutefois, si tous les accès aux données SQL par AD FS impliquent une configuration intranet sécurisée, le chiffrement n’est peut-être pas nécessaire. En outre, toute utilisation du chiffrement doit comprendre une stratégie de maintenance pour les mots de passe, les clés et les certificats.  
  
    Si vous pensez que des données SQL risquent d'être exposées ou altérées via votre réseau, utilisez la sécurité du protocole Internet (IPsec) ou SSL (Secure Sockets Layer) pour sécuriser vos connexions SQL. Toutefois, cela peut avoir un effet négatif sur les performances de SQL Server, ce qui peut affecter ou limiter AD FS performances dans certains cas. Par exemple, AD FS les performances de l’émission de jetons peuvent se dégrader lorsque les recherches d’attribut à partir d’un magasin d’attributs SQL sont critiques pour l’émission de jeton. Pour éliminer le risque d'altération des données SQL, vous pouvez privilégier une configuration dans laquelle la sécurité est concentrée sur un périmètre clairement défini. Par exemple, une meilleure solution pour sécuriser votre installation SQL Server consiste à la rendre inaccessible aux utilisateurs et ordinateurs Internet, de manière à ce que seuls les utilisateurs ou les ordinateurs appartenant à l'environnement de votre centre de données puissent y accéder.  
  
    Pour plus d’informations, consultez [chiffrement des connexions à SQL Server](https://go.microsoft.com/fwlink/?LinkID=189234) ou [SQL Server le chiffrement](https://go.microsoft.com/fwlink/?LinkID=189233).  
  
-   **Configurez un accès conçu en toute sécurité à l’aide de procédures stockées pour effectuer toutes les recherches basées sur SQL en AD FS de données stockées SQL.**  
  
    Pour améliorer l'isolation des services et des données, vous pouvez créer des procédures stockées pour toutes les commandes de recherche dans le magasin d'attributs. Vous pouvez créer un rôle de base de données auquel vous accordez ensuite l'autorisation d'exécuter les procédures stockées. Affectez l’identité de service du service Windows AD FS à ce rôle de base de données. Le service Windows AD FS ne doit pas être en mesure d’exécuter une autre instruction SQL, à l’exception des procédures stockées appropriées utilisées pour la recherche d’attributs. Le verrouillage de l'accès à la base de données SQL Server de cette manière réduit le risque d'une attaque par élévation de privilège.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
