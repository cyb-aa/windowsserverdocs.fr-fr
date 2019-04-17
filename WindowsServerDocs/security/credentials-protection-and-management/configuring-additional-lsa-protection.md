---
title: "Configuration de la Protection LSA supplémentaire"
description: "Sécurité de Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 038e7c2b-c032-491f-8727-6f3f01116ef9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: fcfb0dab10d28413cf4ad06dd583274f217c91fa
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="configuring-additional-lsa-protection"></a>Configuration de la Protection LSA supplémentaire

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique destinée aux professionnels de l’informatique explique comment configurer une protection supplémentaire pour le processus de l’autorité de sécurité locale (LSA) en empêchant l’injection de code qui pourrait compromettre les informations d’identification.

L’autorité LSA, qui comprend le processus LSASS Local Security Authority Server Service (), valide les utilisateurs pour les connexions locales et distantes et applique les stratégies de sécurité locale. Le système d’exploitation Windows8.1 offre une protection supplémentaire pour l’autorité LSA empêcher la lecture de la mémoire et une injection de code par des processus non protégés. Cela fournit plus de sécurité pour les informations d’identification qui l’autorité LSA stocke et gère. Le paramètre de processus protégé pour l’autorité LSA peut être configuré dans Windows8.1, mais il ne peut pas être configuré dans WindowsRT8.1. Lorsque ce paramètre est utilisé conjointement avec le démarrage sécurisé, une protection supplémentaire est obtenue car la désactivation de la clé de registre HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa n’a aucun effet.

### <a name="protected-process-requirements-for-plug-ins-or-drivers"></a>Configuration requise de processus protégé pour les plug-ins ou pilotes
Pour un plug-ins LSA ou un pilote correctement chargé en tant que processus protégé, il doit respecter les critères suivants:

1.  Vérification de la signature

    Le mode protégé exige que tout plug-in qui est chargé dans l’autorité LSA est signé numériquement avec une signature Microsoft. Par conséquent, tous les plug-ins qui ne sont pas signés ou qui ne sont pas signés avec une signature Microsoft ne pourra pas charger dans LSA. Exemples de ces plug-ins sont des pilotes de carte à puce, les plug-ins de chiffrement et les filtres de mot de passe.

    Plug-ins LSA qui sont des pilotes, tels que des pilotes de carte à puce, doivent être signés à l’aide de la Certification WHQL. Pour plus d’informations, voir [Signature de version WHQL (pilotes Windows)](https://msdn.microsoft.com/library/windows/hardware/ff553976%28v=vs.85%29.aspx).

    Plug-ins LSA qui n’ont pas d’un processus de Certification WHQL doivent être signés à l’aide de la [fichier de signature de service pour LSA](https://go.microsoft.com/fwlink/?LinkId=392590).

2.  Adhésion au Guide de processus du cycle de vie de développement de sécurité Microsoft (SDL)

    Tous les plug-ins doivent être conformes au Guide de processus SDL applicable. Pour plus d’informations, voir la [annexe (MicrosoftSecurity Development Lifecycle)](https://msdn.microsoft.com/library/windows/desktop/cc307891.aspx).

    Même si les plug-ins sont correctement signés avec une signature Microsoft, une non-conformité avec le processus SDL peut entraîner Échec de chargement d’un plug-in.

#### <a name="recommended-practices"></a>Pratiques recommandées
Utilisez la liste suivante pour vérifier soigneusement que la protection LSA est activée avant de déployer globalement la fonctionnalité:

-   Identifiez tous les plug-ins LSA et pilotes qui sont en cours d’utilisation au sein de votre organisation. 
    Cela inclut des pilotes non Microsoft ou des plug-ins tels que les pilotes de carte à puce et les plug-ins de chiffrement, et un logiciel qui est utilisé pour appliquer des filtres de mot de passe ou notifications de modification de mot de passe développé en interne.

-   Assurez-vous que tous les plug-ins LSA sont signés numériquement avec un certificat Microsoft afin que le plug-in pas chargement échouera.

-   Vérifiez que tous les plug-ins correctement signés peuvent être chargés dans l’autorité LSA et qu’ils fonctionnent comme prévu.

-   Utilisez les journaux d’audit pour identifier les plug-ins et pilotes LSA qui ne parviennent pas à exécuter en tant que processus protégé.

## <a name="how-to-identify-lsa-plug-ins-and-drivers-that-fail-to-run-as-a-protected-process"></a>Comment identifier les plug-ins et pilotes LSA qui ne parviennent pas à exécuter en tant que processus protégé
Les événements décrits dans cette section sont trouvent dans l’opérationnel journal des Applications et des services\microsoft\windows\codeintegrity. Ils peuvent vous aider à identifier les plug-ins LSA et pilotes qui sont chargement échoue pour des raisons de signature. Pour gérer ces événements, vous pouvez utiliser le **wevtutil** outil de ligne de commande. Pour plus d’informations sur cet outil, consultez [Wevtutil](../../administration/windows-commands/Wevtutil.md).

### <a name="before-opting-in-how-to-identify-plug-ins-and-drivers-loaded-by-the-lsassexe"></a>Avant de vous inscrire: comment identifier les plug-ins et pilotes chargés par lsass.exe
Vous pouvez utiliser le mode audit pour identifier les plug-ins LSA et pilotes qui ne pourront pas au chargement en mode de Protection LSA. En mode audit, le système génère des journaux des événements, qui identifient tous les plug-ins et pilotes qui ne pourront pas être chargés sous LSA si la Protection LSA est activée. Les messages sont consignés sans bloquer les plug-ins ou pilotes.

##### <a name="to-enable-the-audit-mode-for-lsassexe-on-a-single-computer-by-editing-the-registry"></a>Pour activer le mode audit pour Lsass.exe sur un seul ordinateur en modifiant le Registre

1.  Ouvrez l’Éditeur du Registre (RegEdit.exe) et accédez à la clé de Registre qui se trouve à: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image fichier exécution Options\LSASS.exe.

2.  Définissez la valeur de la clé de Registre **AuditLevel = 00000008**.

3.  Redémarrez l’ordinateur.

Analysez les résultats de l’événement 3065 et événement 3066.

-   **Événement3065**: cet événement rapporte que la vérification de l’intégrité du code a déterminé qu’un processus (généralement lsass.exe) a tenté de charger un pilote spécifique qui ne répondait pas aux exigences de sécurité pour les Sections partagées. Toutefois, en raison de la stratégie système qui est définie, l’image a été autorisée à charger.

-   **Événement3066**: cet événement rapporte que la vérification de l’intégrité du code a déterminé qu’un processus (généralement lsass.exe) a tenté de charger un pilote spécifique qui ne répondait pas aux exigences de niveau de signature Microsoft. Toutefois, en raison de la stratégie système qui est définie, l’image a été autorisée à charger.

> [!IMPORTANT]
> Ces événements opérationnels ne sont pas générés lorsqu’un débogueur du noyau est attaché et activé sur un système.
> 
> Si un plug-in ou pilote contient les Sections partagées, événement 3066 est consigné avec l’événement 3065. Suppression des Sections partagées doit empêcher les deux événements, sauf si le plug-in ne remplit pas les exigences de niveau de signature Microsoft.

Pour activer le mode audit pour plusieurs ordinateurs dans un domaine, vous pouvez utiliser l’Extension côté Client du Registre de stratégie de groupe pour déployer la valeur de Registre au niveau d’audit Lsass.exe. Vous devez modifier la clé de Registre HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image fichier exécution Options\LSASS.exe.

##### <a name="to-create-the-auditlevel-value-setting-in-a-gpo"></a>Pour créer le paramètre de valeur AuditLevel dans un objet de stratégie de groupe

1.  Ouvrez la Console de gestion de stratégie de groupe (GPMC).

2.  Créer une nouvelle stratégie de groupe objet (GPO) qui est lié au niveau du domaine ou qui est lié à l’unité d’organisation qui contient vos comptes d’ordinateurs. Ou vous pouvez sélectionner un objet de stratégie de groupe qui est déjà déployé.

3.  Avec le bouton droit de l’objet de stratégie de groupe, puis cliquez sur **modifier** pour ouvrir l’éditeur de gestion de stratégie de groupe.

4.  Développez **Configuration ordinateur**, développez **préférences**, puis développez **paramètres Windows**.

5.  Avec le bouton droit **Registre**, pointez sur **New**, puis cliquez sur **élément de Registre**. Le **nouvelles propriétés de Registre** boîte de dialogue s’affiche.

6.  Dans le **la ruche**, cliquez sur **HKEY_LOCAL_MACHINE.**

7.  Dans le **chemin d’accès de la clé** liste, accédez à **SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image fichier exécution Options\LSASS.exe**.

8.  Dans le **nom de la valeur**, tapez **AuditLevel**.

9. Dans le **type de valeur** zone, cliquez pour sélectionner le **REG_DWORD**.

10. Dans le **données de la valeur**, tapez **00000008**.

11. Cliquez sur **OK**.

> [!NOTE]
> Pour l’objet de stratégie de groupe prennent effet, la modification de l’objet stratégie de groupe doit être répliquée sur tous les contrôleurs de domaine dans le domaine.

Pour bénéficier d’une protection LSA supplémentaire sur plusieurs ordinateurs, vous pouvez utiliser l’Extension côté Client du Registre de stratégie de groupe en modifiant HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa. Pour connaître la procédure à suivre, voir [comment configurer la protection LSA supplémentaire des informations d’identification](#BKMK_HowToConfigure) dans cette rubrique.

### <a name="after-opting-in-how-to-identify-plug-ins-and-drivers-loaded-by-the-lsassexe"></a>Après vous être inscrit: comment identifier les plug-ins et pilotes chargés par lsass.exe
Vous pouvez utiliser le journal des événements pour identifier les plug-ins LSA et pilotes qui n’a pas pu charger en mode de Protection LSA. Lorsque le processus protégé LSA est activé, le système génère des journaux des événements qui identifient tous les plug-ins et pilotes qui n’a pas pu être chargés sous LSA.

Analysez les résultats de l’événement 3033 et de l’événement 3063.

-   **Événement3033**: cet événement rapporte que la vérification de l’intégrité du code a déterminé qu’un processus (généralement lsass.exe) a tenté de charger un pilote qui ne répondait pas aux exigences de niveau de signature Microsoft.

-   **Événement3063**: cet événement rapporte que la vérification de l’intégrité du code a déterminé qu’un processus (généralement lsass.exe) a tenté de charger un pilote qui ne répondait pas aux exigences de sécurité pour les Sections partagées.

Les Sections partagées sont généralement le résultat de techniques de programmation qui permettent aux données d’instance d’interagir avec d’autres processus qui utilisent le même contexte de sécurité. Cela peut créer des failles de sécurité.

## <a name="BKMK_HowToConfigure"></a>Comment configurer la protection LSA supplémentaire des informations d’identification
Sur les appareils exécutant Windows8.1 (avec ou sans démarrage sécurisé ou UEFI), la configuration est possible en effectuant les procédures décrites dans cette section. Pour les appareils exécutant WindowsRT8.1, la protection lsass.exe est toujours activée et ne peut pas être désactivée.

### <a name="on-x86-based-or-x64-based-devices-using-secure-boot-and-uefi-or-not"></a>Sur les appareils x86 ou x64 64 ou non à l’aide de démarrage sécurisé et UEFI
Sur les appareils x86 ou x64 64 qui utilisent le démarrage sécurisé et UEFI, une variable UEFI est définie dans le microprogramme UEFI lorsque la protection LSA est activée à l’aide de la clé de Registre. Lorsque le paramètre est stocké dans le microprogramme, la variable UEFI ne peut pas être supprimée ou modifiée dans la clé de Registre. La variable UEFI doit être réinitialisée.

x86 ou x64 64périphériques ne prenant pas en charge le démarrage sécurisé ou UEFI sont désactivés, ne peut pas stocker la configuration de la protection LSA dans le microprogramme et vous baser uniquement sur la présence de la clé de Registre. Dans ce scénario, il est possible de désactiver la protection LSA à l’aide de l’accès à distance à l’appareil.

Vous pouvez utiliser les procédures suivantes pour activer ou désactiver la protection LSA:

##### <a name="to-enable-lsa-protection-on-a-single-computer"></a>Pour activer la protection LSA sur un seul ordinateur

1.  Ouvrez l’Éditeur du Registre (RegEdit.exe) et accédez à la clé de Registre qui se trouve à: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa.

2.  Définissez la valeur de la clé de Registre: «RunAsPPL» = DWORD: 00000001.

3.  Redémarrez l’ordinateur.

##### <a name="to-enable-lsa-protection-using-group-policy"></a>Pour activer la protection LSA à l’aide de la stratégie de groupe

1.  Ouvrez la Console de gestion de stratégie de groupe (GPMC).

2.  Créer un nouvel objet GPO qui est lié au niveau du domaine ou qui est lié à l’unité d’organisation qui contient vos comptes d’ordinateurs. Ou vous pouvez sélectionner un objet de stratégie de groupe qui est déjà déployé.

3.  Avec le bouton droit de l’objet de stratégie de groupe, puis cliquez sur **modifier** pour ouvrir l’éditeur de gestion de stratégie de groupe.

4.  Développez **Configuration ordinateur**, développez **préférences**, puis développez **paramètres Windows**.

5.  Avec le bouton droit **Registre**, pointez sur **New**, puis cliquez sur **élément de Registre**. Le **nouvelles propriétés de Registre** boîte de dialogue s’affiche.

6.  Dans le **la ruche**, cliquez sur **HKEY_LOCAL_MACHINE**.

7.  Dans le **chemin d’accès de la clé** liste, accédez à **SYSTEM\CurrentControlSet\Control\Lsa**.

8.  Dans le **nom de la valeur**, tapez **RunAsPPL**.

9. Dans le **type de valeur**, cliquez sur le **REG_DWORD**.

10. Dans le **données de la valeur**, tapez **00000001**.

11. Cliquez sur **OK**.

##### <a name="to-disable-lsa-protection"></a>Pour désactiver la protection LSA

1.  Ouvrez l’Éditeur du Registre (RegEdit.exe) et accédez à la clé de Registre qui se trouve à: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa.

2.  Supprimez la valeur suivante à partir de la clé de Registre: «RunAsPPL» = DWORD: 00000001.

3.  Utilisez l’outil d’exclusion de processus protégé autorité de sécurité locale (LSA) pour supprimer la variable UEFI si l’appareil utilise le démarrage sécurisé.

    Pour plus d’informations sur l’outil d’exclusion, consultez [télécharger sécurité autorité locale (LSA) protégé exclusion de processus à partir du centre de téléchargement Microsoft officiel](https://www.microsoft.com/download/details.aspx?id=40897).

    Pour plus d’informations sur la gestion du démarrage sécurisé, voir [microprogramme UEFI](https://technet.microsoft.com/library/hh824898.aspx).

    > [!WARNING]
    > Lorsque le démarrage sécurisé est désactivé, tous les configurations liées UEFI et démarrage sécurisé sont rétablies. Vous devez désactiver le démarrage sécurisé uniquement lorsque toutes les autres moyens pour désactiver la protection LSA ont échoué.

### <a name="verifying-lsa-protection"></a>Vérification de la protection LSA
Pour détecter si LSA a démarré en mode protégé au démarrage de Windows, recherchez l’événement WinInit suivant dans le **système** ouvrir une session sous **journaux Windows**:

-   12: LSASS.exe a démarré en tant que processus protégé avec le niveau: 4

## <a name="additional-resources"></a>Ressources supplémentaires
[Gestion et Protection des informations d’identification](credentials-protection-and-management.md)

[Fichier de signature de service pour LSA](https://go.microsoft.com/fwlink/?LinkId=392590)


