# HelloID-Conn-SA-Full-Exchange-Online-UpdateRoomMailbox

| :information_source: Information                                                                                                                                                                                                                                                                                                                                                          |
|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| This repository contains the connector and configuration code only. The implementer is responsible for acquiring the connection details such as username, password, certificate, etc. You might even need to sign a contract or agreement with the supplier before implementing this connector. Please contact the client's application manager to coordinate the connector requirements. |

## Description
_HelloID-Conn-SA-Full-Exchange-Online-UpdateRoomMailbox_ is a template designed for use with HelloID Service Automation (SA) Delegated Forms. It can be imported into HelloID and customized according to your requirements.

This delegated form updates existing room mailboxes in Exchange Online. The delegated form supports the following flow:
1. Search and select an existing room mailbox
2. Update mailbox attributes (display name, resource capacity, email address, alias)
3. Validate display name, email address, and alias uniqueness in Microsoft Entra ID
4. Apply mailbox updates in Exchange Online

## Getting started
### Requirements

- **HelloID On-Premises Service Automation agent**
  For security reasons, the HelloID cloud agent does not support importing PowerShell modules. This form requires the ExchangeOnlineManagement module to be imported, therefore an on-premises agent is required.
- **ExchangeOnlineManagement PowerShell module**:
  The delegated form uses Exchange Online cmdlets to query and update room mailboxes.
- **Microsoft Entra application registration (certificate-based)**:
  The connector authenticates with app-only certificate authentication.
- **Office 365 Exchange Online application permission**:
  Configure and grant admin consent for `Exchange.ManageAsApp`.
- **Microsoft Graph application permissions**:
  Configure and grant admin consent for:
  - `User.Read.All`
  - `Domain.Read.All`

### Connection settings

The following user-defined variables are used by the connector.

| Setting                        | Description                                                                | Mandatory |
|--------------------------------|----------------------------------------------------------------------------|-----------|
| EntraIdTenantId                | Microsoft Entra tenant ID                                                  | Yes       |
| EntraIdOrganization            | Exchange Online organization domain (for example: contoso.onmicrosoft.com) | Yes       |
| EntraIdAppId                   | Application (client) ID of the app registration                            | Yes       |
| EntraIdCertificateBase64String | Base64 encoded certificate (including private key) used for authentication | Yes       |
| EntraIdCertificatePassword     | Password for the certificate                                               | Yes       |

## Remarks

### Proxy Address Update Logic
- The task keeps existing proxy addresses and only replaces the matching address when needed.
- If **Set as primary email address** is selected, the new address is written as `SMTP:` (primary) and existing primary SMTP entries are converted to secondary (`smtp:`).

### Alias Handling
- Alias is optional and can be updated separately from email addresses.
- If supplied, alias is applied through `Set-Mailbox` and validated for uniqueness in Microsoft Entra ID.

### Validation And Selection Data Sources
- Mailbox selection is based on room mailboxes retrieved from Exchange Online.
- Display name, email, and alias validations use Microsoft Graph before task execution.
- Available mail domains are retrieved from Microsoft Entra ID domains.

## Development resources

### API endpoints

The following endpoints are used by the connector.

| Endpoint                                                    | Description                                                        |
|-------------------------------------------------------------|--------------------------------------------------------------------|
| `https://login.microsoftonline.com/{tenantId}/oauth2/token` | Retrieve Microsoft Graph access token using certificate-based auth |
| `https://graph.microsoft.com/v1.0/users?...`                | Validate uniqueness of display name, alias, and email address      |
| `https://graph.microsoft.com/v1.0/domains`                  | Retrieve available mail domains                                    |
| `Connect-ExchangeOnline`                                    | Open app-only Exchange Online session                              |
| `Get-EXOMailbox`                                            | Retrieve room mailboxes for selection                              |
| `Set-Mailbox`                                               | Update room mailbox properties (name, addresses, capacity, alias)  |
| `Disconnect-ExchangeOnline`                                 | Close Exchange Online session                                      |

### API documentation

- https://learn.microsoft.com/graph/api/overview
- https://learn.microsoft.com/graph/api/user-list
- https://learn.microsoft.com/graph/api/domain-list
- https://learn.microsoft.com/powershell/module/exchange/connect-exchangeonline
- https://learn.microsoft.com/powershell/module/exchangepowershell/get-exomailbox
- https://learn.microsoft.com/powershell/module/exchange/set-mailbox

## Getting help
> :bulb: **Tip:**
> _For more information on Delegated Forms, please refer to our [documentation](https://docs.helloid.com/en/service-automation/delegated-forms.html) pages_.

## HelloID docs
The official HelloID documentation can be found at: https://docs.helloid.com/