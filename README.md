# HostMeNow.org WHMCS Domain Reseller Module

This WHMCS module enables seamless integration of the HostMeNow.org Domain API, allowing resellers to automate the process of domain registration, transfer, and renewal directly from their WHMCS installations. A HostMeNow.org Domain Reseller account is required to use the API functionality.

After creating an account, users can obtain API access by registering for a Domain Reseller account on HostMeNow.org.

## Overview
The HostMeNow Domain Reseller module integrates with WHMCS to allow domain resellers full control over domain management tasks. Resellers can manage their domains, automate domain actions, and provide their customers with a streamlined domain management experience within the WHMCS Client Area.

By using this module, you can manage domain registrations, transfers, renewals, and other domain-related functions from a remote WHMCS installation. 

## Requirements
To use this module, make sure your system meets the following requirements:
- WHMCS (v5.3 or higher)
- HostMeNow.org API key (available after registering for a Domain Reseller account)

## How to Install
Follow these steps to install the HostMeNow.org Domain Reseller module:
1. Download the module files from your HostMeNow.org reseller dashboard.
2. Upload the files to your WHMCS directory (usually located at `/home/user/whmcs`).
3. In WHMCS Admin, go to **Setup** -> **Products/Services** -> **Domain Registrars** and activate the `HostMeNow` registrar module.
4. Enter your API credentials (API username and secret), which can be found in your HostMeNow.org dashboard.
5. Ensure your account is funded. The module uses your HostMeNow.org account balance for domain transactions. Payment methods include PayPal, credit cards, and several cryptocurrencies, with a $20 minimum funding requirement.

## Available API Features
With the HostMeNow Domain Reseller API, you have access to the following domain management features:
- Modify DNS records (A, AAAA, CNAME, MX, TXT)
- Email forwarding management
- Private domain registration
- Contact information updates
- Enable/disable Registrar Lock
- Retrieve domain authorization (EPP) codes
- Domain forwarding
- Register, transfer, and renew domain names
- Manage and delete domains
- Obtain and update nameservers
- ID protection and privacy control
- Domain availability checks and domain suggestions
- Access to a list of available TLDs
- Check domain synchronization status
- Retrieve account balance and API version

For detailed API documentation, visit the [HostMeNow.org API Guide](https://hostmenow.org/domain-reseller.html).

## Getting Help
If you need assistance with the module or have any inquiries, feel free to reach out to our support team by submitting a ticket through our [Support Portal](https://hostmenow.org/client_area/submitticket.php?deptid=10).

## Reporting Issues
Encountered a bug or issue? Let us know by opening a support ticket via our [Helpdesk](https://hostmenow.org/backstage/submitticket.php).

## License
This module is open-source and available under the [MIT License](http://opensource.org/licenses/MIT).
