# HostMeNow.org Domain Reseller API Documentation

Welcome to the HostMeNow.org Domain Reseller API. This comprehensive guide will help you integrate domain registration, renewal, transfer, and management into your platform - whether you're using WHMCS, building custom modules for other billing systems like Blesta, or creating automation tools and bots.

## Join Our Reseller Program

To get started as a HostMeNow reseller and receive your API key:

- **Email:** support@hostmenow.org
- **Live Chat:** Visit [HostMeNow.org](https://hostmenow.org) and chat with our support team

---

## Table of Contents

- [Quick Start with WHMCS](#quick-start-with-whmcs)
- [API Integration for Custom Billing Systems](#api-integration-for-custom-billing-systems)
- [Building Custom Modules with AI](#building-custom-modules-with-ai)
- [Authentication](#authentication)
- [API Endpoints](#api-endpoints)
- [Error Handling](#error-handling)
- [Code Examples](#code-examples)

---

## Quick Start with WHMCS

The fastest way to get started is with our pre-built WHMCS module.

### Installation

1. **Download the Module**
   - Download the `domain_reseller_registrar` folder from this repository

2. **Upload to WHMCS**
   ```bash
   # Upload the folder to your WHMCS installation
   /path/to/whmcs/modules/registrars/domain_reseller_registrar/
   ```

3. **Activate the Module**
   - Log in to your WHMCS Admin Panel
   - Navigate to **Setup > Products/Services > Domain Registrars**
   - Find "Domain Reseller for WHMCS" and click **Activate**

4. **Configure API Settings**
   - **API Endpoint:** `https://hostmenow.org/backstage/modules/addons/domain_reseller/api.php`
   - **API Key:** Generate your API key from the Domain Reseller Area at https://hostmenow.org/backstage/
   - **Enable Module Log:** Check this option for debugging (optional)

5. **Import TLD Pricing**
   - Go to **Setup > Products/Services > Domain Pricing**
   - Click **Import Pricing** and select "Domain Reseller for WHMCS"
   - Set your markup and profit margins

6. **Start Selling Domains**
   - Your WHMCS installation is now ready to sell domains!

### Features Included

- ✅ Domain registration, renewal, and transfer
- ✅ Nameserver management
- ✅ DNS record management
- ✅ Contact information updates
- ✅ EPP code retrieval
- ✅ Domain locking/unlocking
- ✅ WHOIS privacy/ID protection
- ✅ Automatic TLD pricing import
- ✅ Domain sync with registry

---

## API Integration for Custom Billing Systems

If you're building for Blesta, HostBill, or another billing system, you can integrate directly with our API.

### API Endpoint

```
https://hostmenow.org/backstage/modules/addons/domain_reseller/api.php
```

### Request Structure

All requests are POST requests with JSON payloads:

```json
{
  "api_key": "your_api_key_here",
  "action": "ActionName",
  "params": {
    "param1": "value1",
    "param2": "value2"
  }
}
```

### Response Structure

**Success:**
```json
{
  "status": "success",
  "data": {
    "result_data": "value"
  }
}
```

**Error:**
```json
{
  "status": "error",
  "message": "Error description"
}
```

### Essential API Actions

| Action | Purpose | Key Parameters |
|--------|---------|----------------|
| `RegisterDomain` | Register a new domain | `domainname`, `regperiod`, `contacts` |
| `RenewDomain` | Renew existing domain | `domainname`, `regperiod` |
| `TransferDomain` | Transfer domain in | `domainname`, `eppcode`, `contacts` |
| `CheckAvailability` | Check if domain is available | `domainname` |
| `GetTldPricing` | Get pricing for all TLDs | `currency` |
| `GetNameservers` | Get current nameservers | `domainname` |
| `SaveNameservers` | Update nameservers | `domainname`, `ns1`, `ns2` |
| `GetContactDetails` | Get domain contacts | `domainname` |
| `SaveContactDetails` | Update domain contacts | `domainname`, `contactdetails` |
| `GetEPPCode` | Retrieve EPP/auth code | `domainname` |
| `GetRegistrarLock` | Check lock status | `domainname` |
| `SaveRegistrarLock` | Lock/unlock domain | `domainname`, `lockstatus` |
| `IDProtectToggle` | Enable/disable privacy | `domainname`, `idprotect` |
| `GetDNS` | Get DNS records | `domainname` |
| `SaveDNS` | Update DNS records | `domainname`, `records` |
| `Sync` | Sync domain status | `domainname`, `sld`, `tld` |

For detailed parameter specifications, see [API Endpoints](#api-endpoints) below.

---

## Building Custom Modules with AI

You can use AI tools like ChatGPT, Claude, or GitHub Copilot to accelerate your module development.

### Sample Prompt for AI

```
I need to build a domain registrar module for [YOUR BILLING SYSTEM] that integrates
with the HostMeNow.org Domain Reseller API.

API Endpoint: https://hostmenow.org/backstage/modules/addons/domain_reseller/api.php

The API uses POST requests with JSON payloads in this format:
{
  "api_key": "string",
  "action": "string",
  "params": {}
}

Key actions I need to implement:
- RegisterDomain
- RenewDomain
- TransferDomain
- CheckAvailability
- GetNameservers / SaveNameservers
- GetContactDetails / SaveContactDetails
- GetEPPCode
- GetRegistrarLock / SaveRegistrarLock

Please create a module that follows [YOUR BILLING SYSTEM]'s module development
guidelines and implements these API calls.
```

### Development Tips

1. **Start with the WHMCS module** - Use `domain_reseller_registrar.php` as a reference implementation
2. **Test incrementally** - Implement and test one action at a time
3. **Handle errors gracefully** - Always check for `status: "error"` in responses
4. **Use module logging** - Log API requests/responses during development
5. **Validate inputs** - Check required parameters before making API calls

### Example Module Skeleton (Generic PHP)

```php
<?php

class DomainResellerModule {
    private $apiEndpoint = 'https://hostmenow.org/backstage/modules/addons/domain_reseller/api.php';
    private $apiKey;

    public function __construct($apiKey) {
        $this->apiKey = $apiKey;
    }

    private function callAPI($action, $params) {
        $ch = curl_init();
        curl_setopt($ch, CURLOPT_URL, $this->apiEndpoint);
        curl_setopt($ch, CURLOPT_POST, 1);
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
        curl_setopt($ch, CURLOPT_HTTPHEADER, ['Content-Type: application/json']);
        curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode([
            'api_key' => $this->apiKey,
            'action' => $action,
            'params' => $params,
        ]));
        curl_setopt($ch, CURLOPT_TIMEOUT, 60);

        $response = curl_exec($ch);
        curl_close($ch);

        $result = json_decode($response, true);

        if ($result['status'] === 'success') {
            return $result['data'];
        } else {
            throw new Exception($result['message'] ?? 'API Error');
        }
    }

    public function registerDomain($domain, $years, $contacts) {
        return $this->callAPI('RegisterDomain', [
            'domainname' => $domain,
            'regperiod' => $years,
            'contacts' => $contacts,
        ]);
    }

    public function checkAvailability($domain) {
        return $this->callAPI('CheckAvailability', [
            'domainname' => $domain,
            'domain' => $domain,
        ]);
    }

    // Add more methods as needed...
}
```

---

## Authentication

All API requests require authentication via an API key.

### Getting Your API Key

1. Log in to HostMeNow.org at https://hostmenow.org/backstage/
2. Navigate to the **Domain Reseller** addon area
3. Click **Generate API Key** or view your existing key
4. Copy the API key - you'll need it for all API requests

### Using the API Key

Include your API key in every request:

```json
{
  "api_key": "your_generated_api_key_here",
  "action": "ActionName",
  "params": {}
}
```

**Security Best Practices:**
- Never expose your API key in client-side code
- Store API keys in environment variables or secure configuration
- Rotate API keys periodically
- Use HTTPS for all API communications (enforced)

---

## API Endpoints

### Domain Registration

**Action:** `RegisterDomain`

Registers a new domain name.

```json
{
  "action": "RegisterDomain",
  "params": {
    "domainid": 123,
    "domainname": "example.com",
    "regperiod": 1,
    "dnsmanagement": true,
    "emailforwarding": false,
    "idprotection": true,
    "contacts": {
      "registrant": {
        "firstname": "John",
        "lastname": "Doe",
        "companyname": "Example Corp",
        "address1": "123 Main St",
        "address2": "Apt 4",
        "city": "New York",
        "state": "NY",
        "postcode": "10001",
        "country": "US",
        "phonenumber": "+1.2125551234",
        "email": "john@example.com"
      },
      "admin": { /* Same structure */ },
      "tech": { /* Same structure */ },
      "billing": { /* Same structure */ }
    }
  }
}
```

---

### Domain Renewal

**Action:** `RenewDomain`

```json
{
  "action": "RenewDomain",
  "params": {
    "domainid": 123,
    "domainname": "example.com",
    "regperiod": 1
  }
}
```

---

### Domain Transfer

**Action:** `TransferDomain`

```json
{
  "action": "TransferDomain",
  "params": {
    "domainid": 123,
    "domainname": "example.com",
    "eppcode": "ABC123XYZ",
    "regperiod": 1,
    "nameservers": ["ns1.example.com", "ns2.example.com"],
    "contacts": { /* Same as RegisterDomain */ }
  }
}
```

---

### Check Domain Availability

**Action:** `CheckAvailability`

```json
{
  "action": "CheckAvailability",
  "params": {
    "domainname": "example.com",
    "domain": "example.com"
  }
}
```

**Response:**
```json
{
  "status": "success",
  "data": {
    "status": "available"
  }
}
```

Possible statuses: `available`, `registered`, `reserved`, `premium`

---

### Get TLD Pricing

**Action:** `GetTldPricing`

```json
{
  "action": "GetTldPricing",
  "params": {
    "currency": "USD"
  }
}
```

**Response:**
```json
{
  "status": "success",
  "data": {
    "currency": {
      "code": "USD",
      "prefix": "$"
    },
    "tlds": {
      ".com": {
        "register": {
          "1yr": 10.99,
          "2yr": 21.98
        },
        "renew": {
          "1yr": 10.99
        },
        "transfer": {
          "1yr": 10.99
        }
      }
    }
  }
}
```

---

### Nameserver Management

**Get Nameservers:** `GetNameservers`
```json
{
  "action": "GetNameservers",
  "params": {
    "domainname": "example.com"
  }
}
```

**Save Nameservers:** `SaveNameservers`
```json
{
  "action": "SaveNameservers",
  "params": {
    "domainname": "example.com",
    "ns1": "ns1.newhost.com",
    "ns2": "ns2.newhost.com",
    "ns3": "",
    "ns4": "",
    "ns5": ""
  }
}
```

**Register Nameserver (Glue Record):** `RegisterNameserver`
```json
{
  "action": "RegisterNameserver",
  "params": {
    "domainname": "example.com",
    "nameserver": "ns1.example.com",
    "ipaddress": "192.0.2.1"
  }
}
```

---

### Contact Management

**Get Contact Details:** `GetContactDetails`
```json
{
  "action": "GetContactDetails",
  "params": {
    "domainname": "example.com"
  }
}
```

**Response:**
```json
{
  "status": "success",
  "data": {
    "Registrant": {
      "First_Name": "John",
      "Last_Name": "Doe",
      "Email": "john@example.com",
      "Address_1": "123 Main St",
      "City": "New York",
      "State": "NY",
      "Zip": "10001",
      "Country": "US",
      "Phone": "+1.2125551234"
    },
    "Admin": { /* Same structure */ },
    "Technical": { /* Same structure */ },
    "Billing": { /* Same structure */ }
  }
}
```

**Save Contact Details:** `SaveContactDetails`
```json
{
  "action": "SaveContactDetails",
  "params": {
    "domainname": "example.com",
    "contactdetails": {
      "Registrant": { /* Contact fields */ },
      "Admin": { /* Contact fields */ },
      "Technical": { /* Contact fields */ },
      "Billing": { /* Contact fields */ }
    }
  }
}
```

---

### EPP Code / Auth Code

**Action:** `GetEPPCode`

```json
{
  "action": "GetEPPCode",
  "params": {
    "domainname": "example.com"
  }
}
```

**Response:**
```json
{
  "status": "success",
  "data": {
    "eppcode": "ABC123XYZ789"
  }
}
```

---

### Domain Lock Management

**Get Lock Status:** `GetRegistrarLock`
```json
{
  "action": "GetRegistrarLock",
  "params": {
    "domainname": "example.com"
  }
}
```

**Save Lock Status:** `SaveRegistrarLock`
```json
{
  "action": "SaveRegistrarLock",
  "params": {
    "domainname": "example.com",
    "lockstatus": true
  }
}
```

---

### WHOIS Privacy / ID Protection

**Action:** `IDProtectToggle`

```json
{
  "action": "IDProtectToggle",
  "params": {
    "domainname": "example.com",
    "idprotect": true
  }
}
```

---

### DNS Management

**Get DNS Records:** `GetDNS`
```json
{
  "action": "GetDNS",
  "params": {
    "domainname": "example.com"
  }
}
```

**Response:**
```json
{
  "status": "success",
  "data": {
    "records": [
      {
        "hostname": "example.com",
        "type": "A",
        "address": "192.0.2.1",
        "priority": null,
        "ttl": 3600
      },
      {
        "hostname": "www",
        "type": "CNAME",
        "address": "example.com",
        "priority": null,
        "ttl": 3600
      }
    ]
  }
}
```

**Save DNS Records:** `SaveDNS`
```json
{
  "action": "SaveDNS",
  "params": {
    "domainname": "example.com",
    "records": [
      {
        "hostname": "example.com",
        "type": "A",
        "address": "192.0.2.1",
        "ttl": 3600
      }
    ]
  }
}
```

---

### Domain Synchronization

**Sync Domain Status:** `Sync`
```json
{
  "action": "Sync",
  "params": {
    "domainname": "example.com",
    "sld": "example",
    "tld": "com"
  }
}
```

**Response:**
```json
{
  "status": "success",
  "data": {
    "active": true,
    "cancelled": false,
    "transferredAway": false,
    "expirydate": "2026-01-25"
  }
}
```

**Transfer Sync:** `TransferSync`
```json
{
  "action": "TransferSync",
  "params": {
    "domainname": "example.com",
    "sld": "example",
    "tld": "com"
  }
}
```

**Response:**
```json
{
  "status": "success",
  "data": {
    "completed": true,
    "failed": false,
    "expirydate": "2027-01-25",
    "reason": ""
  }
}
```

---

### Domain Deletion

**Action:** `RequestDelete`

```json
{
  "action": "RequestDelete",
  "params": {
    "domainname": "example.com"
  }
}
```

---

### Domain Release (UK Domains)

**Action:** `ReleaseDomain`

For transferring .UK domains to another registrar.

```json
{
  "action": "ReleaseDomain",
  "params": {
    "domainname": "example.co.uk",
    "newtag": "NEWREGISTRAR"
  }
}
```

---

### Domain Suggestions

**Action:** `GetDomainSuggestions`

```json
{
  "action": "GetDomainSuggestions",
  "params": {
    "searchTerm": "example",
    "tlds": [".com", ".net", ".org"]
  }
}
```

---

## Error Handling

Always check the `status` field in API responses.

### Error Response Format

```json
{
  "status": "error",
  "message": "Detailed error description"
}
```

### Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| Invalid API key | Wrong or missing API key | Verify your API key in HostMeNow.org backstage |
| Insufficient credit | Not enough balance | Add funds to your reseller account |
| Domain already registered | Domain is taken | Check availability first |
| Invalid domain format | Malformed domain name | Validate domain format before submission |
| Missing parameters | Required fields not provided | Check API documentation for required params |
| Registry error | Upstream registry issue | Contact support with error details |

### Error Handling Best Practices

```php
try {
    $result = callAPI('RegisterDomain', $params);
    // Success - process $result['data']
} catch (Exception $e) {
    // Log the error
    error_log("Domain registration failed: " . $e->getMessage());

    // Show user-friendly message
    return "Unable to register domain. Please try again or contact support.";
}
```

---

## Code Examples

### PHP (WHMCS-style)

```php
<?php

function callDomainAPI($apiKey, $action, $params) {
    $apiEndpoint = 'https://hostmenow.org/backstage/modules/addons/domain_reseller/api.php';

    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, $apiEndpoint);
    curl_setopt($ch, CURLOPT_POST, 1);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_HTTPHEADER, ['Content-Type: application/json']);
    curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode([
        'api_key' => $apiKey,
        'action' => $action,
        'params' => $params,
    ]));
    curl_setopt($ch, CURLOPT_TIMEOUT, 60);

    $response = curl_exec($ch);
    curl_close($ch);

    $result = json_decode($response, true);

    if ($result['status'] === 'success') {
        return $result['data'];
    } else {
        throw new Exception($result['message'] ?? 'Unknown API error');
    }
}

// Example: Check domain availability
$apiKey = 'your_api_key_here';

try {
    $result = callDomainAPI($apiKey, 'CheckAvailability', [
        'domainname' => 'example.com',
        'domain' => 'example.com',
    ]);

    echo "Domain status: " . $result['status'];
} catch (Exception $e) {
    echo "Error: " . $e->getMessage();
}
?>
```

---

### Python

```python
import requests
import json

API_ENDPOINT = 'https://hostmenow.org/backstage/modules/addons/domain_reseller/api.php'
API_KEY = 'your_api_key_here'

def call_domain_api(action, params):
    payload = {
        'api_key': API_KEY,
        'action': action,
        'params': params
    }

    response = requests.post(
        API_ENDPOINT,
        json=payload,
        headers={'Content-Type': 'application/json'},
        timeout=60
    )

    result = response.json()

    if result['status'] == 'success':
        return result['data']
    else:
        raise Exception(result.get('message', 'Unknown API error'))

# Example: Get TLD pricing
try:
    pricing = call_domain_api('GetTldPricing', {'currency': 'USD'})
    print(f"Available TLDs: {list(pricing['tlds'].keys())}")
except Exception as e:
    print(f"Error: {e}")
```

---

### Node.js / JavaScript

```javascript
const axios = require('axios');

const API_ENDPOINT = 'https://hostmenow.org/backstage/modules/addons/domain_reseller/api.php';
const API_KEY = 'your_api_key_here';

async function callDomainAPI(action, params) {
    try {
        const response = await axios.post(API_ENDPOINT, {
            api_key: API_KEY,
            action: action,
            params: params
        }, {
            headers: {'Content-Type': 'application/json'},
            timeout: 60000
        });

        if (response.data.status === 'success') {
            return response.data.data;
        } else {
            throw new Error(response.data.message || 'Unknown API error');
        }
    } catch (error) {
        throw error;
    }
}

// Example: Register a domain
(async () => {
    try {
        const result = await callDomainAPI('RegisterDomain', {
            domainname: 'example.com',
            regperiod: 1,
            contacts: {
                registrant: {
                    firstname: 'John',
                    lastname: 'Doe',
                    email: 'john@example.com',
                    address1: '123 Main St',
                    city: 'New York',
                    state: 'NY',
                    postcode: '10001',
                    country: 'US',
                    phonenumber: '+1.2125551234'
                }
            }
        });
        console.log('Domain registered successfully');
    } catch (error) {
        console.error('Error:', error.message);
    }
})();
```

---

### Telegram Bot (Python)

```python
from telegram import Update
from telegram.ext import Application, CommandHandler, ContextTypes
import requests

API_ENDPOINT = 'https://hostmenow.org/backstage/modules/addons/domain_reseller/api.php'
API_KEY = 'your_api_key_here'

def call_api(action, params):
    response = requests.post(API_ENDPOINT, json={
        'api_key': API_KEY,
        'action': action,
        'params': params
    }, timeout=60)

    result = response.json()
    if result['status'] == 'success':
        return result['data']
    raise Exception(result.get('message', 'API Error'))

async def check_domain(update: Update, context: ContextTypes.DEFAULT_TYPE):
    if not context.args:
        await update.message.reply_text('Usage: /check example.com')
        return

    domain = context.args[0]

    try:
        result = call_api('CheckAvailability', {
            'domainname': domain,
            'domain': domain
        })

        status = result['status']
        await update.message.reply_text(f'✅ {domain} is {status}')
    except Exception as e:
        await update.message.reply_text(f'❌ Error: {str(e)}')

async def get_pricing(update: Update, context: ContextTypes.DEFAULT_TYPE):
    try:
        result = call_api('GetTldPricing', {'currency': 'USD'})
        tlds = list(result['tlds'].keys())[:10]  # First 10 TLDs

        message = "💰 Available TLDs:\n" + "\n".join(tlds)
        await update.message.reply_text(message)
    except Exception as e:
        await update.message.reply_text(f'❌ Error: {str(e)}')

def main():
    app = Application.builder().token("YOUR_TELEGRAM_BOT_TOKEN").build()

    app.add_handler(CommandHandler("check", check_domain))
    app.add_handler(CommandHandler("pricing", get_pricing))

    app.run_polling()

if __name__ == '__main__':
    main()
```

---

### Discord Bot (Python)

```python
import discord
from discord.ext import commands
import requests

API_ENDPOINT = 'https://hostmenow.org/backstage/modules/addons/domain_reseller/api.php'
API_KEY = 'your_api_key_here'

bot = commands.Bot(command_prefix='!')

def call_api(action, params):
    response = requests.post(API_ENDPOINT, json={
        'api_key': API_KEY,
        'action': action,
        'params': params
    })
    result = response.json()
    if result['status'] == 'success':
        return result['data']
    raise Exception(result.get('message', 'API Error'))

@bot.command()
async def checkdomain(ctx, domain: str):
    """Check if a domain is available"""
    try:
        result = call_api('CheckAvailability', {
            'domainname': domain,
            'domain': domain
        })

        status = result['status']
        emoji = "✅" if status == "available" else "❌"
        await ctx.send(f'{emoji} {domain} is **{status}**')
    except Exception as e:
        await ctx.send(f'❌ Error: {str(e)}')

bot.run('YOUR_DISCORD_BOT_TOKEN')
```

---

## Support & Resources

### Getting Help

- **Website:** [HostMeNow.org](https://hostmenow.org)
- **Client Area:** [https://hostmenow.org/backstage/](https://hostmenow.org/backstage/)
- **Support:** Open a ticket through the client area
- **API Status:** Check the client area for API status updates

---

## License

This API and documentation are provided by HostMeNow.org for authorized resellers. Unauthorized use is prohibited.

---

**Last Updated:** January 2026
**API Version:** 1.1
