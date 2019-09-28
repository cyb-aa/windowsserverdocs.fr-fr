---
ms.assetid: 99a68050-8d19-4c58-ad86-e08a3dcdb4f7
title: Annexe L-événements à surveiller
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 07/30/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: e069fe004d9256682e5754fc90ae6cba88ee7cb3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402723"
---
# <a name="appendix-l-events-to-monitor"></a>Annexe L : événements à analyser

>S'applique à : Windows Server

Le tableau suivant répertorie les événements que vous devez surveiller dans votre environnement, conformément aux recommandations fournies dans [surveillance Active Directory pour les signes de compromission](../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md). Dans le tableau suivant, la colonne « ID d’événement Windows actuel » répertorie l’ID d’événement tel qu’il est implémenté dans les versions de Windows et de Windows Server actuellement en support standard.  
  
La colonne « ID d’événement Windows hérité » répertorie l’ID d’événement correspondant dans les versions héritées de Windows, telles que les ordinateurs clients exécutant Windows XP ou une version antérieure, ainsi que les serveurs exécutant Windows Server 2003 ou une version antérieure. La colonne « importance potentielle » indique si l’événement doit être considéré comme ayant une importance faible, moyenne ou élevée dans la détection des attaques et que la colonne « Résumé des événements » fournit une brève description de l’événement.  
  
Une importance potentielle élevée signifie qu’une occurrence de l’événement doit être examinée. Une gravité potentielle moyenne ou faible signifie que ces événements ne doivent être examinés que s’ils se produisent de manière inattendue ou dans des nombres qui dépassent considérablement la ligne de base prévue dans un laps de temps mesuré. Toutes les organisations doivent tester ces recommandations dans leurs environnements avant de créer des alertes qui requièrent obligatoirement une investigation. Chaque environnement est différent et certains événements classés avec une importance potentielle élevée peuvent se produire en raison d’autres événements inoffensifs.  
  
|||||  
|-|-|-|-|  
|**ID d’événement Windows actuel**|**ID d’événement Windows hérité**|**Importance potentielle**|**Résumé de l’événement**|  
|4618|N/A|Élevé|Un modèle d’événement de sécurité contrôlé s’est produit.|  
|4649|N/A|Élevé|Une attaque par relecture a été détectée. Il peut s’agir d’un faux positif inoffensif en raison d’une erreur de configuration.|  
|4719|612|Élevé|La stratégie d’audit du système a été modifiée.|  
|4765|N/A|Élevé|L’historique des SID a été ajouté à un compte.|  
|4766|N/A|Élevé|Une tentative d’ajout de l’historique SID à un compte a échoué.|  
|4794|N/A|Élevé|Une tentative de définition du mode de restauration des services d’annuaire a été effectuée.|  
|4897|801|Élevé|Séparation des rôles activée :|  
|4964|N/A|Élevé|Des groupes spéciaux ont été attribués à une nouvelle ouverture de session.|  
|5124|N/A|Élevé|Un paramètre de sécurité a été mis à jour sur le service du répondeur OCSP|  
|N/A|550|Moyen à élevé|Attaque par déni de service (DoS) possible|  
|1102|517|Moyen à élevé|Le journal d’audit a été effacé|  
|4621|N/A|Moyenne|Système restauré par l’administrateur à partir de CrashOnAuditFail. Les utilisateurs qui ne sont pas administrateurs sont désormais autorisés à se connecter. Certaines activités pouvant être auditées n’ont peut-être pas été enregistrées.|  
|4675|N/A|Moyenne|Les SID ont été filtrés.|  
|4692|N/A|Moyenne|Une tentative de sauvegarde de la clé principale de protection des données a été effectuée.|  
|4693|N/A|Moyenne|La récupération de la clé principale de protection des données a été tentée.|  
|4706|610|Moyenne|Une nouvelle approbation a été créée sur un domaine.|  
|4713|617|Moyenne|La stratégie Kerberos a été modifiée.|  
|4714|618|Moyenne|La stratégie de récupération de données chiffrées a été modifiée.|  
|4715|N/A|Moyenne|La stratégie d’audit (SACL) sur un objet a été modifiée.|  
|4716|620|Moyenne|Les informations de domaine approuvé ont été modifiées.|  
|4724|628|Moyenne|Une tentative de réinitialisation du mot de passe d’un compte a été effectuée.|  
|4727|631|Moyenne|Un groupe global sécurisé a été créé.|  
|4735|639|Moyenne|Un groupe local avec sécurité activée a été modifié.|  
|4737|641|Moyenne|Un groupe global avec sécurité activée a été modifié.|  
|4739|643|Moyenne|La stratégie de domaine a été modifiée.|  
|4754|658|Moyenne|Un groupe universel avec sécurité activée a été créé.|  
|4755|659|Moyenne|Un groupe universel avec sécurité activée a été modifié.|  
|4764|667|Moyenne|Un groupe dont la sécurité est désactivée a été supprimé|  
|4764|668|Moyenne|Le type d’un groupe a été modifié.|  
|4780|684|Moyenne|La liste de contrôle d’accès a été définie sur les comptes qui sont membres de groupes Administrateurs.|  
|4816|N/A|Moyenne|RPC a détecté une violation d’intégrité lors du déchiffrement d’un message entrant.|  
|4865|N/A|Moyenne|Une entrée d’informations de forêt approuvée a été ajoutée.|  
|4866|N/A|Moyenne|Une entrée d’informations de forêt approuvée a été supprimée.|  
|4867|N/A|Moyenne|Une entrée d’informations de forêt approuvée a été modifiée.|  
|4868|772|Moyenne|Le gestionnaire de certificats a refusé une requête de certificat en cours.|  
|4870|774|Moyenne|Les services de certificats ont révoqué un certificat.|  
|4882|786|Moyenne|Les autorisations de sécurité des services de certificats ont changé.|  
|4885|789|Moyenne|Le filtre d’audit des services de certificats a changé.|  
|4890|794|Moyenne|Les paramètres du gestionnaire de certificats des services de certificats ont changé.|  
|4892|796|Moyenne|La propriété des services de certificats a changé.|  
|4896|800|Moyenne|Une ou plusieurs rangées ont été supprimées de la base de données de certificats.|  
|4906|N/A|Moyenne|La valeur de CrashOnAuditFail a changé.|  
|4907|N/A|Moyenne|Les paramètres d’audit sur l’objet ont été modifiés.|  
|4908|N/A|Moyenne|La table de connexion des groupes spéciaux a été modifiée.|  
|4912|807|Moyenne|La stratégie d’audit par utilisateur a été modifiée.|  
|4960|N/A|Moyenne|IPsec a supprimé un paquet entrant qui a échoué lors d’une vérification de l’intégrité. Si ce problème persiste, cela peut indiquer un problème réseau ou que des paquets sont en cours de modification en transit vers cet ordinateur. Vérifiez que les paquets envoyés depuis l’ordinateur distant sont les mêmes que ceux reçus par cet ordinateur. Cette erreur peut également indiquer des problèmes d’interopérabilité avec d’autres implémentations IPsec.|  
|4961|N/A|Moyenne|IPsec a supprimé un paquet entrant qui n’a pas réussi une vérification de relecture. Si ce problème persiste, cela peut indiquer une attaque par relecture contre cet ordinateur.|  
|4962|N/A|Moyenne|IPsec a supprimé un paquet entrant qui n’a pas réussi une vérification de relecture. Le nombre de séquences du paquet entrant est trop faible pour garantir qu’il ne s’agissait pas d’une relecture.|  
|4963|N/A|Moyenne|IPsec a supprimé un paquet de texte en clair entrant qui aurait dû être sécurisé. Cela est généralement dû au fait que l’ordinateur distant modifie sa stratégie IPsec sans en informer cet ordinateur. Il peut également s’agir d’une tentative d’attaque par usurpation d’identité.|  
|4965|N/A|Moyenne|IPsec a reçu un paquet d’un ordinateur distant avec un index de paramètre de sécurité (SPI) incorrect. Cela est généralement dû à un matériel défectueux qui endommage les paquets. Si ces erreurs persistent, vérifiez que les paquets envoyés depuis l’ordinateur distant sont les mêmes que ceux reçus par cet ordinateur. Cette erreur peut également indiquer des problèmes d’interopérabilité avec d’autres implémentations d’IPsec. Dans ce cas, si la connectivité n’est pas entravée, ces événements peuvent être ignorés.|  
|4976|N/A|Moyenne|Au cours de la négociation en mode principal, IPsec a reçu un paquet de négociation non valide. Si ce problème persiste, cela peut indiquer un problème réseau ou une tentative de modification ou de relecture de cette négociation.|  
|4977|N/A|Moyenne|Lors de la négociation en mode rapide, IPsec a reçu un paquet de négociation non valide. Si ce problème persiste, cela peut indiquer un problème réseau ou une tentative de modification ou de relecture de cette négociation.|  
|4978|N/A|Moyenne|Au cours de la négociation en mode étendu, IPsec a reçu un paquet de négociation non valide. Si ce problème persiste, cela peut indiquer un problème réseau ou une tentative de modification ou de relecture de cette négociation.|  
|4983|N/A|Moyenne|Échec d’une négociation en mode étendu IPsec. L’Association de sécurité en mode principal correspondante a été supprimée.|  
|4984|N/A|Moyenne|Échec d’une négociation en mode étendu IPsec. L’Association de sécurité en mode principal correspondante a été supprimée.|  
|5027|N/A|Moyenne|Le service pare-feu Windows n’a pas pu récupérer la stratégie de sécurité à partir du stockage local. Il va continuer à appliquer la stratégie active.|  
|5028|N/A|Moyenne|Le service pare-feu Windows n’a pas pu analyser la nouvelle stratégie de sécurité. Il va continuer à appliquer la stratégie active.|  
|5029|N/A|Moyenne|Le service pare-feu Windows n’a pas pu initialiser le pilote. Il va continuer à appliquer la stratégie active.|  
|5030|N/A|Moyenne|Le service pare-feu Windows n’a pas pu démarrer.|  
|5035|N/A|Moyenne|Le pilote du pare-feu Windows n’a pas pu démarrer.|  
|5037|N/A|Moyenne|Le pilote du pare-feu Windows a détecté une erreur critique du Runtime. Arrêt en cours.|  
|5038|N/A|Moyenne|L’intégrité du code a déterminé que le hachage de l’image d’un fichier n’est pas valide. Le fichier peut être endommagé en raison d’une modification non autorisée, ou le hachage non valide peut indiquer une erreur de périphérique de disque potentielle.|  
|5120|N/A|Moyenne|Service du répondeur OCSP démarré|  
|5121|N/A|Moyenne|Arrêt du service du répondeur OCSP|  
|5122|N/A|Moyenne|Une entrée de configuration a changé dans le service du répondeur OCSP|  
|5123|N/A|Moyenne|Une entrée de configuration a changé dans le service du répondeur OCSP|  
|5376|N/A|Moyenne|Les informations d’identification du gestionnaire d’informations d’identification ont été sauvegardées.|  
|5377|N/A|Moyenne|Les informations d’identification du gestionnaire d’informations d’identification ont été restaurées à partir d’une sauvegarde.|  
|5453|N/A|Moyenne|Une négociation IPsec avec un ordinateur distant a échoué, car le service IKEEXT (module de génération de clé IPsec) IKE et AuthIP n’a pas démarré.|  
|5480|N/A|Moyenne|Les services IPsec n’ont pas pu obtenir la liste complète des interfaces réseau sur l’ordinateur. Cela pose un risque de sécurité potentiel, car certaines interfaces réseau peuvent ne pas obtenir la protection fournie par les filtres IPsec appliqués. Utilisez le composant logiciel enfichable Moniteur de sécurité IP pour diagnostiquer le problème.|  
|5483|N/A|Moyenne|Les services IPsec n’ont pas pu initialiser le serveur RPC. Les services IPsec n’ont pas pu être démarrés.|  
|5484|N/A|Moyenne|Les services IPsec ont rencontré une défaillance critique et ont été arrêtés. L’arrêt des services IPsec peut mettre l’ordinateur à un plus grand risque d’attaque réseau ou exposer l’ordinateur à des risques de sécurité potentiels.|  
|5485|N/A|Moyenne|Les services IPsec n’ont pas pu traiter certains filtres IPsec sur un événement Plug-and-Play pour les interfaces réseau. Cela pose un risque de sécurité potentiel, car certaines interfaces réseau peuvent ne pas obtenir la protection fournie par les filtres IPsec appliqués. Utilisez le composant logiciel enfichable Moniteur de sécurité IP pour diagnostiquer le problème.|  
|6145|N/A|Moyenne|Une ou plusieurs erreurs se sont produites lors du traitement de la stratégie de sécurité dans les objets stratégie de groupe.|  
|6273|N/A|Moyenne|Le serveur de stratégie réseau a refusé l’accès à un utilisateur.|  
|6274|N/A|Moyenne|Le serveur de stratégie réseau a ignoré la demande d’un utilisateur.|  
|6275|N/A|Moyenne|Le serveur NPS ignore la demande de gestion des comptes pour un utilisateur.|  
|6276|N/A|Moyenne|Le serveur de stratégie réseau a mis en quarantaine un utilisateur.|  
|6277|N/A|Moyenne|Le serveur NPS a accordé l’accès à un utilisateur, mais le met sous tension, car l’ordinateur hôte ne répond pas à la stratégie de contrôle d’intégrité définie.|  
|6278|N/A|Moyenne|Le serveur NPS a accordé un accès complet à un utilisateur, car celui-ci a atteint la stratégie de contrôle d’intégrité définie.|  
|6279|N/A|Moyenne|Le serveur NPS a verrouillé le compte d’utilisateur en raison de l’échec répété des tentatives d’authentification.|  
|6280|N/A|Moyenne|Le serveur de stratégie réseau a déverrouillé le compte d’utilisateur.|  
|-|640|Moyenne|Base de données de compte générale modifiée|  
|-|619|Moyenne|Modification de la stratégie de qualité de service|  
|24586|N/A|Moyenne|Une erreur s’est produite lors de la conversion du volume|  
|24592|N/A|Moyenne|Une tentative de redémarrage automatique de la conversion sur le volume% 2 a échoué.|  
|24593|N/A|Moyenne|Écriture des métadonnées : Le volume% 2 a renvoyé des erreurs lors de la tentative de modification des métadonnées. Si les échecs se poursuivent, déchiffrez le volume|  
|24594|N/A|Moyenne|Régénération des métadonnées : Une tentative d’écriture d’une copie des métadonnées sur le volume% 2 a échoué et peut apparaître en tant que disque endommagé. Si les échecs se poursuivent, déchiffrez le volume.|  
|4608|512|Faible|Démarrage de Windows.|  
|4609|513|Faible|Windows est en cours d’arrêt.|  
|4610|514|Faible|Un package d’authentification a été chargé par l’autorité de sécurité locale.|  
|4611|515|Faible|Un processus d’ouverture de session approuvé a été inscrit auprès de l’autorité de sécurité locale.|  
|4612|516|Faible|Les ressources internes allouées à la mise en file d’attente des messages d’audit ont été épuisées, entraînant la perte de certains audits.|  
|4614|518|Faible|Un package de notification a été chargé par le gestionnaire de compte de sécurité.|  
|4615|519|Faible|Utilisation non valide du port LPC.|  
|4616|520|Faible|L’heure système a été modifiée.|  
|4622|N/A|Faible|Un package de sécurité a été chargé par l’autorité de sécurité locale.|  
|4624|528 540|Faible|Un compte a été correctement connecté.|  
|4625|529-537539|Faible|Un compte n’a pas pu se connecter.|  
|4634|538|Faible|Un compte a été déconnecté.|  
|4646|N/A|Faible|Le mode de prévention DoS IKE a démarré.|  
|4647|551|Faible|Fermeture de session initiée par l’utilisateur.|  
|4648|552|Faible|Une tentative d’ouverture de session a été effectuée à l’aide d’informations d’identification explicites.|  
|4650|N/A|Faible|Une association de sécurité en mode principal IPsec a été établie. Le mode étendu n’a pas été activé. L’authentification par certificat n’a pas été utilisée.|  
|4651|N/A|Faible|Une association de sécurité en mode principal IPsec a été établie. Le mode étendu n’a pas été activé. Un certificat a été utilisé pour l’authentification.|  
|4652|N/A|Faible|Échec d’une négociation en mode principal IPsec.|  
|4653|N/A|Faible|Échec d’une négociation en mode principal IPsec.|  
|4654|N/A|Faible|Échec d’une négociation en mode rapide IPsec.|  
|4655|N/A|Faible|Une association de sécurité en mode principal IPsec s’est terminée.|  
|4656|560|Faible|Un handle vers un objet a été demandé.|  
|4657|567|Faible|Une valeur de registre a été modifiée.|  
|4658|562|Faible|Le descripteur d’un objet a été fermé.|  
|4659|N/A|Faible|Un handle vers un objet a été demandé avec intention de supprimer.|  
|4660|564|Faible|Un objet a été supprimé.|  
|4661|565|Faible|Un handle vers un objet a été demandé.|  
|4662|566|Faible|Une opération a été effectuée sur un objet.|  
|4663|567|Faible|Une tentative d’accès à un objet a été effectuée.|  
|4664|N/A|Faible|Une tentative de création d’un lien physique a été effectuée.|  
|4665|N/A|Faible|Une tentative a été effectuée pour créer un contexte client d’application.|  
|4666|N/A|Faible|Une application a tenté une opération :|  
|4667|N/A|Faible|Un contexte client d’application a été supprimé.|  
|4668|N/A|Faible|Une application a été initialisée.|  
|4670|N/A|Faible|Les autorisations sur un objet ont été modifiées.|  
|4671|N/A|Faible|Une application a tenté d’accéder à un ordinal bloqué par le biais du TBS.|  
|4672|576|Faible|Privilèges spéciaux attribués à une nouvelle ouverture de session.|  
|4673|577|Faible|Un service privilégié a été appelé.|  
|4674|578|Faible|Une opération a été tentée sur un objet privilégié.|  
|4688|592|Faible|Un nouveau processus a été créé.|  
|4689|593|Faible|Un processus s’est arrêté.|  
|4690|594|Faible|Une tentative a été effectuée pour dupliquer un handle vers un objet.|  
|4691|595|Faible|Un accès indirect à un objet a été demandé.|  
|4694|N/A|Faible|La protection des données protégées pouvant être auditées a été tentée.|  
|4695|N/A|Faible|Une tentative de déprotection des données protégées pouvant être auditées a été effectuée.|  
|4696|600|Faible|Un jeton principal a été assigné au processus.|  
|4697|601|Faible|Tentative d’installation d’un service|  
|4698|602|Faible|Une tâche planifiée a été créée.|  
|4699|602|Faible|Une tâche planifiée a été supprimée.|  
|4700|602|Faible|Une tâche planifiée a été activée.|  
|4701|602|Faible|Une tâche planifiée a été désactivée.|  
|4702|602|Faible|Une tâche planifiée a été mise à jour.|  
|4704|608|Faible|Un droit d’utilisateur a été attribué.|  
|4705|609|Faible|Un droit d’utilisateur a été supprimé.|  
|4707|611|Faible|Une approbation à un domaine a été supprimée.|  
|4709|N/A|Faible|Les services IPsec ont été démarrés.|  
|4710|N/A|Faible|Les services IPsec ont été désactivés.|  
|4711|N/A|Faible|Peut contenir l’un des éléments suivants : Le moteur PAStore a appliqué une copie mise en cache localement de Active Directory stratégie IPsec de stockage sur l’ordinateur. Le moteur PAStore a appliqué Active Directory stratégie IPsec de stockage sur l’ordinateur. Le moteur PAStore a appliqué la stratégie IPsec de stockage du registre local sur l’ordinateur. Le moteur PAStore n’a pas pu appliquer la copie mise en cache localement de Active Directory stratégie IPsec de stockage sur l’ordinateur. Le moteur PAStore n’a pas pu appliquer Active Directory stratégie IPsec de stockage sur l’ordinateur. Le moteur PAStore n’a pas pu appliquer la stratégie IPsec de stockage du registre local sur l’ordinateur. Le moteur PAStore n’a pas pu appliquer certaines règles de la stratégie IPsec active sur l’ordinateur. Le moteur PAStore n’a pas pu charger la stratégie IPsec de stockage d’annuaire sur l’ordinateur. Le moteur PAStore a chargé la stratégie IPsec de stockage d’annuaire sur l’ordinateur. Le moteur PAStore n’a pas pu charger la stratégie IPsec de stockage local sur l’ordinateur. Le moteur PAStore a chargé la stratégie IPsec de stockage local sur l’ordinateur. Le moteur PAStore a interrogé les modifications apportées à la stratégie IPsec active et n’a détecté aucune modification. |  
|4712|N/A|Faible|Les services IPsec ont rencontré un échec potentiellement sérieux.|  
|4717|621|Faible|L’accès à la sécurité du système a été accordé à un compte.|  
|4718|622|Faible|L’accès à la sécurité système a été supprimé d’un compte.|  
|4720|624|Faible|Un compte d’utilisateur a été créé.|  
|4722|626|Faible|Un compte d’utilisateur a été activé.|  
|4723|627|Faible|Une tentative de modification du mot de passe d’un compte a été effectuée.|  
|4725|629|Faible|Un compte d’utilisateur a été désactivé.|  
|4726|630|Faible|Un compte d’utilisateur a été supprimé.|  
|4728|632|Faible|Un membre a été ajouté à un groupe global avec sécurité activée.|  
|4729|633|Faible|Un membre a été supprimé d’un groupe global avec sécurité activée.|  
|4730|634|Faible|Un groupe global avec sécurité activée a été supprimé.|  
|4731|635|Faible|Un groupe local avec sécurité activée a été créé.|  
|4732|636|Faible|Un membre a été ajouté à un groupe local avec sécurité activée.|  
|4733|637|Faible|Un membre a été supprimé d’un groupe local avec sécurité activée.|  
|4734|638|Faible|Un groupe local avec sécurité activée a été supprimé.|  
|4738|642|Faible|Un compte d’utilisateur a été modifié.|  
|4740|644|Faible|Un compte d’utilisateur a été verrouillé.|  
|4741|645|Faible|Un compte d’ordinateur a été modifié.|  
|4742|646|Faible|Un compte d’ordinateur a été modifié.|  
|4743|647|Faible|Un compte d’ordinateur a été supprimé.|  
|4744|648|Faible|Un groupe local de sécurité désactivée a été créé.|  
|4745|649|Faible|Un groupe local dont la sécurité est désactivée a été modifié.|  
|4746|650|Faible|Un membre a été ajouté à un groupe local dont la sécurité est désactivée.|  
|4747|651|Faible|Un membre a été supprimé d’un groupe local dont la sécurité est désactivée.|  
|4748|652|Faible|Un groupe local dont la sécurité est désactivée a été supprimé.|  
|4749|653|Faible|Un groupe global dont la sécurité est désactivée a été créé.|  
|4750|654|Faible|Un groupe global dont la sécurité est désactivée a été modifié.|  
|4751|655|Faible|Un membre a été ajouté à un groupe global dont la sécurité est désactivée.|  
|4752|656|Faible|Un membre a été supprimé d’un groupe global dont la sécurité est désactivée.|  
|4753|657|Faible|Un groupe global dont la sécurité est désactivée a été supprimé.|  
|4756|660|Faible|Un membre a été ajouté à un groupe universel avec sécurité activée.|  
|4757|661|Faible|Un membre a été supprimé d’un groupe universel avec sécurité activée.|  
|4758|662|Faible|Un groupe universel avec sécurité activée a été supprimé.|  
|4759|663|Faible|Un groupe universel dont la sécurité est désactivée a été créé.|  
|4760|664|Faible|Un groupe universel dont la sécurité est désactivée a été modifié.|  
|4761|665|Faible|Un membre a été ajouté à un groupe universel dont la sécurité est désactivée.|  
|4762|666|Faible|Un membre a été supprimé d’un groupe universel dont la sécurité est désactivée.|  
|4767|671|Faible|Un compte d’utilisateur a été déverrouillé.|  
|4768|672 676|Faible|Un ticket d’authentification Kerberos (TGT) a été demandé.|  
|4769|673|Faible|Un ticket de service Kerberos a été demandé.|  
|4770|674|Faible|Un ticket de service Kerberos a été renouvelé.|  
|4771|675|Faible|Échec de la pré-authentification Kerberos.|  
|4772|672|Faible|Une demande de ticket d’authentification Kerberos a échoué.|  
|4774|678|Faible|Un compte a été mappé pour l’ouverture de session.|  
|4775|679|Faible|Un compte n’a pas pu être mappé pour l’ouverture de session.|  
|4776|680 681|Faible|Le contrôleur de domaine a tenté de valider les informations d’identification d’un compte.|  
|4777|N/A|Faible|Le contrôleur de domaine n’a pas pu valider les informations d’identification d’un compte.|  
|4778|682|Faible|Une session a été reconnectée à une station Windows.|  
|4779|683|Faible|Une session a été déconnectée d’une station Windows.|  
|4781|685|Faible|Le nom d’un compte a été modifié :|  
|4782|N/A|Faible|Hachage du mot de passe auquel un compte a accédé.|  
|4783|667|Faible|Un groupe d’applications de base a été créé.|  
|4784|N/A|Faible|Un groupe d’applications de base a été modifié.|  
|4785|689|Faible|Un membre a été ajouté à un groupe d’applications de base.|  
|4786|690|Faible|Un membre a été supprimé d’un groupe d’applications de base.|  
|4787|691|Faible|Un non membre a été ajouté à un groupe d’applications de base.|  
|4788|692|Faible|Un non membre a été supprimé d’un groupe d’applications de base.|  
|4789|693|Faible|Un groupe d’applications de base a été supprimé.|  
|4790|694|Faible|Un groupe de requêtes LDAP a été créé.|  
|4793|N/A|Faible|L’API de vérification de la stratégie de mot de passe a été appelée.|  
|4800|N/A|Faible|La station de travail a été verrouillée.|  
|4801|N/A|Faible|La station de travail a été déverrouillée.|  
|4802|N/A|Faible|L’économiseur d’écran a été appelé.|  
|4803|N/A|Faible|L’économiseur d’écran a été ignoré.|  
|4864|N/A|Faible|Une collision d’espace de noms a été détectée.|  
|4869|773|Faible|Les services de certificats ont reçu une requête de certificat soumise à nouveau.|  
|4871|775|Faible|Les services de certificats ont reçu une requête pour publier la liste de révocation des certificats (CRL).|  
|4872|776|Faible|Les services de certificats ont publié la liste de révocation des certificats (CRL).|  
|4873|777|Faible|Une extension de requête de certificats a changé.|  
|4874|778|Faible|Un ou plusieurs attributs de requête de certificats ont changé.|  
|4875|779|Faible|Les services de certificats ont reçu une requête d’arrêt.|  
|4876|780|Faible|La sauvegarde des services de certificats a démarré.|  
|4877|781|Faible|La sauvegarde des services de certificats est terminée.|  
|4878|782|Faible|La restauration des services de certificats a démarré.|  
|4879|783|Faible|La restauration des services de certificats est terminée.|  
|4880|784|Faible|Les services de certificats ont démarré.|  
|4881|785|Faible|Les services de certificats se sont arrêtés.|  
|4883|787|Faible|Les services de certificats ont récupéré une clé archivée.|  
|4884|788|Faible|Les services de certificats ont importé un certificat dans sa base de données.|  
|4886|790|Faible|Les services de certificats ont reçu une requête de certificat.|  
|4887|791|Faible|Les services de certificats ont approuvé une requête de certificat et émis un certificat.|  
|4888|792|Faible|Les services de certificats ont refusé une requête de certificat.|  
|4889|793|Faible|Les services de certificats ont défini le statut d’une requête de certificat comme étant en attente.|  
|4891|795|Faible|Une entrée de configuration a changé dans les services de certificats.|  
|4893|797|Faible|Les services de certificats ont archivé une clé.|  
|4894|798|Faible|Les services de certificats ont importé et archivé une clé.|  
|4895|799|Faible|Les services de certificats ont publié le certificat de l’autorité de certification dans Active Directory Domain Services.|  
|4898|802|Faible|Les services de certificats ont chargé une modèle.|  
|4902|N/A|Faible|La table de stratégie d’audit par utilisateur a été créée.|  
|4904|N/A|Faible|Une tentative a été effectuée pour inscrire une source d’événement de sécurité.|  
|4905|N/A|Faible|Une tentative d’annulation de l’inscription d’une source d’événement de sécurité a été effectuée.|  
|4909|N/A|Faible|Les paramètres de stratégie locale pour le TBS ont été modifiés.|  
|4910|N/A|Faible|Les paramètres de stratégie de groupe du TBS ont été modifiés.|  
|4928|N/A|Faible|Un contexte de nommage de la source de réplica Active Directory a été établi.|  
|4929|N/A|Faible|Un contexte de nommage de la source de réplica Active Directory a été supprimé.|  
|4930|N/A|Faible|Une Active Directory contexte de nommage de la source de réplica a été modifiée.|  
|4931|N/A|Faible|Un contexte d’appellation de destination de réplica Active Directory a été modifié.|  
|4932|N/A|Faible|La synchronisation d’un réplica d’un contexte de nommage de Active Directory a commencé.|  
|4933|N/A|Faible|La synchronisation d’un réplica d’un contexte d’appellation Active Directory s’est terminée.|  
|4934|N/A|Faible|Les attributs d’un objet Active Directory ont été répliqués.|  
|4935|N/A|Faible|Échec de la réplication.|  
|4936|N/A|Faible|L’échec de la réplication se termine.|  
|4937|N/A|Faible|Un objet en attente a été supprimé d’un réplica.|  
|4944|N/A|Faible|La stratégie suivante était active au démarrage du pare-feu Windows.|  
|4945|N/A|Faible|Une règle a été listée au démarrage du pare-feu Windows.|  
|4946|N/A|Faible|Une modification a été apportée à la liste des exceptions du pare-feu Windows. Une règle a été ajoutée.|  
|4947|N/A|Faible|Une modification a été apportée à la liste des exceptions du pare-feu Windows. Une règle a été modifiée.|  
|4948|N/A|Faible|Une modification a été apportée à la liste des exceptions du pare-feu Windows. Une règle a été supprimée.|  
|4949|N/A|Faible|Les paramètres du pare-feu Windows ont été rétablis aux valeurs par défaut.|  
|4950|N/A|Faible|Un paramètre de pare-feu Windows a changé.|  
|4951|N/A|Faible|Une règle a été ignorée, car son numéro de version principale n’a pas été reconnu par le pare-feu Windows.|  
|4952|N/A|Faible|Certaines parties d’une règle ont été ignorées, car son numéro de version mineure n’a pas été reconnu par le pare-feu Windows. Les autres parties de la règle seront appliquées.|  
|4953|N/A|Faible|Une règle a été ignorée par le pare-feu Windows, car elle n’a pas pu analyser la règle.|  
|4954|N/A|Faible|Les paramètres de stratégie de groupe du pare-feu Windows ont changé. Les nouveaux paramètres ont été appliqués.|  
|4956|N/A|Faible|Le pare-feu Windows a modifié le profil actif.|  
|4957|N/A|Faible|Le pare-feu Windows n’a pas appliqué la règle suivante :|  
|4958|N/A|Faible|Le pare-feu Windows n’a pas appliqué la règle suivante, car la règle faisait appel à des éléments non configurés sur cet ordinateur :|  
|4979|N/A|Faible|Les associations de sécurité en mode principal IPsec et en mode étendu ont été établies.|  
|4980|N/A|Faible|Les associations de sécurité en mode principal IPsec et en mode étendu ont été établies.|  
|4981|N/A|Faible|Les associations de sécurité en mode principal IPsec et en mode étendu ont été établies.|  
|4982|N/A|Faible|Les associations de sécurité en mode principal IPsec et en mode étendu ont été établies.|  
|4985|N/A|Faible|L’état d’une transaction a changé.|  
|5024|N/A|Faible|Le service pare-feu Windows a démarré avec succès.|  
|5025|N/A|Faible|Le service pare-feu Windows a été arrêté.|  
|5031|N/A|Faible|Le service pare-feu Windows a empêché une application d’accepter les connexions entrantes sur le réseau.|  
|5032|N/A|Faible|Le pare-feu Windows n’a pas pu informer l’utilisateur qu’il a empêché une application d’accepter les connexions entrantes sur le réseau.|  
|5033|N/A|Faible|Le pilote du pare-feu Windows a démarré avec succès.|  
|5034|N/A|Faible|Le pilote du pare-feu Windows a été arrêté.|  
|5039|N/A|Faible|Une clé de registre a été virtualisée.|  
|5040|N/A|Faible|Une modification a été apportée aux paramètres IPsec. Un ensemble d’authentification a été ajouté.|  
|5041|N/A|Faible|Une modification a été apportée aux paramètres IPsec. Un ensemble d’authentification a été modifié.|  
|5042|N/A|Faible|Une modification a été apportée aux paramètres IPsec. Un ensemble d’authentification a été supprimé.|  
|5043|N/A|Faible|Une modification a été apportée aux paramètres IPsec. Une règle de sécurité de connexion a été ajoutée.|  
|5044|N/A|Faible|Une modification a été apportée aux paramètres IPsec. Une règle de sécurité de connexion a été modifiée.|  
|5045|N/A|Faible|Une modification a été apportée aux paramètres IPsec. Une règle de sécurité de connexion a été supprimée.|  
|5046|N/A|Faible|Une modification a été apportée aux paramètres IPsec. Un jeu de chiffrement a été ajouté.|  
|5047|N/A|Faible|Une modification a été apportée aux paramètres IPsec. Un jeu de chiffrement a été modifié.|  
|5048|N/A|Faible|Une modification a été apportée aux paramètres IPsec. Un jeu de chiffrement a été supprimé.|  
|5050|N/A|Faible|Tentative de désactivation par programmation du pare-feu Windows à l’aide d’un appel à InetFwProfile. FirewallEnabled (false)|  
|5051|N/A|Faible|Un fichier a été virtualisé.|  
|5056|N/A|Faible|Un auto-test de chiffrement a été effectué.|  
|5057|N/A|Faible|Une opération de la primitive de chiffrement a échoué.|  
|5058|N/A|Faible|Opération de fichier de clé.|  
|5059|N/A|Faible|Opération de migration de clé.|  
|5060|N/A|Faible|Échec de l’opération de vérification.|  
|5061|N/A|Faible|Opération de chiffrement.|  
|5062|N/A|Faible|Un auto-test de chiffrement en mode noyau a été effectué.|  
|5063|N/A|Faible|Une opération de fournisseur de services de chiffrement a été tentée.|  
|5064|N/A|Faible|Une opération de contexte de chiffrement a été tentée.|  
|5065|N/A|Faible|Une tentative de modification du contexte de chiffrement a été effectuée.|  
|5066|N/A|Faible|Une opération de fonction de chiffrement a été tentée.|  
|5067|N/A|Faible|Une tentative de modification d’une fonction de chiffrement a été effectuée.|  
|5068|N/A|Faible|Une opération de fournisseur de fonctions de chiffrement a été tentée.|  
|5069|N/A|Faible|Une opération de propriété de fonction de chiffrement a été tentée.|  
|5070|N/A|Faible|Une tentative de modification de propriété de fonction de chiffrement a été effectuée.|  
|5125|N/A|Faible|Une demande a été envoyée au service du répondeur OCSP|  
|5126|N/A|Faible|Le certificat de signature a été automatiquement mis à jour par le service du répondeur OCSP|  
|5127|N/A|Faible|Le fournisseur de révocation OCSP a correctement mis à jour les informations de révocation|  
|5136|566|Faible|Un objet de service d’annuaire a été modifié.|  
|5137|566|Faible|Un objet de service d’annuaire a été créé.|  
|5138|N/A|Faible|Un objet de service d’annuaire n’a pas été supprimé.|  
|5139|N/A|Faible|Un objet de service d’annuaire a été déplacé.|  
|5140|N/A|Faible|Un objet partage réseau a fait l’objet d’un accès.|  
|5141|N/A|Faible|Un objet du service d’annuaire a été supprimé.|  
|5152|N/A|Faible|La plateforme de filtrage Windows a bloqué un paquet.|  
|5153|N/A|Faible|Un filtre de plateforme de filtrage Windows plus restrictif a bloqué un paquet.|  
|5154|N/A|Faible|La plateforme de filtrage Windows a autorisé une application ou un service à écouter sur un port les connexions entrantes.|  
|5155|N/A|Faible|La plateforme de filtrage Windows a empêché une application ou un service d’écouter sur un port pour les connexions entrantes.|  
|5156|N/A|Faible|La plateforme de filtrage Windows a autorisé une connexion.|  
|5157|N/A|Faible|La plateforme de filtrage Windows a bloqué une connexion.|  
|5158|N/A|Faible|La plateforme de filtrage Windows a autorisé une liaison à un port local.|  
|5159|N/A|Faible|La plateforme de filtrage Windows a bloqué une liaison à un port local.|  
|5378|N/A|Faible|La délégation des informations d’identification demandée n’a pas été autorisée par la stratégie.|  
|5440|N/A|Faible|La légende suivante était présente lors du démarrage du moteur de filtrage de base de la plateforme de filtrage Windows.|  
|5441|N/A|Faible|Le filtre suivant était présent lors du démarrage du moteur de filtrage de base de la plateforme de filtrage Windows.|  
|5442|N/A|Faible|Le fournisseur suivant était présent lors du démarrage du moteur de filtrage de base de la plateforme de filtrage Windows.|  
|5443|N/A|Faible|Le contexte de fournisseur suivant était présent lors du démarrage du moteur de filtrage de base de la plateforme de filtrage Windows.|  
|5444|N/A|Faible|La sous-couche suivante était présente lors du démarrage du moteur de filtrage de base de la plateforme de filtrage Windows.|  
|5446|N/A|Faible|Une légende de plateforme de filtrage Windows a été modifiée.|  
|5447|N/A|Faible|Un filtre de plateforme de filtrage Windows a été modifié.|  
|5448|N/A|Faible|Un fournisseur de plateforme de filtrage Windows a été modifié.|  
|5449|N/A|Faible|Un contexte de fournisseur de plateforme de filtrage Windows a été modifié.|  
|5450|N/A|Faible|Une sous-couche de la plateforme de filtrage Windows a été modifiée.|  
|5451|N/A|Faible|Une association de sécurité en mode rapide IPsec a été établie.|  
|5452|N/A|Faible|Une association de sécurité en mode rapide IPsec s’est terminée.|  
|5456|N/A|Faible|Le moteur PAStore a appliqué Active Directory stratégie IPsec de stockage sur l’ordinateur.|  
|5457|N/A|Faible|Le moteur PAStore n’a pas pu appliquer Active Directory stratégie IPsec de stockage sur l’ordinateur.|  
|5458|N/A|Faible|Le moteur PAStore a appliqué une copie mise en cache localement de Active Directory stratégie IPsec de stockage sur l’ordinateur.|  
|5459|N/A|Faible|Le moteur PAStore n’a pas pu appliquer la copie mise en cache localement de Active Directory stratégie IPsec de stockage sur l’ordinateur.|  
|5460|N/A|Faible|Le moteur PAStore a appliqué la stratégie IPsec de stockage du registre local sur l’ordinateur.|  
|5461|N/A|Faible|Le moteur PAStore n’a pas pu appliquer la stratégie IPsec de stockage du registre local sur l’ordinateur.|  
|5462|N/A|Faible|Le moteur PAStore n’a pas pu appliquer certaines règles de la stratégie IPsec active sur l’ordinateur. Utilisez le composant logiciel enfichable Moniteur de sécurité IP pour diagnostiquer le problème.|  
|5463|N/A|Faible|Le moteur PAStore a interrogé les modifications apportées à la stratégie IPsec active et n’a détecté aucune modification.|  
|5464|N/A|Faible|Le moteur PAStore a interrogé les modifications apportées à la stratégie IPsec active, a détecté des modifications et les a appliquées aux services IPsec.|  
|5465|N/A|Faible|Le moteur PAStore a reçu un contrôle pour le rechargement forcé de la stratégie IPsec et a correctement traité le contrôle.|  
|5466|N/A|Faible|Le moteur PAStore a interrogé les modifications apportées à la stratégie IPsec Active Directory, déterminé que Active Directory est inaccessible et utilise la copie mise en cache de la stratégie IPsec Active Directory à la place. Toutes les modifications apportées à la stratégie IPsec Active Directory depuis la dernière interrogation n’ont pas pu être appliquées.|  
|5467|N/A|Faible|Le moteur PAStore a interrogé les modifications apportées à la stratégie IPsec Active Directory, déterminé que Active Directory peut être atteint et n’a trouvé aucune modification de la stratégie. La copie mise en cache de l’Active Directory stratégie IPsec n’est plus utilisée.|  
|5468|N/A|Faible|Le moteur PAStore a interrogé les modifications apportées à la stratégie IPsec Active Directory, déterminé que Active Directory peut être atteint, a détecté des modifications apportées à la stratégie et appliqué ces modifications. La copie mise en cache de l’Active Directory stratégie IPsec n’est plus utilisée.|  
|5471|N/A|Faible|Le moteur PAStore a chargé la stratégie IPsec de stockage local sur l’ordinateur.|  
|5472|N/A|Faible|Le moteur PAStore n’a pas pu charger la stratégie IPsec de stockage local sur l’ordinateur.|  
|5473|N/A|Faible|Le moteur PAStore a chargé la stratégie IPsec de stockage d’annuaire sur l’ordinateur.|  
|5474|N/A|Faible|Le moteur PAStore n’a pas pu charger la stratégie IPsec de stockage d’annuaire sur l’ordinateur.|  
|5477|N/A|Faible|Le moteur PAStore n’a pas pu ajouter le filtre en mode rapide.|  
|5479|N/A|Faible|Les services IPsec ont été arrêtés avec succès. L’arrêt des services IPsec peut mettre l’ordinateur à un plus grand risque d’attaque réseau ou exposer l’ordinateur à des risques de sécurité potentiels.|  
|5632|N/A|Faible|Une demande a été effectuée pour l’authentification auprès d’un réseau sans fil.|  
|5633|N/A|Faible|Une demande a été effectuée pour l’authentification auprès d’un réseau câblé.|  
|5712|N/A|Faible|Un appel de procédure distante (RPC) a été tenté.|  
|5888|N/A|Faible|Un objet dans le catalogue COM+ a été modifié.|  
|5889|N/A|Faible|Un objet a été supprimé du catalogue COM+.|  
|5890|N/A|Faible|Un objet a été ajouté au catalogue COM+.|  
|6008|N/A|Faible|L’arrêt du système précédent était inattendu|  
|6144|N/A|Faible|La stratégie de sécurité dans les objets stratégie de groupe a été appliquée avec succès.|  
|6272|N/A|Faible|Le serveur de stratégie réseau a accordé l’accès à un utilisateur.|  
|N/A|561|Faible|Un handle vers un objet a été demandé.|  
|N/A|563|Faible|Objet ouvert pour la suppression|  
|N/A|625|Faible|Type de compte d’utilisateur modifié|  
|N/A|613|Faible|Agent de stratégie IPsec démarré|  
|N/A|614|Faible|Agent de stratégie IPsec désactivé|  
|N/A|615|Faible|Agent de stratégie IPsec|  
|N/A|616|Faible|L’agent de stratégie IPsec a rencontré un problème grave potentiel|  
|24577|N/A|Faible|Chiffrement du volume démarré|  
|24578|N/A|Faible|Chiffrement du volume arrêté|  
|24579|N/A|Faible|Chiffrement du volume terminé|  
|24580|N/A|Faible|Déchiffrement du volume démarré|  
|24581|N/A|Faible|Déchiffrement du volume arrêté|  
|24582|N/A|Faible|Déchiffrement du volume terminé|  
|24583|N/A|Faible|Thread de travail de conversion pour le volume démarré|  
|24584|N/A|Faible|Thread de travail de conversion pour le volume temporairement arrêté|  
|24588|N/A|Faible|L’opération de conversion sur le volume% 2 a rencontré une erreur de secteur incorrect. Veuillez valider les données sur ce volume|  
|24595|N/A|Faible|Le volume% 2 contient des clusters incorrects. Ces clusters seront ignorés lors de la conversion.|  
|24621|N/A|Faible|Vérification de l’état initial : Transaction de conversion de volume enchaînée sur% 2.|  
|5049|N/A|Faible|Une association de sécurité IPsec a été supprimée.|  
|5478|N/A|Faible|Les services IPsec ont démarré avec succès.|  
  
> [!NOTE]  
> Reportez-vous aux [événements d’audit de sécurité Windows](https://www.microsoft.com/download/details.aspx?id=50034) pour obtenir la liste de nombreux ID d’événement de sécurité et leurs significations.  
>
> Exécuter **wevtutil GP Microsoft-Windows-Security-auditer/GE/GM : true** pour obtenir une liste très détaillée de tous les ID d’événement de sécurité  
  
Pour plus d’informations sur les ID d’événements de sécurité Windows et leurs significations, consultez l’article Support Microsoft [Description des événements de sécurité dans Windows 7 et Windows Server 2008 R2](https://support.microsoft.com/kb/977519). Vous pouvez également télécharger [des événements d’audit de sécurité pour les détails des événements de sécurité Windows 7 et Windows server 2008 R2](https://www.microsoft.com/download/details.aspx?id=21561) et Windows [8 et Windows Server 2012](https://www.microsoft.com/download/details.aspx?id=35753), qui fournissent des informations détaillées sur les événements pour les systèmes d’exploitation référencés dans une feuille de calcul. format.  
