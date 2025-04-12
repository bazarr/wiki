If you need to reinstall using the Windows Installer (it could be to upgrade embedded Python version), please follow these steps:

1. If you've changed the user that's used to start Bazarr service, make sure to note it from the Bazarr service in `services.msc` console.
1. Once that's done, uninstall Bazarr (it won't delete your settings).
1. Follow instructions here to install Bazarr using the latest Windows Installer: [[Instructions]](/Getting-Started/Installation/Windows/installer/)
1. Once completed, make sure to reconfigure the Bazarr service to start under the previously noted user then restart the service.

Enjoy Bazarr again!
