---
title: Instructions pour la résolution des problèmes KMS
description: Fournit des informations sur le service KMS, et suggère des outils et des approches pour la résolution des problèmes d’activation.
ms.topic: troubleshooting
ms.date: 9/24/2019
ms.technology: server-general
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: fc673d2c3e1404dbd750d4c0ef05ec6db50017aa
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "71963075"
---
# <a name="guidelines-for-troubleshooting-the-key-management-service-kms"></a>Instructions pour le dépannage du service de gestion de clés (KMS)

Dans le cadre de leur processus de déploiement, de nombreux clients d’entreprise configurent le service de gestion de clés (KMS, Key Management Service) pour permettre l’activation de Windows dans leur environnement. Le processus de configuration de l’hôte KMS est simple, après quoi les clients KMS découvrent l’hôte et essaient de s’activer eux-mêmes. Mais que se passe-t-il si ce processus ne fonctionne pas ? Que faire alors ? Cet article décrit les ressources dont vous avez besoin pour résoudre le problème. Pour plus d’informations sur les entrées du journal des événements et le script Slmgr.vbs, consultez [Informations techniques de référence sur l’activation en volume](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn502529(v=ws.11)).

## <a name="kms-overview"></a>Vue d’ensemble du service KMS

Commençons par un bref rappel concernant l’activation du service KMS. Le service KMS est un modèle client-serveur. D’un point de vue conceptuel, il ressemble à DHCP. Au lieu de transmettre des adresses IP aux clients quand ils le demandent, le service KMS autorise l’activation du produit. Le service KMS est aussi un modèle de renouvellement, dans lequel les clients essaient de se réactiver à intervalles réguliers. Il existe deux rôles : l’*hôte KMS* et le *client KMS*.

- L’**hôte KMS** exécute le service d’activation et permet l’activation dans l’environnement. Pour configurer un hôte KMS, vous devez installer une clé KMS à partir du centre MVLSC (Microsoft Volume License Service Center), puis activer le service.
- Le **client KMS** est le système d’exploitation Windows qui est déployé dans l’environnement et doit s’activer. Les clients KMS peuvent exécuter n’importe quelle édition de Windows qui utilise l’activation en volume. Les clients KMS sont fournis avec une clé pré-installée, appelée clé de licence en volume générique (GVLK, Generic Volume License Key) ou clé d’installation du client KMS. La présence de la clé GVLK est ce qui fait d’un système un client KMS. Les clients KMS utilisent des enregistrements SRV DNS (_vlmcs._tcp) pour identifier l’hôte KMS. Ensuite, les clients essaient automatiquement de découvrir et d’utiliser ce service pour s’activer. Pendant la période de grâce des 30 premiers jours, ils essaient de s’activer toutes les deux heures. Après l’activation, les clients KMS essaient de renouveler leur activation tous les sept jours.

Du point de vue de la résolution des problèmes, vous devrez peut-être examiner les deux côtés (hôte et client) pour déterminer ce qui se passe.

## <a name="kms-host"></a>Hôte KMS

Il y a deux zones à examiner sur l’hôte KMS. Tout d’abord, vérifiez l’état du service de licence du logiciel hôte. Ensuite, recherchez les événements relatifs à la gestion des licences ou à l’activation dans l’Observateur d’événements.

### <a name="slmgrvbs-and-the-software-licensing-service"></a>Slmgr.vbs et le service de gestion de licences des logiciels

Pour afficher la sortie détaillée du service de gestion de licences des logiciels, ouvrez une fenêtre d’invite de commandes avec élévation de privilèges et entrez **slmgr.vbs /dlv** à l’invite. La capture d’écran suivante montre les résultats de cette commande sur l’un de nos hôtes KMS au sein de Microsoft.

![Sortie de Slmgr sur l’hôte KMS](./media/ee939272.kms_slmgr_output(en-us,technet.10).png)

Les champs les plus importants pour la résolution des problèmes sont les suivants. Ce que vous recherchez peut être différent, en fonction du problème à résoudre.

- **Informations sur la version**. En haut de la sortie de **slmgr.vbs /dlv** figure la version du service de gestion de licences des logiciels. Elle peut être utile pour déterminer si la version actuelle du service est installée. Par exemple, les mises à jour du service KMS sur Windows Server 2003 prennent en charge différentes clés d’hôtes KMS. Vous pouvez utiliser ces données pour déterminer si la version est actuelle et prend en charge la clé d’hôte KMS que vous essayez d’installer. Pour plus d’informations sur ces mises à jour, consultez [Une mise à jour est disponible pour Windows Vista et Windows Server 2008 afin d’étendre la prise en charge de l’activation KMS pour Windows 7 et Windows Server 2008 R2](https://support.microsoft.com/help/968912/an-update-is-available-for-windows-vista-and-for-windows-server-2008-t).
- **Nom**. Indique l’édition de Windows installée sur le système hôte KMS. Peut être important si vous rencontrez des problèmes lors de l’ajout ou de la modification de la clé d’hôte KMS (par exemple, pour vérifier que la clé est prise en charge sur cette édition du système d’exploitation).
- **Description**. C’est ici que vous voyez la clé qui est installée. Utilisez ce champ pour vérifier quelle clé a été utilisée pour activer le service, et s’il s’agit de la clé correcte pour les clients KMS que vous avez déployés.
- **État de la licence**. Il s’agit de l’état du système hôte KMS. La valeur doit être **avec licence**. Toute autre valeur signifie qu’il y a un problème et que vous devrez peut-être réactiver l’hôte.
- **Décompte actuel**. Le nombre affiché sera compris entre **0** et **50**. Le décompte est cumulatif (entre les systèmes d’exploitation) et indique le nombre de systèmes valides qui ont tenté de s’activer au cours d’une période de 30 jours.  
  
  Si le nombre est **0**, cela signifie que le service a été activé récemment ou qu’aucun client valide ne s’est connecté à l’hôte KMS.  
  
  Le décompte n’augmente pas au-dessus de **50**, quel que soit le nombre de systèmes valides dans l’environnement. Cela est dû au fait qu’il est configuré pour mettre en cache uniquement deux fois la stratégie de licence maximale retournée par un client KMS. La stratégie maximale aujourd’hui est définie par le système d’exploitation client Windows, qui requiert un décompte de **25** ou plus à partir de l’hôte KMS pour s’activer. Par conséquent, le décompte le plus élevé sur l’hôte KMS est de 2 x 25, soit 50. Notez que dans les environnements qui contiennent uniquement des clients KMS Windows Server, le décompte maximal sur l’hôte KMS sera **10**. En effet, le seuil pour les éditions de Windows Server est de **5** (2 x 5, soit 10).  
  
  Un problème courant lié au décompte se produit si l’environnement a un hôte KMS activé et un nombre suffisant de clients, mais que le décompte n’augmente pas au-delà de un. La racine du problème est que l’image du client déployée n’a pas été configurée correctement (**sysprep/generalize**) et que les systèmes n’ont pas d’ID d’ordinateur client (CMID) uniques. Pour plus d’informations, consultez [Client KMS](#kms-client) et [Le décompte actuel du service KMS n’augmente pas quand vous ajoutez de nouveaux ordinateurs clients Windows Vista ou Windows 7 au réseau](https://support.microsoft.com/help/929829/the-kms-current-count-does-not-increase-when-you-add-new-windows-vista). L’un de nos ingénieurs support résolution a également rédigé un blog sur ce problème, intitulé [KMS Host Client Count not Increasing Due to Duplicate CMID’S](https://blogs.technet.microsoft.com/askcore/2009/10/16/kms-host-client-count-not-increasing-due-to-duplicate-cmids/) (Le décompte des clients hôtes KMS n’augmente pas à cause de doublons de CMID).  
  
  Il se peut aussi que le décompte n’augmente pas car il y a trop d’hôtes KMS dans l’environnement, et que le décompte est réparti sur tous ces hôtes.
- **Écoute sur le port**. La communication avec le service KMS utilise un appel RPC anonyme. Par défaut, les clients utilisent le port TCP 1688 pour se connecter à l’hôte KMS. Vérifiez que ce port est ouvert entre vos clients KMS et l’hôte KMS. Vous pouvez changer ou configurer le port sur l’hôte KMS. Pendant leur communication, l’hôte KMS envoie la désignation de port aux clients KMS. Si vous changez le port sur un client KMS, la désignation de port est remplacée quand ce client contacte l’hôte.

Les utilisateurs nous posent souvent des questions concernant la section « demandes cumulatives » de la sortie de **slmgr.vbs /dlv**. En général, ces données ne sont pas utiles pour la résolution des problèmes. L’hôte KMS tient à jour un enregistrement continu de l’état de chaque client KMS qui tente de s’activer ou de se réactiver. Les échecs de demandes indiquent des clients KMS qui ne sont pas pris en charge par l’hôte KMS. Par exemple, si un client KMS Windows 7 tente de s’activer auprès d’un hôte KMS qui a été activé à l’aide d’une clé KMS Windows Vista, l’activation échoue. Les lignes « Demandes avec l’état de licence » décrivent tous les états de licence possibles, passés et présents. Du point de vue de la résolution des problèmes, ces données ne sont pertinentes que si le décompte n’incrémente pas comme prévu. Dans ce cas, vous devriez observer une augmentation du nombre d’échecs de demandes. Cela indique que vous devez vérifier la clé de produit qui a été utilisée pour activer le système hôte KMS. Notez également que les valeurs de demandes cumulatives sont réinitialisées uniquement si vous réinstallez le système hôte KMS.

### <a name="useful-kms-host-events"></a>Événements d’hôte KMS utiles

#### <a name="event-id-12290"></a>ID d’événement 12290

L’hôte KMS enregistre l’ID d’événement 12290 quand un client KMS le contacte afin de s’activer. L’ID d’événement 12290 fournit une quantité importante d’informations que vous pouvez utiliser pour déterminer le type de client ayant contacté l’hôte et la raison pour laquelle un échec s’est produit. Le segment suivant d’entrée d’ID d’événement 12290 provient du journal des événements du service de gestion de clés de notre hôte KMS.  

![Événement KMS 12290](./media/ee939272.kms_12290_event(en-us,technet.10).png)

Les détails de l’événement incluent les informations suivantes :

- **Nombre minimal nécessaire pour l’activation**. Le client KMS signale que le décompte rapporté par l’hôte KMS doit être **5** pour que l’activation soit possible. Cela signifie qu’il s’agit d’un système d’exploitation Windows Server, bien qu’aucune édition spécifique ne soit indiquée. Si vos clients ne s’activent pas, vérifiez que le décompte est suffisant sur l’hôte.
- **ID de l’ordinateur client (CMID)** . Il s’agit d’une valeur unique sur chaque système. Si cette valeur n’est pas unique, cela est dû au fait qu’une image n’a pas été préparée correctement pour la distribution (**sysprep /generalize**). Ce problème se manifeste sur l’hôte KMS par un décompte qui n’augmente pas, même s’il y a suffisamment de clients dans l’environnement. Pour plus d’informations, consultez [Le décompte actuel du service KMS n’augmente pas quand vous ajoutez de nouveaux ordinateurs clients Windows Vista ou Windows 7 au réseau](https://support.microsoft.com/help/929829/the-kms-current-count-does-not-increase-when-you-add-new-windows-vista).
- **État de la licence et durée jusqu’à l’expiration de l’état**. Il s’agit de l’état actuel de la licence du client. Cela peut vous aider à différencier un client qui tente de s’activer pour la première fois d’un client qui tente de se réactiver. L’entrée de temps indique la durée pendant laquelle le client restera dans cet état si rien ne change.

Si vous dépannez un client et que vous ne trouvez pas d’ID d’événement 12290 correspondant sur l’hôte KMS, ce client ne se connecte pas à l’hôte KMS. Voici quelques raisons pour lesquelles une entrée d’ID d’événement 12290 peut ne pas exister :

- Une panne réseau s’est produite
- L’hôte ne peut pas être résolu ou n’est pas inscrit auprès du système DNS
- Le pare-feu bloque le port TCP 1688.
   Le port peut être bloqué à de nombreux emplacements dans l’environnement, notamment sur le système hôte KMS lui-même. Par défaut, l’hôte KMS a une exception de pare-feu pour le service KMS, mais elle n’est pas activée automatiquement. Vous devez activer l’exception
- Le journal des événements est plein.

Les clients KMS enregistrent deux événements correspondants, l’ID d’événement 12288 et l’ID d’événement 12289. Pour plus d’informations sur ces événements, consultez la section [Client KMS](#kms-client).

#### <a name="event-id-12293"></a>ID d’événement 12293

Un autre événement pertinent à rechercher sur votre hôte KMS est l’ID d’événement 12293. Il indique que l’hôte n’a pas publié les enregistrements requis dans le système DNS. Cette situation est connue pour provoquer des échecs, et c’est quelque chose que vous devez vérifier *après* avoir configuré votre hôte et *avant* de déployer des clients. Pour plus d’informations sur les problèmes liés au système DNS, consultez [Procédures de dépannage courantes pour les problèmes KMS et DNS](common-troubleshooting-procedures-kms-dns.md).

## <a name="kms-client"></a>Client KMS

Sur les clients, vous utilisez les mêmes outils (Slmgr et l’Observateur d’événements) pour résoudre les problèmes d’activation.

### <a name="slmgrvbs-and-the-software-licensing-service"></a>Slmgr.vbs et le service de gestion de licences des logiciels

Pour afficher la sortie détaillée du service de gestion de licences des logiciels, ouvrez une fenêtre d’invite de commandes avec élévation de privilèges et entrez **slmgr.vbs /dlv** à l’invite. La capture d’écran suivante montre les résultats de cette commande sur l’un de nos hôtes KMS au sein de Microsoft.

![Sortie de Slmgr sur le client KMS](./media/ee939272.kms_client_slmgr_output(en-us,technet.10).png)

La liste suivante présente les champs les plus importants pour la résolution des problèmes. Ce que vous recherchez peut être différent, en fonction du problème à résoudre.

- **Nom**. Cette valeur indique l’édition de Windows installée sur le système client KMS. Utilisez-la pour vérifier que la version de Windows que vous essayez d’activer peut utiliser le service KMS. Par exemple, notre support technique a observé des incidents lors desquels les clients essaient d’installer la clé d’installation du client KMS sur une édition de Windows qui n’utilise pas l’activation en volume, telle que Windows Vista Édition Intégrale.
- **Description**. Cette valeur indique la clé installée. VOLUME_KMSCLIENT indique que la clé d’installation du client KMS (ou GVLK) est installée (il s’agit de la configuration par défaut pour le support de licence en volume) et que ce système tente automatiquement de procéder à l’activation par le biais d’un hôte KMS. Si vous voyez autre chose ici, par exemple MAK, vous devrez réinstaller la clé GVLK pour configurer ce système en tant que client KMS. Vous pouvez installer manuellement la clé à l’aide de **slmgr.vbs /ipk &lt;*GVLK*&gt;** (comme décrit dans [Clés d’installation du client KMS](kmsclientkeys.md)) ou utiliser l’Outil Gestion de l’activation en volume (VAMT). Pour plus d’informations sur l’obtention et l’utilisation de l’outil VAMT, consultez [Informations techniques de référence sur l’outil Gestion de l’activation en volume](https://docs.microsoft.com/windows/deployment/volume-activation/volume-activation-management-tool).
- **Clé de produit partielle**. Comme pour le champ **Nom**, vous pouvez utiliser ces informations afin de déterminer si la clé d’installation correcte du client KMS est installée sur cet ordinateur (en d’autres termes, si la clé correspond au système d’exploitation installé sur le client KMS). Par défaut, la clé correcte est présente sur les systèmes qui sont créés à l’aide d’un support à partir du portail du centre MVLSC. Dans certains cas, les clients peuvent utiliser l’activation MAK (Multiple Activation Key) jusqu’à ce qu’il y ait suffisamment de systèmes dans l’environnement pour prendre en charge l’activation KMS. La clé d’installation du client KMS doit être installée sur ces systèmes afin de les faire passer de MAK à KMS. Utilisez l’outil VAMT pour installer cette clé et vérifiez que la clé correcte est appliquée.
- **État de la licence**. Cette valeur indique l’état du système client KMS. Pour un système qui a été activé à l’aide du service KMS, cette valeur doit être **avec licence**. Toute autre valeur peut indiquer un problème. Par exemple, si l’hôte KMS fonctionne correctement et que le client KMS ne s’active pas (par exemple, il reste à l’état **Grâce**), il se peut que quelque chose empêche le client de joindre le système hôte (par exemple un problème de pare-feu, une panne réseau ou similaire).
- **ID de l’ordinateur client (CMID)** . Chaque client KMS doit avoir un CMID unique. Comme mentionné dans la section [Hôte KMS](#kms-host), un problème courant lié au décompte se produit si l’environnement a un hôte KMS activé et un nombre suffisant de clients, mais que le décompte n’augmente pas au-delà de **1**. Pour plus d’informations, consultez [Le décompte actuel du service KMS n’augmente pas quand vous ajoutez de nouveaux ordinateurs clients Windows Vista ou Windows 7 au réseau](https://support.microsoft.com/help/929829/the-kms-current-count-does-not-increase-when-you-add-new-windows-vista).
- **Nom d’ordinateur KMS à partir de DNS**. Cette valeur indique le nom de domaine complet de l’hôte KMS que le client a utilisé pour l’activation, et le port TCP utilisé pour la communication.
- **Mise en cache de l’hôte KMS**. La valeur finale indique si la mise en cache est activée ou non. Par défaut, elle est activée. Cela signifie que le client KMS met en cache le nom de l’hôte KMS qu’il a utilisé pour l’activation, et qu’il communique directement avec cet hôte (au lieu d’interroger le système DNS) quand il est temps de procéder à la réactivation. Si le client ne parvient pas à contacter l’hôte KMS mis en cache, il interroge le système DNS pour découvrir un nouvel hôte KMS.

### <a name="useful-kms-client-events"></a>Événements utiles du client KMS

#### <a name="event-id-12288-and-event-id-12289"></a>ID d’événement 12288 et ID d’événement 12289

Quand un client KMS s’active ou se réactive avec succès, il enregistre deux événements : l’ID d’événement 12288 et l’ID d’événement 12289. Le segment suivant d’entrée d’ID d’événement 12288 provient du journal des événements du service de gestion de clés de notre client KMS.

![ID d’événement 12288 du client KMS](./media/ee939272.client_12288(en-us,technet.10).png)

Si vous voyez uniquement l’ID d’événement 12288 (sans ID d’événement 12289 correspondant), cela signifie que le client KMS n’a pas pu joindre l’hôte KMS, que l’hôte KMS n’a pas répondu ou que le client n’a pas reçu la réponse. Dans ce cas, vérifiez que l’hôte KMS est détectable et que les clients KMS peuvent le contacter.  

Les informations les plus pertinentes de l’ID d’événement 12288 sont les données figurant dans la section Informations. Par exemple, cette section indique l’état actuel du client ainsi que le nom de domaine complet et le port TCP utilisés par le client quand il a tenté de s’activer. Vous pouvez utiliser le nom de domaine complet pour résoudre les problèmes lors desquels le décompte sur un hôte KMS n’incrémente pas. Par exemple, si le nombre d’hôtes KMS accessibles aux clients est trop élevé (systèmes légitimes ou non fiables), le décompte peut être réparti sur tous ces clients.

Un échec d’activation ne signifie pas toujours que le client a l’ID 12288 et non 12289. Un échec d’activation ou de réactivation peut également avoir les deux événements. Dans ce cas, vous devez examiner le deuxième événement pour vérifier la cause de l’échec.

![ID d’événement 12289 du client KMS](./media/ee939272.client_12289(en-us,technet.10).png)

La section Informations de l’ID d’événement 12289 fournit les informations suivantes :

- **Indicateur d’activation**. Cette valeur indique si l’activation a réussi (**1**) ou échoué (**0**).
- **Décompte actuel sur l’hôte KMS**. Cette valeur reflète la valeur du décompte sur l’hôte KMS quand le client tente de s’activer. Si l’activation échoue, cela peut être dû au fait que le décompte est insuffisant pour ce système d’exploitation client ou qu’il n’y a pas assez de systèmes dans l’environnement pour générer le décompte.

## <a name="what-does-support-ask-for"></a>Que demande le support technique ?

Si vous devez appeler le support technique pour résoudre un problème d’activation, l’ingénieur du support technique vous demandera généralement les informations suivantes :

- Sortie de la commande **Slmgr.vbs /dlv** sur l’hôte KMS et les systèmes clients KMS. Que vous utilisiez wscript ou cscript pour exécuter la commande, vous pouvez utiliser Ctrl+C pour copier la sortie, puis la coller dans le Bloc-notes pour l’envoyer au contact du support technique.
- Les journaux des événements de l’hôte KMS (journal du service de gestion de clés) et des systèmes clients KMS (journal des applications)

## <a name="see-also"></a>Voir aussi
- [Ask the Core Team : #Activation](https://blogs.technet.microsoft.com/askcore/tag/Activation/)


