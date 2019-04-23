---
title: Meilleures pratiques relatives au serveur NPS
description: Cette rubrique fournit les meilleures pratiques pour déployer et gérer le serveur NPS dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 90e544bd-e826-4093-8c3b-6a6fc2dfd1d6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6cbd606edec06a80767ee997ef6a1397b66b7843
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834230"
---
# <a name="network-policy-server-best-practices"></a>Meilleures pratiques relatives au serveur NPS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur les meilleures pratiques pour déployer et gérer le serveur de stratégie réseau \(NPS\).

Les sections suivantes fournissent les meilleures pratiques pour les différents aspects de votre déploiement de serveur NPS.

## <a name="accounting"></a>Gestion des comptes

Voici les meilleures pratiques pour la journalisation du serveur NPS.

Il existe deux types de comptabilité ou de journalisation dans NPS :

- Journalisation des événements d’un serveur NPS. Vous pouvez utiliser la journalisation des événements à enregistrer des événements NPS dans les journaux des événements système et sécurité. Cela est utilisé principalement pour l’audit et la résolution des problèmes de tentatives de connexion.

- Enregistrement des demandes de comptabilité et de l’authentification des utilisateurs. Vous pouvez consigner les demandes de l’authentification et de gestion des comptes utilisateur dans des fichiers journaux au format texte ou le format de base de données, ou vous pouvez vous connecter à une procédure stockée dans une base de données SQL Server 2000. Enregistrement des demandes sert principalement à des fins de facturation et d’analyse de connexion et est également utile comme outil d’enquête de sécurité, vous offrant une méthode de suivi vers le bas de l’activité d’une personne malveillante.

Pour rendre l’utilisation plus efficace de journalisation du serveur NPS :

- Activer la journalisation \(initialement\) pour l’authentification et de comptabilité. Modifier ces sélections après avoir déterminé ce qui est approprié pour votre environnement.

- Vérifiez que la journalisation des événements est configurée avec une capacité suffisante gérer vos journaux.

- Sauvegardez tous les fichiers journaux sur une base régulière, car ils ne peuvent pas être recréés lorsqu’ils sont endommagés ou supprimés.

- Utiliser l’attribut de classe RADIUS pour suivre l’utilisation et simplifier l’identification du service ou de l’utilisateur à facturer l’utilisation. Bien que l’attribut de classe généré automatiquement est unique pour chaque demande, les enregistrements en double peuvent exister dans les cas où la réponse au serveur d’accès est perdue et que la demande est renvoyée. Vous devrez peut-être supprimer les demandes dupliquées à partir de vos journaux pour suivre précisément l’utilisation.

- Si vos serveurs d’accès réseau et les serveurs proxy RADIUS envoient régulièrement des messages de demande de connexion fictive pour NPS pour vérifier que le serveur NPS est en ligne, utilisez le **un test ping du nom d’utilisateur** paramètre de Registre. Ce paramètre configure le serveur NPS pour refuser automatiquement ces demandes de connexion false sans les traiter. En outre, NPS n’enregistre pas les transactions impliquant le nom d’utilisateur fictif dans tous les fichiers journaux, ce qui rend le journal des événements plus faciles à interpréter.

- Désactiver le transfert de Notification NAS. Vous pouvez désactiver le transfert de démarrer et arrêter des messages à partir de serveurs d’accès réseau (NAS) aux membres d’un serveur RADIUS à distance groupe qui est configuré dans NPS. Pour plus d’informations, consultez [désactiver un transfert de Notification NAS](nps-disable-nas-notifications.md).

Pour plus d’informations, consultez [configurer réseau stratégie serveur comptabilité](nps-accounting-configure.md).

- Pour fournir le basculement et la redondance avec la journalisation SQL Server, placez deux ordinateurs exécutant SQL Server sur des sous-réseaux différents. Utiliser le serveur SQL **Assistant Création de Publication** pour configurer la réplication de base de données entre les deux serveurs. Pour plus d’informations, consultez [Documentation technique de SQL Server](https://msdn.microsoft.com/library/ms130214.aspx) et [la réplication SQL Server](https://msdn.microsoft.com/library/ms151198.aspx).

## <a name="authentication"></a>Authentification

Voici les meilleures pratiques pour l’authentification.

- Utilisez les méthodes d’authentification par certificat comme Protected Extensible Authentication Protocol \(PEAP\) et Extensible Authentication Protocol \(EAP\) pour une authentification forte. N’utilisez pas de méthodes d’authentification de mot de passe uniquement, car elles sont vulnérables à une variété d’attaques et ne sont pas sécurisés. Pour l’authentification sans fil sécurisée, à l’aide de PEAP\-MS\-CHAP v2 est recommandée, car le serveur NPS prouve son identité aux clients sans fil à l’aide d’un certificat de serveur, tandis que les utilisateurs prouvent leur identité avec le nom d’utilisateur et mot de passe.  Pour plus d’informations sur l’utilisation de serveur NPS dans votre déploiement sans fil, consultez [par mot de passe déployer accès 802. 1 X authentifié sans fil](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access).
- Déployer votre propre autorité de certification \(autorité de certification\) avec Active Directory&reg; les Services de certificats \(AD CS\) lorsque vous utilisez une authentification forte basée sur certificat méthodes, telles que PEAP et EAP, qui nécessitent l’utilisation d’un certificat de serveur sur NPSs. Vous pouvez également utiliser votre autorité de certification pour inscrire des certificats d’ordinateur et les certificats de l’utilisateur. Pour plus d’informations sur le déploiement des certificats de serveur sur des serveurs NPS et accès à distance, consultez [déployer des certificats de serveur pour les déploiements de sans fil et câblé à 802.1 X](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="client-computer-configuration"></a>Configuration de l’ordinateur client

Voici les meilleures pratiques pour la configuration de l’ordinateur client.

- Configurer automatiquement tous vos membres de domaine 802.1 X des ordinateurs clients à l’aide de stratégie de groupe. Pour plus d’informations, consultez la section « Configurer des stratégies sans fil (IEEE 802.11) de réseau » dans la rubrique [déploiement de l’accès sans fil](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies).

## <a name="installation-suggestions"></a>Suggestions d’installation

Voici les meilleures pratiques pour installer le serveur NPS.

- Avant d’installer le serveur NPS, installez et testez chacun de vos serveurs d’accès réseau à l’aide de méthodes d’authentification local avant de les configurer en tant que clients RADIUS dans NPS.

- Une fois que vous installez et configurez le serveur NPS, enregistrer la configuration à l’aide de la commande Windows PowerShell [NpsConfiguration d’exportation](https://technet.microsoft.com/library/jj872749.aspx). Enregistrer la configuration NPS avec cette commande chaque fois que vous reconfigurez le serveur NPS.

>[!CAUTION]
>- Le fichier de configuration de serveur NPS exporté contient des secrets partagés non chiffrés pour les clients RADIUS et les membres de groupes de serveurs RADIUS distants. Pour cette raison, assurez-vous que vous enregistrez le fichier dans un emplacement sécurisé.
>- Le processus d’exportation n’inclut pas les paramètres de journalisation pour Microsoft SQL Server dans le fichier exporté. Si vous importez le fichier exporté vers un autre serveur NPS, vous devez configurer manuellement la journalisation SQL Server sur le nouveau serveur.

## <a name="performance-tuning-nps"></a>NPS de réglage des performances

Voici les meilleures pratiques pour NPS d’optimisation des performances.

- Pour optimiser les temps de réponse d’authentification et l’autorisation de serveur NPS et réduire le trafic réseau, installer le serveur NPS sur un contrôleur de domaine.

- Lorsque les noms principaux universels \(UPN\) ou utilisés des domaines Windows Server 2008 et Windows Server 2003, NPS utilise le catalogue global pour authentifier les utilisateurs. Pour réduire le temps que nécessaire pour ce faire, installez NPS sur un serveur de catalogue global ou un serveur qui se trouve sur le même sous-réseau que le serveur de catalogue global.

- Lorsque vous avez des groupes de serveurs RADIUS distants configurés et, dans les stratégies de demande de connexion NPS, vous désactivez le **enregistrent les informations de gestion des comptes sur les serveurs dans le groupe de serveurs RADIUS distant suivant** case à cocher, ces groupes sont toujours envoyé du serveur d’accès réseau \(NAS\) démarrer et arrêter des messages de notification. Cette opération crée un trafic réseau inutile. Pour éliminer ce trafic, désactiver le transfert de notification de NAS pour des serveurs individuels dans chaque groupe de serveurs RADIUS distants en désactivant le **transférer le démarrage réseau et d’arrêter les notifications pour ce serveur** case à cocher.

## <a name="using-nps-in-large-organizations"></a>À l’aide du serveur NPS dans les grandes organisations

Voici les meilleures pratiques pour l’utilisation du serveur NPS dans les grandes organisations.

- Si vous utilisez des stratégies de réseau pour restreindre l’accès pour tous les mais certains des groupes, créer un groupe universel pour tous les utilisateurs pour lesquels vous souhaitez autoriser l’accès, puis créez une stratégie de réseau qui accorde l’accès pour ce groupe universel. Ne placez pas tous vos utilisateurs directement dans le groupe universel, surtout si vous avez un grand nombre d'entre eux sur votre réseau. Au lieu de cela, créez des groupes distincts qui sont membres du groupe universel et ajouter des utilisateurs à ces groupes.

- Utiliser un nom d’utilisateur principal pour faire référence aux utilisateurs chaque fois que possible. Un utilisateur peut avoir le même nom d’utilisateur principal, quel que soit l’appartenance au domaine. Cette pratique assure l’évolutivité qui peut-être être nécessaires dans les organisations avec un grand nombre de domaines.

- Si vous avez installé le serveur de stratégie réseau \(NPS\) sur un ordinateur autre qu’un domaine contrôleur et le serveur NPS reçoit un grand nombre de demandes d’authentification par seconde, vous pouvez améliorer les performances de serveur NPS en augmentant le nombre de authentifications simultanées autorisées entre le serveur NPS et le contrôleur de domaine. Pour plus d'informations, voir 

## <a name="security-issues"></a>Problèmes de sécurité

Voici les meilleures pratiques pour réduire les problèmes de sécurité.

Lorsque vous administrez à distance un serveur NPS, ne pas envoyer les données sensibles ou confidentielles (par exemple, les secrets partagés ou les mots de passe) sur le réseau en texte clair. Il existe deux méthodes recommandées pour l’administration à distance de NPSs :

- Utiliser des Services Bureau à distance pour accéder au serveur NPS. Lorsque vous utilisez des Services Bureau à distance, les données ne sont pas envoyées entre client et serveur. Seule l’interface utilisateur du serveur (par exemple, le bureau du système d’exploitation et une image de la console NPS) est envoyé au client des Services Bureau à distance, qui est nommé connexion Bureau à distance dans Windows&reg; 10. Le client envoie l’entrée, clavier et souris qui est traité localement par le serveur qui a des Services Bureau à distance est activée. Lorsque les utilisateurs des Services Bureau à distance se connectent, ils peuvent afficher uniquement leurs propres sessions clientes, qui sont gérées par le serveur et sont indépendantes les unes des autres. En outre, la connexion Bureau à distance fournit chiffrement 128 bits entre client et serveur.

- Utilisez Internet Protocol security (IPsec) pour chiffrer les données confidentielles. Vous pouvez utiliser IPsec pour chiffrer les communications entre le serveur NPS et l’ordinateur client distant que vous utilisez pour administrer le serveur NPS. Pour administrer le serveur à distance, vous pouvez installer le [distant Server Administration Tools pour Windows 10](https://www.microsoft.com/download/details.aspx?id=45520) sur l’ordinateur client. Après l’installation, utilisez la console Microsoft Management Console (MMC) pour ajouter le composant logiciel enfichable NPS à la console.

>[!IMPORTANT]
>Vous pouvez installer à distance Server Administration Tools pour Windows 10 uniquement sur la version complète de Windows 10 Professionnel ou Windows 10 entreprise.

Pour plus d’informations sur le serveur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).

