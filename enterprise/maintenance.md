# Maintenance
## Private Packagist Enterprise

### Updating or Replacing the SSL Certificate
If you would like to upload a new SSL certificate or if you would like to reload an existing SSL certification from the host filesystem please open the Replicated Management Console at port 8800.
Then open the settings menu with the gear icon on the top right and select "Console Settings". Scroll to "TLS Key & Cert" or click on the respective option in the menu. Here you can select a new certificate.
Save the page to apply the new certificate to the Replicated Management Console. If you are using a server path and replaced the SSL certificate without changing the path, save the page to reload the certificate.
Go back to the dashboard on the Replicated Management Console and restart the application for the SSL certificate to be applied to the whole Private Packagist application.

