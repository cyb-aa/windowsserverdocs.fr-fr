---
ms.assetid: 99a68050-8d19-4c58-ad86-e08a3dcdb4f7
title: Annexe L - événements à surveiller
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 07/30/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c245c5a6b2165385096f32713a92916236cdddfb
ms.sourcegitcommit: a3958dba4c2318eaf2e89c7532e36c78b1a76644
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66719693"
---
# <a name="appendix-l-events-to-monitor"></a>Annexe l : événements à analyser

>S'applique à : Windows Server

Le tableau suivant répertorie les événements que vous devez surveiller dans votre environnement, selon les recommandations fournies dans [surveillance d’Active Directory pour les signes de compromission](../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md). Dans le tableau suivant, la colonne « ID d’événement Windows actuel » répertorie l’ID d’événement tel qu’il est implémenté dans les versions de Windows et Windows Server qui se trouvent actuellement dans le support standard.  
  
La colonne « ID d’événement Windows hérité » répertorie l’ID d’événement correspondant dans les versions héritées de Windows telles que les ordinateurs clients exécutant Windows XP ou version antérieure et les serveurs exécutant Windows Server 2003 ou version antérieure. La colonne « importance potentielle » colonne identifie si l’événement doit être considéré comme de faible, moyenne ou haute importance dans la détection des attaques et la colonne « Événement résumé » fournit une brève description de l’événement.  
  
Une importance potentielle élevée signifie qu’une occurrence de l’événement doit être examiné. Importance potentielle moyenne ou faible signifie que ces événements doivent seulement être analysés s’ils se produisent de façon inattendue ou de nombres qui dépasse considérablement la ligne de base attendu dans un laps de temps mesuré. Toutes les organisations doivent tester ces recommandations dans leurs environnements avant de créer des alertes qui requièrent obligatoirement une investigation. Chaque environnement est différent, et certains événements classés avec une importance potentielle élevée peuvent survenir en raison d’autres événements inoffensifs.  
  
|||||  
|-|-|-|-|  
|**ID d’événement Windows actuel**|**ID d’événement Windows hérité**|**Importance potentielle**|**Résumé des événements**|  
|4618|N/A|Élevée|Un modèle d’événement de sécurité surveillé s’est produite.|  
|4649|N/A|Élevée|Une attaque par relecture a été détectée. Peut être un faux positif sans incidence en raison d’une erreur de configuration incorrecte.|  
|4719|612|Élevée|Stratégie d’audit système a été modifié.|  
|4765|N/A|Élevée|SID History a été ajouté à un compte.|  
|4766|N/A|Élevée|Échec de la tentative d’ajout du SID History à un compte.|  
|4794|N/A|Élevée|Une tentative a été effectuée pour définir le Mode de restauration des Services de répertoire.|  
|4897|801|Élevée|Séparation de rôle activée :|  
|4964|N/A|Élevée|Groupes spéciaux ont été affectés à une nouvelle ouverture de session.|  
|5124|N/A|Élevée|Un paramètre de sécurité a été mis à jour sur le Service de répondeur OCSP|  
|N/A|550|Taille moyenne à élevée|Attaque de possibles par déni de service (DoS)|  
|1102|517|Taille moyenne à élevée|Le journal d’audit a été effacé|  
|4621|N/A|Moyen|Administrateur récupéré un système à partir de CrashOnAuditFail. Les utilisateurs qui ne sont pas administrateurs pourront désormais se connecter. Une activité pouvant être auditée ne peut pas ont été enregistrée.|  
|4675|N/A|Moyen|SID ont été filtrées.|  
|4692|N/A|Moyen|Sauvegarde de la clé principale de protection des données a été tentée.|  
|4693|N/A|Moyen|Récupération de clé principale de protection des données a été tentée.|  
|4706|610|Moyen|Une nouvelle relation d’approbation a été créée pour un domaine.|  
|4713|617|Moyen|Stratégie Kerberos a été modifié.|  
|4714|618|Moyen|Stratégie de récupération a été modifié.|  
|4715|N/A|Moyen|La stratégie d’audit (SACL) sur un objet a été modifiée.|  
|4716|620|Moyen|Informations de domaine approuvé a été modifiées.|  
|4724|628|Moyen|Une tentative a été effectuée pour réinitialiser le mot de passe d’un compte.|  
|4727|631|Moyen|Un groupe global de sécurité activée a été créé.|  
|4735|639|Moyen|Un groupe local de sécurité activée a été modifié.|  
|4737|641|Moyen|Un groupe global de sécurité activée a été modifié.|  
|4739|643|Moyen|Stratégie de domaine a été modifiée.|  
|4754|658|Moyen|Un groupe universel de sécurité activée a été créé.|  
|4755|659|Moyen|Un groupe universel de sécurité activée a été modifié.|  
|4764|667|Moyen|Un groupe de sécurité est désactivée a été supprimé.|  
|4764|668|Moyen|Type de groupe a été modifié.|  
|4780|684|Moyen|L’ACL a été définie sur les comptes qui sont membres de groupes d’administrateurs.|  
|4816|N/A|Moyen|RPC a détecté une violation d’intégrité lors du déchiffrement d’un message entrant.|  
|4865|N/A|Moyen|Une entrée d’informations de forêt approuvée a été ajoutée.|  
|4866|N/A|Moyen|Une entrée d’informations de forêt approuvée a été supprimée.|  
|4867|N/A|Moyen|Une entrée d’informations de forêt approuvée a été modifiée.|  
|4868|772|Moyen|Le gestionnaire de certificats a refusé une requête de certificat en cours.|  
|4870|774|Moyen|Les services de certificats ont révoqué un certificat.|  
|4882|786|Moyen|Les autorisations de sécurité des services de certificats ont changé.|  
|4885|789|Moyen|Le filtre d’audit des services de certificats a changé.|  
|4890|794|Moyen|Les paramètres du gestionnaire de certificats des services de certificats ont changé.|  
|4892|796|Moyen|La propriété des services de certificats a changé.|  
|4896|800|Moyen|Une ou plusieurs rangées ont été supprimées de la base de données de certificats.|  
|4906|N/A|Moyen|La valeur CrashOnAuditFail a changé.|  
|4907|N/A|Moyen|Paramètres d’audit sur l’objet ont été modifiées.|  
|4908|N/A|Moyen|Table de groupes d’ouverture de session spéciale modifié.|  
|4912|807|Moyen|Par la stratégie d’Audit de l’utilisateur a été modifié.|  
|4960|N/A|Moyen|IPsec a supprimé un paquet entrant qui a échoué à un contrôle d’intégrité. Si le problème persiste, cela peut indiquer qu'un problème réseau ou qui les paquets sont en cours de modification en transit vers cet ordinateur. Vérifiez que les paquets envoyés à partir de l’ordinateur distant sont les mêmes que celles reçues par cet ordinateur. Cette erreur peut également indiquer des problèmes d’interopérabilité avec d’autres implémentations d’IPsec.|  
|4961|N/A|Moyen|IPsec a supprimé un paquet entrant qui a échoué d’une vérification de la relecture. Si le problème persiste, cela peut indiquer une attaque par relecture par rapport à cet ordinateur.|  
|4962|N/A|Moyen|IPsec a supprimé un paquet entrant qui a échoué d’une vérification de la relecture. Le paquet entrant a trop faible un numéro de séquence pour vous assurer qu’il n’était pas une relecture.|  
|4963|N/A|Moyen|IPsec a supprimé un paquet entrant clair qui devrait être sécurisé. Il s’agit généralement en raison de l’ordinateur distant, modification de la stratégie IPsec sans en informer de cet ordinateur. Cela peut également être une tentative d’attaque d’usurpation d’identité.|  
|4965|N/A|Moyen|IPsec a reçu un paquet à partir d’un ordinateur distant avec l’Index de paramètre de sécurité (SPI) un incorrect. Cela est généralement dû à un matériel défectueux qui endommage les paquets. Si ces erreurs persistent, vérifiez que les paquets envoyés à partir de l’ordinateur distant sont les mêmes que celles reçues par cet ordinateur. Cette erreur peut également indiquer des problèmes d’interopérabilité avec d’autres implémentations d’IPsec. Dans ce cas, si la connectivité n’est pas bloquée, puis ces événements peuvent être ignorés.|  
|4976|N/A|Moyen|Lors de la négociation de Mode principal IPsec a reçu un paquet de négociation non valide. Si le problème persiste, cela peut indiquer un problème réseau ou une tentative de modifier ou relire cette négociation.|  
|4977|N/A|Moyen|Lors de la négociation en Mode rapide, IPsec a reçu un paquet de négociation non valide. Si le problème persiste, cela peut indiquer un problème réseau ou une tentative de modifier ou relire cette négociation.|  
|4978|N/A|Moyen|Lors de la négociation en Mode étendu, IPsec a reçu un paquet de négociation non valide. Si le problème persiste, cela peut indiquer un problème réseau ou une tentative de modifier ou relire cette négociation.|  
|4983|N/A|Moyen|Échec d’une négociation IPsec Extended Mode. L’association de sécurité de Mode principal correspondante a été supprimée.|  
|4984|N/A|Moyen|Échec d’une négociation IPsec Extended Mode. L’association de sécurité de Mode principal correspondante a été supprimée.|  
|5027|N/A|Moyen|Le Service de pare-feu Windows n’a pas pu récupérer la stratégie de sécurité à partir du stockage local. Il va continuer à appliquer la stratégie active.|  
|5028|N/A|Moyen|Le Service de pare-feu Windows n’a pas pu analyser la nouvelle stratégie de sécurité. Il va continuer à appliquer la stratégie active.|  
|5029|N/A|Moyen|Le Service de pare-feu Windows Impossible d’initialiser le pilote. Il va continuer à appliquer la stratégie active.|  
|5030|N/A|Moyen|Échec du démarrage du Service de pare-feu Windows.|  
|5035|N/A|Moyen|Impossible de démarrer le pilote du pare-feu Windows.|  
|5037|N/A|Moyen|Le pilote du pare-feu Windows a détecté l’erreur d’exécution critiques. Arrêt en cours.|  
|5038|N/A|Moyen|L’intégrité du code déterminé que le hachage de l’image d’un fichier n’est pas valide. Le fichier est peut-être endommagé en raison de toute modification non autorisée ou le hachage non valide peut indiquer une erreur de périphérique de disque potentielle.|  
|5120|N/A|Moyen|Service de répondeur OCSP a démarré|  
|5121|N/A|Moyen|Arrêté du Service de répondeur OCSP|  
|5122|N/A|Moyen|Une entrée de configuration modifiée dans le Service de répondeur OCSP|  
|5123|N/A|Moyen|Une entrée de configuration modifiée dans le Service de répondeur OCSP|  
|5376|N/A|Moyen|Informations d’identification du Gestionnaire d’informations d’identification ont été sauvegardées.|  
|5377|N/A|Moyen|Informations d’identification du Gestionnaire d’informations d’identification ont été restaurées à partir d’une sauvegarde.|  
|5453|N/A|Moyen|Une négociation IPsec avec un ordinateur distant a échoué car le service IKE et AuthIP IPsec de génération de clés Modules (IKEEXT) n’est pas démarré.|  
|5480|N/A|Moyen|Services IPsec Impossible d’obtenir la liste complète des interfaces réseau sur l’ordinateur. Cela pose un risque de sécurité potentiel, car certaines interfaces réseau peuvent ne pas obtenir la protection offerte par les filtres IPsec appliqués. Utilisez le composant logiciel enfichable Moniteur de sécurité IP pour diagnostiquer le problème.|  
|5483|N/A|Moyen|Services IPsec Impossible d’initialiser le serveur RPC. Services IPsec n’a pas pu être démarrés.|  
|5484|N/A|Moyen|Services IPsec a rencontré une défaillance critique et a été arrêté. L’arrêt des Services IPsec peut placer l’ordinateur plus vulnérable aux attaques du réseau ou exposer l’ordinateur à des risques de sécurité potentiels.|  
|5485|N/A|Moyen|Services IPsec Échec du traitement de certains filtres IPsec sur un événement plug-and-play pour les interfaces réseau. Cela pose un risque de sécurité potentiel, car certaines interfaces réseau peuvent ne pas obtenir la protection offerte par les filtres IPsec appliqués. Utilisez le composant logiciel enfichable Moniteur de sécurité IP pour diagnostiquer le problème.|  
|6145|N/A|Moyen|Une ou plusieurs erreurs se sont produites lors du traitement de stratégie de sécurité dans les objets de stratégie de groupe.|  
|6273|N/A|Moyen|Serveur de stratégie réseau, accès est refusé à un utilisateur.|  
|6274|N/A|Moyen|Serveur de stratégie réseau rejeté la demande pour un utilisateur.|  
|6275|N/A|Moyen|Serveur de stratégie réseau rejeté la demande de comptabilité pour un utilisateur.|  
|6276|N/A|Moyen|Serveur de stratégie réseau mis en quarantaine d’un utilisateur.|  
|6277|N/A|Moyen|Network Policy Server accordé l’accès à un utilisateur tout en plaçant en probation car l’hôte ne respectait pas la stratégie d’intégrité défini.|  
|6278|N/A|Moyen|Network Policy Server a accordé un accès complet à un utilisateur, car l’hôte remplie de la stratégie d’intégrité défini.|  
|6279|N/A|Moyen|Serveur de stratégie réseau verrouillé le compte d’utilisateur en raison de tentatives répétées d’authentification.|  
|6280|N/A|Moyen|Serveur de stratégie réseau déverrouillées le compte d’utilisateur.|  
|-|640|Moyen|Base de données de compte général modifiée|  
|-|619|Moyen|Qualité de Service stratégie modifiée|  
|24586|N/A|Moyen|Une erreur s’est produite conversion du volume|  
|24592|N/A|Moyen|Une tentative de redémarrer automatiquement la conversion sur le volume %2 a échoué.|  
|24593|N/A|Moyen|Écriture de métadonnées : Volume %2 renvoie des erreurs lors de la tentative de modification des métadonnées. Si les échecs se répètent, déchiffrez le volume|  
|24594|N/A|Moyen|Reconstruction de métadonnées : Une tentative d’écriture d’une copie des métadonnées sur le volume %2 a échoué et ne peut apparaître comme une corruption de disque. Si les échecs se répètent, déchiffrez le volume.|  
|4608|512|Faible|Démarrage de Windows.|  
|4609|513|Faible|Windows s’arrête.|  
|4610|514|Faible|Un package d’authentification a été chargé par l’autorité de sécurité locale.|  
|4611|515|Faible|Un processus d’ouverture de session a été enregistré avec l’autorité de sécurité locale.|  
|4612|516|Faible|Les ressources internes allouées pour la file d’attente des messages d’audit ont été épuisées, entraînant la perte de certains des audits.|  
|4614|518|Faible|Un package de notification a été chargé par le Gestionnaire de compte de sécurité.|  
|4615|519|Faible|Utilisation non valide de port LPC.|  
|4616|520|Faible|L’heure système a été modifiée.|  
|4622|N/A|Faible|Un package de sécurité a été chargé par l’autorité de sécurité locale.|  
|4624|528,540|Faible|Un compte a été correctement connecté.|  
|4625|529-537,539|Faible|Un compte Impossible de se connecter.|  
|4634|538|Faible|Un compte a été déconnecté.|  
|4646|N/A|Faible|Mode de prévention DoS IKE démarré.|  
|4647|551|Faible|Déconnexion initiée par l’utilisateur.|  
|4648|552|Faible|Une ouverture de session a été tentée à l’aide des informations d’identification explicites.|  
|4650|N/A|Faible|Une association de sécurité de Mode principal IPsec a été établie. Mode étendu n’était pas activé. L’authentification par certificat n’a pas été utilisée.|  
|4651|N/A|Faible|Une association de sécurité de Mode principal IPsec a été établie. Mode étendu n’était pas activé. Un certificat a été utilisé pour l’authentification.|  
|4652|N/A|Faible|Échec d’une négociation de Mode principal IPsec.|  
|4653|N/A|Faible|Échec d’une négociation de Mode principal IPsec.|  
|4654|N/A|Faible|Échec d’une négociation de Mode rapide IPsec.|  
|4655|N/A|Faible|Une association de sécurité de Mode principal IPsec est terminée.|  
|4656|560|Faible|Un handle vers un objet a été demandé.|  
|4657|567|Faible|Une valeur de Registre a été modifiée.|  
|4658|562|Faible|Le handle à un objet a été fermé.|  
|4659|N/A|Faible|Un handle vers un objet a été demandé avec intention de supprimer.|  
|4660|564|Faible|Un objet a été supprimé.|  
|4661|565|Faible|Un handle vers un objet a été demandé.|  
|4662|566|Faible|Une opération a été effectuée sur un objet.|  
|4663|567|Faible|Une tentative a été effectuée pour accéder à un objet.|  
|4664|N/A|Faible|Une tentative a été effectuée pour créer un lien physique.|  
|4665|N/A|Faible|Une tentative a été effectuée pour créer un contexte d’application client.|  
|4666|N/A|Faible|Une application a tenté une opération :|  
|4667|N/A|Faible|Un contexte client d’application a été supprimé.|  
|4668|N/A|Faible|Une application a été initialisée.|  
|4670|N/A|Faible|Autorisations sur un objet ont été modifiées.|  
|4671|N/A|Faible|Une application a tenté d’accéder à un ordinal bloqué via le service TBS.|  
|4672|576|Faible|Privilèges spéciaux assignés à la nouvelle session.|  
|4673|577|Faible|Un service privilégié a été appelé.|  
|4674|578|Faible|Une opération a été tentée sur un objet avec privilèges.|  
|4688|592|Faible|Un nouveau processus a été créé.|  
|4689|593|Faible|Un processus s’est arrêté.|  
|4690|594|Faible|Une tentative a été effectuée pour dupliquer un handle vers un objet.|  
|4691|595|Faible|Accès indirect à un objet a été demandé.|  
|4694|N/A|Faible|Protection des données protégées pouvant être auditées a été tentée.|  
|4695|N/A|Faible|Unprotection des données protégées pouvant être auditées a été tentée.|  
|4696|600|Faible|Un jeton principal a été affecté à traiter.|  
|4697|601|Faible|Tentez d’installer un service|  
|4698|602|Faible|Une tâche planifiée a été créée.|  
|4699|602|Faible|Une tâche planifiée a été supprimée.|  
|4700|602|Faible|Une tâche planifiée a été activée.|  
|4701|602|Faible|Une tâche planifiée a été désactivée.|  
|4702|602|Faible|Une tâche planifiée a été mis à jour.|  
|4704|608|Faible|Un droit d’utilisateur a été attribué.|  
|4705|609|Faible|Un droit d’utilisateur a été supprimé.|  
|4707|611|Faible|Une relation d’approbation à un domaine a été supprimée.|  
|4709|N/A|Faible|Services IPsec a été démarré.|  
|4710|N/A|Faible|Services IPsec a été désactivé.|  
|4711|N/A|Faible|Peut contenir l’un des éléments suivants : Moteur PAStore a appliqué la copie mise en cache localement de la stratégie IPsec de stockage Active Directory sur l’ordinateur. Moteur PAStore a appliqué la stratégie IPsec de stockage Active Directory sur l’ordinateur. Moteur PAStore a appliqué la stratégie IPsec de stockage du Registre local sur l’ordinateur. Le moteur PAStore Impossible d’appliquer la copie mise en cache localement de la stratégie IPsec de stockage Active Directory sur l’ordinateur. Le moteur PAStore Impossible d’appliquer la stratégie IPsec de stockage Active Directory sur l’ordinateur. Le moteur PAStore Impossible d’appliquer la stratégie IPsec de stockage de Registre local sur l’ordinateur. Le moteur PAStore Impossible d’appliquer certaines règles de la stratégie IPsec active sur l’ordinateur. Le moteur PAStore Échec du chargement de stockage de répertoire stratégie IPsec sur l’ordinateur. Moteur PAStore a chargé le stockage de répertoire stratégie IPsec sur l’ordinateur. Le moteur PAStore Échec du chargement de la stratégie IPsec sur l’ordinateur de stockage local. Moteur PAStore a chargé la stratégie IPsec sur l’ordinateur de stockage local. Le moteur PAStore interrogée concernant les modifications apportées à la stratégie IPsec active et a détecté aucune modification. |  
|4712|N/A|Faible|Services IPsec a rencontré un problème potentiellement sérieux.|  
|4717|621|Faible|Accès au système de sécurité a été accordé à un compte.|  
|4718|622|Faible|Accès au système de sécurité a été supprimé à partir d’un compte.|  
|4720|624|Faible|Un compte d’utilisateur a été créé.|  
|4722|626|Faible|Un compte d’utilisateur a été activé.|  
|4723|627|Faible|Une tentative a été effectuée pour modifier le mot de passe d’un compte.|  
|4725|629|Faible|Un compte d’utilisateur a été désactivé.|  
|4726|630|Faible|Un compte d’utilisateur a été supprimé.|  
|4728|632|Faible|Un membre a été ajouté à un groupe global de sécurité activée.|  
|4729|633|Faible|Un membre a été supprimé d’un groupe global de sécurité activée.|  
|4730|634|Faible|Un groupe global de sécurité activée a été supprimé.|  
|4731|635|Faible|Un groupe local de sécurité activée a été créé.|  
|4732|636|Faible|Un membre a été ajouté à un groupe local de sécurité activée.|  
|4733|637|Faible|Un membre a été supprimé d’un groupe local de sécurité activée.|  
|4734|638|Faible|Un groupe local de sécurité activée a été supprimé.|  
|4738|642|Faible|Un compte d’utilisateur a été modifié.|  
|4740|644|Faible|Un compte d’utilisateur a été verrouillé.|  
|4741|645|Faible|Un compte d’ordinateur a été modifié.|  
|4742|646|Faible|Un compte d’ordinateur a été modifié.|  
|4743|647|Faible|Un compte d’ordinateur a été supprimé.|  
|4744|648|Faible|Un groupe local de sécurité désactivée a été créé.|  
|4745|649|Faible|Un groupe local de sécurité désactivée a été modifié.|  
|4746|650|Faible|Un membre a été ajouté à un groupe local de sécurité est désactivée.|  
|4747|651|Faible|Un membre a été supprimé d’un groupe local de sécurité est désactivée.|  
|4748|652|Faible|Un groupe local de sécurité désactivée a été supprimé.|  
|4749|653|Faible|Un groupe global de sécurité désactivée a été créé.|  
|4750|654|Faible|Un groupe global de sécurité désactivée a été modifié.|  
|4751|655|Faible|Un membre a été ajouté à un groupe global de sécurité est désactivée.|  
|4752|656|Faible|Un membre a été supprimé d’un groupe global de sécurité est désactivée.|  
|4753|657|Faible|Un groupe global de sécurité désactivée a été supprimé.|  
|4756|660|Faible|Un membre a été ajouté à un groupe universel de sécurité activée.|  
|4757|661|Faible|Un membre a été supprimé d’un groupe universel de sécurité activée.|  
|4758|662|Faible|Un groupe universel de sécurité activée a été supprimé.|  
|4759|663|Faible|Un groupe universel de sécurité désactivée a été créé.|  
|4760|664|Faible|Un groupe universel de sécurité désactivée a été modifié.|  
|4761|665|Faible|Un membre a été ajouté à un groupe universel de sécurité est désactivée.|  
|4762|666|Faible|Un membre a été supprimé d’un groupe universel de sécurité est désactivée.|  
|4767|671|Faible|Un compte d’utilisateur est déverrouillé.|  
|4768|672,676|Faible|Un ticket d’authentification Kerberos (TGT) a été demandé.|  
|4769|673|Faible|Un ticket de service Kerberos a été demandé.|  
|4770|674|Faible|Un ticket de service Kerberos a été renouvelé.|  
|4771|675|Faible|Échec de la pré-authentification Kerberos.|  
|4772|672|Faible|Une demande de ticket d’authentification Kerberos a échoué.|  
|4774|678|Faible|Un compte a été mappé pour l’ouverture de session.|  
|4775|679|Faible|Un compte d’ouverture de session n’a pas pu être mappé.|  
|4776|680,681|Faible|Le contrôleur de domaine a tenté de valider les informations d’identification d’un compte.|  
|4777|N/A|Faible|Le contrôleur de domaine a échoué valider les informations d’identification d’un compte.|  
|4778|682|Faible|Une session a été reconnectée à une Station de fenêtre.|  
|4779|683|Faible|Une session a été fermée à partir d’une Station Windows.|  
|4781|685|Faible|Le nom d’un compte a été modifié :|  
|4782|N/A|Faible|Le hachage de mot de passe un compte a accédé.|  
|4783|667|Faible|Un groupe de l’application de base a été créé.|  
|4784|N/A|Faible|Un groupe d’applications de base a été modifié.|  
|4785|689|Faible|Un membre a été ajouté à un groupe d’applications de base.|  
|4786|690|Faible|Un membre a été supprimé d’un groupe d’applications de base.|  
|4787|691|Faible|Un non-membre a été ajouté à un groupe d’applications de base.|  
|4788|692|Faible|Un non-membre a été supprimé d’un groupe d’application de base.|  
|4789|693|Faible|Un groupe d’applications de base a été supprimé.|  
|4790|694|Faible|Un groupe de requêtes LDAP a été créé.|  
|4793|N/A|Faible|L’API de vérification de la stratégie de mot de passe a été appelée.|  
|4800|N/A|Faible|La station de travail a été verrouillée.|  
|4801|N/A|Faible|La station de travail a été déverrouillée.|  
|4802|N/A|Faible|L’économiseur d’écran a été appelé.|  
|4803|N/A|Faible|L’économiseur d’écran a été fermée.|  
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
|4895|799|Faible|Services de certificats a publié le certificat d’autorité de certification aux Services de domaine Active Directory.|  
|4898|802|Faible|Les services de certificats ont chargé une modèle.|  
|4902|N/A|Faible|La table de stratégie d’audit par utilisateur a été créée.|  
|4904|N/A|Faible|Une tentative a été effectuée pour inscrire une source d’événements de sécurité.|  
|4905|N/A|Faible|Une tentative a été effectuée pour annuler l’inscription d’une source d’événements de sécurité.|  
|4909|N/A|Faible|Les paramètres de stratégie locale pour le to ont été modifiés.|  
|4910|N/A|Faible|Les paramètres de stratégie de groupe pour le to ont été modifiés.|  
|4928|N/A|Faible|Un contexte d’appellation de Active Directory réplica source a été établi.|  
|4929|N/A|Faible|Un contexte d’appellation de Active Directory réplica source a été supprimé.|  
|4930|N/A|Faible|Un contexte d’appellation de la source Active Directory réplica a été modifié.|  
|4931|N/A|Faible|Un contexte d’appellation de la destination Active Directory réplica a été modifié.|  
|4932|N/A|Faible|Synchronisation d’un réplica d’un contexte de nommage Active Directory a commencé.|  
|4933|N/A|Faible|Fin de la synchronisation d’un réplica d’un contexte de nommage Active Directory.|  
|4934|N/A|Faible|Attributs d’un objet Active Directory ont été répliquées.|  
|4935|N/A|Faible|Échec de la réplication commence.|  
|4936|N/A|Faible|Échec de la réplication se termine.|  
|4937|N/A|Faible|Un objet en attente a été supprimé d’un réplica.|  
|4944|N/A|Faible|La stratégie suivante a été active lorsque le pare-feu Windows est démarré.|  
|4945|N/A|Faible|Une règle a été répertoriée lorsque le pare-feu Windows est démarré.|  
|4946|N/A|Faible|Une modification a été apportée à la liste des exceptions de pare-feu de Windows. Une règle a été ajoutée.|  
|4947|N/A|Faible|Une modification a été apportée à la liste des exceptions de pare-feu de Windows. Une règle a été modifiée.|  
|4948|N/A|Faible|Une modification a été apportée à la liste des exceptions de pare-feu de Windows. Une règle a été supprimée.|  
|4949|N/A|Faible|Paramètres de pare-feu de Windows ont été restaurés sur les valeurs par défaut.|  
|4950|N/A|Faible|Un paramètre de pare-feu de Windows a changé.|  
|4951|N/A|Faible|Une règle a été ignorée, car son numéro de version principale n’a pas été reconnu par le pare-feu Windows.|  
|4952|N/A|Faible|Parties d’une règle ont été ignorés parce que son numéro de version secondaire n’a pas été reconnu par le pare-feu Windows. Les autres parties de la règle seront appliquées.|  
|4953|N/A|Faible|Une règle a été ignorée par le pare-feu Windows, car il a pas pu analyser la règle.|  
|4954|N/A|Faible|Paramètres de stratégie de groupe du pare-feu Windows ont été modifiés. Les nouveaux paramètres ont été appliqués.|  
|4956|N/A|Faible|Pare-feu de Windows a changé le profil actif.|  
|4957|N/A|Faible|Pare-feu de Windows n’a pas appliqué de la règle suivante :|  
|4958|N/A|Faible|Pare-feu de Windows n’a pas appliqué la règle suivante, car la règle fait référence aux éléments ne pas configurés sur cet ordinateur :|  
|4979|N/A|Faible|Associations de sécurité de Mode principal IPsec et le Mode étendu ont été établies.|  
|4980|N/A|Faible|Associations de sécurité de Mode principal IPsec et le Mode étendu ont été établies.|  
|4981|N/A|Faible|Associations de sécurité de Mode principal IPsec et le Mode étendu ont été établies.|  
|4982|N/A|Faible|Associations de sécurité de Mode principal IPsec et le Mode étendu ont été établies.|  
|4985|N/A|Faible|L’état d’une transaction a changé.|  
|5024|N/A|Faible|Le Service de pare-feu Windows a démarré avec succès.|  
|5025|N/A|Faible|Le Service de pare-feu Windows a été arrêté.|  
|5031|N/A|Faible|Le Service de pare-feu Windows a bloqué une application d’accepter les connexions entrantes sur le réseau.|  
|5032|N/A|Faible|Pare-feu de Windows n’a pas pu notifier l’utilisateur qu’il a bloqué une application d’accepter les connexions entrantes sur le réseau.|  
|5033|N/A|Faible|Le pilote du pare-feu Windows a correctement démarré.|  
|5034|N/A|Faible|Le pilote du pare-feu Windows a été arrêté.|  
|5039|N/A|Faible|Une clé de Registre a été virtualisée.|  
|5040|N/A|Faible|Une modification a été apportée aux paramètres d’IPsec. Un ensemble de l’authentification a été ajouté.|  
|5041|N/A|Faible|Une modification a été apportée aux paramètres d’IPsec. Un ensemble de l’authentification a été modifié.|  
|5042|N/A|Faible|Une modification a été apportée aux paramètres d’IPsec. Un ensemble de l’authentification a été supprimé.|  
|5043|N/A|Faible|Une modification a été apportée aux paramètres d’IPsec. Une règle de sécurité de connexion a été ajoutée.|  
|5044|N/A|Faible|Une modification a été apportée aux paramètres d’IPsec. Une règle de sécurité de connexion a été modifiée.|  
|5045|N/A|Faible|Une modification a été apportée aux paramètres d’IPsec. Une règle de sécurité de connexion a été supprimée.|  
|5046|N/A|Faible|Une modification a été apportée aux paramètres d’IPsec. Un jeu de chiffrement a été ajouté.|  
|5047|N/A|Faible|Une modification a été apportée aux paramètres d’IPsec. Un jeu de chiffrement a été modifié.|  
|5048|N/A|Faible|Une modification a été apportée aux paramètres d’IPsec. Un jeu de chiffrement a été supprimé.|  
|5050|N/A|Faible|Une tentative de désactivation par programmation le pare-feu Windows à l’aide d’un appel à InetFwProfile.FirewallEnabled(False)|  
|5051|N/A|Faible|Un fichier a été virtualisé.|  
|5056|N/A|Faible|Un auto-test chiffrement a été effectué.|  
|5057|N/A|Faible|Échec d’une opération de primitive cryptographique.|  
|5058|N/A|Faible|Opération de fichier de clé.|  
|5059|N/A|Faible|Opération de migration de clé.|  
|5060|N/A|Faible|Échec de l’opération de vérification.|  
|5061|N/A|Faible|Opération de chiffrement.|  
|5062|N/A|Faible|Un noyau cryptographique de test a été effectuée.|  
|5063|N/A|Faible|Une opération de fournisseur de chiffrement a été tentée.|  
|5064|N/A|Faible|Une opération de chiffrement de contexte a été tentée.|  
|5065|N/A|Faible|Une modification de contexte de chiffrement a été tentée.|  
|5066|N/A|Faible|Une opération de la fonction de chiffrement a été tentée.|  
|5067|N/A|Faible|Une modification de la fonction de chiffrement a été tentée.|  
|5068|N/A|Faible|Une opération de fournisseur de fonction de chiffrement a été tentée.|  
|5069|N/A|Faible|Une opération de propriété de fonction de chiffrement a été tentée.|  
|5070|N/A|Faible|Une modification de propriété de fonction de chiffrement a été tentée.|  
|5125|N/A|Faible|Une demande a été envoyée au Service de répondeur OCSP|  
|5126|N/A|Faible|Certificat de signature a été automatiquement mis à jour par le Service de répondeur OCSP|  
|5127|N/A|Faible|Le fournisseur de révocation OCSP mis à jour les informations de révocation|  
|5136|566|Faible|Un objet de service d’annuaire a été modifié.|  
|5137|566|Faible|Un objet de service d’annuaire a été créé.|  
|5138|N/A|Faible|Un objet de service d’annuaire a été annulée.|  
|5139|N/A|Faible|Un objet de service d’annuaire a été déplacé.|  
|5140|N/A|Faible|Un objet de partage de réseau a accédé.|  
|5141|N/A|Faible|Un objet de service d’annuaire a été supprimé.|  
|5152|N/A|Faible|La plateforme de filtrage Windows bloqué un paquet.|  
|5153|N/A|Faible|Un filtre de plateforme de filtrage Windows plus restrictif a bloqué un paquet.|  
|5154|N/A|Faible|La plateforme de filtrage Windows a autorisé une application ou un service à écouter sur un port pour les connexions entrantes.|  
|5155|N/A|Faible|La plateforme de filtrage Windows a bloqué une application ou un service d’écouter sur un port pour les connexions entrantes.|  
|5156|N/A|Faible|La plateforme de filtrage Windows a permis une connexion.|  
|5157|N/A|Faible|La plateforme de filtrage Windows a bloqué une connexion.|  
|5158|N/A|Faible|La plateforme de filtrage Windows a autorisé une liaison à un port local.|  
|5159|N/A|Faible|La plateforme de filtrage Windows a bloqué une liaison à un port local.|  
|5378|N/A|Faible|La délégation des informations d’identification demandées a été interdit par la stratégie.|  
|5440|N/A|Faible|L’appel suivant était présent au démarrage de la plateforme de filtrage Windows moteur de filtrage Base.|  
|5441|N/A|Faible|Le filtre suivant était présent au démarrage de la plateforme de filtrage Windows moteur de filtrage Base.|  
|5442|N/A|Faible|Le fournisseur suivant était présent lors de la plateforme de filtrage Windows moteur de filtrage Base a démarré.|  
|5443|N/A|Faible|Le contexte du fournisseur suivant était présent au démarrage de la plateforme de filtrage Windows moteur de filtrage Base.|  
|5444|N/A|Faible|La sous-couche suivante était présente au démarrage de la plateforme de filtrage Windows moteur de filtrage Base.|  
|5446|N/A|Faible|Une légende de la plateforme de filtrage Windows a été modifiée.|  
|5447|N/A|Faible|Un filtre de la plateforme de filtrage Windows a été modifié.|  
|5448|N/A|Faible|Un fournisseur de plateforme de filtrage Windows a été modifié.|  
|5449|N/A|Faible|Un contexte de fournisseur de plateforme de filtrage Windows a été modifié.|  
|5450|N/A|Faible|Une sous-couche de plateforme de filtrage Windows a été modifiée.|  
|5451|N/A|Faible|Une association de sécurité de Mode rapide IPsec a été établie.|  
|5452|N/A|Faible|Une association de sécurité de Mode rapide IPsec s’est terminée.|  
|5456|N/A|Faible|Moteur PAStore a appliqué la stratégie IPsec de stockage Active Directory sur l’ordinateur.|  
|5457|N/A|Faible|Le moteur PAStore Impossible d’appliquer la stratégie IPsec de stockage Active Directory sur l’ordinateur.|  
|5458|N/A|Faible|Moteur PAStore a appliqué la copie mise en cache localement de la stratégie IPsec de stockage Active Directory sur l’ordinateur.|  
|5459|N/A|Faible|Le moteur PAStore Impossible d’appliquer la copie mise en cache localement de la stratégie IPsec de stockage Active Directory sur l’ordinateur.|  
|5460|N/A|Faible|Moteur PAStore a appliqué la stratégie IPsec de stockage du Registre local sur l’ordinateur.|  
|5461|N/A|Faible|Le moteur PAStore Impossible d’appliquer la stratégie IPsec de stockage de Registre local sur l’ordinateur.|  
|5462|N/A|Faible|Le moteur PAStore Impossible d’appliquer certaines règles de la stratégie IPsec active sur l’ordinateur. Utilisez le composant logiciel enfichable Moniteur de sécurité IP pour diagnostiquer le problème.|  
|5463|N/A|Faible|Le moteur PAStore interrogée concernant les modifications apportées à la stratégie IPsec active et a détecté aucune modification.|  
|5464|N/A|Faible|Le moteur PAStore interrogée concernant les modifications apportées à la stratégie IPsec active a détecté des modifications et les a appliquées aux Services IPsec.|  
|5465|N/A|Faible|Moteur PAStore a reçu un contrôle pour un chargement forcé de la stratégie IPsec et traiter le contrôle avec succès.|  
|5466|N/A|Faible|Le moteur PAStore interrogée concernant les modifications apportées à la stratégie IPsec Active Directory, déterminé que Active Directory ne peut pas être atteint et utilisera la copie mise en cache de la stratégie IPsec Active Directory à la place. Toutes les modifications apportées à la stratégie IPsec Active Directory depuis la dernière interrogation n’a pas pu être appliquée.|  
|5467|N/A|Faible|Le moteur PAStore interrogée concernant les modifications apportées à la stratégie IPsec Active Directory, déterminé que Active Directory peut être atteint et a détecté aucune modification à la stratégie. La copie mise en cache de la stratégie IPsec Active Directory n’est plus en cours utilisée.|  
|5468|N/A|Faible|Le moteur PAStore interrogée concernant les modifications apportées à la stratégie IPsec Active Directory, déterminé que Active Directory peut être atteint, a trouvé les modifications apportées à la stratégie et appliqué ces modifications. La copie mise en cache de la stratégie IPsec Active Directory n’est plus en cours utilisée.|  
|5471|N/A|Faible|Moteur PAStore a chargé la stratégie IPsec sur l’ordinateur de stockage local.|  
|5472|N/A|Faible|Le moteur PAStore Échec du chargement de la stratégie IPsec sur l’ordinateur de stockage local.|  
|5473|N/A|Faible|Moteur PAStore a chargé le stockage de répertoire stratégie IPsec sur l’ordinateur.|  
|5474|N/A|Faible|Le moteur PAStore Échec du chargement de stockage de répertoire stratégie IPsec sur l’ordinateur.|  
|5477|N/A|Faible|Le moteur PAStore Impossible d’ajouter le filtre de mode rapide.|  
|5479|N/A|Faible|Services IPsec a été arrêté avec succès. L’arrêt des Services IPsec peut placer l’ordinateur plus vulnérable aux attaques du réseau ou exposer l’ordinateur à des risques de sécurité potentiels.|  
|5632|N/A|Faible|Une demande a été faite pour s’authentifier sur un réseau sans fil.|  
|5633|N/A|Faible|Une demande a été faite pour s’authentifier auprès d’un réseau câblé.|  
|5712|N/A|Faible|Un appel de procédure distante (RPC) a été tentée.|  
|5888|N/A|Faible|Un objet dans le catalogue COM + a été modifié.|  
|5889|N/A|Faible|Un objet a été supprimé à partir du catalogue COM +.|  
|5890|N/A|Faible|Un objet a été ajouté au catalogue COM +.|  
|6008|N/A|Faible|L’arrêt du système précédent était inattendu|  
|6144|N/A|Faible|Stratégie de sécurité dans les objets de stratégie de groupe a été appliquée avec succès.|  
|6272|N/A|Faible|Serveur de stratégie réseau accordé l’accès à un utilisateur.|  
|N/A|561|Faible|Un handle vers un objet a été demandé.|  
|N/A|563|Faible|Objet ouvert pour suppression|  
|N/A|625|Faible|Type de compte d’utilisateur modifié|  
|N/A|613|Faible|Démarré l’agent de stratégie IPsec|  
|N/A|614|Faible|Agent de stratégie désactivée|  
|N/A|615|Faible|Agent de stratégie IPsec|  
|N/A|616|Faible|Agent de stratégie IPsec a rencontré une défaillance grave|  
|24577|N/A|Faible|Chiffrement de volume démarré|  
|24578|N/A|Faible|Chiffrement du volume arrêté|  
|24579|N/A|Faible|Chiffrement de volume est terminée|  
|24580|N/A|Faible|Déchiffrement du volume démarré|  
|24581|N/A|Faible|Déchiffrement du volume arrêté|  
|24582|N/A|Faible|Déchiffrement du volume est terminée|  
|24583|N/A|Faible|Thread de travail de conversion de volume démarré|  
|24584|N/A|Faible|Momentanément arrêté le thread de travail de conversion de volume|  
|24588|N/A|Faible|L’opération de conversion sur le volume %2 a rencontré une erreur de secteur défectueux. Veuillez valider les données sur ce volume|  
|24595|N/A|Faible|Volume %2 contient des clusters défectueux. Ces clusters seront ignorés pendant la conversion.|  
|24621|N/A|Faible|Vérification de l’état initial : Transaction propagée de conversion du volume sur %2.|  
|5049|N/A|Faible|Une Association de sécurité IPsec a été supprimée.|  
|5478|N/A|Faible|Services IPsec a démarré avec succès.|  
  
> [!NOTE]  
> Reportez-vous à [événements d’audit de sécurité Windows](https://www.microsoft.com/download/details.aspx?id=50034) pour obtenir la liste des nombreux ID d’événement de sécurité et leurs significations.  
>
> Exécutez **wevtutil gp Microsoft-Windows-Security-Auditing /ge /gm:true** pour obtenir une liste très détaillée de la sécurité de tous les ID d’événement  
  
Pour plus d’informations sur les ID d’événement de sécurité Windows et leur signification, consultez l’article du Support de Microsoft [Description des événements de sécurité dans Windows 7 et Windows Server 2008 R2](https://support.microsoft.com/kb/977519). Vous pouvez également télécharger [événements d’Audit de sécurité pour Windows 7 et Windows Server 2008 R2](https://www.microsoft.com/download/details.aspx?id=21561) et [Windows 8 et Windows Server 2012 sécurité détails de l’événement](https://www.microsoft.com/download/details.aspx?id=35753), qui fournissent des événements des informations détaillées sur le systèmes d’exploitation référencés au format de feuille de calcul.  
