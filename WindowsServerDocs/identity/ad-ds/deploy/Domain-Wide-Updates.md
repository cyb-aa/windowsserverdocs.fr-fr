---
ms.assetid: 2a5d5271-6ac6-4c1b-b4ef-9b568932a55a
title: "Mises à jour de schéma de domaine ActiveDirectory"
description: "Mises à jour de schéma de l’échelle du domaine effectuées par adprep /domainprep lors de la promotion d’un contrôleur de domaine"
author: MicrosoftGuyJFlo
ms.author: joflore
manager: femila
ms.date: 07/14/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 59efb7c854b7f3693776bf9a1a371f98c2206d60
ms.sourcegitcommit: 79c1359232bece2e5c3ee5507f1e4bf19b44f487
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2018
---
# <a name="domain-wide-schema-updates"></a>Mises à jour de schéma de l’échelle du domaine

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Vous pouvez passer en revue l’ensemble des modifications pour aider à comprendre et de préparer les mises à jour de schéma sont effectuées par adprep /domainprep dans Windows Server. 

À compter de Windows Server 2012, les commandes Adprep exécutent automatiquement en fonction des besoins lors de l’installation des services AD DS. Ils peuvent également être exécutés séparément avant l’installation des services AD DS. Pour plus d’informations, voir [exécution d’Adprep.exe](https://technet.microsoft.com/library/dd464018(v=ws.10).aspx).

Pour plus d’informations sur la façon d’interpréter les chaînes d’entrée de contrôle accès, voir [chaînes ACE](https://msdn.microsoft.com/library/aa374928(VS.85).aspx). Pour plus d’informations sur la façon d’interpréter les chaînes d’ID (SID) de sécurité, voir [chaînes SID](https://msdn.microsoft.com/library/aa379602(VS.85).aspx).

## <a name="windows-server-semi-annual-channel-domain-wide-updates"></a>Windows Server (canal annuel un point-virgule): Mises à jour du domaine

Après les opérations effectuées par **domainprep** dans Windows Server2016 (opération 89) terminée, le **révision** attribut pour CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = domaine_racine_forêt objet est défini sur **16**.

|Nombre d’opérations et les GUID|Description|Autorisations|
|------------------------------|---------------|--------------|---------------|
|**Opération 89**: {A0C238BA-9E30-4EE6-80A6-43F731E9A5CD}|Supprimer l’ACE octroi du contrôle total aux administrateurs de clé de l’entreprise et ajoutez un as accord entreprise clé administrateurs un contrôle total sur simplement l’attribut msdsKeyCredentialLink.|Supprimer (A; ÉLÉMENT DE CONFIGURATION; rPWPCRLCLOCCDCRCWDWOSDDTSW;; Administrateurs d’entreprise clés) <br /> <br />Ajoutez (ne disposant; ÉLÉMENT DE CONFIGURATION; RPWP; 5b47d60f-6090-40b2-9f37-2a4de88f3063; administrateurs d’entreprise clés)|

## <a name="windows-server-2016-domain-wide-updates"></a>Windows Server2016: Les mises à jour au niveau du domaine

Après les opérations effectuées par **domainprep** dans Windows Server2016 (opérations 82-88) terminée, le **révision** attribut pour CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = domaine_racine_forêt objet est défini sur **15**.

|Nombre d’opérations et les GUID|Description|Attributs|Autorisations|
|------------------------------|---------------|--------------|---------------|
|**Opération 82**: {83C53DA7-427E-47A4-A07A-A324598B88F7}|Créer CN = conteneur de clés à la racine du domaine|-objectClass: conteneur<br />-description: conteneur par défaut pour les objets de clé d’informations d’identification<br />-ShowInAdvancedViewOnly: TRUE|(A; ÉLÉMENT DE CONFIGURATION; rPWPCRLCLOCCDCRCWDWOSDDTSW;; EA)<br />(A; ÉLÉMENT DE CONFIGURATION; rPWPCRLCLOCCDCRCWDWOSDDTSW;; D A)<br />(A; ÉLÉMENT DE CONFIGURATION; rPWPCRLCLOCCDCRCWDWOSDDTSW;; SY)<br />(A; ÉLÉMENT DE CONFIGURATION; rPWPCRLCLOCCDCRCWDWOSDDTSW;; D D)<br />(A; ÉLÉMENT DE CONFIGURATION; rPWPCRLCLOCCDCRCWDWOSDDTSW;; ED)|
|**Opération 83**: {C81FC9CC-0130-4FD1-B272-634D74818133}|Ajouter un contrôle total Autoriser ACE à CN = conteneur de clés pour «domain\Key administrateurs» et «rootdomain\Enterprise administrateurs clé».|NON APPLICABLE|(A; ÉLÉMENT DE CONFIGURATION; rPWPCRLCLOCCDCRCWDWOSDDTSW;; Administrateurs de clé)<br />(A; ÉLÉMENT DE CONFIGURATION; rPWPCRLCLOCCDCRCWDWOSDDTSW;; Administrateurs d’entreprise clés)|
|**Opération 84**: {E5F9E791-D96D-4FC9-93C9-D53E1DC439BA}|Modifier l’attribut otherWellKnownObjects pour pointer vers le CN = conteneur de clés.|-otherWellKnownObjects: B:32:683A24E2E8164BD3AF86AC3C2CF3F981:CN = clés, %ws|NON APPLICABLE|
|**Opération 85**: {e6d5fd00-385d-4e65-b02d-9da3493ed850}|Modifier le domaine CN pour permettre la «domain\Key administrateurs» et «rootdomain\Enterprise administrateurs clé» pour modifier l’attribut msds-KeyCredentialLink. |NON APPLICABLE|(NE DISPOSANT; ÉLÉMENT DE CONFIGURATION; rPWP; 5b47d60f-6090-40b2-9f37-2a4de88f3063; administrateurs de clé)<br />(NE DISPOSANT; ÉLÉMENT DE CONFIGURATION; rPWP; 5b47d60f-6090-40b2-9f37-2a4de88f3063; administrateurs de l’entreprise clé dans le domaine racine, mais dans des domaines non racine a entraîné un ACE relatifs à un domaine fictif avec le SID-527 non résolu)|
|**Opération 86**: {3a6b3fbf-3168-4312-a10d-dd5b3393952d}|Accorder la voiture DS-validé-écriture-ordinateur créateur propriétaire et automatique|NON APPLICABLE|(NE DISPOSANT; cIIO;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11D0-a285-00aa003049e2;PS)<br />(NE DISPOSANT; cIIO;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11D0-a285-00aa003049e2;CO)|
|**Opération 87**: {7F950403-0AB3-47F9-9730-5D7B0269F9BD}|Supprimer l’ACE octroi du contrôle total au groupe Administrateurs de l’entreprise clé incorrect relatifs à un domaine et ajoutez un as octroi du contrôle total au groupe Administrateurs de clé d’entreprise. |NON APPLICABLE|Supprimer (A; ÉLÉMENT DE CONFIGURATION; rPWPCRLCLOCCDCRCWDWOSDDTSW;; Administrateurs d’entreprise clés)<br /> <br />Ajoutez (A; ÉLÉMENT DE CONFIGURATION; rPWPCRLCLOCCDCRCWDWOSDDTSW;; Administrateurs d’entreprise clés)|
|**Opération 88**: {434bb40d-dbc9-4fe7-81d4-d57229f7b080}|Ajoutez «msDS-ExpirePasswordsOnSmartCardOnlyAccounts» sur l’objet de contexte de nommage de domaine et définir la valeur par défaut à False (faux)|NON APPLICABLE|NON APPLICABLE|

Les groupes Administrateurs de clé de l’entreprise et administrateurs de clé sont créés uniquement après qu’un contrôleur de domaine Windows Server2016 est promu et prend le rôle FSMO d’émulateur de contrôleur de domaine principal.

## <a name="windows-server-2012-r2-domain-wide-updates"></a>Windows Server2012R2: Mises à jour du domaine

Même si aucune opérations ne sont effectuées par **domainprep** dans Windows Server2012R2, une fois la commande terminée, le **révision** attribut pour CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = domaine_racine_forêt objet est défini sur **10**.

## <a name="windows-server-2012-domain-wide-updates"></a>Windows Server2012: Les mises à jour au niveau du domaine

Après les opérations effectuées par **domainprep** dans Windows Server2012 (opérations 78, 79, 80 et 81) terminée, le **révision** attribut pour CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = domaine_racine_forêt objet est défini sur **9**.

|Nombre d’opérations et les GUID|Description|Attributs|Autorisations|
|------------------------------|---------------|--------------|---------------|
|**Opération 78**: {c3c927a6-cc1d-47c0-966b-be8f9b63d991}|Créer un nouvel objet CN = périphériques de module de plateforme sécurisée dans la partition de domaine.|Classe d’objet: msTPM-InformationObjectsContainer|NON APPLICABLE|
|**Opération 79**: {54afcfb9-637a-4251-9f47-4d50e7021211}|Créé une entrée de contrôle d’accès pour le service de module de plateforme sécurisée.|NON APPLICABLE|(NE DISPOSANT; CIIO; WP;ea1b7b93-5e48-46d5-bc6c-4df4fda78a35;bf967a86-0de6-11D0-a285-00aa003049e2;PS)|
|**Opération 80**: {f4728883-84dd-483c-9897-274f2ebcf11e}|Accorder «Clone du contrôleur de domaine «étendu vers la droite pour **contrôleurs de domaine clonables** groupe|NON APPLICABLE|(NE DISPOSANT; cR; 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e; *SID du domaine*-522)|
|**Opération 81**: {ff4f9d27-7157-4cb0-80a9-5d6f2b14c8ff}|Accordez ms-DS-Allowed-To-Act-On-Behalf-Of-Other-Identity à Principal Self sur tous les objets.|NON APPLICABLE|(NE DISPOSANT; CIOI; RPWP; 3f78c3e5-f79a-46bd-a0b8-9d18116ddc79; PS)|
