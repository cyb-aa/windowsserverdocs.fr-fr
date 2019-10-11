---
title: Options de Slmgr.vbs pour obtenir des informations sur l’activation en volume
description: Liste les options disponibles pour le script Slmg.vbs et explique comment les utiliser.
TOCTitle: Slmgr.vbs Options
ms.date: 09/24/2019
ms.technology: server-general
ms.topic: article
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
appliesto:
- Windows Server 2012 R2
- Windows 10
- Windows 8.1
ms.openlocfilehash: 3b1ce54fe1e1e855d605aba58a0f0e37f0324469
ms.sourcegitcommit: 9855d6b59b1f8722f39ae74ad373ce1530da0ccf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71963065"
---
# <a name="slmgrvbs-options-for-obtaining-volume-activation-information"></a>Options de Slmgr.vbs pour obtenir des informations sur l’activation en volume

Voici la syntaxe du script Slmgr.vbs ; les tableaux de cet article décrivent chaque option de la ligne de commande.

```cmd
slmgr.vbs [<ComputerName> [<User> <Password>]] [<Options>]
```

> [!NOTE]
> Dans cet article, des crochets \[] sont placés autour des arguments facultatifs, et des chevrons \<> sont placés autour des espaces réservés. Quand vous tapez ces instructions, omettez les crochets et remplacez les espaces réservés par les valeurs correspondantes.

> [!NOTE]
> Pour plus d’informations sur les autres produits logiciels qui utilisent l’activation en volume, consultez les documents écrits spécifiquement pour ces applications.

## <a name="using-slmgr-on-remote-computers"></a>Utilisation de Slmgr sur des ordinateurs distants

Pour gérer des clients distants, utilisez l'outil Gestion de l'activation en volume (VAMT) version 1.2 ou ultérieure, ou créez des scripts WMI personnalisés prenant en compte les différences entre les plateformes. Pour plus d’informations sur les propriétés et méthodes WMI pour l’activation en volume, consultez [Propriétés et méthodes WMI pour l’activation en volume](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn502536(v=ws.11)).

> [!IMPORTANT]
> En raison des modifications apportées à WMI dans Windows 7 et Windows Server 2008 R2, le script Slmgr.vbs n’est pas conçu pour fonctionner sur toutes les plateformes. L’utilisation de Slmgr.vbs pour gérer un système Windows 7 ou Windows Server 2008 R2 à partir du système d’exploitation Windows Vista® n’est pas prise en charge. Une tentative de gérer un système antérieur à Windows 7 ou Windows Server 2008 R2 génère une erreur de non-concordance des versions. Par exemple, l’exécution de **cscript slmgr.vbs \<nom\_machine\_vista\> /dlv** produit le résultat suivant :
>  
>> Microsoft (R) Windows Script Host Version 5.8 Copyright (C) Microsoft Corporation. Tous droits réservés.
>>  
>> L’ordinateur distant ne prend pas en charge cette version de SLMgr.vbs

## <a name="general-slmgrvbs-options"></a>Options générales de Slmgr.vbs

|Option |Description |
| - | - |
|\[\<Nom_ordinateur\>] |Nom d'un ordinateur distant (par défaut, il s'agit de l'ordinateur local) |
|\[\<Utilisateur\>] |Compte disposant du privilège nécessaire sur l’ordinateur distant |
|\[\<Mot_de_passe\>] |Mot de passe du compte disposant des privilèges nécessaires sur l’ordinateur distant |

## <a name="global-options"></a>Options globales

|Option |Description |
| - | - |
|\/ipk&nbsp;&lt;Clé_produit&gt; |Tente d’installer une clé de produit 5×5. La clé de produit fournie par le paramètre est confirmée comme étant valide et applicable sur le système d'exploitation installé.<br />Sinon, une erreur est renvoyée.<br />Si la clé est valide et applicable, elle est installée. Si une autre clé est déjà installée, elle est remplacée sans avertissement.<br />Pour éviter toute instabilité au niveau du service de licences, le système ou le service de protection logicielle doit être redémarré.<br />Cette opération doit être effectuée à partir d’une fenêtre d’invite de commandes avec élévation de privilèges. Sinon, la valeur de Registre Standard User Operations doit être définie afin d’étendre l’accès des utilisateurs non privilégiés au service de protection logicielle. |
|/ato&nbsp;\[\<ID&nbsp;d’activation\>] |Pour les versions vendues au détail et les systèmes en volume sur lesquels est installée une clé d’hôte KMS ou une clé d’activation multiple (MAK), l’option **/ato** invite Windows à tenter une activation en ligne.<br />Pour les systèmes où une clé de licence en volume générique (GVLK) est installée, cette option invite à tenter une activation KMS. Quand l’option **/ato** est spécifiée, les systèmes qui ont été configurés pour suspendre les tentatives automatiques d’activation KMS ( **/stao**) effectuent quand même des tentatives d’activation KMS.<br />**Remarque :** À compter de Windows 8 (et de Windows Server 2012), l’option **/stao** est dépréciée. Utilisez à la place l’option **/act-type**.<br />Le paramètre \<**ID d’activation**\> étend la prise en charge de l’option **/ato** pour identifier une édition Windows installée sur l’ordinateur. La spécification du paramètre \<**ID d’activation**\> limite les effets de l’option à l’édition associée à cet ID d’activation. Exécutez **slmgr.vbs /dlv all** pour obtenir tous les ID d’activation correspondant à la version de Windows installée. Si vous devez prendre en charge d’autres applications, consultez les recommandations spécifiques fournies par l’application en question.<br />L'activation KMS ne nécessite pas de privilèges élevés. En revanche, l'activation en ligne nécessite des privilèges élevés, ou la valeur de Registre "Standard User Operations" doit être définie afin d'étendre l'accès des utilisateurs non autorisés au service de protection logicielle. |
|\/dli&nbsp;\[<ID&nbsp;d’activation\>&nbsp;\|&nbsp;All\] |Affiche les informations relatives à la licence.<br />Par défaut, l'option **/dli** affiche les informations de licence relatives à l'édition de Windows actuellement installée. La spécification du paramètre \<**ID d’activation**\> affiche les informations de licence pour l’édition spécifiée associée à cet ID d’activation. La spécification du paramètre **All** affiche les informations de licence pour tous les produits installés applicables.<br />Cette opération ne nécessite pas de privilèges élevés. |
|\/dlv&nbsp;\[<ID&nbsp;d’activation\>&nbsp;\|&nbsp;All\] |Affiche des informations détaillées relatives à la licence.<br />Par défaut, l'option **/dlv** affiche les informations de licence relatives au système d'exploitation installé. La spécification du paramètre \<**ID d’activation**\> affiche les informations de licence pour l’édition spécifiée associée à cet ID d’activation. La spécification du paramètre **All** affiche les informations de licence pour tous les produits installés applicables.<br />Cette opération ne nécessite pas de privilèges élevés. |
|\/xpr \[\<ID&nbsp;d’activation\>] |Affiche la date d'expiration de l'activation du produit. Par défaut, cette option s'applique à l'édition actuelle de Windows. Elle est principalement utilisée pour les clients KMS, car l'activation MAK et de la version commerciale est perpétuelle.<br />La spécification du paramètre \<**ID d’activation**\> affiche la date d’expiration de l’activation de l’édition spécifiée associée à cet ID d’activation. Cette opération ne nécessite pas de privilèges élevés. |

## <a name="advanced-options"></a>Options avancées

|Option |Description |
| - | - |
|\/cpky |Pour s'exécuter, certaines opérations de service ont besoin que la clé de produit soit disponible dans le Registre en mode OOBE (Out-of-Box Experience). L'option **/cpky** supprime la clé de produit du Registre pour empêcher son vol par du code malveillant.<br />Pour les installations de versions commerciales déployant des clés, nous vous recommandons, à titre de meilleure pratique, d'utiliser cette option. Cette option n'est pas nécessaire pour les clés d'hôte MAK et KMS, car il s'agit pour elles du comportement par défaut. Cette option est nécessaire seulement pour les autres types de clés dont le comportement par défaut est de ne pas être supprimées du Registre.<br />Cette opération doit être exécutée à partir d’une fenêtre d’invite de commandes avec élévation de privilèges. |
|\/ilc&nbsp;&lt;fichier_licence&gt; |Cette option installe le fichier de licence spécifié par le paramètre nécessaire. Ces licences peuvent être installées pour les besoins de dépannage, pour prendre en charge l'activation basée sur les jetons, ou dans le cadre d'une installation manuelle d'une application embarquée.<br />Les licences ne sont pas validées lors de ce processus : La validation de la licence est en dehors de l’étendue de Slmgr.vbs. À la place, la validation est réalisée par le service de protection logicielle au moment de l'exécution.<br />Cette opération doit être effectuée à partir d’une fenêtre d’invite de commandes avec élévation de privilèges. Sinon, la valeur de Registre **Standard User Operations** doit être définie afin d’étendre l’accès des utilisateurs non privilégiés au service de protection logicielle. |
|\/rilc |Cette option réinstalle toutes les licences stockées dans les dossiers %SystemRoot%\system32\oem et %SystemRoot%\System32\spp\tokens. Il s’agit de copies reconnues fiables qui ont été stockées lors de l’installation.<br />Toute licence correspondante trouvée dans le magasin de confiance est remplacée. Les licences supplémentaires, par exemple les licences d’émission (IL) d’une autorité de confiance (TA) ou les licences d’applications, ne sont pas affectées.<br />Cette opération doit être effectuée dans une fenêtre d’invite de commandes avec élévation de privilèges. Sinon, la valeur de Registre **Standard User Operations** doit être définie afin d’étendre l’accès des utilisateurs non privilégiés au service de protection logicielle. |
|\/rearm |Cette option réinitialise les minuteurs d'activation. Le processus **/rearm** est également appelé par **sysprep /generalize**.<br />Cette opération reste sans effet si l’entrée de Registre **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SoftwareProtectionPlatform\SkipRearm** est définie sur **1**. Pour plus d’informations sur cette entrée de Registre, consultez [Paramètres du Registre pour l’activation en volume](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn502532(v=ws.11)).<br />Cette opération doit être effectuée dans une fenêtre d’invite de commandes avec élévation de privilèges. Sinon, la valeur de Registre **Standard User Operations** doit être définie afin d’étendre l’accès des utilisateurs non privilégiés au service de protection logicielle. |
|\/rearm-app &lt;ID&nbsp;d’application&gt; |Réinitialise l'état de la licence de l'application spécifiée. |
|\/rearm-sku &lt;ID&nbsp;d’application&gt; |Réinitialise l'état de la licence de la référence SKU spécifiée. |
|\/upk&nbsp;\[&lt;ID&nbsp;d’application&gt;] |Cette option désinstalle la clé de produit de l'édition actuelle de Windows. Après son redémarrage, le système sera associé à l'état "Sans licence" jusqu'à ce qu'une nouvelle clé de produit soit installée.<br />Si vous le souhaitez, vous pouvez utiliser le paramètre \<**ID d’activation**\> pour spécifier un autre produit installé.<br />Cette opération doit être effectuée à partir d’une fenêtre d’invite de commandes avec élévation de privilèges. |
|\/dti&nbsp;\[\<ID&nbsp;d’activation\>] |Affiche l'ID d'installation pour une activation hors connexion. |
|\/atp &lt;ID&nbsp;de confirmation&gt; |Active le produit avec l’ID de confirmation fourni par l’utilisateur. |

## <a name="kms-client-options"></a>options pour les clients KMS

|Option |Description |
| - | - |
|\/skms \<Nom\[:Port]&nbsp;\|&nbsp;\:&nbsp;port\> \[\<ID&nbsp;d’activation\>] |Cette option spécifie le nom et, éventuellement, le port de l'ordinateur hôte KMS à contacter. Si cette valeur est définie, la détection automatique de l'hôte KMS est désactivée.<br />Si l’hôte KMS utilise seulement le protocole Internet version 6 (IPv6), l’adresse doit être spécifiée au format \<nom_hôte\>:\<port\>. Les adresses IPv6 contiennent des signes deux-points (:), qui ne sont pas analysés correctement par le script Slmgr.vbs.<br />Cette opération doit être exécutée à partir d’une fenêtre d’invite de commandes avec élévation de privilèges. |
|\/skms-domain&nbsp;&lt;Nom_domaine_complet&gt; \[\<ID&nbsp;d’activation\>] |Définit le domaine DNS spécifique dans lequel se trouvent tous les enregistrements SRV d'hôte KMS. Ce paramètre reste sans effet si l’hôte KMS spécifique est défini avec l’option **/skms**. Utilisez cette option, en particulier dans les environnements d'espace de noms dissocié, pour forcer KMS à ignorer la liste de recherche de suffixes DNS et à rechercher à la place les enregistrements d'hôte KMS dans le domaine DNS spécifié. |
|\/ckms&nbsp;\[\<ID&nbsp;d’activation\>] |Cette option supprime le nom, l'adresse et les informations de port de l'hôte KMS spécifié dans le Registre, et rétablit le comportement de découverte automatique du KMS.<br />Cette opération doit être exécutée à partir d’une fenêtre d’invite de commandes avec élévation de privilèges. |
|\/skhc |Cette option active la mise en cache de l’hôte KMS (par défaut). Une fois que le client découvre un hôte KMS opérationnel, ce paramètre empêche la priorité et le poids DNS (Domain Name System) d’affecter plus avant la communication avec l’hôte. Si le système ne peut plus contacter l’hôte KMS opérationnel, le client essaye de découvrir un nouvel hôte.<br />Cette opération doit être exécutée à partir d’une fenêtre d’invite de commandes avec élévation de privilèges. |
|\/ckhc |Cette option désactive la mise en cache de l'hôte KMS. Ce paramètre indique au client d’utiliser la découverte automatique DNS à chaque tentative d’activation KMS (recommandé avec la priorité et la pondération).<br />Cette opération doit être exécutée à partir d’une fenêtre d’invite de commandes avec élévation de privilèges. |

## <a name="kms-host-configuration-options"></a>Options de configuration de l’hôte KMS

|Option |Description |
| - | - |
|\/sai&nbsp;&lt;Intervalle&gt; |Cette option définit l’intervalle, en minutes, entre chaque tentative de connexion des clients non activés à KMS. L’intervalle d’activation doit être compris entre 15 minutes et 30 jours, bien que la valeur par défaut (2 heures) soit recommandée.<br />Le client KMS récupère initialement cet intervalle du Registre, mais utilise ensuite le paramètre KMS après avoir reçu la première réponse de l'hôte KMS.<br />Cette opération doit être exécutée à partir d’une fenêtre d’invite de commandes avec élévation de privilèges. |
|\/sri &lt;Intervalle&gt; |Cette option définit l’intervalle de renouvellement, en minutes, entre chaque tentative de connexion des clients activés à KMS. L'intervalle de renouvellement doit être compris entre 15 minutes et 30 jours. Cette option est définie initialement sur le client et le serveur KMS. La valeur par défaut est égale à 10 080 minutes (7 jours).<br />Le client KMS récupère cet intervalle du Registre, mais utilise le paramètre KMS après avoir reçu la première réponse KMS.<br />Cette opération doit être exécutée à partir d’une fenêtre d’invite de commandes avec élévation de privilèges. |
|\/sprt&nbsp;&lt;Port&gt; |Cette option définit le port sur lequel l'hôte KMS écoute les demandes d'activation de client. Le port TCP par défaut est le port 1688.<br />Cette opération doit être effectuée à partir d’une fenêtre d’invite de commandes avec élévation de privilèges. |
|\/sdns |Active la publication DNS par l'hôte KMS (valeur par défaut).<br />Cette opération doit être exécutée à partir d’une fenêtre d’invite de commandes avec élévation de privilèges. |
|\/cdns |Désactive la publication DNS par l'hôte KMS.<br />Cette opération doit être exécutée à partir d’une fenêtre d’invite de commandes avec élévation de privilèges. |
|\/spri |Définit la priorité KMS à standard (valeur par défaut).<br />Cette opération doit être exécutée à partir d’une fenêtre d’invite de commandes avec élévation de privilèges. |
|\/cpri |Définit la priorité KMS à basse.<br />Utilisez cette option pour limiter la contention du KMS dans un environnement à deux hôtes. Notez que cela peut entraîner l’épuisement de KMS, selon les autres applications ou les autres rôles serveur actifs. À utiliser avec précaution.<br />Cette opération doit être exécutée à partir d’une fenêtre d’invite de commandes avec élévation de privilèges. |
|\/act-type \[\<Type_activation\>] \[\<ID&nbsp;d’activation\>] |Cette option définit une valeur dans le Registre qui limite l'activation en volume à un seul type. Le type d'activation **1** limite l'activation à Active Directory seulement, **2** à l'activation KMS et **3** à l'activation basée sur les jetons. La valeur par défaut **0** autorise tous les types d'activation. |

## <a name="token-based-activation-configuration-options"></a>Options de configuration de l’activation basée sur les jetons

|Option |Description |
| - | - |
|/lil |Répertorie toutes les licences d'émission installées avec l'activation basée sur les jetons. |
|\/ril&nbsp;&lt;ILID&gt;&nbsp;&lt;ILvID&gt; |Supprime une licence d'émission installée avec l'activation basée sur les jetons.<br />Cette opération doit être effectuée à partir d’une fenêtre d’invite de commandes avec élévation de privilèges. |
|\/stao |Définit l'indicateur **Token-based Activation Only**, ce qui désactive l'activation automatique du KMS.<br />Cette opération doit être exécutée à partir d’une fenêtre d’invite de commandes avec élévation de privilèges.<br />Cette option a été supprimée dans Windows Server 2012 R2 et Windows 8.1. Utilisez plutôt l'option **/act–type**. |
|\/ctao |Efface l'indicateur **Token-based Activation Only** (par défaut), ce qui active l'activation automatique du KMS.<br />Cette opération doit être exécutée à partir d’une fenêtre d’invite de commandes avec élévation de privilèges.<br />Cette option a été supprimée dans Windows Server 2012 R2 et Windows 8.1. Utilisez à la place l’option **/act–type**</strong>. |
|\/ltc |Répertorie les certificats valides d'activation basée sur les jetons qui peuvent activer les logiciels installés. |
|\/fta &lt;Empreinte&nbsp;certificat&gt; \[&lt;Code PIN&gt;] |Force l’activation basée sur les jetons avec le certificat identifié. Le code PIN facultatif est spécifié pour déverrouiller la clé privée sans invite de saisie du code PIN si vous utilisez des certificats avec protection matérielle (par exemple des cartes à puce). |

## <a name="active-directory-based-activation-configuration-options"></a>Options de configuration de l’activation basée sur Active Directory

|Option |Description |
| - | - |
|\/ad-activation-online &lt;Clé&nbsp;produit&gt; \[\<Nom&nbsp;objet&nbsp;d’activation\>] |Collecte les données d’Active Directory et démarrer l’activation de la forêt Active Directory avec les informations d’identification utilisées par l’invite de commandes. L’accès d’administrateur local n’est pas nécessaire. Un accès en lecture/écriture au conteneur d’objets d’activation dans le domaine racine de la forêt est néanmoins nécessaire. |
|\/ad-activation-get-IID &lt;Clé&nbsp;produit&gt; |Cette option démarre l'activation de la forêt Active Directory en mode téléphone. Le résultat est l’ID d’installation (IID) qui peut être utilisé pour activer la forêt par téléphone en l’absence de connectivité Internet. Après que l'IID a été fourni dans l'appel téléphonique d'activation, un CID est renvoyé et utilisé pour terminer l'activation. |
|\/ad-activation-apply-cid &lt;Clé&nbsp;produit&gt; &lt;ID&nbsp;de confirmation&gt; \[\<Nom&nbsp;objet&nbsp;d’activation>] |Quand vous utilisez cette option, entrez l’ID de confirmation fourni dans l’appel téléphonique d’activation pour effectuer l’activation. |
|\[/name: &lt;Nom_objet_activation&gt;] |Vous pouvez éventuellement ajouter l'option **/name** à toutes ces commandes pour attribuer un nom à l'objet d'activation stocké dans Active Directory. Le nom ne doit pas dépasser 40 caractères Unicode. Utilisez des guillemets doubles pour définir explicitement la chaîne du nom.<br />Dans Windows Server 2012 R2 et Windows 8.1, vous pouvez ajouter le nom directement après **/ad-activation-online &lt;Clé&nbsp;produit&gt;** **/ad-activation-apply-cid** sans devoir utiliser l’option **/name**. |
|\/ao-list |Affiche tous les objets d'activation disponibles sur l'ordinateur local. |
|\/del-ao &lt;DN_objet_activation&gt;<br />\/del-ao &lt;RDN_objet_activation&gt; |Supprime l'objet d'activation spécifié de la forêt. |

## <a name="see-also"></a>Voir également

- [Informations techniques de référence sur l’activation en volume](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn502529%28v%3dws.11%29)
- [Vue d’ensemble de l’activation en volume](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831612%28v%3dws.11%29)

