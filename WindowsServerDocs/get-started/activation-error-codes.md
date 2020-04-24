---
title: Résoudre les codes d’erreur d’activation de Windows
description: Découvrez comment résoudre les codes d’erreur d’activation
ms.topic: article
ms.date: 9/18/2019
ms.technology: server-general
author: kaushika-msft
ms.author: kaushika-msft; v-tea
ms.localizationpriority: medium
ms.openlocfilehash: 50c50353316db4288f01893125ecd651db63cbb7
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80826352"
---
# <a name="resolve-windows-activation-error-codes"></a>Résoudre les codes d’erreur d’activation de Windows

> **Utilisateurs à domicile**  
> Cet article est destiné aux professionnels de l’informatique et agents d’assistance. Si vous recherchez plus d’informations sur les messages d’erreur d’activation Windows, consultez la section [Obtenir de l’aide sur les erreurs d’activation de Windows](https://support.microsoft.com/help/10738/windows-10-get-help-with-activation-errors).  

Cet article fournit des informations de dépannage qui vous aident à résoudre les messages d’erreur que vous pouvez recevoir lorsque vous essayez d’utiliser une clé d’activation multiple (MAK) ou le service de gestion de clés (KMS) pour effectuer une activation en volume sur un ou plusieurs ordinateurs Windows. Recherchez le code d’erreur dans le tableau suivant, puis sélectionnez le lien pour afficher plus d’informations sur ce code d’erreur et sur la manière de le résoudre.

Pour plus d’informations sur l’activation en volume, consultez la section [Planifier l’activation en volume](https://docs.microsoft.com/windows/deployment/volume-activation/plan-for-volume-activation-client).

Pour plus d’informations sur l’activation en volume des versions actuelles et récentes de Windows, consultez la section [Activation en volume [client]](https://docs.microsoft.com/windows/deployment/volume-activation/volume-activation-windows-10).

Pour plus d’informations sur l’activation en volume pour les versions antérieures de Windows, consultez l’article 929712 de la base de connaissances, , [Informations sur l’activation en volume pour Windows Vista, Windows Server 2008, Windows Server 2008 R2 et Windows 7](https://support.microsoft.com/help/929712/volume-activation-information-for-windows-vista-windows-server-2008-wi).

## <a name="diagnostic-tool"></a>Outil de diagnostic

L’Assistant Support et récupération de Microsoft simplifie la résolution des problèmes d’activation basée sur le service Windows KMS. Téléchargez [ici](https://aka.ms/SaRA-WindowsActivation) l’outil de diagnostic.

Cet outil essaie d’activer Windows. S’il retourne un code d’erreur d’activation, l’outil affiche des solutions ciblées pour les codes d’erreur connus.

Les codes d’erreur suivants sont pris en charge : 0xC004F038, 0xC004F039, 0xC004F041, 0xC004F074, 0xC004C008.

## <a name="summary-of-error-codes"></a>Résumé des codes d’erreur

|Code d'erreur |Message d’erreur |Type&nbsp;d’activation|
|-----------|--------------|----------------|
|[0x8004FE21](#0x8004fe21-this-computer-is-not-running-genuine-windows) |Cet ordinateur n’exécute pas Windows authentique.  |Clé MAK<br />Client KMS |
|[0x80070005](#0x80070005-access-denied) |Accès refusé. L’action demandée exige des privilèges plus élevés. |Clé MAK<br />Client KMS<br />Hôte KMS |
|[0x8007007b](#0x8007007b-dns-name-does-not-exist) |0x8007007b Le nom DNS n’existe pas. |Client KMS |
|[0x80070490](#0x80070490-the-product-key-you-entered-didnt-work) |La clé de produit que vous avez entrée n’a pas fonctionné. Vérifiez la clé de produit ou entrez-en une autre. |Clé MAK |
|[0x800706BA](#0x800706ba-the-rpc-server-is-unavailable) |Le serveur RPC est indisponible. |Client KMS |
|[0x8007232A](#0x8007232a-dns-server-failure) |Échec du serveur DNS.  |Hôte KMS  |
|[0x8007232B](#0x8007232b-dns-name-does-not-exist) |Le nom DNS n’existe pas. |Client KMS |
|[0x8007251D](#0x8007251d-no-records-found-for-dns-query) |Aucun enregistrement trouvé pour la requête DNS. |Client KMS |
|[0x80092328](#0x80092328-dns-name-does-not-exist) |Le nom DNS n’existe pas.  |Client KMS |
|[0xC004B100](#0xc004b100-the-activation-server-determined-that-the-computer-could-not-be-activated) |Le serveur d’activation a déterminé que l’ordinateur n’a pas pu être activé. |Clé MAK |
|[0xC004C001](#0xc004c001-the-activation-server-determined-the-specified-product-key-is-invalid) |Le serveur d’activation a déterminé que la clé de produit spécifiée n’est pas valide |Clé MAK|
|[0xC004C003](#0xc004c003-the-activation-server-determined-the-specified-product-key-is-blocked) |Le serveur d’activation a déterminé que la clé de produit spécifiée est bloquée |Clé MAK |
|[0xC004C008](#0xc004c008-the-activation-server-determined-that-the-specified-product-key-could-not-be-used) |Le serveur d’activation a déterminé que la clé de produit spécifiée n’a pas pu être utilisée. |KMS |
|[0xC004C020](#0xc004c020-the-activation-server-reported-that-the-multiple-activation-key-has-exceeded-its-limit) |Le serveur d’activation a signalé que la limite de la clé d’activation multiple a été dépassée. |Clé MAK |
|[0xC004C021](#0xc004c021-the-activation-server-reported-that-the-multiple-activation-key-extension-limit-has-been-exceeded) |Le serveur d’activation a signalé que la limite de l’extension de la clé d’activation multiple a été dépassée. |Clé MAK |
|[0xC004F009](#0xc004f009-the-software-protection-service-reported-that-the-grace-period-expired) |Le service de gestion de licences a signalé que la période de grâce a expiré. |Clé MAK |
|[0xC004F00F](#0xc004f00f-the-software-licensing-server-reported-that-the-hardware-id-binding-is-beyond-level-of-tolerance) |Le serveur de gestion de licences logicielles a signalé que la liaison de l’ID de matériel est au-delà du niveau de tolérance. |Clé MAK<br />Client KMS<br />Hôte KMS |
|[0xC004F014](#0xc004f014-the-software-protection-service-reported-that-the-product-key-is-not-available) |Le service de gestion de licences a signalé que la clé de produit n’est pas disponible |Clé MAK<br />Client KMS |
|[0xC004F02C](#0xc004f02c-the-software-protection-service-reported-that-the-format-for-the-offline-activation-data-is-incorrect) |Le service de gestion de licences a signalé que les données d’activation hors connexion sont incorrectes. |Clé MAK<br />Client KMS |
|[0xC004F035](#0xc004f035-invalid-volume-license-key) |Le service de protection logicielle a indiqué que l’ordinateur n’a pas pu être activé avec une clé de produit de licence en volume. |Client KMS<br />Hôte KMS |
|[0xC004F038](#0xc004f038-the-count-reported-by-your-key-management-service-kms-is-insufficient) |Le service de gestion de licences a signalé que l’ordinateur n’a pas pu être activé. Le compte indiqué par votre service de gestion de clés (KMS) est insuffisant. Contactez votre administrateur système. |Client KMS |
|[0xC004F039](#0xc004f039-the-key-management-service-kms-is-not-enabled) |Le service de gestion de licences a signalé que l’ordinateur n’a pas pu être activé. Le service de gestion de clés (KMS) n’est pas activé. |Client KMS |
|[0xC004F041](#0xc004f041-the-software-protection-service-determined-that-the-key-management-server-kms-is-not-activated) |Le service de protection logicielle a déterminé que le service KMS n’est pas activé. KMS doit être activé.  |Client KMS |
|[0xC004F042](#0xc004f042-the-software-protection-service-determined-that-the-specified-key-management-service-kms-cannot-be-used) |Le service de gestion de licences a déterminé que le service de gestion de clés (KMS) ne peut pas être utilisé. |Client KMS |
|[0xC004F050](#0xc004f050-the-software-protection-service-reported-that-the-product-key-is-invalid) |Le service de gestion de licences a signalé que la clé de produit n’est pas valide. |Clé MAK<br />KMS<br />Client KMS |
|[0xC004F051](#0xc004f051-the-software-protection-service-reported-that-the-product-key-is-blocked) |Le service de gestion de licences a signalé que la clé de produit est bloquée. |Clé MAK<br />KMS |
|[0xC004F064](#0xc004f064-the-software-protection-service-reported-that-the-non-genuine-grace-period-expired) |Le service de protection logicielle a signalé que la période de grâce non authentique a expiré. |Clé MAK |
|[0xC004F065](#0xc004f065-the-software-protection-service-reported-that-the-application-is-running-within-the-valid-non-genuine-period) |Le service de protection logicielle a signalé que l’application est exécutée pendant la période non authentique valide. |Clé MAK<br />Client KMS |
|[0xC004F06C](#0xc004f06c-the-request-timestamp-is-invalid) |Le service de gestion de licences a signalé que l’ordinateur n’a pas pu être activé. Le service de gestion de clés (KMS) a déterminé que l’horodatage de la demande n’est pas valide.  |Client KMS |
|[0xC004F074](#0xc004f074-no-key-management-service-kms-could-be-contacted) |Le service de gestion de licences a signalé que l’ordinateur n’a pas pu être activé. Aucun Gestionnaire de clés (KMS) n’a pu être contacté. Pour plus d’informations, consultez le journal des événements des applications.  |Client KMS |

## <a name="causes-and-resolutions"></a>Causes et solutions

### <a name="0x8004fe21-this-computer-is-not-running-genuine-windows"></a>0x8004FE21 Cet ordinateur n’exécute pas une version authentique de Windows  

#### <a name="possible-cause"></a>Cause possible

Ce problème peut se produire pour plusieurs raisons. La raison la plus probable est que les modules linguistiques ont été installés sur des ordinateurs qui exécutent des éditions de Windows qui ne sont pas concédées sous licence pour les modules linguistiques supplémentaires.  

> [!NOTE]
> Ce problème n’est pas nécessairement une indication de falsification. Certaines applications peuvent installer une prise en charge multilingue même lorsque cette édition de Windows n’est pas concédée sous licence pour ces modules linguistiques.)  

Ce problème peut également se produire si Windows a été modifié par un logiciel malveillant pour permettre l’installation de fonctionnalités supplémentaires. Ce problème peut également se produire si certains fichiers système sont endommagés.  

#### <a name="resolution"></a>Solution

Pour résoudre ce problème, vous devez réinstaller le système d’exploitation.  

### <a name="0x80070005-access-denied"></a>0x80070005 Accès refusé

Le texte complet de ce message d’erreur ressemble à ce qui suit :

> Accès refusé. L’action demandée exige des privilèges plus élevés.

#### <a name="possible-cause"></a>Cause possible

Le contrôle de compte d’utilisateur (UAC) interdit l’exécution des processus d’activation dans une fenêtre d’invite de commandes sans élévation de privilèges.  

#### <a name="resolution"></a>Solution

Exécutez la commande **slmgr.vbs** depuis une invite de commandes avec élévation de privilèges. Pour ce faire, dans le **menu Démarrer**, cliquez avec le bouton droit sur **cmd.exe**, puis cliquez sur **Exécuter en tant qu’administrateur**.  

### <a name="0x8007007b-dns-name-does-not-exist"></a>0x8007007b Le nom DNS n’existe pas

#### <a name="possible-cause"></a>Cause possible

Ce problème peut se produire si le client KMS ne trouve pas les enregistrements de ressource SRV KMS dans le DNS.  

#### <a name="resolution"></a>Solution

Pour plus d’informations sur la résolution de ces problèmes liés au DNS, consultez la section [Procédures de dépannage courantes pour les problèmes KMS et DNS](common-troubleshooting-procedures-kms-dns.md).  

### <a name="0x80070490-the-product-key-you-entered-didnt-work"></a>0x80070490 La clé de produit que vous avez entrée n’a pas fonctionné

Le texte complet de cette erreur ressemble à ce qui suit :
> La clé de produit que vous avez entrée n’a pas fonctionné. Vérifiez la clé de produit ou entrez-en une autre.  

#### <a name="possible-cause"></a>Cause possible

Ce problème se produit car la clé qui a été entrée n’était pas valide, ou en raison d’un problème connu dans Windows Server 2019.  

#### <a name="resolution"></a>Solution

Pour contourner ce problème et activer l’ordinateur, exécutez **slmgr -ipk <5x5 key>** à partir d’une invite de commandes avec élévation de privilèges.

### <a name="0x800706ba-the-rpc-server-is-unavailable"></a>0x800706BA Le serveur RPC n’est pas disponible

#### <a name="possible-cause"></a>Cause possible

Les paramètres de pare-feu ne sont pas configurés sur l’hôte KMS ou les enregistrements SRV DNS sont obsolètes.  

#### <a name="resolution"></a>Solution

Sur l’hôte KMS, vérifiez que l’exception de pare-feu est activée sur le service de gestion des clés (port TCP 1688).

Vérifiez que les enregistrements SRV DNS pointent vers un hôte KMS valide. 

Résolvez les problèmes de connexions réseau.  

Pour plus d’informations sur la résolution de ces problèmes liés au DNS, consultez la section [Procédures de dépannage courantes pour les problèmes KMS et DNS](common-troubleshooting-procedures-kms-dns.md).  

### <a name="0x8007232a-dns-server-failure"></a>0x8007232A Échec du serveur DNS

#### <a name="possible-cause"></a>Cause possible

Le système rencontre des problèmes réseau ou DNS.

#### <a name="resolution"></a>Solution

Résolvez les problèmes réseau et DNS.  

### <a name="0x8007232b-dns-name-does-not-exist"></a>0x8007232B Le nom DNS n’existe pas

#### <a name="possible-cause"></a>Cause possible

Le client KMS ne parvient pas à trouver les enregistrements de ressources du serveur (RR SRV) dans le DNS.  

#### <a name="resolution"></a>Solution

Vérifiez qu’un hôte KMS a été installé et que la publication DNS est activée (par défaut). Si le DNS n’est pas disponible, pointez le client KMS vers l’hôte KMS en utilisant **slmgr.vbs /skms <*kms_host_name*>** .  

Si vous n’avez pas d’hôte KMS, obtenez et installez une clé MAK. Ensuite, activez le système.

Pour plus d’informations sur la résolution de ces problèmes liés au DNS, consultez la section [Procédures de dépannage courantes pour les problèmes KMS et DNS](common-troubleshooting-procedures-kms-dns.md).  

### <a name="0x8007251d-no-records-found-for-dns-query"></a>0x8007251D Aucun enregistrement trouvé pour la requête DNS

#### <a name="possible-cause"></a>Cause possible

Le client KMS ne trouve pas les enregistrements SRV KMS dans le DNS.

#### <a name="resolution"></a>Solution

Résolvez les problèmes de connexions réseau et DNS. Pour plus d’informations sur la procédure de résolution de ces problèmes liés au DNS, consultez la section [Procédures de dépannage courantes pour les problèmes KMS et DNS](common-troubleshooting-procedures-kms-dns.md).  

### <a name="0x80092328-dns-name-does-not-exist"></a>0x80092328 Le nom DNS n’existe pas

#### <a name="possible-cause"></a>Cause possible

Ce problème peut se produire si le client KMS ne trouve pas les enregistrements de ressource SRV KMS dans le DNS.

#### <a name="resolution"></a>Solution

Pour plus d’informations sur la résolution de ces problèmes liés au DNS, consultez la section [Procédures de dépannage courantes pour les problèmes KMS et DNS](common-troubleshooting-procedures-kms-dns.md).  

### <a name="0xc004b100-the-activation-server-determined-that-the-computer-could-not-be-activated"></a>0xC004B100 Le serveur d’activation a déterminé que l’ordinateur n’a pas pu être activé

#### <a name="possible-cause"></a>Cause possible

La clé MAK n’est pas prise en charge.  

#### <a name="resolution"></a>Solution

Pour résoudre ce problème, vérifiez que la clé MAK que vous utilisez est celle qui a été fournie par Microsoft. Pour vérifier que la clé MAK est valide, contactez les [centres d’activation de licences Microsoft](https://www.microsoft.com/Licensing/existing-customer/activation-centers).

### <a name="0xc004c001-the-activation-server-determined-the-specified-product-key-is-invalid"></a>0xC004C001 Le serveur d’activation a déterminé que la clé de produit spécifiée n’est pas valide

#### <a name="possible-cause"></a>Cause possible

La clé MAK que vous avez entrée n’est pas valide.

#### <a name="resolution"></a>Solution

Vérifiez que la clé est la clé MAK fournie par Microsoft. Pour obtenir une assistance supplémentaire, contactez les [centres d’activation de licences Microsoft](https://www.microsoft.com/Licensing/existing-customer/activation-centers).

### <a name="0xc004c003-the-activation-server-determined-the-specified-product-key-is-blocked"></a>0xC004C003 Le serveur d’activation a déterminé que la clé de produit spécifiée est bloquée

#### <a name="possible-cause"></a>Cause possible

La clé MAK est bloquée sur le serveur d’activation.

#### <a name="resolution"></a>Solution

Pour obtenir une nouvelle clé MAK, contactez les [centres d’activation de licences Microsoft](https://www.microsoft.com/Licensing/existing-customer/activation-centers). Une fois que vous avez obtenu la nouvelle clé MAK, réessayez d’installer et d’activer Windows.  

### <a name="0xc004c008-the-activation-server-determined-that-the-specified-product-key-could-not-be-used"></a>0xC004C008 Le serveur d’activation a déterminé que la clé de produit spécifiée n’a pas pu être utilisée

#### <a name="possible-cause"></a>Cause possible

La limitation d’activation de la clé KMS a été dépassée. Une clé d’hôte KMS peut être activée jusqu’à 10 fois sur six ordinateurs différents.  

#### <a name="resolution"></a>Solution

S vous avez besoin d’activations supplémentaires, contactez les [centres d’activation de licences Microsoft](https://www.microsoft.com/Licensing/existing-customer/activation-centers).  

### <a name="0xc004c020-the-activation-server-reported-that-the-multiple-activation-key-has-exceeded-its-limit"></a>0xC004C020 Le serveur d’activation a signalé que la limite de la clé d’activation multiple a été dépassée

#### <a name="possible-cause"></a>Cause possible

La limitation d’activation de la clé MAK a été dépassée. Par défaut, les clés MAK peuvent être activées un nombre limité de fois.

#### <a name="resolution"></a>Solution

S vous avez besoin d’activations supplémentaires, contactez les [centres d’activation de licences Microsoft](https://www.microsoft.com/Licensing/existing-customer/activation-centers).

### <a name="0xc004c021-the-activation-server-reported-that-the-multiple-activation-key-extension-limit-has-been-exceeded"></a>0xC004C021 Le serveur d’activation a signalé que la limite de l’extension de la clé d’activation multiple a été dépassée

#### <a name="possible-cause"></a>Cause possible

La limitation d’activation de la clé MAK a été dépassée. Par défaut, les clés MAK ne peuvent être activées qu’un nombre limité de fois.

#### <a name="resolution"></a>Solution

S vous avez besoin d’activations supplémentaires, contactez les [centres d’activation de licences Microsoft](https://www.microsoft.com/Licensing/existing-customer/activation-centers).

### <a name="0xc004f009-the-software-protection-service-reported-that-the-grace-period-expired"></a>0xC004F009 Le service de protection logicielle a signalé que la période de grâce a expiré

#### <a name="possible-cause"></a>Cause possible

La période de grâce a expiré avant que le système soit activé. Le système est en l’état Notifications.  

#### <a name="resolution"></a>Solution

Pour obtenir une assistance, contactez les [centres d’activation de licences Microsoft](https://www.microsoft.com/Licensing/existing-customer/activation-centers).

### <a name="0xc004f00f-the-software-licensing-server-reported-that-the-hardware-id-binding-is-beyond-level-of-tolerance"></a>0xC004F00F Le serveur de gestion de licences logicielles a signalé que la liaison de l’ID de matériel est au-delà du niveau de tolérance

#### <a name="possible-cause"></a>Cause possible

Le matériel a changé ou les pilotes ont été mis à jour sur le système.  

#### <a name="resolution"></a>Solution

Si vous utilisez l’activation basée sur une clé d’activation multiple (MAK), utilisez une activation en ligne ou par téléphone pour réactiver le système pendant la période de grâce hors tolérance.  

Si vous utilisez l’activation KMS, redémarrez Windows ou exécutez **slmgr.vbs /ato**.

### <a name="0xc004f014-the-software-protection-service-reported-that-the-product-key-is-not-available"></a>0xC004F014 Le service de protection logicielle a signalé que la clé de produit n’est pas disponible

#### <a name="possible-cause"></a>Cause possible

Aucune clé de produit n’est installée sur le système.  

#### <a name="resolution"></a>Solution

Si vous utilisez l’activation basée sur une clé d’activation multiple (MAK), installez une clé de produit MAK. 

Si vous utilisez l’activation KMS, consultez le fichier Pid.txt (situé sur le support d’installation dans le dossier \sources) pour obtenir une clé d’installation KMS. Installez la clé.

### <a name="0xc004f02c-the-software-protection-service-reported-that-the-format-for-the-offline-activation-data-is-incorrect"></a>0xC004F02C Le service de protection logicielle a signalé que les données d’activation hors connexion sont incorrectes

#### <a name="possible-cause"></a>Cause possible

Le système a détecté que les données entrées pendant une activation par téléphone ne sont pas correctes.  

#### <a name="resolution"></a>Solution

Vérifiez que le CID est entré correctement.  

### <a name="0xc004f035-invalid-volume-license-key"></a>0xC004F035 Clé de licence en volume non valide

Le texte complet de ce message d’erreur ressemble à ce qui suit :

> Erreur : Clé de licence en volume non valide. Pour l’activer, vous devez remplacer votre clé de produit par une clé d’activation multiple (MAK) ou une clé commercialisée. Vous devez disposer d’une licence de système d’exploitation éligible ET d’une licence en volume à des fins de mise à niveau Windows 7, ou d’une licence complète pour Windows 7 issue d’une source commercialisée. TOUTE AUTRE INSTALLATION DE CE LOGICIEL EST EN VIOLATION DE VOTRE CONTRAT ET DE LA LÉGISLATION RELATIVE AUX DROITS D’AUTEUR APPLICABLE.  

Le texte d’erreur est correct, mais ambigu. Cette erreur indique que le BIOS de l’ordinateur ne contient pas de marqueur Windows qui l’identifie en tant que système OEM exécutant une version éligible de Windows. Ces informations sont nécessaires pour l’activation du client KMS. La signification plus spécifique de ce code est « Erreur : Clé de licence en volume non valide »

#### <a name="possible-cause"></a>Cause possible

Les éditions de licence en volume Windows 7 sont concédées sous licence uniquement à des fins de mises à niveau. Microsoft ne prend pas en charge l’installation d’un système d’exploitation en volume sur un ordinateur qui ne dispose pas d’un système d’exploitation éligible installé.  

#### <a name="resolution"></a>Solution

Pour pouvoir activer, vous devez effectuer une des actions suivantes :

- Remplacez votre clé de produit par une clé d’activation multiple (MAK) ou une clé de produit commercialisé. Vous devez disposer d’une licence de système d’exploitation éligible ET d’une licence en volume à des fins de mise à niveau Windows 7, ou d’une licence complète pour Windows 7 issue d’une source commercialisée.
  > [!NOTE]
  > Si vous recevez une erreur 0x80072ee2 quand vous tentez de l’activer, utilisez à la place la méthode d’activation par téléphone qui suit.
- Activez par téléphone en suivant ces étapes :
   1. Exécutez **slmgr /dti**, puis enregistrez la valeur de l’ID d’installation. </li>
   1. Contactez les [Centres d’activation de licence Microsoft](https://www.microsoft.com/Licensing/existing-customer/activation-centers) et fournissez l’ID d’installation afin de recevoir un ID de confirmation.</li>
   1. Pour activer avec l’ID de confirmation, exécutez **slmgr /atp &lt;ID de confirmation&gt;** .

### <a name="0xc004f038-the-count-reported-by-your-key-management-service-kms-is-insufficient"></a>0xC004F038 Le compte indiqué par votre service de gestion de clés (KMS) est insuffisant

Le texte complet de ce message d’erreur ressemble à ce qui suit :

> Le service de gestion de licences a signalé que l’ordinateur n’a pas pu être activé. Le compte indiqué par votre service de gestion de clés (KMS) est insuffisant. Contactez votre administrateur système.  

#### <a name="possible-cause"></a>Cause possible

Le nombre indiqué par l’hôte KMS n’est pas assez élevé. Pour Windows Server, le nombre de KMS doit être supérieur ou égal à 5. Pour Windows (client), le nombre de KMS doit être supérieur ou égal à 25.  

#### <a name="resolution"></a>Solution
Avant de pouvoir utiliser KMS pour activer Windows, vous devez disposer d’un plus grand nombre d’ordinateurs dans le pool KMS. Pour obtenir le nombre actuel sur l’hôte KMS, exécutez **Slmgr.vbs /dli**.  

### <a name="0xc004f039-the-key-management-service-kms-is-not-enabled"></a>0xC004F039 Le service de gestion de clés (KMS) n’est pas activé

Le texte complet de ce message d’erreur ressemble à ce qui suit :

> Le service de gestion de licences a signalé que l’ordinateur n’a pas pu être activé. Le service de gestion de clés (KMS) n’est pas activé.  

#### <a name="possible-cause"></a>Cause possible

Le service de gestion des clés (KMS) n’a pas répondu à la demande KMS.

#### <a name="resolution"></a>Solution

Résolvez les problèmes de connexion entre l’hôte KMS et le client. Vérifiez que le port TCP 1688 (par défaut) n’est pas bloqué par un pare-feu ou filtré par un autre dispositif.

### <a name="0xc004f041-the-software-protection-service-determined-that-the-key-management-server-kms-is-not-activated"></a>0xC004F041 Le service de protection logicielle a déterminé que le service KMS n’est pas activé

Le texte complet de ce message d’erreur ressemble à ce qui suit :

> Le service de protection logicielle a déterminé que le service KMS n’est pas activé. KMS doit être activé.  

#### <a name="possible-cause"></a>Cause possible

L’hôte KMS n’est pas activé.  

#### <a name="resolution"></a>Solution

Activez l’hôte KMS par une activation en ligne ou par téléphone.  

### <a name="0xc004f042-the-software-protection-service-determined-that-the-specified-key-management-service-kms-cannot-be-used"></a>0xC004F042 Le service de protection logicielle a déterminé que le service de gestion de clés (KMS) ne peut pas être utilisé

#### <a name="possible-cause"></a>Cause possible

Cette erreur se produit quand un client KMS contacte un hôte KMS qui n’a pas pu activer le logiciel client. Cela peut se produire dans des environnements mixtes qui contiennent des hôtes KMS spécifiques à une application et à un système d’exploitation, par exemple.  

#### <a name="resolution"></a>Solution

Assurez-vous que si vous utilisez des hôtes KMS spécifiques pour activer des applications ou des systèmes d’exploitation spécifiques, les clients KMS se connectent aux ordinateurs hôtes appropriés.

### <a name="0xc004f050-the-software-protection-service-reported-that-the-product-key-is-invalid"></a>0xC004F050 Le service de protection logicielle a signalé que la clé de produit n’est pas valide

#### <a name="possible-cause"></a>Cause possible

Cela peut être dû à une erreur de saisie de la clé KMS ou à l’utilisation d’une clé bêta avec une version finale du système d’exploitation.  

#### <a name="resolution"></a>Solution

Installez la clé KMS qui correspond à la version de Windows. Vérifiez la saisie. Si la clé est copiée/collée, vérifiez que les tirets cadratins (|) n’ont pas été remplacés par des tirets dans la clé.  

### <a name="0xc004f051-the-software-protection-service-reported-that-the-product-key-is-blocked"></a>0xC004F051 Le service de protection logicielle a signalé que la clé de produit est bloquée

#### <a name="possible-cause"></a>Cause possible

Le serveur d’activation a déterminé que Microsoft a bloqué la clé de produit.  

#### <a name="resolution"></a>Solution

Obtenez une nouvelle clé MAK ou KMS, installez-la sur le système et activez-la.

### <a name="0xc004f064-the-software-protection-service-reported-that-the-non-genuine-grace-period-expired"></a>0xC004F064 zLe service de protection logicielle a signalé que la période de grâce non authentique a expiré

#### <a name="possible-cause"></a>Cause possible

Les outils d’activation Windows (WAT) ont déterminé que le système n’est pas authentique.  

#### <a name="resolution"></a>Solution

Pour obtenir une assistance, contactez les [centres d’activation de licences Microsoft](https://www.microsoft.com/Licensing/existing-customer/activation-centers).

### <a name="0xc004f065-the-software-protection-service-reported-that-the-application-is-running-within-the-valid-non-genuine-period"></a>0xC004F065Le service de protection logicielle a signalé que l’application est exécutée pendant la période non authentique valide

#### <a name="possible-cause"></a>Cause possible

Les outils d’activation Windows ont déterminé que le système n’est pas authentique. Le système continuera à s’exécuter pendant la période de grâce non authentique.  

#### <a name="resolution"></a>Solution

Obtenez et installez une clé de produit authentique et activez le système pendant la période de grâce. Sinon, le système passera à l’état Notifications à la fin de la période de grâce.

### <a name="0xc004f06c-the-request-timestamp-is-invalid"></a>0xC004F06C L’horodatage de la demande n’est pas valide

Le texte complet de ce message d’erreur ressemble à ce qui suit :

> Le service de gestion de licences a signalé que l’ordinateur n’a pas pu être activé. Le service de gestion de clés (KMS) a déterminé que l’horodatage de la demande n’est pas valide.  

#### <a name="possible-cause"></a>Cause possible

L’heure système sur l’ordinateur client est différente de l’heure sur l’hôte KMS. La synchronisation de l’heure est importante pour la sécurité système et réseau pour différentes raisons.  

#### <a name="resolution"></a>Solution

Corrigez ce problème en modifiant l’heure système sur le client pour qu’elle soit synchronisée avec l’heure sur l’hôte KMS. Nous vous recommandons d’utiliser la source de temps NTP ou Active Directory Domain Services pour la synchronisation de l’heure. Cette erreur utilise l’heure UTP et ne dépend pas de la sélection d’un fuseau horaire.  

### <a name="0xc004f074-no-key-management-service-kms-could-be-contacted"></a>0xC004F074 Aucun service de gestion de clés (KMS) n’a pu être contacté

Le texte complet de ce message d’erreur ressemble à ce qui suit :

> Le service de gestion de licences a signalé que l’ordinateur n’a pas pu être activé. Aucun Gestionnaire de clés (KMS) n’a pu être contacté. Pour plus d’informations, consultez le journal des événements des applications.  

#### <a name="possible-cause"></a>Cause possible

Tous les systèmes hôte KMS ont retourné une erreur.  

#### <a name="resolution"></a>Solution

Dans le journal des événements de l’application, identifiez chaque événement ayant l’ID d’événement 12288 et associé à la tentative d’activation. Résolvez les erreurs à partir de ces événements.

Pour plus d’informations sur la résolution des problèmes liés au DNS, consultez la section [Procédures de dépannage courantes pour les problèmes KMS et DNS](common-troubleshooting-procedures-kms-dns.md).  
