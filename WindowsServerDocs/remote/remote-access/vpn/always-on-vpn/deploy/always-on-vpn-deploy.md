---
title: Déploiement du VPN Toujours actif (AlwaysOn) pour Windows Server et Windows 10
description: Vous pouvez utiliser ce déploiement pour déployer des connexions toujours sur réseau privé virtuel (VPN) pour les employés distants à l’aide d’un accès à distance dans Windows Server 2016 ou version ultérieure et les profils VPN Always On pour les ordinateurs clients Windows 10.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 5ae1a40b-4f10-4ace-8aaf-13f7ab581f4f
ms.localizationpriority: medium
ms.date: 12/20/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 533f0273f6802be209ae5ad79b57f46dd6775149
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749472"
---
# <a name="always-on-vpn-deployment-for-windows-server-and-windows-10"></a>Déploiement de VPN Always On pour Windows Server et Windows 10

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Précédent :** Accès à distance](../../../Remote-Access.md)<br>
- [**prochain :** En savoir plus sur les fonctionnalités de VPN Always On](../../vpn-map-da.md)

VPN Always On fournit une solution unique et cohérente pour l’accès à distance et prend en charge joints au domaine, joint à un domaine (groupe de travail) ou Azure AD : appareils joints à un, même les appareils personnels. Avec VPN Toujours actif (AlwaysOn), le type de connexion ne doit pas nécessairement être exclusivement utilisateur ou appareil, mais peut être une combinaison des deux. Par exemple, vous pouvez activer l'authentification de l’appareil pour la gestion d’appareils à distance, puis activer l'authentification utilisateur pour la connectivité aux sites et services internes de la société.

## <a name="prerequisites"></a>Prérequis

Vous avez probablement accès les technologies de déploiement que vous pouvez utiliser pour déployer le VPN Always On. Autre que vos serveurs de contrôleur de domaine/DNS, le déploiement de VPN Always On nécessite un serveur NPS (RADIUS), un serveur d’autorité de Certification (CA) et un serveur d’accès à distance (routage/VPN). Une fois que l’infrastructure est configuré, vous devez inscrire les clients et puis connecter les clients à votre réseau local en toute sécurité par le biais de plusieurs modifications de réseau.

- Infrastructure Active Directory domaine, y compris un ou plusieurs serveurs de système DNS (Domain Name). Les zones système DNS (Domain Name) internes et externes sont nécessaires, ce qui suppose que la zone interne est un sous-domaine délégué de la zone externe (par exemple, corp.contoso.com et contoso.com).
- Active Directory basé infrastructure à clé publique (PKI) et les Services de certificats Active Directory (AD CS).
- Serveur, virtuel ou physique, existant ou nouveau, pour installer le serveur NPS (Network Policy Server). Si vous avez déjà des serveurs NPS sur votre réseau, vous pouvez modifier une configuration de serveur NPS existante plutôt que d’ajouter un nouveau serveur.
- Accès à distance comme un serveur VPN de passerelle RAS avec un petit sous-ensemble de fonctionnalités prenant en charge les connexions VPN IKEv2 et le routage de réseau local.
- Réseau de périmètre qui inclut deux pare-feu.  Vérifiez que vos pare-feu autorise le trafic qui est nécessaire pour les communications de VPN et RADIUS fonctionner correctement. Pour plus d’informations, consultez [toujours sur VPN présentation de la technologie](../always-on-vpn-technology-overview.md).
- Serveur physique ou machine virtuelle (VM) sur votre réseau de périmètre avec deux cartes réseau Ethernet physiques pour installer l’accès à distance comme un serveur VPN de passerelle RAS. Machines virtuelles nécessitent un réseau local virtuel (VLAN) pour l’hôte. 
- L’appartenance à des administrateurs, ou équivalent, est la condition minimale requise.
- Lisez la section Planification de ce guide pour vous assurer que vous êtes prêt pour ce déploiement avant d’effectuer le déploiement.
- Passez en revue les guides de conception et de déploiement pour chacune des technologies utilisées. Ces guides peuvent vous aider à déterminer si les scénarios de déploiement fournissent les services et la configuration dont vous avez besoin pour le réseau de votre organisation. Pour plus d’informations, consultez [toujours sur VPN présentation de la technologie](../always-on-vpn-technology-overview.md).
- Plateforme de gestion de votre choix pour le déploiement de la configuration VPN Always On, car le CSP n’est pas spécifique au fournisseur.

>[!IMPORTANT]
>Pour ce déploiement, il n’est pas obligatoire que vos serveurs d’infrastructure, tels que des ordinateurs exécutant les Services de domaine Active Directory, Services de certificats Active Directory et serveur NPS, exécutent Windows Server 2016. Vous pouvez utiliser les versions antérieures de Windows Server, tels que Windows Server 2012 R2, pour les serveurs d’infrastructure et pour le serveur qui a accès à distance est en cours d’exécution.
>
>N’essayez pas de déployer l’accès à distance sur une machine virtuelle (VM) dans Microsoft Azure. L’accès à distance dans Microsoft Azure n'est pas pris en charge, y compris l’accès à distance VPN et DirectAccess. Pour plus d’informations, consultez [prise en charge du logiciel Microsoft server pour machines virtuelles Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

## <a name="about-this-deployment"></a>À propos de ce déploiement

Les instructions fournies vous guident dans le processus de déploiement de l’accès à distance en tant qu’un seul locataire passerelle RAS de VPN pour les connexions VPN de point-to-site, à l’aide de tous les scénarios mentionnés ci-dessous, pour les ordinateurs clients distants qui exécutent Windows 10. Vous trouverez également des instructions pour modifier certains de votre infrastructure existante pour le déploiement. Tout au long de ce déploiement, vous trouverez des liens pour vous aider à en savoir plus sur le processus de connexion VPN, les serveurs à configurer, ProfileXML VPNv2 CSP nœud et autres technologies de déploiement VPN Always On.

**Scénarios de déploiement VPN Always On :**

1. Déployez toujours sur le VPN uniquement.
2. Déployer le VPN Always On avec l’accès conditionnel pour la connectivité VPN à l’aide d’Azure AD.

Pour plus d’informations et des flux de travail des scénarios décrits, consultez [déployer VPN Always On](always-on-vpn-deploy-deployment.md).

## <a name="what-isnt-provided-in-this-deployment"></a>Ce qui n’est pas fourni dans ce déploiement

Ce déploiement ne fournit pas d’instructions pour :

- Services de domaine Active Directory (AD DS).
- Services de certificats Active Directory (AD CS) et une Infrastructure à clé publique (PKI).
- Dynamic Host Configuration Protocol (DHCP).
- Réseau matériel, comme le câblage Ethernet, les pare-feux, les commutateurs et les hubs.
- Ressources réseau supplémentaires, tels que les serveurs de fichiers et d’applications, que les utilisateurs distants peuvent accéder via une connexion VPN Always On.
- Connectivité Internet ou l’accès conditionnel pour la connectivité Internet à l’aide d’Azure AD. Pour plus d’informations, consultez [accès conditionnel dans Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).

## <a name="next-steps"></a>Étapes suivantes

- [En savoir plus sur les fonctionnalités de VPN Always On](../../vpn-map-da.md)

- [En savoir plus sur les améliorations de VPN Always On](../always-on-vpn-enhancements.md)

- [En savoir plus sur certaines des fonctionnalités avancées VPN Always On](always-on-vpn-adv-options.md)

- [En savoir plus sur la technologie VPN Always On](../always-on-vpn-technology-overview.md)

- [Commencer à planifier votre déploiement VPN Always On](always-on-vpn-deploy-deployment.md)