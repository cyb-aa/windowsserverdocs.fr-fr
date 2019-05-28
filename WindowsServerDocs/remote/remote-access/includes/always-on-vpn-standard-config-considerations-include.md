## <a name="standard-configuration-considerations"></a>Considérations relatives à la configuration standard

VPN Always On dispose de nombreuses options de configuration. Toutefois, vous choisissez votre configuration VPN, cependant, incluez les informations suivantes :

-   **Type de connexion.** Sélection de protocole de connexion est importante et finalement va de pair avec le type d’authentification que vous allez utiliser. Pour plus d’informations sur les protocoles de tunneling disponibles, consultez [types de connexions VPN](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-connection-type/).

-   **Le routage.** Dans ce contexte, les règles de routage déterminent si les utilisateurs peuvent utiliser d’autres itinéraires réseau tout en étant connecté au VPN.

    -   _Le tunneling fractionné_ permet un accès simultané à d’autres réseaux, tels qu’Internet.

    -   _Le tunneling forcé_ nécessite tout le trafic passe exclusivement via le VPN et n’autorise pas l’accès simultané à d’autres réseaux.

-   **Déclenchement.** _Déclenchement_ détermine comment et quand une connexion VPN est initié par (par exemple, lors de l’ouverture d’une application, lorsque l’appareil est sous tension, manuellement par l’utilisateur). Pour déclencher des options, consultez le [options déclenché automatiquement le profil VPN](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-auto-trigger-profile/).

-   **Appareil ou l’authentification utilisateur.** VPN Always On utilise des certificats d’appareil et de connexion initiée par l’appareil via une fonctionnalité appelée [appareil Tunnel](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/vpn-device-tunnel-config). Cette connexion peut être lancée automatiquement et est persistante, qui ressemble à une connexion de tunnel d’infrastructure DirectAccess.

>[!TIP]
>Lorsque vous migrez à partir de DirectAccess pour VPN Always On, envisagez de commencer avec des options de configuration qui sont comparables à ce que vous avez, puis développez à partir de là.

À l’aide de certificats de l’utilisateur, le client VPN Always On se connecte automatiquement, mais il le fait au niveau de l’utilisateur (après la connexion de l’utilisateur) et non au niveau du périphérique (avant la connexion de l’utilisateur). L’expérience est toujours transparente à l’utilisateur, mais il prend en charge les mécanismes d’authentification plus avancées, telles que Windows Hello entreprise.