To check which installed certificate a VPN connection is using on Windows, you can follow these steps:

1. Open the "Network & Internet" Settings:
    
    - Click on the Windows Start button.
    - Click on "Settings" (the gear-shaped icon).
    - Click on "Network & Internet."
2. Access the VPN Settings:
    
    - In the "Network & Internet" settings, click on "VPN" in the left sidebar. This will display your list of configured VPN connections.
3. View Connection Properties:
    
    - Click on the VPN connection you want to check the certificate for.
    - Click on the "Advanced options" link. This will open a new window with advanced settings for the selected VPN connection.
4. Examine the VPN Certificate:
    
    - In the "Advanced options" window, scroll down to the "Server certificate" section.
    - Here, you will see details about the server certificate that the VPN connection is using. You can view information such as the certificate issuer, expiration date, and thumbprint.

This will allow you to see the details of the certificate being used by your VPN connection. Keep in mind that this method shows information about the server's certificate. If the VPN connection uses client certificates for authentication, those are typically managed and stored separately. If you need to check a client certificate, you would need to look in the certificate store on your computer.

To view and manage certificates on your Windows computer, you can use the following steps:

1. Press Win + R on your keyboard to open the Run dialog.
    
2. Type `certmgr.msc` and press Enter. This will open the Windows Certificate Manager.
    
3. In the Certificate Manager, you can navigate through the certificate stores, including "Personal" for user certificates and "Trusted Root Certification Authorities" for trusted root certificates. Check the appropriate store for the certificate you want to inspect.
    
4. You can double-click on a certificate to view its details, including the issuer, expiration date, and thumbprint.
    

By using the Network & Internet settings and the Windows Certificate Manager, you can check both the server certificate used by your VPN connection and any client certificates if applicable.