# Plugins

> The following issue and solution are available for version `1.0.0` Community Edition.

### How to Handle Errors When Installing Plugins?

**Issue**: When you see an error message: `plugin verification has been enabled, and the plugin you want to install has a bad signature`, how to handle the issue?

**Solution**: Add the following line to the end of your `.env` configuration file: `FORCE_VERIFYING_SIGNATURE=false`

After adding this field, the Dify platform will permit installation of all plugins not listed (and thus not verified) in the Dify Marketplace. 

For security reasons, please install unknown source plugins in a test or sandbox environment first and confirm their safety before deploying to the production environment.


