---
title: Vérifier la Configuration après modification du serveur NPS
description: Vous pouvez utiliser cette rubrique pour vérifier la configuration de serveur NPS Windows Server 2016 après une adresse IP ou un nom modifié sur le serveur.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fc77450e-2af1-47ba-bb23-1fd36d9efdbf
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 144e414e32d413e4863b90ada671753155bc96d7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880670"
---
# <a name="verify-configuration-after-nps-changes"></a>Vérifier la Configuration après modification du serveur NPS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour vérifier la configuration NPS après une adresse IP ou un nom modifié sur le serveur.

## <a name="verify-configuration-after-an-nps-ip-address-change"></a>Vérifier la Configuration après un changement d’adresse IP du serveur NPS

Il peut exister des circonstances où vous devez modifier l’adresse IP d’un serveur NPS ou le proxy, tels que lorsque vous déplacez le serveur vers un autre sous-réseau IP. 

Si vous modifiez une adresse IP de serveur NPS ou proxy, il est nécessaire de reconfigurer les parties de votre déploiement de serveur NPS. 

Utilisez les directives générales suivantes pour vous aider à vérifier que les modifications d’adresse IP n’interrompent pas l’authentification d’accès réseau, l’autorisation ou gestion des comptes sur votre réseau pour les serveurs NPS RADIUS et serveurs proxy RADIUS.

Vous devez être membre du **administrateurs**, ou équivalent, pour effectuer ces procédures.

### <a name="to-verify-configuration-after-an-nps-ip-address-change"></a>Pour vérifier la configuration après une modification d’adresse IP de NPS

1. Reconfigurez tous les clients RADIUS, tels que les points d’accès sans fil et les serveurs VPN, avec la nouvelle adresse IP du serveur NPS.

2. Si le serveur NPS est membre du groupe de serveurs RADIUS distants, reconfigurez le proxy de serveur NPS avec la nouvelle adresse IP du serveur NPS.

3. Si vous avez configuré le serveur NPS pour utiliser la journalisation de SQL Server, vérifiez que la connectivité entre l’ordinateur exécutant SQL Server et le serveur NPS fonctionne correctement.

4. Si vous avez déployé IPsec pour sécuriser le trafic RADIUS entre le serveur NPS et un proxy de serveur NPS ou d’autres serveurs ou périphériques, reconfigurez la stratégie IPsec ou la règle de sécurité de connexion dans le pare-feu Windows avec fonctions avancées de sécurité à utiliser la nouvelle adresse IP du serveur NPS.

5. Si le serveur NPS est multirésident et que vous avez configuré le serveur à lier à une carte réseau spécifique, reconfigurer les paramètres de port de serveur NPS avec la nouvelle adresse IP.

### <a name="to-verify-configuration-after-an-nps-proxy-ip-address-change"></a>Pour vérifier la configuration après un serveur NPS modification d’adresses IP de proxy

1. Reconfigurez tous les clients RADIUS, tels que les points d’accès sans fil et les serveurs VPN, avec la nouvelle adresse IP du proxy NPS.

2. Si le proxy de serveur NPS est multirésident et que vous avez configuré le proxy à lier à une carte réseau spécifique, reconfigurer les paramètres de port de serveur NPS avec la nouvelle adresse IP.

3. Reconfigurez tous les membres de tous les groupes de serveurs RADIUS distants avec l’adresse IP du serveur proxy. Pour accomplir cette tâche, à chaque serveur NPS qui a le proxy de serveur NPS configuré comme client RADIUS :

    a. Double-cliquez sur **NPS (Local)**, double-cliquez sur **Clients et serveurs RADIUS**, cliquez sur **Clients RADIUS**, puis, dans le volet de détails, double-cliquez sur le client RADIUS que vous avez vous souhaitez modifier.

    b. Dans le client RADIUS **propriétés**, dans **adresse \(IP ou DNS\)**, tapez la nouvelle adresse IP du proxy NPS.

4. Si vous avez configuré le proxy de serveur NPS pour utiliser la journalisation de SQL Server, vérifiez que la connectivité entre l’ordinateur exécutant SQL Server et le proxy NPS fonctionne correctement.

## <a name="verify-configuration-after-renaming-an-nps"></a>Vérifier la Configuration après avoir renommé un serveur NPS

Il peut arriver lorsque vous avez besoin modifier le nom d’un serveur NPS ou le proxy, tels que lorsque vous reconcevoir les conventions d’affectation de noms pour vos serveurs.

Si vous modifiez un nom de serveur NPS ou proxy, il est nécessaire de reconfigurer les parties de votre déploiement de serveur NPS. 

Utilisez les directives générales suivantes pour vous aider à vérifier qu’un changement de nom de serveur n’interrompt pas l’authentification d’accès réseau, l’autorisation ou comptabilité.

Vous devez être membre du **administrateurs**, ou équivalent, pour effectuer cette procédure.

### <a name="to-verify-configuration-after-an-nps-or-proxy-name-change"></a>Pour vérifier la configuration après un changement de nom de serveur NPS ou proxy

1. Si le serveur NPS est membre du groupe de serveurs RADIUS distants et le groupe est configuré avec les noms d’ordinateur plutôt que des adresses IP, reconfigurez le groupe de serveurs RADIUS distants avec le nouveau nom de serveur NPS.

2. Si les méthodes d’authentification basée sur certificat sont déployés sur le serveur NPS, la modification du nom invalide le certificat de serveur. Vous pouvez demander un nouveau certificat à partir de l’administrateur d’autorité de certification ou, si l’ordinateur est un ordinateur membre du domaine et que vous inscrivez automatiquement les certificats pour les membres du domaine, vous pouvez actualiser la stratégie de groupe pour obtenir un nouveau certificat via l’inscription automatique . Pour actualiser la stratégie de groupe :

    a. Ouvrez une invite de commandes ou Windows PowerShell.

    b. Tapez **gpupdate** et appuyez sur Entrée.


3. Une fois que vous avez un nouveau certificat de serveur, demander que l’administrateur de l’autorité de certification révoquer l’ancien certificat. 

     Une fois que l’ancien certificat est révoqué, NPS continue à l’utiliser jusqu'à l’expiration de l’ancien certificat. Par défaut, l’ancien certificat reste valide pendant une durée maximale d’une semaine et 10 heures. Cette période peut être différente selon que l’expiration de la liste de révocation de certificats (CRL) et l’expiration de durée de cache de sécurité TLS (Transport Layer) ont été modifiées à partir de leurs valeurs par défaut. L’expiration de la révocation de certificats par défaut est une semaine ; la valeur par défaut TLS cache heure d’expiration est de 10 heures. 

     Si vous souhaitez configurer NPS pour utiliser le nouveau certificat immédiatement, toutefois, vous pouvez reconfigurer manuellement les stratégies de réseau avec le nouveau certificat.

4. Une fois que l’ancien certificat expire, NPS commence automatiquement à l’aide du nouveau certificat. 

5. Si vous avez configuré le serveur NPS pour utiliser la journalisation de SQL Server, vérifiez que la connectivité entre l’ordinateur exécutant SQL Server et le serveur NPS fonctionne correctement.

