---
title: Vérifier la Configuration après modification du serveur NPS
description: Vous pouvez utiliser cette rubrique pour vérifier la configuration de serveur de stratégie réseau Windows Server2016après modification d’une adresse IP ou un nom pour le serveur.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fc77450e-2af1-47ba-bb23-1fd36d9efdbf
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f4d7e003fb037d18c5e5f2036a419383885eaf9e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="verify-configuration-after-nps-server-changes"></a>Vérifier la Configuration après modification du serveur NPS

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour vérifier la configuration du serveur NPS après modification d’une adresse IP ou un nom pour le serveur.

## <a name="verify-configuration-after-an-nps-server-ip-address-change"></a>Vérifier la Configuration après une modification d’adresse IP serveur NPS

Il peut exister des cas où vous devez modifier l’adresse IP d’un serveur NPS ou un proxy, par exemple, lorsque vous déplacez le serveur vers un autre sous-réseau IP. 

Si vous modifiez un serveur NPS ou l’adresse IP du proxy, il est nécessaire de reconfigurer des parties de votre déploiement de NPS. 

Utilisez les directives générales suivantes pour vous aider à vérifier qu’une modification d’adresse IP n’interrompt pas l’authentification d’accès réseau, d’autorisation ou de gestion des comptes sur votre réseau pour les serveurs NPS RADIUS et les serveurs proxy RADIUS.

Vous devez être membre du **administrateurs**, ou équivalent, pour effectuer ces procédures.

### <a name="to-verify-configuration-after-an-nps-server-ip-address-change"></a>Pour vérifier la configuration après une NPS modification d’adresses IP de serveur

1. Reconfigurer tous les clients RADIUS, tels que les points d’accès sans fil et les serveurs VPN, la nouvelle adresse IP du serveur NPS.

2. Si le serveur NPS est membre du groupe de serveurs RADIUS distants, reconfigurez le proxy NPS avec la nouvelle adresse IP du serveur NPS.

3. Si vous avez configuré le serveur NPS pour utiliser la journalisation SQLServer, vérifiez que la connectivité entre l’ordinateur exécutant SQLServer et le serveur NPS fonctionne correctement.

4. Si vous avez déployé IPsec pour sécuriser le trafic RADIUS entre votre serveur NPS et un proxy NPS ou autres serveurs ou les périphériques, reconfigurez la stratégie IPsec ou la règle de sécurité de connexion dans le pare-feu Windows avec fonctions avancées de sécurité à utiliser la nouvelle adresse IP du serveur NPS.

5. Si le serveur NPS est multirésident et que vous avez configuré le serveur pour effectuer une liaison à une carte réseau spécifique, reconfigurez les paramètres de port de serveur NPS avec la nouvelle adresse IP.

### <a name="to-verify-configuration-after-an-nps-proxy-ip-address-change"></a>Pour vérifier la configuration après une NPS modification d’adresses IP de proxy

1. Reconfigurer tous les clients RADIUS, tels que les points d’accès sans fil et les serveurs VPN, la nouvelle adresse IP du proxy NPS.

2. Si le proxy NPS est multirésident et que vous avez configuré le serveur proxy pour la liaison à une carte réseau spécifique, reconfigurez les paramètres de port de serveur NPS avec la nouvelle adresse IP.

3. Reconfigurer tous les membres de tous les groupes de serveurs RADIUS distants avec l’adresse IP du serveur proxy. Pour ce faire, sur chaque serveur NPS qui a le proxy NPS configuré comme client RADIUS:

    un. Double-cliquez sur **NPS (Local)**, double-cliquez sur **Clients et serveurs RADIUS**, cliquez sur **Clients RADIUS**, puis dans le volet d’informations, double-cliquez sur le client RADIUS que vous souhaitez modifier.

    b. Dans le client RADIUS **propriétés**, dans **adresse \(IP or DNS\)**, tapez la nouvelle adresse IP du proxy NPS.

4. Si vous avez configuré le serveur de proxy NPS pour utiliser la journalisation SQLServer, vérifiez que la connectivité entre l’ordinateur exécutant SQLServer et le proxy NPS fonctionne correctement.

## <a name="verify-configuration-after-renaming-an-nps-server"></a>Vérifier la Configuration après modification du nom d’un serveur NPS

Il peut arriver lorsque vous devez modifier le nom d’un serveur NPS ou un proxy, par exemple, lorsque vous recréez les conventions d’affectation de noms pour vos serveurs.

Si vous modifiez un serveur NPS ou le nom du serveur proxy, il est nécessaire de reconfigurer des parties de votre déploiement de NPS. 

Utilisez les directives générales suivantes pour vous aider à vérifier qu’un changement de nom de serveur n’interrompt pas l’authentification d’accès réseau, d’autorisation ou comptabilité.

Vous devez être membre du **administrateurs**, ou équivalent, pour effectuer cette procédure.

### <a name="to-verify-configuration-after-an-nps-server-or-proxy-name-change"></a>Pour vérifier la configuration après un changement de nom de serveur ou le proxy NPS

1. Si le serveur NPS est membre du groupe de serveurs RADIUS distants et le groupe est configuré avec les noms d’ordinateur plutôt que des adresses IP, reconfigurez le groupe de serveurs RADIUS distants avec le nouveau nom du serveur NPS.

2. Si les méthodes d’authentification par certificat sont déployés sur le serveur NPS, le changement de nom invalide le certificat de serveur. Vous pouvez demander un nouveau certificat de l’administrateur d’autorité de certification ou, si l’ordinateur est un ordinateur membre du domaine et que vous les certificats-inscription automatique aux membres du domaine, vous pouvez actualiser la stratégie de groupe pour obtenir un nouveau certificat par le biais de l’inscription automatique. Pour actualiser la stratégie de groupe:

    un. Ouvrez une invite de commandes ou Windows PowerShell.

    b. Type **gpupdate**, puis appuyez sur ENTRÉE.


3. Une fois que vous avez un nouveau certificat de serveur, demander que l’administrateur d’autorité de certification révoquer l’ancien certificat. 

     Une fois que l’ancien certificat est révoqué, NPS continue à l’utiliser jusqu'à ce que l’ancien certificat arrive à expiration. Par défaut, l’ancien certificat reste valide pendant une durée maximale d’une semaine et 10heures. Ce délai peut être différent selon si l’expiration de la liste de révocation de certificats (CRL) et l’expiration de temps de cache de sécurité TLS (Transport Layer) ont été modifiés à partir de leurs valeurs par défaut. L’expiration de la révocation de certificats par défaut est une semaine; la valeur par défaut TLS cache heure d’expiration est de 10heures. 

     Si vous souhaitez configurer NPS pour utiliser le nouveau certificat immédiatement, toutefois, vous pouvez reconfigurer manuellement des stratégies de réseau avec le nouveau certificat.

4. Une fois que l’ancien certificat expire, NPS commence automatiquement à l’aide du nouveau certificat. 

5. Si vous avez configuré le serveur NPS pour utiliser la journalisation SQLServer, vérifiez que la connectivité entre l’ordinateur exécutant SQLServer et le serveur NPS fonctionne correctement.

