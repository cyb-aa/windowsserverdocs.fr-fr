---
title: Serveur de stratégie réseau meilleures pratiques
description: Cette rubrique fournit des meilleures pratiques pour le déploiement et la gestion de serveur de stratégie de réseau dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 90e544bd-e826-4093-8c3b-6a6fc2dfd1d6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5a9a68965d0d19504352ad67f7ab268b3d344fb2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="network-policy-server-best-practices"></a>Serveur de stratégie réseau meilleures pratiques

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur les meilleures pratiques pour le déploiement et la gestion de serveur de stratégie réseau \(NPS\).

Les sections suivantes fournissent les meilleures pratiques pour les différents aspects de votre déploiement de NPS.

## <a name="accounting"></a>Gestion des comptes

Voici les meilleures pratiques pour l’enregistrement du serveur NPS.

Il existe deux types de gestion des comptes ou la journalisation, dans NPS:

- Journalisation des événements d’un serveur NPS. Vous pouvez utiliser la journalisation des événements pour consigner les événements NPS dans les journaux des événements système et sécurité. Il est utilisé principalement pour l’audit et la résolution des problèmes de tentatives de connexion.

- Journalisation de l’authentification des utilisateurs et les demandes de gestion. Vous pouvez consigner les demandes d’authentification et de gestion des comptes d’utilisateur dans les fichiers journaux au format texte ou de base de données, ou vous pouvez vous connecter à une procédure stockée dans une base de données SQL Server 2000. Enregistrement dans le journal est utilisée principalement pour des raisons de facturation et d’analyse de connexion et est également utile comme un outil d’enquête de sécurité, vous fournissant ainsi une méthode de suivi vers le bas de l’activité d’une personne malveillante.

Pour rendre le meilleur parti de la journalisation NPS:

- Activer \(initially\) journalisation pour l’authentification et de comptabilité. Modifiez ces sélections après avoir déterminé ce qui est approprié pour votre environnement.

- Assurez-vous que la journalisation des événements est configurée avec une capacité suffisante gérer vos journaux.

- Sauvegarder tous les fichiers journaux de façon régulière, car ils ne peuvent pas être recréés lorsqu’ils sont endommagés ou supprimés.

- Utilisez l’attribut de classe RADIUS pour suivre l’utilisation et de simplifier l’identification du service ou de l’utilisateur pendant l’utilisation. Bien que l’attribut de classe généré automatiquement soit unique pour chaque demande, des enregistrements dupliqués peuvent exister dans les cas où la réponse au serveur d’accès est perdue et la demande est renvoyée. Vous devrez peut-être supprimer les demandes dupliquées de vos journaux pour suivre l’utilisation.

- Si vos serveurs d’accès réseau et les serveurs proxy RADIUS régulièrement envoient des messages de demande de connexion fictif à NPS pour vérifier que le serveur NPS est en ligne, utilisez le **ping nom d’utilisateur** paramètre du Registre. Ce paramètre configure le serveur NPS pour refuser automatiquement les demandes de connexion false sans les traiter. En outre, NPS n’enregistre pas les transactions impliquant le nom d’utilisateur fictif dans tous les fichiers journaux, ce qui facilite le journal des événements interpréter.

- Désactiver le transfert de notifications NAS. Vous pouvez désactiver le transfert du menu Démarrer et arrêter des messages à partir des serveurs d’accès réseau (NAS) aux membres d’un serveur RADIUS distant de groupe qui est configuré dans NPS. Pour plus d’informations, voir [désactiver un transfert de notifications NAS](nps-disable-nas-notifications.md).

Pour plus d’informations, voir [comptabilité serveur de stratégie réseau configurer](nps-accounting-configure.md).

- Pour fournir la redondance et basculement avec la journalisation SQL Server, placez deux ordinateurs exécutant SQL Server sur des sous-réseaux différents. Utiliser le serveur SQL Server **Assistant Création de Publication** pour configurer la réplication de base de données entre les deux serveurs. Pour plus d’informations, voir [Documentation technique de SQL Server](https://msdn.microsoft.com/library/ms130214.aspx) et [la réplication SQL Server](https://msdn.microsoft.com/library/ms151198.aspx).

## <a name="authentication"></a>Authentification

Voici les meilleures pratiques pour l’authentification.

- Utilisez les méthodes de l’authentification par certificat comme Protected Extensible Authentication Protocol \(PEAP\) et \(EAP\) Extensible Authentication Protocol pour l’authentification forte. N’utilisez pas les méthodes d’authentification de mot de passe uniquement, car elles sont vulnérables aux diverses attaques et ne sont pas sécurisés. Pour l’authentification sans fil sécurisée, à l’aide de PEAP\-MS\-CHAP v2 est recommandée, car le serveur NPS prouve son identité aux clients sans fil à l’aide d’un certificat de serveur, alors que les utilisateurs prouvent leur identité avec leur nom d’utilisateur et un mot de passe.  Pour plus d’informations sur l’utilisation du serveur NPS dans votre déploiement sans fil, consultez [déployer le mot de passe accès 802. 1 X authentifié sans fil](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access).
- Déployer votre propre autorité de certification \(CA\) avec Active Directory&reg; \(AD CS\) Services de certificats lorsque vous utilisez les méthodes de l’authentification forte basées sur certificat, telles que PEAP et EAP, qui nécessitent l’utilisation d’un certificat de serveur sur les serveurs NPS. Vous pouvez également utiliser votre autorité de certification pour inscrire des certificats d’ordinateur et les certificats d’utilisateur. Pour plus d’informations sur le déploiement de certificats de serveur sur les serveurs NPS et accès à distance, voir [déployer des certificats de serveur pour 802.1 X câblé et sans fil déploiements](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="client-computer-configuration"></a>Configuration de l’ordinateur client

Voici les meilleures pratiques pour la configuration de l’ordinateur client.

- Configurer automatiquement tous vos membre de domaine 802.1 X les ordinateurs clients à l’aide de stratégie de groupe. Pour plus d’informations, consultez la section «Configurer Wireless Network (IEEE 802.11) Policies» dans la rubrique [déploiement de l’accès sans fil](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies).

## <a name="installation-suggestions"></a>Suggestions d’installation

Voici les meilleures pratiques pour installer le serveur NPS.

- Avant d’installer le serveur NPS, installez et testez chacun de vos serveurs d’accès réseau à l’aide des méthodes d’authentification local avant de les configurer en tant que clients RADIUS dans NPS.

- Une fois que vous installez et configurez le serveur NPS, enregistrer la configuration à l’aide de la commande Windows PowerShell [Export-NpsConfiguration](https://technet.microsoft.com/en-us/library/jj872749.aspx). Enregistrer la configuration du serveur NPS avec cette commande chaque fois que vous reconfigurez le serveur NPS.

>[!CAUTION]
>- Le fichier de configuration NPS exporté contient des secrets partagés non chiffrés pour les clients RADIUS et les membres de groupes de serveurs RADIUS distants. Pour cette raison, assurez-vous que vous enregistrez le fichier à un emplacement sécurisé.
>- Le processus d’exportation n’inclut pas les paramètres de journalisation pour Microsoft SQL Server dans le fichier exporté. Si vous importez le fichier exporté vers un autre serveur NPS, vous devez configurer manuellement la journalisation SQL Server sur le nouveau serveur.

## <a name="performance-tuning-nps"></a>NPS de réglage des performances

Voici les meilleures pratiques pour le serveur NPS de réglage des performances.

- Pour optimiser les temps de réponse d’authentification et d’autorisation NPS et réduire le trafic réseau, installer le serveur NPS sur un contrôleur de domaine.

- Lorsque des noms principaux universels \(UPNs\) ou des domaines Windows Server 2008 et Windows Server 2003 sont utilisées, NPS utilise le catalogue global pour authentifier les utilisateurs. Pour réduire le temps que nécessaire pour ce faire, installer le serveur NPS sur un serveur de catalogue global ou un serveur qui se trouve sur le même sous-réseau que le serveur de catalogue global.

- Lorsque vous avez des groupes de serveurs RADIUS distants configurés et, dans les stratégies de demande de connexion NPS, vous désactivez le **enregistrer les informations de gestion sur les serveurs dans le groupe de serveurs RADIUS distant suivant** case à cocher, ces groupes sont toujours envoyés réseau accès serveur \(NAS\) Démarrer et arrêter les messages de notification. Cela crée un trafic réseau inutile. Pour éliminer ce trafic, désactivez le transfert de notifications NAS pour des serveurs individuels dans chaque groupe de serveurs RADIUS distants en désactivant le **transférer le démarrage réseau et arrêter les notifications de ce serveur** case à cocher.

## <a name="using-nps-in-large-organizations"></a>À l’aide du serveur NPS dans les grandes organisations

Voici les meilleures pratiques pour l’utilisation du serveur NPS dans les grandes entreprises.

- Si vous utilisez des stratégies de réseau pour limiter l’accès pour tous les mais certains groupes, créer un groupe universel pour tous les utilisateurs pour lesquels vous souhaitez autoriser l’accès, puis créer une stratégie de réseau qui accorde l’accès pour ce groupe universel. Ne placez pas tous vos utilisateurs directement dans le groupe universel, en particulier si vous avez un grand nombre d'entre eux sur votre réseau. Au lieu de cela, créez des groupes distincts qui sont membres du groupe universel et ajouter des utilisateurs à ces groupes.

- Utiliser un nom d’utilisateur principal pour faire référence aux utilisateurs chaque fois que possible. Un utilisateur peut avoir le même nom d’utilisateur principal, quelle que soit l’appartenance au domaine. Cette pratique assure l’évolutivité qui peut-être être nécessaires dans les organisations avec un grand nombre de domaines.

- Si vous avez installé le serveur de stratégie réseau \(NPS\) sur un ordinateur autre que d’un contrôleur de domaine et le serveur NPS reçoit un grand nombre de demandes d’authentification par seconde, vous pouvez améliorer les performances de NPS en augmentant le nombre d’authentifications simultanées autorisées entre le serveur NPS et le contrôleur de domaine. Pour plus d’informations, voir 

## <a name="security-issues"></a>Problèmes de sécurité

Voici les meilleures pratiques pour réduire les problèmes de sécurité.

Lorsque vous administrez à distance un serveur NPS, ne pas envoyer les données confidentielles ou sensibles (par exemple, les secrets partagés ou les mots de passe) sur le réseau en texte brut. Il existe deux méthodes recommandées pour l’administration à distance des serveurs NPS:

- Utilisez les Services Bureau à distance pour accéder au serveur NPS. Lorsque vous utilisez des Services Bureau à distance, les données ne sont pas envoyées entre client et serveur. Seule l’interface utilisateur du serveur (par exemple, le bureau du système d’exploitation et une image de la console NPS) est envoyé au client des Services Bureau à distance, qui est nommé connexion Bureau à distance dans Windows&reg; 10. Le client envoie l’entrée, clavier et souris qui est traitée localement par le serveur qui a des Services Bureau à distance est activé. Lorsque les utilisateurs des Services Bureau à distance une session, ils peuvent afficher que leurs sessions client individuel, qui sont gérées par le serveur et sont indépendants les uns des autres. En outre, connexion Bureau à distance fournit le chiffrement de 128 bits entre client et serveur.

- Utiliser Internet Protocol security (IPsec) pour chiffrer les données confidentielles. Vous pouvez utiliser IPsec pour chiffrer les communications entre le serveur NPS et l’ordinateur client distant que vous utilisez pour administrer le serveur NPS. Pour administrer le serveur à distance, vous pouvez installer le [Remote Server Administration Tools pour Windows 10](https://www.microsoft.com/download/details.aspx?id=45520) sur l’ordinateur client. Après l’installation, utilisez la Console MMC (Microsoft Management Console) pour ajouter le composant logiciel enfichable serveur NPS à la console.

>[!IMPORTANT]
>Vous pouvez installer Remote Server Administration Tools pour Windows 10 uniquement sur la version complète de Windows 10 Professionnel ou Windows 10 entreprise.

Pour plus d’informations sur le serveur NPS, voir [serveur NPS (Network Policy)](nps-top.md).

