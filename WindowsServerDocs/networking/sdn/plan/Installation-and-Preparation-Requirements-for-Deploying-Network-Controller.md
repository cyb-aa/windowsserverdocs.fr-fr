---
title: Installation et configuration requise de préparation pour le déploiement de contrôleur de réseau
description: Vous pouvez utiliser cette rubrique pour préparer votre centre de données pour le déploiement du contrôleur de réseau.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 7f899e62-6e5b-4fca-9a59-130d4766ee2f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c50a2167a894871ea76fb96523c19531100648ce
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="installation-and-preparation-requirements-for-deploying-network-controller"></a>Installation et configuration requise de préparation pour le déploiement de contrôleur de réseau

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour préparer votre centre de données pour le déploiement du contrôleur de réseau.  
  
> [!NOTE]  
> Outre cette rubrique, la documentation suivante sur le contrôleur de réseau est disponible.  
> 
> - [Contrôleur de réseau](../technologies/network-controller/Network-Controller.md)
> - [Haute disponibilité du contrôleur de réseau](../technologies/network-controller/network-controller-high-availability.md)
> - [Déployer le contrôleur de réseau à l’aide de Windows PowerShell](../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)  
> - [Installer le rôle de serveur de contrôleur de réseau à l’aide du Gestionnaire de serveur](../technologies/network-controller/Install-the-Network-Controller-server-role-using-Server-Manager.md)  

Voici les étapes d’installation et logiciels et autres exigences et de préparation que vous devez prendre avant de déployer le contrôleur de réseau.

## <a name="installation-requirements"></a>Conditions d’installation

Voici la configuration requise pour le contrôleur de réseau.

- Pour les déploiements de Windows Server2016, vous pouvez déployer le contrôleur de réseau sur un ou plusieurs ordinateurs, un ou plusieurs ordinateurs virtuels ou une combinaison des ordinateurs et des ordinateurs virtuels. Tous les ordinateurs virtuels et ordinateurs planifiées en tant que nœuds de contrôleur de réseau doivent exécuter Windows Server2016 Datacenter edition.

## <a name="software-requirements"></a>Configuration logicielle requise

Déploiement de contrôleur de réseau requiert un ou plusieurs ordinateurs ou ordinateurs virtuels qui servent le contrôleur de réseau et à un ordinateur ou une machine virtuelle à utiliser comme un client de gestion pour le contrôleur de réseau. Ces ordinateurs ou les ordinateurs virtuels doivent exécuter les systèmes d’exploitation suivants.  

- N’importe quel ordinateur ou une machine virtuelle (VM) sur lequel vous installez le contrôleur de réseau doit exécuter l’Édition Datacenter de Windows Server2016.  
  
- L’ordinateur client de gestion ou d’une machine virtuelle pour le contrôleur de réseau doit être en cours d’exécution Windows8, Windows8.1 ou Windows10.  
  
## <a name="additional-requirements"></a>Configuration supplémentaire requise

Voici les étapes supplémentaires à suivre avant de déployer le contrôleur de réseau.
  
### <a name="configure-security-groups"></a>Configurer des groupes de sécurité
  
Si les ordinateurs ou les ordinateurs virtuels pour le contrôleur de réseau et le client de gestion sont joints au domaine, configurez les groupes de sécurité suivants pour l’authentification Kerberos.

- Créer un groupe de sécurité et ajoutez tous les utilisateurs qui sont autorisés à configurer le contrôleur de réseau. Par exemple, créez un groupe nommé **administrateurs du contrôleur de réseau **. Tous les utilisateurs que vous ajoutez à ce groupe doivent également être membres de la **utilisateurs du domaine** groupe dans ActiveDirectory Users and Computers.  
  
    > [!NOTE]  
    > Pour plus d’informations sur la création d’un groupe dans ActiveDirectory Users and Computers, voir [créer un nouveau groupe ](https://technet.microsoft.com/en-us/library/cc783256(v=ws.10).aspx).  

- Créer un groupe de sécurité et ajoutez tous les utilisateurs qui sont autorisés à configurer et gérer le réseau à l’aide du contrôleur de réseau.  Par exemple, créer un groupe nommé **utilisateurs du contrôleur réseau **. Tous les utilisateurs que vous ajoutez au nouveau groupe doivent également être membres de la **utilisateurs du domaine** groupe dans ActiveDirectory Users and Computers. Configuration du contrôleur de réseau et la gestion est effectuée à l’aide de \(REST\) Representational State Transfer.

### <a name="configure-log-file-locations-if-needed"></a>Configurer les emplacements des fichiers journaux si nécessaire

Vous pouvez stocker les journaux de débogage de contrôleur de réseau sur l’ordinateur de contrôleur de réseau ou d’un ordinateur virtuel ou sur un partage de fichiers distant. Si vous souhaitez stocker les journaux dans un partage de fichiers distant, assurez-vous que le partage est accessible à partir du contrôleur de réseau.

### <a name="configure-dynamic-dns-registration-for-network-controller"></a>Configurer l’inscription DNS dynamique pour le contrôleur de réseau
  
Vous pouvez déployer les nœuds de cluster de contrôleur de réseau sur le même sous-réseau ou sur des sous-réseaux différents. 

>[!NOTE]
>Si les nœuds de contrôleur de réseau sont sur le même sous-réseau, vous devez fournir l’adresse IP du contrôleur de réseau reste lorsque vous configurez l’inscription DNS dynamique pour le contrôleur de réseau. Si les nœuds sont sur différents sous-réseaux, vous devez fournir le nom DNS du contrôleur de réseau reste lorsque vous configurez l’inscription DNS dynamique.

Si les nœuds de contrôleur de réseau sont sur des sous-réseaux différents, vous devez effectuer la configuration DNS supplémentaire suivante:

- Créer un nom DNS pour le contrôleur de réseau pendant le processus de déploiement

- Configurer les mises à jour dynamiques DNS pour le nom DNS du contrôleur réseau sur le serveur DNS

- Limiter les mises à jour dynamiques DNS contrôleur de réseau uniquement aux nœuds

Vous pouvez utiliser les procédures suivantes pour configurer les mises à jour dynamiques DNS et pour restreindre la mise à jour dynamique de l’enregistrement du nom du contrôleur de réseau.

> [!NOTE]
> L’appartenance au groupe **Admins du domaine**, ou équivalente, est la condition minimale requise pour effectuer ces procédures.
  
#### <a name="to-allow-dns-dynamic-updates-for-a-zone"></a>Pour autoriser les mises à jour dynamiques DNS pour une zone

1. Ouvrez le Gestionnaire DNS.

2. Dans l’arborescence de la console, cliquez sur la zone concernée, puis cliquez sur **propriétés**. La zone **propriétés** boîte de dialogue s’ouvre.

3. Sur le **général**, vérifiez que le type de zone est **principal** ou **intégré à ActiveDirectory**.

4. Dans **mises à jour dynamiques**, vérifiez que **sécurisé uniquement** est sélectionné. Si elle n’est pas sélectionnée, modifiez la valeur de **mises à jour dynamiques** à **sécurisé uniquement**, puis cliquez sur **OK**.

#### <a name="to-configure-dns-zone-security-permissions-for-network-controller-nodes"></a>Pour configurer les autorisations de sécurité de zone DNS pour les nœuds de contrôleur de réseau

1.  Ouvrez le Gestionnaire DNS.

2.  Dans l’arborescence de la console, cliquez sur la zone concernée, puis cliquez sur **propriétés**. La zone **propriétés** boîte de dialogue s’ouvre.

3.  Cliquez sur le **sécurité** onglet, puis cliquez sur **avancé**. Le **paramètres de sécurité avancés** boîte de dialogue s’ouvre.

4. Dans **paramètres de sécurité avancés**, cliquez sur **ajouter**. Le **entrée d’autorisation** boîte de dialogue s’ouvre.
  
5. Cliquez sur **sélectionnez un principal**. Le **sélectionner un utilisateur, ordinateur, compte de Service ou groupe** boîte de dialogue s’ouvre.

6. Dans le **sélectionner un utilisateur, ordinateur, compte de Service ou groupe** boîte de dialogue, cliquez sur **Types d’objet**. Le **Types d’objet** boîte de dialogue s’ouvre. 

7. Dans **Types d’objet**, sélectionnez **ordinateurs**, puis cliquez sur **OK**.

8. Dans le **sélectionner un utilisateur, ordinateur, compte de Service ou groupe** boîte de dialogue, tapez le nom NetBIOS d’un des nœuds du contrôleur de réseau dans votre déploiement, puis cliquez sur **OK**.

9. Dans **entrée d’autorisation**, vérifiez que la valeur de **Type** est **autoriser**et la valeur de **s’applique aux** est **cet objet et tous ceux descendants**.
  
10. Dans autorisations, sélectionnez **écrire toutes les propriétés** et **supprimer**, puis cliquez sur **OK**.

11. Répétez les étapes **5** via **10** pour tous les ordinateurs et les ordinateurs virtuels du cluster de contrôleur de réseau.

Pour plus d’informations, voir [planifier une Infrastructure de réseau défini par logiciel](https://technet.microsoft.com/windows-server-docs/networking/sdn/plan/plan-a-software-defined-network-infrastructure).
