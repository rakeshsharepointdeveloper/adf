## Simple guide to using Azure Data Factory (ADF) pipeline to read Project Online OData and write to a database.

In this example only the project data is being read and only a subset of columns, using $select in the Url.  
All of the records are written to a staging table.
Currently this is a one-off process with no detection of difference or incremental loads, it is really aimed at getting the authentication working.

The out of the box OData connector for ADF does not support an ROPC (Resource Owner Password Credentials) OAuth2 connection, 
and for Project Online OData this type of grant is required as just an App Id is not supported.  
It needs App Id and an identified user, so in this case ROPC is the only option.

The example hold the username, passowrd and secrets of the App registration in Azure Key Vault.  
I also stored the Client ID and scope there, although this isn't really 'secret'.

A web activity makes the call with the required grant type set in the body.  The returned token is stored in a variable.

The token is used in the ReST activity call to the ReST linked Service and dataset, then mapped to a SQL datbase in Azure with the same limited set of fields.
