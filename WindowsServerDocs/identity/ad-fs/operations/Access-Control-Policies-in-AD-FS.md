---
ms.assetid: 102eeeb1-6c55-42a2-b321-71a7dab46146
title: Stratégies de contrôle d’accès dans ADFS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 101cab68d7c79bb107f1d6ef73900d9a4475b6ea
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861300"
---
# <a name="access-control-policies-in-windows-server-2016-ad-fs"></a>Stratégies de contrôle d’accès dans AD FS Windows Server 2016

>S'applique à : Windows Server 2016

  
## <a name="access-control-policy-templates-in-ad-fs"></a>Modèles de stratégie de contrôle de l’accès dans AD FS  
Active Directory Federation Services prend désormais en charge l’utilisation des modèles de stratégie de contrôle de l’accès.  À l’aide de modèles de stratégie de contrôle de l’accès, un administrateur peut appliquer des paramètres de stratégie en assignant le modèle de stratégie à un groupe des parties de confiance (RPs). Administrateur peut également mettre à jour le modèle de stratégie et les modifications s’appliqueront aux parties de confiance automatiquement s’il n’existe aucune interaction utilisateur nécessitée.  
  
## <a name="what-are-access-control-policy-templates"></a>Quels sont les modèles de stratégie de contrôle d’accès ?  
Le pipeline de core AD FS pour le traitement de la stratégie comprend trois phases : émission de l’authentification, autorisation et de revendication. Actuellement, les administrateurs AD FS ont de configurer une stratégie pour chacune de ces phases séparément.  Cela implique également de comprendre les implications de ces stratégies et si ces stratégies ont une inter dépendance. En outre, les administrateurs doivent comprendre les revendication règle langage et l’auteur des règles personnalisées pour activer une stratégie simple/common (par ex. bloquer l’accès externe).  
  
Quelle stratégie de contrôle d’accès modèles faire est de remplacer cet ancien modèle où les administrateurs doivent configurer des règles d’autorisation d’émission de revendications de langage.  Les applets de commande PowerShell ancien de règles d’autorisation d’émission s’appliquent toujours, mais il est mutuellement exclusif le nouveau modèle. Les administrateurs peuvent choisir d’utiliser le nouveau modèle ou l’ancien modèle.  Le nouveau modèle permet aux administrateurs de contrôler quand accorder l’accès, y compris faire appliquer l’authentification multifacteur.  
  
Modèles de stratégie de contrôle de l’accès utilisent un modèle d’autorisation.  Par défaut, aucune autre n’a accès et que l’accès doit être accordé explicitement.  Toutefois, il s’agit pas simplement un tout ou rien autoriser.  Les administrateurs peuvent ajouter des exceptions à la règle d’autorisation.  Par exemple, un administrateur peut souhaiter accorder l’accès basé sur un réseau spécifique en sélectionnant cette option et en spécifiant la plage d’adresses IP.  Toutefois, l’administrateur peut ajouter et d’exception, par exemple, l’administrateur peut ajouter une exception à partir d’un réseau spécifique et spécifiez la plage d’adresses IP.  
  
![stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP2.PNG)  
  
## <a name="built-in-access-control-policy-templates-vs-custom-access-control-policy-templates"></a>Modèles de stratégie contrôle les modèles Visual Studio accès personnalisé accès intégré contrôle stratégie  
AD FS comprend plusieurs modèles de stratégie de contrôle d’accès intégré.  Cibles des scénarios courants qui ont le même ensemble d’exigences de stratégie, par exemple les stratégie d’accès client pour Office 365.  Ces modèles ne peut pas être modifiés.  
  
![stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP3.PNG)  
  
Pour fournir une flexibilité accrue pour répondre aux besoins de votre entreprise, les administrateurs peuvent créer leur propre accès modèles de stratégie.  Elles peuvent être modifiées après la création et modifications apportées au modèle de stratégie personnalisée seront applique à tous les fournisseurs de ressources qui sont contrôlées par les modèles de stratégie.  Pour ajouter un modèle de stratégie personnalisé cliquez simplement ajouter une stratégie contrôle l’accès à partir de dans Gestion AD FS.  
  
Pour créer un modèle de stratégie, un administrateur doit tout d’abord spécifier quelles conditions une demande sera autorisée pour d’émission de jeton et/ou de délégation. Options de condition et une action s’affichent dans le tableau ci-dessous.   Conditions en gras peuvent être affinées par l’administrateur avec des valeurs différentes ou nouveau. Administrateur peut également spécifier des exceptions cas échéant. Lorsqu’une condition est remplie, une action d’autorisation ne sera pas déclenchée si une exception spécifiée et la demande entrante correspond à la condition spécifiée dans l’exception.  
  
|Autoriser les utilisateurs|À l’exception| 
| --- | --- | 
 |À partir de **spécifique** réseau|À partir de **spécifique** réseau<br /><br />À partir de **spécifique** groupes<br /><br />À partir d’appareils avec **spécifique** niveaux de confiance<br /><br />Avec **spécifique** revendications dans la demande|  
|À partir de **spécifique** groupes|À partir de **spécifique** réseau<br /><br />À partir de **spécifique** groupes<br /><br />À partir d’appareils avec **spécifique** niveaux de confiance<br /><br />Avec **spécifique** revendications dans la demande|  
|À partir d’appareils avec **spécifique** niveaux de confiance|À partir de **spécifique** réseau<br /><br />À partir de **spécifique** groupes<br /><br />À partir d’appareils avec **spécifique** niveaux de confiance<br /><br />Avec **spécifique** revendications dans la demande|  
|Avec **spécifique** revendications dans la demande|À partir de **spécifique** réseau<br /><br />À partir de **spécifique** groupes<br /><br />À partir d’appareils avec **spécifique** niveaux de confiance<br /><br />Avec **spécifique** revendications dans la demande|  
|Et exiger une authentification multifacteur|À partir de **spécifique** réseau<br /><br />À partir de **spécifique** groupes<br /><br />À partir d’appareils avec **spécifique** niveaux de confiance<br /><br />Avec **spécifique** revendications dans la demande|  
  
Si un administrateur sélectionne plusieurs conditions, elles sont de **AND** relation. Les actions sont mutuellement exclusifs et de règle d’une stratégie, vous pouvez uniquement choisir une action. Si l’administrateur sélectionne plusieurs exceptions, ils sont d’un **ou** relation. Voici quelques exemples de règle de stratégie :  
  
|**Règlement**|**Règles de stratégie**|
| --- | --- |  
|MFA obligatoire pour l’accès extranet<br /><br />Tous les utilisateurs sont autorisés.|**Règle #1**<br /><br />à partir de **extranet**<br /><br />et avec l’authentification Multifacteur<br /><br />Autoriser<br /><br />**Rule#2**<br /><br />from **intranet**<br /><br />Autoriser|  
|Accès externe ne sont pas autorisées à l’exception non FTE<br /><br />Accès à l’intranet pour FTE sur un appareil joint à un espace de travail sont autorisés.|**Règle #1**<br /><br />À partir de **extranet**<br /><br />et à partir de **non FTE** groupe<br /><br />Autoriser<br /><br />**Règle #2**<br /><br />from **intranet**<br /><br />et à partir de **rattaché** appareil<br /><br />et à partir de **FTE** groupe<br /><br />Autoriser|  
|MFA obligatoire pour l’accès extranet à l’exception de « service admin »<br /><br />Tous les utilisateurs sont autorisés à accéder|**Règle #1**<br /><br />à partir de **extranet**<br /><br />et avec l’authentification Multifacteur<br /><br />Autoriser<br /><br />À l’exception **groupe d’administration de service**<br /><br />**Règle #2**<br /><br />Toujours<br /><br />Autoriser|  
|APPAREIL joint à un lieu de travail non - accès à partir d’extranet exige l’authentification Multifacteur<br /><br />Autoriser l’infrastructure AD pour l’accès intranet et extranet|**Règle #1**<br /><br />from **intranet**<br /><br />Et à partir de **AD Fabric** groupe<br /><br />Autoriser<br /><br />**Règle #2**<br /><br />à partir de **extranet**<br /><br />et à partir de **non-rattaché** appareil<br /><br />et à partir de **AD Fabric** groupe<br /><br />et avec l’authentification Multifacteur<br /><br />Autoriser<br /><br />**Règle #3**<br /><br />à partir de **extranet**<br /><br />et à partir de **rattaché** appareil<br /><br />et à partir de **AD Fabric** groupe<br /><br />Autoriser|  
  
## <a name="parameterized-policy-template-vs-non-parameterized-policy-template"></a>Modèle de stratégie non paramétrées stratégie paramétrable modèle vs  
Stratégies de contrôle d’accès peuvent être  
  
Un modèle de stratégie paramétrable est un modèle de stratégie comportant des paramètres. Un administrateur a besoin d’entrer la valeur de ces paramètres lors de l’attribution de ce modèle à RPs.An administrateur ne peut pas apporter des modifications au modèle de stratégie paramétrable après que qu’il a été créé.  Un exemple d’une stratégie paramétrable est la stratégie intégrée, un groupe spécifique d’autoriser.  Chaque fois que cette stratégie est appliquée à une partie de confiance, ce paramètre doit être spécifié.  
  
![stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP5.PNG)  
  
Un modèle de stratégie non paramétrable est un modèle de stratégie qui n’a pas de paramètres. Un administrateur peut affecter ce modèle RPs sans aucune entrée nécessitée et peut apporter des modifications à un modèle de stratégie non paramétrées après que qu’il a été créé.  Un exemple de ceci est la stratégie intégrée, autoriser tout le monde et exiger une authentification Multifacteur.  
  
![stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP4.PNG)  
  
## <a name="how-to-create-a-non-parameterized-access-control-policy"></a>Comment créer une stratégie de contrôle d’accès non paramétrées  
Pour créer un accès non paramétrée, stratégie de contrôle d’utiliser la procédure suivante  
  
#### <a name="to-create-a-non-parameterized-access-control-policy"></a>Pour créer une stratégie de contrôle d’accès non paramétrées  
  
1.  À partir de gestion AD FS sur la gauche, sélectionnez Stratégies de contrôle d’accès et sur la droite, cliquez sur Ajouter une stratégie de contrôle d’accès.  
  
2.  Entrez un nom et une description.  Exemple :  Autoriser les utilisateurs avec des appareils authentifiés.  
  
3.  Sous **autoriser l’accès si une des règles suivantes est remplie**, cliquez sur **ajouter**.  
  
4.  Sous Autoriser, cochez la case dans la zone à côté **à partir d’appareils avec le niveau de confiance spécifique**  
  
5.  En bas, sélectionnez le texte souligné **spécifique**  
  
6.  Dans la fenêtre pop-up autrement, sélectionnez **authentifié** à partir de la liste déroulante.  Cliquez sur **OK**.  
  
    ![stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP6.PNG)  
  
7.  Cliquez sur **OK**. Cliquez sur **OK**.  
  
    ![stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP7.PNG)  
  
## <a name="how-to-create-a-parameterized-access-control-policy"></a>Comment créer une stratégie de contrôle d’accès paramétrable  
Pour créer un contrôle d’accès paramétrable stratégie procédez comme suit  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>Pour créer une stratégie de contrôle d’accès paramétrable  
  
1.  À partir de gestion AD FS sur la gauche, sélectionnez Stratégies de contrôle d’accès et sur la droite, cliquez sur Ajouter une stratégie de contrôle d’accès.  
  
2.  Entrez un nom et une description.  Exemple :  Autoriser les utilisateurs avec une revendication spécifique.  
  
3.  Sous **autoriser l’accès si une des règles suivantes est remplie**, cliquez sur **ajouter**.  
  
4.  Sous Autoriser, cochez la case dans la zone à côté **avec des revendications spécifiques dans la demande**  
  
5.  En bas, sélectionnez le texte souligné **spécifique**  
  
6.  Dans la fenêtre pop-up autrement, sélectionnez **paramètre spécifié lorsque la stratégie de contrôle d’accès est attribuée**.  Cliquez sur **OK**.  
  
    ![stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP8.PNG)  
  
7.  Cliquez sur **OK**. Cliquez sur **OK**.  
  
    ![stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP9.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-an-exception"></a>Comment créer une stratégie de contrôle d’accès personnalisé avec une exception  
Pour créer un contrôle d’accès stratégie avec une exception utiliser la procédure suivante.  
  
#### <a name="to-create-a-custom-access-control-policy-with-an-exception"></a>Pour créer une stratégie de contrôle d’accès personnalisé avec une exception  
  
1.  À partir de gestion AD FS sur la gauche, sélectionnez Stratégies de contrôle d’accès et sur la droite, cliquez sur Ajouter une stratégie de contrôle d’accès.  
  
2.  Entrez un nom et une description.  Exemple :  Autoriser les utilisateurs avec authentifié des appareils, mais pas géré.  
  
3.  Sous **autoriser l’accès si une des règles suivantes est remplie**, cliquez sur **ajouter**.  
  
4.  Sous Autoriser, cochez la case dans la zone à côté **à partir d’appareils avec le niveau de confiance spécifique**  
  
5.  En bas, sélectionnez le texte souligné **spécifique**  
  
6.  Dans la fenêtre pop-up autrement, sélectionnez **authentifié** à partir de la liste déroulante.  Cliquez sur **OK**.  
  
7.  Sous sauf, cochez la case à côté dans la zone **à partir d’appareils avec le niveau de confiance spécifique**  
  
8.  En bas, sauf sous, sélectionnez le texte souligné **spécifique**  
  
9. Dans la fenêtre pop-up autrement, sélectionnez **gérés** à partir de la liste déroulante.  Cliquez sur **OK**.  
  
10. Cliquez sur **OK**. Cliquez sur **OK**.  
  
    ![stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP10.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-multiple-permit-conditions"></a>Comment créer une stratégie de contrôle d’accès personnalisé avec plusieurs conditions de l’autorisation  
Pour créer une stratégie de contrôle d’accès avec autorisation de plusieurs conditions procédez comme suit  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>Pour créer une stratégie de contrôle d’accès paramétrable  
  
1.  À partir de gestion AD FS sur la gauche, sélectionnez Stratégies de contrôle d’accès et sur la droite, cliquez sur Ajouter une stratégie de contrôle d’accès.  
  
2.  Entrez un nom et une description.  Exemple :  Autoriser les utilisateurs avec une revendication spécifique et à partir de groupe spécifique.  
  
3.  Sous **autoriser l’accès si une des règles suivantes est remplie**, cliquez sur **ajouter**.  
  
4.  Sous Autoriser, cochez la case dans la zone à côté **à partir d’un groupe spécifique** et **avec des revendications spécifiques dans la demande**  
  
5.  En bas, sélectionnez le texte souligné **spécifique** pour la première condition, en regard des groupes  
  
6.  Dans la fenêtre pop-up autrement, sélectionnez **paramètre spécifié lorsque la stratégie est affectée**.  Cliquez sur **OK**.  
  
7.  En bas, sélectionnez le texte souligné **spécifique** pour la deuxième condition, en regard de revendications  
  
8.  Dans la fenêtre pop-up autrement, sélectionnez **paramètre spécifié lorsque la stratégie de contrôle d’accès est attribuée**.  Cliquez sur **OK**.  
  
9. Cliquez sur **OK**. Cliquez sur **OK**.  
  
![stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP12.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-a-new-application"></a>Comment attribuer une stratégie de contrôle d’accès à une nouvelle application  
Affectation d’une stratégie de contrôle d’accès à une nouvelle application est relativement simple et a maintenant été intégrée dans l’Assistant pour l’ajout d’un fournisseur de ressources.  À partir de l’Assistant d’approbation de partie de confiance de confiance, vous pouvez sélectionner la stratégie de contrôle d’accès que vous souhaitez affecter.  Il s’agit d’une exigence lors de la création d’une nouvelle partie de confiance.  
  
![stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP13.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-an-existing-application"></a>Guide pratique pour attribuer une stratégie de contrôle d’accès à une application existante  
Affectation d’une stratégie de contrôle d’accès à une application existante Sélectionnez simplement l’application à partir de la partie de confiance et sur le bouton droit sur **modifier une stratégie de contrôle d’accès**.  
  
![stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP14.PNG)  
  
À ce stade, vous pouvez sélectionner la stratégie de contrôle d’accès et s’appliquent à l’application.  
  
![stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP15.PNG)  
  
## <a name="see-also"></a>Voir aussi  
[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md) 

