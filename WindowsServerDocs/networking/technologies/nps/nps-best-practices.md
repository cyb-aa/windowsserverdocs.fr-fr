---
title: Meilleures pratiques relatives au serveur NPS
description: Cette rubrique présente les meilleures pratiques pour le déploiement et la gestion du serveur NPS (Network Policy Server) dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 90e544bd-e826-4093-8c3b-6a6fc2dfd1d6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 364783e2188152fc5c57bba04991ae124b0bb8d2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405496"
---
# <a name="network-policy-server-best-practices"></a>Meilleures pratiques relatives au serveur NPS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur les meilleures pratiques pour le déploiement et la gestion du serveur de stratégie réseau \(les\)NPS.

Les sections suivantes présentent les meilleures pratiques pour différents aspects de votre déploiement NPS.

## <a name="accounting"></a>Gestion des comptes

Voici les meilleures pratiques pour la journalisation NPS.

Il existe deux types de comptabilité, ou journalisation, dans NPS :

- Journalisation des événements pour NPS. Vous pouvez utiliser la journalisation des événements pour enregistrer des événements NPS dans les journaux des événements système et de sécurité. Cela est principalement utilisé pour l’audit et le dépannage des tentatives de connexion.

- Enregistrement des demandes d’authentification des utilisateurs et de gestion des comptes. Vous pouvez consigner les demandes d’authentification des utilisateurs et de gestion des comptes dans les fichiers journaux au format texte ou au format de base de données, ou vous pouvez vous connecter à une procédure stockée dans une base de données SQL Server 2000. La journalisation des demandes est principalement utilisée pour l’analyse de la connexion et la facturation. elle est également utile en tant qu’outil d’investigation de sécurité, ce qui vous permet de suivre l’activité d’une personne malveillante.

Pour tirer le meilleur parti de la journalisation NPS :

- Activez la journalisation \(initialement\) pour les enregistrements d’authentification et de gestion des comptes. Modifiez ces sélections après avoir déterminé ce qui convient à votre environnement.

- Assurez-vous que la journalisation des événements est configurée avec une capacité suffisante pour gérer vos journaux.

- Sauvegardez tous les fichiers journaux régulièrement, car ils ne peuvent pas être recréés lorsqu’ils sont endommagés ou supprimés.

- Utilisez l’attribut de classe RADIUS pour suivre l’utilisation et simplifier l’identification du service ou de l’utilisateur qui est facturé pour l’utilisation. Bien que l’attribut de classe généré automatiquement soit unique pour chaque demande, des enregistrements dupliqués peuvent exister dans les cas où la réponse au serveur d’accès est perdue et que la demande est renvoyée. Vous devrez peut-être supprimer les demandes en double de vos journaux pour suivre précisément l’utilisation.

- Si vos serveurs d’accès réseau et serveurs proxy RADIUS envoient périodiquement des messages de demande de connexion fictifs à NPS pour vérifier que le serveur NPS est en ligne, utilisez le paramètre de Registre **ping User-Name** . Ce paramètre configure NPS pour rejeter automatiquement ces fausses demandes de connexion sans les traiter. En outre, NPS n’enregistre pas les transactions impliquant le nom d’utilisateur fictif dans des fichiers journaux, ce qui facilite l’interprétation du journal des événements.

- Désactivez le transfert de notifications NAS. Vous pouvez désactiver le transfert des messages de démarrage et d’arrêt à partir des serveurs d’accès réseau (NAS) vers les membres d’un groupe de serveurs RADIUS distants qui est configuré dans NPS. Pour plus d’informations, consultez [désactiver le transfert de notifications NAS](nps-disable-nas-notifications.md).

Pour plus d’informations, consultez [configurer la gestion des stratégies de serveur](nps-accounting-configure.md)NPS.

- Pour assurer le basculement et la redondance avec SQL Server la journalisation, placez deux ordinateurs exécutant SQL Server sur des sous-réseaux différents. Utilisez l' **Assistant Création** d’une Publication de SQL Server pour configurer la réplication de base de données entre les deux serveurs. Pour plus d’informations, consultez [SQL Server Documentation technique](https://msdn.microsoft.com/library/ms130214.aspx) et [réplication SQL Server](https://msdn.microsoft.com/library/ms151198.aspx).

## <a name="authentication"></a>Authentification

Voici les meilleures pratiques pour l’authentification.

- Utilisez des méthodes d’authentification basées sur les certificats, telles que le protocole PEAP (Protected Extensible Authentication Protocol \(PEAP\) et le protocole EAP \(EAP\) pour une authentification forte. N’utilisez pas les méthodes d’authentification par mot de passe uniquement, car elles sont vulnérables à diverses attaques et ne sont pas sécurisées. Pour une authentification sans fil sécurisée, l’utilisation de PEAP\-MS\-CHAP v2 est recommandée, car le serveur NPS prouve son identité aux clients sans fil à l’aide d’un certificat de serveur, tandis que les utilisateurs prouvent leur identité avec leur nom d’utilisateur et leur mot de passe.  Pour plus d’informations sur l’utilisation de NPS dans votre déploiement sans fil, consultez [déployer un accès sans fil authentifié 802.1 x basé sur un mot de passe](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access).
- Déployez votre propre autorité de certification \(\) de l’autorité de certification avec Active Directory&reg; services de certificats \(AD CS\) quand vous utilisez des méthodes d’authentification fortes basées sur les certificats, telles que PEAP et EAP, qui requièrent l’utilisation d’un certificat de serveur sur NPSs. Vous pouvez également utiliser votre autorité de certification pour inscrire des certificats d’ordinateur et d’utilisateur. Pour plus d’informations sur le déploiement de certificats de serveur sur des serveurs NPS et d’accès à distance, consultez [déployer des certificats de serveur pour les déploiements sans fil et câblés 802.1 x](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

> [!IMPORTANT]
> Le serveur NPS (Network Policy Server) ne prend pas en charge l’utilisation des caractères ASCII étendus dans les mots de passe.

## <a name="client-computer-configuration"></a>Configuration de l’ordinateur client

Voici les meilleures pratiques pour la configuration des ordinateurs clients.

- Configurez automatiquement tous les ordinateurs clients 802.1 X du membre du domaine à l’aide de stratégie de groupe. Pour plus d’informations, consultez la section « configurer des stratégies de réseau sans fil (IEEE 802,11) » dans la rubrique déploiement de l' [accès sans fil](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies).

## <a name="installation-suggestions"></a>Suggestions d’installation

Voici les meilleures pratiques pour l’installation du serveur NPS.

- Avant d’installer le serveur NPS, installez et testez chacun de vos serveurs d’accès réseau à l’aide des méthodes d’authentification locales avant de les configurer en tant que clients RADIUS dans NPS.

- Après avoir installé et configuré NPS, enregistrez la configuration à l’aide de la commande Windows PowerShell [Export-NpsConfiguration](https://technet.microsoft.com/library/jj872749.aspx). Enregistrez la configuration du serveur NPS à l’aide de cette commande chaque fois que vous reconfigurez le serveur NPS.

>[!CAUTION]
>- Le fichier de configuration NPS exporté contient des secrets partagés non chiffrés pour les clients RADIUS et les membres des groupes de serveurs RADIUS distants. Pour cette raison, assurez-vous d’enregistrer le fichier dans un emplacement sécurisé.
>- Le processus d’exportation n’inclut pas les paramètres de journalisation pour Microsoft SQL Server dans le fichier exporté. Si vous importez le fichier exporté vers un autre serveur NPS, vous devez configurer manuellement SQL Server la journalisation sur le nouveau serveur.

## <a name="performance-tuning-nps"></a>Réglage des performances pour le serveur NPS

Voici les meilleures pratiques pour le réglage des performances du serveur NPS.

- Pour optimiser l’authentification NPS et les temps de réponse d’autorisation et réduire le trafic réseau, installez NPS sur un contrôleur de domaine.

- Lorsque les noms de principal universel \(UPN\) ou Windows Server 2008 et les domaines Windows Server 2003 sont utilisés, NPS utilise le catalogue global pour authentifier les utilisateurs. Pour réduire le temps nécessaire à cette opération, installez NPS sur un serveur de catalogue global ou un serveur qui se trouve sur le même sous-réseau que le serveur de catalogue global.

- Lorsque vous avez configuré des groupes de serveurs RADIUS distants et, dans stratégies de demande de connexion NPS, vous désactivez les **informations de gestion des enregistrements sur les serveurs dans le groupe de serveurs RADIUS distants suivant** , ces groupes reçoivent toujours le serveur d’accès réseau \(NAS\) les messages de notification de démarrage et d’arrêt. Cela crée un trafic réseau inutile. Pour éliminer ce trafic, désactivez le transfert de notifications NAS pour des serveurs individuels dans chaque groupe de serveurs RADIUS distants en désactivant la case à cocher **transférer le démarrage réseau et arrêter les notifications à ce serveur** .

## <a name="using-nps-in-large-organizations"></a>Utilisation de NPS dans les grandes organisations

Voici les meilleures pratiques pour l’utilisation de NPS dans les grandes organisations.

- Si vous utilisez des stratégies réseau pour restreindre l’accès à tous les groupes sauf certains, créez un groupe universel pour tous les utilisateurs auxquels vous souhaitez autoriser l’accès, puis créez une stratégie réseau qui accorde l’accès à ce groupe universel. Ne placez pas tous vos utilisateurs directement dans le groupe universel, en particulier si vous en avez un grand nombre sur votre réseau. Au lieu de cela, créez des groupes distincts qui sont membres du groupe universel et ajoutez des utilisateurs à ces groupes.

- Utilisez un nom d’utilisateur principal pour faire référence aux utilisateurs chaque fois que cela est possible. Un utilisateur peut avoir le même nom d’utilisateur principal, quelle que soit l’appartenance à un domaine. Cette pratique fournit une évolutivité qui peut être nécessaire dans les organisations avec un grand nombre de domaines.

- Si vous avez installé le serveur de stratégie réseau \(\) NPS sur un ordinateur autre qu’un contrôleur de domaine et que le serveur NPS reçoit un grand nombre de demandes d’authentification par seconde, vous pouvez améliorer les performances de NPS en renforçant le nombre d’authentifications simultanées autorisées entre le serveur NPS et le contrôleur de domaine. Pour plus d'informations, voir 

## <a name="security-issues"></a>Problèmes de sécurité

Voici les meilleures pratiques pour réduire les problèmes de sécurité.

Lorsque vous administrez un serveur NPS à distance, n’envoyez pas de données sensibles ou confidentielles (par exemple, secrets partagés ou mots de passe) sur le réseau en texte en clair. Il existe deux méthodes recommandées pour l’administration à distance de NPSs :

- Utilisez Services Bureau à distance pour accéder au serveur NPS. Lorsque vous utilisez Services Bureau à distance, les données ne sont pas envoyées entre le client et le serveur. Seule l’interface utilisateur du serveur (par exemple, l’image de bureau du système d’exploitation et de la console NPS) est envoyée au client Services Bureau à distance, qui est nommé Connexion Bureau à distance dans Windows&reg; 10. Le client envoie une entrée au clavier et à la souris, qui est traitée localement par le serveur sur lequel Services Bureau à distance activé. Lorsque Services Bureau à distance utilisateurs ouvrent une session, ils peuvent afficher uniquement leurs sessions clientes individuelles, qui sont gérées par le serveur et sont indépendantes les unes des autres. En outre, Connexion Bureau à distance fournit un chiffrement 128 bits entre le client et le serveur.

- Utilisez la sécurité du protocole Internet (IPsec) pour chiffrer les données confidentielles. Vous pouvez utiliser IPsec pour chiffrer les communications entre le serveur NPS et l’ordinateur client distant que vous utilisez pour administrer le serveur NPS. Pour administrer le serveur à distance, vous pouvez installer le [Outils d’administration de serveur distant pour Windows 10](https://www.microsoft.com/download/details.aspx?id=45520) sur l’ordinateur client. Après l’installation, utilisez la console MMC (Microsoft Management Console) pour ajouter le composant logiciel enfichable NPS à la console.

>[!IMPORTANT]
>Vous pouvez installer Outils d’administration de serveur distant pour Windows 10 uniquement sur la version complète de Windows 10 professionnel ou Windows 10 entreprise.

Pour plus d’informations sur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).

