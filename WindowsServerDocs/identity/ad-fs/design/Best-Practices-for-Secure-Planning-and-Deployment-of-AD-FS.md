---
ms.assetid: 963a3d37-d5f1-4153-b8d5-2537038863cb
title: "Meilleures pratiques pour sécuriser la planification et déploiement d’ADFS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ed8c36d4bec455879ffd00ad40b72fd5e90484ab
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="best-practices-for-secure-planning-and-deployment-of-ad-fs"></a>Meilleures pratiques pour sécuriser la planification et déploiement d’ADFS

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique fournit des informations de meilleures pratiques pour vous aider à planifier et évaluer la sécurité lorsque vous concevez votre déploiement ActiveDirectory Federation Services (ADFS). Cette rubrique est un point de départ pour examiner et évaluer les aspects qui influent sur la sécurité globale de votre utilisation des services ADFS. Les informations contenues dans cette rubrique vise à compléter et étendre votre planification de la sécurité et d’autres meilleures pratiques de conception.  
  
## <a name="core-security-best-practices-for-ad-fs"></a>Principales meilleures pratiques de sécurité pour ADFS  
Principales meilleures pratiques suivantes sont communes à toutes les installations ADFS où vous souhaitez améliorer ou étendre la sécurité de votre conception ou de déploiement:  
  
-   **Utilisez l’Assistant de Configuration de sécurité pour appliquer les meilleures pratiques de sécurité spécifiques à ADFS aux serveurs de fédération et les ordinateurs de proxy de serveur de fédération**  
  
    L’Assistant Configuration de sécurité (SCW) est un outil préinstallé sur tous les Windows Server2008, Windows Server2008R2 et les ordinateurs Windows Server2012. Vous pouvez l’utiliser pour appliquer la sécurité meilleures pratiques qui permettent de réduisent la surface d’attaque pour un serveur, basée sur les rôles de serveur que vous installez.  
  
    Lorsque vous installez ADFS, le programme d’installation crée des fichiers d’extension que vous pouvez utiliser avec le SCW pour créer une stratégie de sécurité qui s’applique à la spécifique rôle serveur ADFS (serveur de fédération ou serveur proxy de fédération) que vous choisissez pendant l’installation de rôle.  
  
    Chaque fichier d’extension de rôle installé représente le type de rôle et de sous-rôle pour lequel chaque ordinateur est configuré. Les fichiers d’extension de rôle suivants sont installés dans le répertoire C:WindowsADFSScw:  
  
    -   Farm.xml  
  
    -   SQLFarm.xml  
  
    -   StandAlone.xml  
  
    -   Proxy.xml (ce fichier est présent uniquement si vous avez configuré l’ordinateur dans le rôle de proxy de serveur de fédération).  
  
    Pour appliquer les extensions de rôle ADFS dans le SCW, effectuez les étapes suivantes dans l’ordre:  
  
    1.  Installez ADFS et choisissez le rôle de serveur approprié pour cet ordinateur. Pour plus d’informations, voir [installer le service de rôle Proxy du Service de fédération](../../ad-fs/deployment/Install-the-Federation-Service-Proxy-Role-Service.md) dans le Guide de déploiement d’ADFS.  
  
    2.  Enregistrez le fichier d’extension de rôle approprié à l’aide de l’outil de ligne de commande Scwcmd. Consultez le tableau suivant pour plus d’informations sur l’utilisation de cet outil dans le rôle pour lequel votre ordinateur est configuré.  
  
    3.  Vérifiez que la commande est terminée correctement en examinant le fichier SCWRegister_log.xml, situé dans le répertoire WindowssecurityMsscwLogs.  
  
    Vous devez effectuer toutes ces étapes sur chaque serveur de fédération ou l’ordinateur proxy du serveur de fédération auquel vous souhaitez appliquer des stratégies de sécurité ADFS dans l’Assistant Configuration.  
  
    Le tableau suivant explique comment inscrire l’extension de rôle SCW appropriée, en fonction du rôle de serveur ADFS que vous avez choisi sur l’ordinateur où vous avez installé ADFS.  
  
    |Rôle de serveur ADFS|Base de données de configuration ADFS utilisée|Tapez la commande suivante à une invite de commandes:|  
    |---------------------|-------------------------------------|---------------------------------------------------|  
    |Serveur de fédération autonome|Base de données interne Windows|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwStandAlone.xml"`|  
    |Serveur de fédération appartenant à une batterie|Base de données interne Windows|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwFarm.xml"`|  
    |Serveur de fédération appartenant à une batterie|SQLServer|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwSQLFarm.xml"`|  
    |Serveur proxy de fédération|NON APPLICABLE|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwProxy.xml"`|  
  
    Pour plus d’informations sur les bases de données que vous pouvez utiliser avec ADFS, voir [le rôle de la base de données de Configuration ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
-   **Utiliser la détection de relecture de jetons dans les situations dans lesquelles la sécurité est très important, par exemple, lorsque les bornes sont utilisés.**  
    Détection de relecture de jetons est une fonctionnalité d’ADFS qui garantit que toute tentative de relecture d’une demande de jeton effectuée auprès du Service de fédération est détectée et que la demande est rejetée. Détection de relecture de jetons est activée par défaut. Il fonctionne pour le profil passif WS-Federation et le profil WebSSO SAML Security Assertion Markup Language () en vous assurant que le même jeton n’est jamais utilisé plusieurs fois.  
  
    Lorsque le Service de fédération démarre, il commence par créer un cache de toutes les demandes de jeton qu’il traite. Au fil du temps, comme les demandes de jeton sont ajoutés au cache, la possibilité de détecter des tentatives de relecture d’une demande de jeton plusieurs fois augmente pour le Service de fédération. Si vous désactivez la détection de relecture de jetons et que vous décidez de la réactiver, n’oubliez pas que le Service de fédération acceptera toujours des jetons pendant une période de temps que vous avez peut-être utilisé précédemment, jusqu'à ce que le cache de lecture a été autorisé suffisamment de temps pour reconstruire son contenu. Pour plus d’informations, voir [le rôle de la base de données de Configuration ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
-   **Utilisez le chiffrement de jetons, en particulier si vous utilisez la résolution d’artefacts SAML prise en charge.**  
  
    Le chiffrement de jetons est vivement conseillé pour accroître la sécurité et protection contre les attaques potentielles man-in-the-middle (intercepteur) qui susceptibles de viser votre déploiement d’ADFS. L’utilisation du chiffrement peut avoir une légère incidence sur tout au long, mais en règle générale, il ne doit pas être remarqué généralement et dans de nombreux déploiements les avantages d’une sécurité accrue procurés en termes de performances du serveur.  
  
    Pour activer le chiffrement de jetons, le premier jeu ajouter un certificat de chiffrement pour vos approbations de partie de confiance. Vous pouvez configurer un certificat de chiffrement quand vous créez une partie de confiance ou version ultérieure. Pour ajouter un certificat de chiffrement ultérieurement à une partie de confiance existante, vous pouvez définir un certificat à utiliser sur le **chiffrement** onglet dans les propriétés d’approbation de tout en utilisant le composant logiciel enfichable ADFS. Pour spécifier un certificat pour une approbation existante à l’aide des applets de commande ADFS, utilisez le paramètre EncryptionCertificate de soit le **Set-ClaimsProviderTrust** ou **Set-RelyingPartyTrust** applets de commande. Pour définir un certificat pour le Service de fédération à utiliser lors de déchiffrement de jetons, utilisez le **Set-ADFSCertificate** applet de commande et spécifiez «`Token-Encryption`» pour le *CertificateType* paramètre. Activation et désactivation de chiffrement pour une partie de confiance spécifique approuvent peut être effectuée à l’aide de la *EncryptClaims* paramètre de la **Set-RelyingPartyTrust** applet de commande.  
  
-   **Utilisez la protection étendue pour l’authentification**  
  
    Pour mieux sécuriser vos déploiements, vous pouvez définir et utiliser la protection étendue pour l’authentification avec ADFS. Ce paramètre spécifie le niveau de la protection étendue pour l’authentification pris en charge par un serveur de fédération.  
  
    La protection étendue pour l’authentification protège contre man-in-the-middle attaques de l’intercepteur, dans lequel une personne malveillante intercepte les informations d’identification du client et les transmet à un serveur. Protection contre les attaques de ce type est possible via un canal de jeton de liaison (CBT) qui peut être soit requises, autorisé ou non par le serveur quand il établit des communications avec les clients.  
  
    Pour activer la fonctionnalité de protection étendue, utilisez le **ExtendedProtectionTokenCheck** paramètre sur le **Set-ADFSProperties** applet de commande. Les valeurs possibles pour ce paramètre et le niveau de sécurité qui fournissent les valeurs sont décrites dans le tableau suivant.  
  
    |Valeur du paramètre|Niveau de sécurité|Paramètre de protection|  
    |-------------------|------------------|----------------------|  
    |Exiger|Le serveur est entièrement sécurisé.|La protection étendue est appliquée et toujours requis.|  
    |Autoriser|Le serveur est partiellement sécurisé.|La protection étendue est appliquée où les systèmes concernés qui ont été corrigés prise en charge.|  
    |None|Le serveur est vulnérable.|La protection étendue n’est pas appliquée.|  
  
-   **Si vous utilisez la journalisation et suivi, protégez la confidentialité des informations sensibles.**  
  
    ADFS ne pas, par défaut, exposer ou effectuer le suivi des informations d’identification personnelle (PII) directement dans le cadre du Service de fédération ou des opérations normales. Lorsque la journalisation des événements et la journalisation du débogage de suivi sont activées dans ADFS, toutefois, en fonction de la stratégie de revendications que vous configurez des revendications types et leurs valeurs associées peuvent contenir PII qui peut-être être consigné dans l’ou les journaux de suivi ADFS.  
  
    Par conséquent, en appliquant le contrôle d’accès sur la configuration ADFS et ses fichiers journaux est fortement recommandé. Si vous ne souhaitez pas que ce type d’information soit visible, vous devez désactiver invisible ou filtrer les informations d’identification personnelle ou les données sensibles dans vos journaux avant de les partager avec d’autres personnes.  
  
    Les conseils suivants peuvent vous aider à empêcher l’exposition involontaire du contenu d’un fichier journal:  
  
    -   Assurez-vous que le journal des événements ADFS et les fichiers journaux de suivi sont protégés par des listes de contrôle d’accès (ACL) qui limitent l’accès à ces administrateurs approuvés ayant besoin d’accéder à ces derniers.  
  
    -   Ne pas copier ou archiver les fichiers journaux à l’aide des extensions de fichiers ou des chemins d’accès qui peuvent être facilement fournis à l’aide d’une demande Web. Par exemple, l’extension de nom de fichier .xml n’est pas un choix judicieux. Vous pouvez consulter le guide d’administration Internet Information Services (IIS) pour afficher la liste des extensions qui peuvent être fournis.  
  
    -   Si vous modifiez le chemin d’accès au fichier journal, veillez à spécifier un chemin d’accès absolu pour l’emplacement du fichier journal, qui doit se trouver en dehors du répertoire public Web hôte racine virtuelle (intervenant) afin d’empêcher l’accès par un tiers externe à l’aide d’un navigateur Web.  

-   **Protection de verrouillage Extranet ADFS**  
    
    Dans le cas d’une attaque sous la forme de demandes d’authentification par mot de passe invalid(bad) fournis par le biais du Proxy d’Application Web, le verrouillage extranet ADFS vous permet de protéger les utilisateurs à partir d’un verrouillage de compte ADFS. Outre la protection de vos utilisateurs à partir d’un ADFS le verrouillage de compte, le verrouillage extranet ADFS protège également contre un mot de passe en force brute deviner des attaques.  Pour plus d’informations, voir [Protection de verrouillage Extranet ADFS](../../ad-fs/operations/Configure-AD-FS-Extranet-Lockout-Protection.md).  
  
## <a name="sql-serverspecific-security-best-practices-for-ad-fs"></a>Meilleures pratiques de sécurité SQLServer – spécifiques pour ADFS  
Les meilleures pratiques suivantes sont spécifiques à l’utilisation de la base de données interne Windows (WID) ou de MicrosoftSQLServer® quand ces technologies de base de données sont utilisées pour gérer les données dans la conception ADFS et le déploiement.  
  
> [!NOTE]  
> Ces recommandations sont destinées à étendre, mais ne remplacent pas, conseils de sécurité SQLServer. Pour plus d’informations sur la planification d’une installation SQLServer sécurisée, voir [considérations de sécurité pour une Installation SQLServer](https://go.microsoft.com/fwlink/?LinkID=139831) (https://go.microsoft.com/fwlink/?LinkID=139831).  
  
-   **Déployez toujours SQLServer derrière un pare-feu dans un environnement réseau physiquement sécurisé.**  
  
    Une installation SQLServer ne doit jamais être exposée directement à Internet. Seuls les ordinateurs qui se trouvent dans votre centre de données doivent être en mesure d’atteindre votre installation SQL server qui prend en charge ADFS. Pour plus d’informations, voir [sécurité meilleures pratiques pour la liste de vérification](https://go.microsoft.com/fwlink/?LinkID=189229) (https://go.microsoft.com/fwlink/?LinkID=189229).  
  
-   **Exécutez SQLServer sous un compte de service au lieu d’utiliser les comptes de service système par défaut.**  
  
    Par défaut, SQLServer est souvent installé et configuré pour utiliser un des comptes système intégrés pris en charge, tels que les comptes LocalSystem ou NetworkService. Pour renforcer la sécurité de votre installation SQLServer pour ADFS, où possible utiliser un compte de service distinct pour accéder à votre service SQLServer et activer l’authentification Kerberos en inscrivant le nom principal de sécurité (SPN) de ce compte dans votre déploiement ActiveDirectory. Cela permet une authentification mutuelle entre le client et serveur. Sans enregistrement SPN d’un compte de service distinct, SQLServer utilise l’authentification NTLM pour Windows, où seul le client est authentifié.  
  
-   **Réduire la surface d’exposition de SQLServer.**  
  
    Activez uniquement les points de terminaison SQLServer qui sont nécessaires. Par défaut, SQLServer fournit un seul point de terminaison intégré TCP qui ne peut pas être supprimé. Pour ADFS, vous devez activer ce point de terminaison TCP pour l’authentification Kerberos. Pour passer en revue les points de terminaison TCP actuels pour voir si des ports TCP supplémentaires définis par l’utilisateur sont ajoutés à une installation SQL, vous pouvez utiliser le «sélectionnez * FROM sys.tcp_endpoints» requête instruction dans une session Transact-SQL (T-SQL). Pour plus d’informations sur la configuration du point de terminaison SQLServer, voir [comment faire: configurer le moteur de base de données pour qu’il écoute sur plusieurs Ports TCP](https://go.microsoft.com/fwlink/?LinkID=189231) (https://go.microsoft.com/fwlink/?LinkID=189231).  
  
-   **Évitez d’utiliser l’authentification basée sur SQL.**  
  
    Pour éviter d’avoir à transférer les mots de passe en texte clair sur votre réseau ou de stockage des mots de passe dans les paramètres de configuration, utilisez l’authentification Windows uniquement avec votre installation SQLServer. L’authentification SQLServer est un mode d’authentification hérité. Stockage des informations d’identification de l’ouverture de session de langage SQL (Structured Query) (noms d’utilisateur SQL et mots de passe) lorsque vous utilisez l’authentification SQLServer n’est pas recommandée. Pour plus d’informations, voir [Modes d’authentification](https://go.microsoft.com/fwlink/?LinkID=189232) (https://go.microsoft.com/fwlink/?LinkID=189232).  
  
-   **Évaluer soigneusement les besoins de sécurité de canal supplémentaires dans votre installation SQL.**  
  
    Même avec l’authentification Kerberos en effet, le SQLServer SSPI Security Support Provider Interface () ne fournit pas au niveau du canal de sécurité. Toutefois, pour les installations dans lesquelles les serveurs sont trouvent en toute sécurité sur un réseau protégé par le pare-feu, le chiffrement des communications SQL peut-être pas nécessaire.  
  
    Bien que le chiffrement est un outil précieux pour aider à garantir la sécurité, il ne doit pas être considéré pour toutes les données ou les connexions. Lorsque vous décidez si vous souhaitez implémenter le chiffrement, prendre en compte la façon dont les utilisateurs accèderont aux données. Si les utilisateurs accéder aux données sur un réseau public, le chiffrement des données peut être requis pour renforcer la sécurité. Toutefois, si tous les accès de données SQL par ADFS implique une configuration intranet sécurisée, le chiffrement peut-être pas nécessaire. Toute utilisation du chiffrement doit également inclure une stratégie de maintenance pour les mots de passe, les clés et certificats.  
  
    S’il existe un problème que des données SQL peuvent être détectées ou altérées via votre réseau, utilisez Internet Protocol security (IPsec) ou sécurisé Secure Sockets Layer (SSL) pour aider à sécuriser vos connexions SQL. Toutefois, cela peut avoir un effet négatif sur les performances de SQLServer, lequel susceptibles d’affecter ou de limiter les performances des services ADFS dans certaines situations. Par exemple, des performances d’émission de jeton ADFS peuvent nuire aux lorsque les recherches d’attribut d’un magasin d’attributs SQL sont critiques pour d’émission de jeton. Vous pouvez mieux éliminer une altération des données en demandant une configuration de sécurité renforcée périmètre SQL. Par exemple, une meilleure solution pour la sécurisation de votre installation SQLServer consiste à s’assurer qu’inaccessible aux utilisateurs d’Internet et les ordinateurs et qu’il reste accessible uniquement par les utilisateurs ou des ordinateurs au sein de votre environnement de centre de données.  
  
    Pour plus d’informations, voir [chiffrement des connexions à SQLServer](https://go.microsoft.com/fwlink/?LinkID=189234) ou [chiffrement SQLServer](https://go.microsoft.com/fwlink/?LinkID=189233).  
  
-   **Configurer Sécurisez l’accès à l’aide des procédures stockées d’effectuer des recherches tout basé sur SQL par des données ADFS de SQL.**  
  
    Pour fournir le meilleur service et isolation des données, vous pouvez créer des procédures stockées pour toutes les commandes de recherche de banque d’attribut. Vous pouvez créer un rôle de base de données auquel vous accordez ensuite l’autorisation d’exécuter les procédures stockées. Affectez l’identité du service WindowsADFS à ce rôle de base de données. Le service WindowsADFS ne doit pas être en mesure d’exécuter aucune instruction SQL, autres que les procédures stockées utilisées pour la recherche d’attribut. Verrouillage d’accès à la base de données SQLServer de cette manière réduit le risque d’une attaque par élévation de privilèges.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
