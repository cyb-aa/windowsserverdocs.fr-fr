---
ms.assetid: 2a5d5271-6ac6-4c1b-b4ef-9b568932a55a
title: Mises à jour des schémas de l’échelle du domaine actives Directory
description: Mises à jour du schéma de l’échelle du domaine effectuées par adprep /domainprep lors de la promotion d’un contrôleur de domaine
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 20598431b9c2376051247fd7ba14e33af00968cc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812320"
---
# <a name="domain-wide-schema-updates"></a>Mises à jour du schéma de l’échelle du domaine

>S'applique à : Windows Server

Vous pouvez consulter l’ensemble suivant de modifications pour aider à comprendre et à préparer les mises à jour de schéma sont effectuées par adprep /domainprep dans Windows Server.

À compter de Windows Server 2012, les commandes Adprep s’exécutent automatiquement en fonction des besoins lors de l’installation d’AD DS. Ils peuvent également être exécutés séparément avant l’installation d’AD DS. Pour plus d’informations, voir [Exécution d’Adprep.exe](https://technet.microsoft.com/library/dd464018(v=ws.10).aspx).

Pour plus d’informations sur la façon d’interpréter les chaînes d’entrée du contrôle accès, consultez [chaînes d’ACE](https://msdn.microsoft.com/library/aa374928(VS.85).aspx). Pour plus d’informations sur la façon d’interpréter les chaînes d’ID (SID) de sécurité, consultez [les chaînes de SID](https://msdn.microsoft.com/library/aa379602(VS.85).aspx).

## <a name="windows-server-semi-annual-channel-domain-wide-updates"></a>Windows Server (canal semi-annuel) : Mises à jour de l’échelle du domaine

Après les opérations sont effectuées par **domainprep** dans Windows Server 2016 (opération 89) terminée, le **révision** attribut pour CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = Objet de domaine_racine_forêt est défini sur **16**.

|Nombre d’opérations et GUID|Description|Autorisations|
|------------------------------|---------------|--------------|---------------|
|**Opération 89**: {A0C238BA-9E30-4EE6-80A6-43F731E9A5CD}|Supprimer l’ACE accorder un contrôle total aux administrateurs de clé de l’entreprise et lui ajouter un ACE octroi Enterprise clé administrateurs un contrôle total sur simplement l’attribut msdsKeyCredentialLink.|Supprimer (A ; ÉLÉMENT DE CONFIGURATION ; RPWPCRLCLOCCDCRCWDWOSDDTSW ; ; Administrateurs de l’entreprise clé) <br /> <br />Ajouter (OA ; ÉLÉMENT DE CONFIGURATION ; RPWP ; 5b47d60f-6090-40b2-9f37-2a4de88f3063 ; Administrateurs de l’entreprise clé)|

## <a name="windows-server-2016-domain-wide-updates"></a>Windows Server 2016 : Mises à jour de l’échelle du domaine

Après les opérations sont effectuées par **domainprep** dans Windows Server 2016 (opérations 82-88) terminée, le **révision** attribut pour CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = domaine_racine_forêt objet est défini sur **15**.

|Nombre d’opérations et GUID|Description|Attributs|Autorisations|
|------------------------------|---------------|--------------|---------------|
|**Opération 82**: {83C53DA7-427E-47A4-A07A-A324598B88F7}|Créer CN = conteneur de clés à la racine du domaine|-objectClass : conteneur<br />-description : Conteneur par défaut pour les objets d’informations d’identification de clé<br />- ShowInAdvancedViewOnly: TRUE|(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;EA)<br />(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;DA)<br />(A ; ÉLÉMENT DE CONFIGURATION ; RPWPCRLCLOCCDCRCWDWOSDDTSW ; ; SY)<br />(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;DD)<br />(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;ED)|
|**Opération 83**: {C81FC9CC-0130-4FD1-B272-634D74818133}|Ajouter un contrôle total Autoriser des ACE pour CN = conteneur de clés pour « domain\Key administrateurs » et « rootdomain\Enterprise administrateurs de clé ».|N/A|(A ; ÉLÉMENT DE CONFIGURATION ; RPWPCRLCLOCCDCRCWDWOSDDTSW ; ; Administrateurs de clé)<br />(A ; ÉLÉMENT DE CONFIGURATION ; RPWPCRLCLOCCDCRCWDWOSDDTSW ; ; Administrateurs de l’entreprise clé)|
|**Opération 84**: {E5F9E791-D96D-4FC9-93C9-D53E1DC439BA}|Modifier l’attribut otherWellKnownObjects pour pointer vers le CN = conteneur de clés.|-otherWellKnownObjects : B:32:683A24E2E8164BD3AF86AC3C2CF3F981:CN=Keys,%ws|N/A|
|**Opération 85**: {e6d5fd00-385d-4e65-b02d-9da3493ed850}|Modifier le domaine du contrôleur de réseau pour autoriser le « domain\Key administrateurs » et « administrateurs de clé rootdomain\Enterprise » pour modifier l’attribut msds-KeyCredentialLink. |N/A|(OA ; ÉLÉMENT DE CONFIGURATION ; RPWP ; 5b47d60f-6090-40b2-9f37-2a4de88f3063 ; Administrateurs de clé)<br />(OA ; ÉLÉMENT DE CONFIGURATION ; RPWP ; 5b47d60f-6090-40b2-9f37-2a4de88f3063 ; Administrateurs de clé d’entreprise dans le domaine racine, mais dans des domaines non racine a entraîné un faux ACE relatifs à un domaine avec un SID-527 non résolu)|
|**Opération 86**: {3a6b3fbf-3168-4312-a10d-dd5b3393952d}|Accorder la voiture DS-validé-écriture-Computer et Créateur propriétaire self|N/A|(OA;CIIO;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11d0-a285-00aa003049e2;PS)<br />(OA;CIIO;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11d0-a285-00aa003049e2;CO)|
|**Opération 87**: {7F950403-0AB3-47F9-9730-5D7B0269F9BD}|Supprimer l’ACE accorder un contrôle total au groupe Administrateurs de l’entreprise clé relatifs à un domaine incorrect et ajouter un ACE d’accorder un contrôle total au groupe Administrateurs de clé d’entreprise. |N/A|Supprimer (A ; ÉLÉMENT DE CONFIGURATION ; RPWPCRLCLOCCDCRCWDWOSDDTSW ; ; Administrateurs de l’entreprise clé)<br /> <br />Ajouter (A ; ÉLÉMENT DE CONFIGURATION ; RPWPCRLCLOCCDCRCWDWOSDDTSW ; ; Administrateurs de l’entreprise clé)|
|**Opération 88**: {434bb40d-dbc9-4fe7-81d4-d57229f7b080}|Ajouter « msDS-ExpirePasswordsOnSmartCardOnlyAccounts » sur l’objet de contexte de nommage de domaine et la valeur FALSE à la valeur par défaut|N/A|N/A|

Les groupes Administrateurs de clé de l’entreprise et administrateurs de clé sont uniquement créés après qu’un contrôleur de domaine Windows Server 2016 est promu et adopte le rôle FSMO d’émulateur PDC.

## <a name="windows-server-2012-r2-domain-wide-updates"></a>Windows Server 2012 R2 : Mises à jour de l’échelle du domaine

Bien qu’aucune opération est effectuée par **domainprep** dans Windows Server 2012 R2, une fois la commande terminée, le **révision** attribut pour CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = domaine_racine_forêt objet est défini sur **10**.

## <a name="windows-server-2012-domain-wide-updates"></a>Windows Server 2012 : Mises à jour de l’échelle du domaine

Après les opérations sont effectuées par **domainprep** dans Windows Server 2012 (opérations 78, 79, 80 et 81) terminée, le **révision** attribut pour CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = domaine_racine_forêt objet est défini sur **9**.

|Nombre d’opérations et GUID|Description|Attributs|Autorisations|
|------------------------------|---------------|--------------|---------------|
|**Opération 78**: {c3c927a6-cc1d-47c0-966b-be8f9b63d991}|Créez un nouvel objet CN = appareils TPM dans la partition de domaine.|Classe d’objet : msTPM-InformationObjectsContainer|N/A|
|**Opération 79**: {54afcfb9-637a-4251-9f47-4d50e7021211}|Création d’une entrée de contrôle d’accès pour le service de module de plateforme sécurisée.|N/A|(OA;CIIO;WP;ea1b7b93-5e48-46d5-bc6c-4df4fda78a35;bf967a86-0de6-11d0-a285-00aa003049e2;PS)|
|**Opération 80**: {f4728883-84dd-483c-9897-274f2ebcf11e}|Accorder « Clone DC » étendu de droite à **contrôleurs de domaine clonables** groupe|N/A|(OA ; CR ; 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e ; *SID du domaine*-522)|
|**Opération 81**: {ff4f9d27-7157-4cb0-80a9-5d6f2b14c8ff}|Octroyer ms-DS-Allowed-To-Act-On-Behalf-Of-Other-Identity à Principal Self sur tous les objets.|N/A|(OA;CIOI;RPWP;3f78c3e5-f79a-46bd-a0b8-9d18116ddc79;;PS)|
