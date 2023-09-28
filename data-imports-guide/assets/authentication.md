# Authentication

The Asset Import utility uses API Keys to authenticate all API calls into Hornbill instances, and KeySafe to securely store credentials for the asset data source.

## API Keys

For the utility to read, create and update records via the Hornbill API, it requires an [API Key](/esp-fundamentals/security/api-keys) to be securely stored alongside the client.

### User

Every action within Hornbill must be performed in the context of a user account. The user account must possess roles for the platform and applications that you are granting access to via the import utility. The above comment about roles refers to the [Hornbill Security Model](/esp-fundamentals/security/account-types) when associating roles with user accounts. This security  measure prevents you from inflating your session rights, or granting a user more rights than you have yourself.

:::important
We strongly recommend that you create a Service Account in your Hornbill instance, and API Keys against that account which can then be used to perform the required API calls back into Hornbill. 

Please read the [API Key documentation](/esp-fundamentals/security/api-keys) and [best practice guide](/esp-fundamentals/best-practice/platform-api-keys) before creating API keys against your user records.
:::

The service account that you create must be of type `User` (not `Basic`), and be granted the following roles:

- **User Role** - Allows the utility to perform entity actions in the Hornbill platform.
- **Asset Management Usert** - Allows the utility to create and update Asset Management records in Service Manager.

### API Key Rules

The Asset Imports require access to the following Hornbill Platform and application APIs, and your [API Key rules](/esp-fundamentals/security/api-keys#api-key-rules) should reflect those, plus additional security hardening in the form of IP rules:

```cmd
admin:getApplicationList
admin:getGroupList2
data:entityAddRecord
data:entityBrowseRecords2
data:entityDeleteRecord
data:entityUpdateRecord
data:queryExec
session:getApplicationList
system:logMessage
apps/com.hornbill.core:getSitesList
apps/com.hornbill.suppliermanager/SupplierAssets:addSupplierAsset
apps/com.hornbill.suppliermanager/SupplierContractAssets:addSupplierContractAsset
```

## KeySafe

For the import utility to access data from your source database, authentication credentials are required to be stored in KeySafe.

:::note
We recommend that you read the [KeySafe documentation](/esp-fundamentals/security/keysafe) before storing credentials in KeySafe.
:::

Once the relevant key has been created, you can then lock access to it down to the API Key created against your service account. See the [KeySafe documentation](/esp-fundamentals/security/keysafe#access-control-and-usability) for more information regarding this.

### Key Types

As the Asset Import utility supports the import of asset data from many different data sources, it must therefore also support many different KeySafe Key types:

* [Certero](/data-imports-guide/assets/authentication#key-type-certero) - Used for the following data sources:
  * `certero` - Certero IT Asset Management.
* [Database Authentication](/data-imports-guide/assets/authentication#key-type-database-authentication) - Used for the following data sources:
  * `mssql` - Microsoft SQL Server (2005 or above).
  * `mysql` - MySQL 4.1 or above, or any version of MariaDB.
  * `mysql320` - MySQL Server v3.2.0 to v4.0.
  * `odbc` - ODBC driver.
  * `swsql` - Supportworks SQL (Core Services v3.x).
* [Google Workspace](/data-imports-guide/assets/authentication#key-type-google-workspace) - Used for the following data sources:
  * `google` - Google Workspace Enterprise Chrome OS.
* [LDAP Authentication](/data-imports-guide/assets/authentication#key-type-ldap) - Used for the following data sources:
  * `ldap` - LDAP data sources (including Active Directory).
* [Username + Password](/data-imports-guide/assets/authentication#key-type-username-password) - Used for the following data sources:
  * `nexthink` - Nexthink.
* [VMWare Workspace One UEM](/data-imports-guide/assets/authentication#key-type-vmware-workspace-one-uem) - Used for the following data sources:
  * `workspaceone` - vmware Workspace One UEM.

:::tip
The `csv - CSV / Text file(s)` data source reads its data from the file system (local or network), and therefore does not require Keysafe keys.
:::

### Key Type - Certero

Keys of this type require a Certero API Key to be created against a user account that has permissions to fetch assets before the details can be stored in KeySafe, and eventually be used by the Asset Import Utility.

* In Hornbill, navigate to `Configuration` > `Platform Configuration` > `KeySafe`.
* Click `+ Create New Key`.
* Choose a key type of `Certero`.
* Give the KeySafe key a Title.
* Optionally add a Description.
* Populate the following fields on the form:
  * `API Key Name` - The username for the API Key associated with a user account that has permissions to fetch assets.
  * `API Key` -  An API Key associated with a user account that has permissions to fetch assets.
  * `API Endpoint` - The API Endpoint for your Cerero account.
* Click `Create Key`.

### Key Type - Database Authentication

* In Hornbill, navigate to `Configuration` > `Platform Configuration` > `KeySafe`.
* Click `+ Create New Key`.
* Choose a key type of `Database Authentication`.
* Give the KeySafe key a Title.
* Optionally add a Description.
* Populate the following fields on the form:
  * `Server` - The IP address or hostname of your database host.
  * `Port` - The port used to connect to your database.
  * `Database` - The database name/ID.
  * `Username` - The username of the account that should be used to authenticate the connection to your database.
  * `Password` - The password for the above account. 
* Click `Create Key`.

### Key Type - Google Workspace

* In Hornbill, navigate to `Configuration` > `Platform Configuration` > `KeySafe`.
* Click `+ Create New Key`.
* Choose a key type of `Google Workspace`.
* Give the KeySafe key a Title.
* Optionally add a Description.
* Click `Create Key`.
* Once the Key is created, you will need to connect to Google Workspace and your account, in order to authorize the Hornbill App to perform the listed Google Workspace options. Click `Connect` and you will be redirected to Google Workspace in a popup window.
* Log in to your Google Workspace account, and then you will be prompted to review the operations you are authorizing the Hornbill App to be allowed to perform with the chosen Google Workspace account.
* Select the scopes/permissions relevant to the import, and click `Continue`. You will then be returned to your KeySafe key.

### Key Type - LDAP

* In Hornbill, navigate to `Configuration` > `Platform Configuration` > `KeySafe`.
* Click `+ Create New Key`.
* Choose a key type of `LDAP Authentication`.
* Give the KeySafe key a Title.
* Optionally add a Description.
* Populate the following fields on the form:
  * `Host` - The IP address or hostname of your LDAP host.
  * `Port` - The port to access your LDAP through. 389 (LDAP) and 636 (LDAPS) are commonly used values.
  * `Username` - The username of the account that should be used to authenticate the connection to your LDAP.
  * `Password` - The password for the above account. 
* Click `Create Key`.

### Key Type - Username + Password

* In Hornbill, navigate to `Configuration` > `Platform Configuration` > `KeySafe`.
* Click `+ Create New Key`.
* Choose a key type of `Username + Password`.
* Give the KeySafe key a Title.
* Optionally add a Description.
* Populate the following fields on the form:
  * `Username` - The username of the account that should be used to authenticate the connection to your account.
  * `Password` - The password for the above account. 
  * `API Endpoint` - The service API endpoint.
* Click `Create Key`.

### Key Type - VMWare Workspace One UEM

VMware Workspace One UEM requires an oAuth Client to be created before the details can be stored in KeySafe, and eventually be used by the Asset Import Utility. See the [VMware client creation documentation](https://docs.vmware.com/en/VMware-Workspace-ONE-UEM/services/UEM_ConsoleBasics/GUID-UsingUEMFunctionalityWithRESTAPI.html) for more information.

* In Hornbill, navigate to `Configuration` > `Platform Configuration` > `KeySafe`.
* Click `+ Create New Key`.
* Choose a key type of `VMWare Workspace One UEM`.
* Give the KeySafe key a Title.
* Optionally add a Description.
* Populate the following fields on the form:
  * `Domain` - The Domain with your Workspace One UEM domain, for example: `https://cn1498.awmdm.com`.
  * `Region` - The region ID where your VMware Workspace One UEM account is hosted. This can be found in the VMware Workspace One UEM URL, for example: `emea`.
  * `Client ID` - The Client ID of your VMware oAuth Client.
  * `Client Secret` - The Client Secret of your VMware oAuth Client.
* Click `Create Key`.