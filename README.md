# HostMeNow Products Reseller API Documentation

## Overview

The **HostMeNow Products Reseller API** allows you to sell HostMeNow products and services directly to your own customers through your billing system. This comprehensive REST API enables you to resell web hosting, cPanel accounts, and other HostMeNow services under your own brand while HostMeNow handles the infrastructure and provisioning.

As a HostMeNow reseller, you can:
- Offer HostMeNow's products to your customers through your own billing platform
- Set your own pricing and profit margins
- Manage customer accounts (create, suspend, terminate, etc.)
- Automate service provisioning and management
- Integrate with any billing system (WHMCS, Blesta, HostBill, or custom platforms)

HostMeNow maintains complete control over the infrastructure, product catalog, and permissions while you focus on growing your business.

### Join Our Reseller Program

To get started as a HostMeNow reseller and receive your API key:

- **Email**: support@hostmenow.org
- **Live Chat**: Visit [HostMeNow.org](https://hostmenow.org) and chat with our support team

Once approved, you'll receive your reseller account credentials and API key to begin integration.

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

### For WHMCS Users

If you're using WHMCS, we provide a ready-to-use server module that handles all API integration automatically.

#### Installation Steps:

1. **Download the Server Module**
   - Download Products Reseller Server Module"
   - Extract the downloaded `products_reseller_server.zip` file

2. **Upload to Your WHMCS**
   - Upload the extracted folder to: `modules/servers/products_reseller_server/`
   - Go to **Setup** → **Products/Services** → **Servers**
   - Click "Add New Server"
   - Configure the server:
     - **Server Name**: Choose any name
     - **Hostname**: hostmenow.org
     - **Username**: backstage
     - **Password**: Your API Key (from your reseller account)
     - **Server Type**: Select "Products Reseller for WHMCS"

3. **Create Products**
   - Go to **Setup** → **Products/Services** → **Products/Services**
   - Create a new product and select "Products Reseller for WHMCS" as the module
   - Configure the product options to map to the provider's products
   - Set your own pricing

---

## API Integration for Custom Billing Systems

### For Blesta, HostBill, or Custom Billing Systems

If you're using a billing system other than WHMCS (like Blesta, HostBill, or your own custom platform), you can integrate directly with our REST API.

### API Endpoint

All API requests should be sent to:

```
https://hostmenow.org/backstage/modules/addons/products_reseller/api.php
```

---

## Building Custom Modules with AI

### Using This Documentation to Build Modules for Other Billing Systems

While we provide a ready-to-use module for WHMCS (in the folder named `products_reseller_server`), you can create modules for other billing systems like Blesta, HostBill, SolidCP, or any custom platform.

#### How to Build a Custom Module

You can use AI assistants like **Claude Code**, ChatGPT, or other AI coding tools to help you build a custom module by providing them with:

1. **This README file** - Contains complete API documentation with all endpoints, parameters, and examples
2. **The WHMCS module** - In the folder named `products_reseller_server` as a reference implementation

#### Example Prompt for AI Assistants:

```
I need to build a [Blesta/HostBill/Custom] module to integrate with the HostMeNow Reseller API.

I'm providing you with:
1. The API documentation (README.md) which shows all available API endpoints
2. The reference WHMCS module implementation

Please create a module for [your billing system] that:
- Connects to the HostMeNow API at https://hostmenow.org/backstage/modules/addons/products_reseller/api.php
- Implements all the core functions: CreateAccount, SuspendAccount, UnsuspendAccount, TerminateAccount, ChangePassword, ChangePackage
- Follows the same API request/response format shown in the documentation
- Handles authentication using the API key
- Includes proper error handling

[Attach or paste the README.md content and relevant files from the WHMCS module]
```

#### What Your Module Should Include:

Your custom module should implement these key functions:

1. **TestConnection** - Verify API connectivity by calling `Get_Products`
2. **CreateAccount** - Provision new services for customers
3. **SuspendAccount** - Suspend services for non-payment or violations
4. **UnsuspendAccount** - Reactivate suspended services
5. **TerminateAccount** - Permanently delete services
6. **ChangePassword** - Update service passwords
7. **ChangePackage** - Upgrade/downgrade services
8. **Get Usage Data** - Retrieve bandwidth and disk usage (for cPanel products)

#### Files to Reference:

The WHMCS module contains these key files that demonstrate the implementation:

- `products_reseller_server.php` - Main module file with all functions
- `pr_server_classes.php` - API communication class
- `hooks.php` - Event hooks for automation

Use these files as a blueprint for your own billing system's module structure.

#### Benefits of AI-Assisted Development:

- **Fast development** - Generate a working module in minutes instead of hours
- **Language flexibility** - Build in any programming language your billing system uses (PHP, Python, Node.js, etc.)
- **Customization** - Easily adapt the module to your specific billing system's architecture
- **Best practices** - AI can help implement error handling, logging, and security features

---

## Authentication

All API requests require authentication using your unique API key.

### API Key

- Obtain your API Key from your reseller account dashboard
- Include it in every API request
- Keep it secure and never share it publicly

### IP Whitelisting (Optional)

If your reseller group has IP restrictions enabled:
- Add your server's IP addresses to your whitelist (maximum 5 IPs)
- Manage IPs from your reseller account settings
- Requests from non-whitelisted IPs will be rejected

---

## API Endpoints

### Request Format

All API requests must be sent as **POST** requests with **JSON** content type.

**Required Headers:**
```
Content-Type: application/json
```

**Request Structure:**
```json
{
  "api_key": "your-api-key-here",
  "action": "ActionName",
  "params": {
    // Action-specific parameters
  }
}
```

### Response Format

All API responses are returned in JSON format:

**Success Response:**
```json
{
  "status": "success",
  "message": "Action completed successfully",
  "data": {
    // Response data specific to the action
  }
}
```

**Error Response:**
```json
{
  "status": "error",
  "message": "Error description here"
}
```

---

### Available API Actions

#### 1. Get Products

Retrieve a list of all products available to your reseller account.

**Action:** `Get_Products`

**Request:**
```json
{
  "api_key": "your-api-key",
  "action": "Get_Products",
  "params": {}
}
```

**Response:**
```json
{
  "status": "success",
  "message": "Products retrieved successfully",
  "data": [
    {
      "product_id": "123",
      "product_name": "Shared Hosting - Basic",
      "description": "Perfect for small websites",
      "billing_cycles": {
        "monthly": {
          "price": "9.99",
          "setup_fee": "0.00"
        },
        "quarterly": {
          "price": "27.99",
          "setup_fee": "0.00"
        },
        "annually": {
          "price": "99.99",
          "setup_fee": "0.00"
        }
      }
    }
  ]
}
```

---

#### 2. Create Account

Create a new service/account for a customer.

**Action:** `CreateAccount`

**Important:** This action will deduct the product price from your reseller account credit balance.

**Request:**
```json
{
  "api_key": "your-api-key",
  "action": "CreateAccount",
  "params": {
    "serviceid": "456",
    "billingcycle": "monthly",
    "qty": "1",
    "domain": "example.com",
    "username": "exampleuser",
    "password": "SecurePassword123!",
    "ip_address": "192.168.1.100",
    "selected_product": "123"
  }
}
```

**Parameters:**
- `serviceid` (required): Your internal service ID from your billing system
- `billingcycle` (required): `monthly`, `quarterly`, `semiannually`, `annually`, `biennially`, `triennially`
- `qty` (optional): Quantity (default: 1)
- `domain` (required): Customer's domain name
- `username` (required): Account username
- `password` (required): Account password
- `ip_address` (optional): Dedicated IP address if applicable
- `selected_product` (required): Product ID from Get_Products response

**Response:**
```json
{
  "status": "success",
  "message": "Account created successfully",
  "data": {
    "serviceid": "456",
    "order_id": "789",
    "invoice_id": "101112"
  }
}
```

**Possible Errors:**
- `Insufficient balance` - Add credit to your reseller account
- `No Product Selected` - Product not assigned to your reseller group
- `Forbidden: Your account does not have permission` - CreateAccount permission not enabled

---

#### 3. Suspend Account

Suspend an active service/account.

**Action:** `SuspendAccount`

**Request:**
```json
{
  "api_key": "your-api-key",
  "action": "SuspendAccount",
  "params": {
    "serviceid": "456",
    "suspendreason": "Non-payment",
    "domain": "example.com",
    "username": "exampleuser",
    "password": "SecurePassword123!",
    "ip_address": "192.168.1.100"
  }
}
```

**Parameters:**
- `serviceid` (required): The service ID to suspend
- `suspendreason` (optional): Reason for suspension
- `domain` (optional): Customer's domain
- `username` (optional): Account username
- `password` (optional): Account password
- `ip_address` (optional): IP address

**Response:**
```json
{
  "status": "success",
  "message": "Account suspended successfully"
}
```

**Possible Errors:**
- `Service not found` - Invalid serviceid
- `Forbidden: Your account does not have permission` - SuspendAccount permission not enabled

---

#### 4. Unsuspend Account

Reactivate a suspended service/account.

**Action:** `UnsuspendAccount`

**Request:**
```json
{
  "api_key": "your-api-key",
  "action": "UnsuspendAccount",
  "params": {
    "serviceid": "456",
    "domain": "example.com",
    "username": "exampleuser",
    "password": "SecurePassword123!",
    "ip_address": "192.168.1.100"
  }
}
```

**Parameters:**
- `serviceid` (required): The service ID to unsuspend
- `domain` (optional): Customer's domain
- `username` (optional): Account username
- `password` (optional): Account password
- `ip_address` (optional): IP address

**Response:**
```json
{
  "status": "success",
  "message": "Account unsuspended successfully"
}
```

---

#### 5. Terminate Account

Permanently delete a service/account.

**Action:** `TerminateAccount`

**Warning:** This action is irreversible.

**Request:**
```json
{
  "api_key": "your-api-key",
  "action": "TerminateAccount",
  "params": {
    "serviceid": "456",
    "domain": "example.com",
    "username": "exampleuser",
    "password": "SecurePassword123!",
    "ip_address": "192.168.1.100"
  }
}
```

**Parameters:**
- `serviceid` (required): The service ID to terminate
- `domain` (optional): Customer's domain
- `username` (optional): Account username
- `password` (optional): Account password
- `ip_address` (optional): IP address

**Response:**
```json
{
  "status": "success",
  "message": "Account terminated successfully"
}
```

---

#### 6. Change Password

Update the password for a service/account.

**Action:** `ChangePassword`

**Request:**
```json
{
  "api_key": "your-api-key",
  "action": "ChangePassword",
  "params": {
    "serviceid": "456",
    "password": "NewSecurePassword456!",
    "domain": "example.com",
    "username": "exampleuser",
    "ip_address": "192.168.1.100"
  }
}
```

**Parameters:**
- `serviceid` (required): The service ID
- `password` (required): New password
- `domain` (optional): Customer's domain
- `username` (optional): Account username
- `ip_address` (optional): IP address

**Response:**
```json
{
  "status": "success",
  "message": "Password changed successfully"
}
```

---

#### 7. Change Package

Upgrade or downgrade a service to a different product/package.

**Action:** `ChangePackage`

**Request:**
```json
{
  "api_key": "your-api-key",
  "action": "ChangePackage",
  "params": {
    "serviceid": "456",
    "domain": "example.com",
    "username": "exampleuser",
    "password": "SecurePassword123!",
    "ip_address": "192.168.1.100"
  }
}
```

**Parameters:**
- `serviceid` (required): The service ID
- `domain` (optional): Customer's domain
- `username` (optional): Account username
- `password` (optional): Account password
- `ip_address` (optional): IP address

**Response:**
```json
{
  "status": "success",
  "message": "Package changed successfully"
}
```

---

#### 8. Get Bandwidth and Disk Usage

Retrieve current resource usage statistics for a service (cPanel products only).

**Action:** `get_Bandwidth_Disk_Usage`

**Request:**
```json
{
  "api_key": "your-api-key",
  "action": "get_Bandwidth_Disk_Usage",
  "params": {
    "serviceid": "456"
  }
}
```

**Parameters:**
- `serviceid` (required): The service ID

**Response:**
```json
{
  "status": "success",
  "message": "Usage retrieved successfully",
  "disk_usage": {
    "used_mb": "1250.50",
    "limit_mb": "5000",
    "percentage_used": "25.01"
  },
  "bandwidth_usage": {
    "current_month_mb": "3500.75",
    "limit_mb": "10000",
    "percentage_used": "35.01"
  },
  "last_updated": "2025-01-03 14:30:00"
}
```

**Note:** For unlimited resources, `limit_mb` will be `0`.

---

#### 9. Get Server Name

Retrieve the server type/name for a service.

**Action:** `GetServerName`

**Request:**
```json
{
  "api_key": "your-api-key",
  "action": "GetServerName",
  "params": {
    "serviceid": "456"
  }
}
```

**Response:**
```json
{
  "status": "success",
  "server_name": "cPanel"
}
```

---

#### 10. Create SSO Session (cPanel Only)

Generate a single sign-on URL for cPanel access.

**Action:** `CreateSSOSession`

**Request:**
```json
{
  "api_key": "your-api-key",
  "action": "CreateSSOSession",
  "params": {
    "serviceid": "456",
    "app": "Email_Accounts"
  }
}
```

**Parameters:**
- `serviceid` (required): The service ID
- `app` (optional): cPanel app to open directly (e.g., `Email_Accounts`, `FileManager_Home`, `Database_MySQL`)

**Response:**
```json
{
  "status": "success",
  "url": "https://cpanel.example.com:2083/login/?session=abc123xyz..."
}
```

---

## Error Handling

### Common Error Messages

| Error Message | Cause | Solution |
|--------------|-------|----------|
| `Invalid API Key` | API key is incorrect or disabled | Verify API key from reseller dashboard |
| `Your IP address is not whitelisted` | Request from unauthorized IP | Add your server IP to whitelist |
| `Forbidden: Your account does not have permission` | API permission not enabled | Contact provider to enable permission in your group |
| `No Product Selected` | Product not available to your group | Verify product ID and group assignment |
| `Insufficient balance` | Not enough credit in reseller account | Add funds to your reseller account |
| `Service not found` | Invalid serviceid | Check the serviceid value |

### HTTP Status Codes

- `200 OK` - Request successful (check `status` field in response)
- `400 Bad Request` - Malformed JSON or missing required fields
- `403 Forbidden` - Authentication failed or permission denied
- `500 Internal Server Error` - Server-side error

---

## Code Examples

### PHP Example (using cURL)

```php
<?php

class ProductResellerAPI {

    private $apiEndpoint;
    private $apiKey;

    public function __construct($hostname, $apiKey) {
        $this->apiEndpoint = "https://{$hostname}/modules/addons/products_reseller/api.php";
        $this->apiKey = $apiKey;
    }

    public function sendRequest($action, $params = []) {
        $ch = curl_init();

        curl_setopt($ch, CURLOPT_URL, $this->apiEndpoint);
        curl_setopt($ch, CURLOPT_POST, 1);
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
        curl_setopt($ch, CURLOPT_HTTPHEADER, [
            'Content-Type: application/json',
        ]);
        curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode([
            'api_key' => $this->apiKey,
            'action' => $action,
            'params' => $params,
        ]));
        curl_setopt($ch, CURLOPT_TIMEOUT, 60);
        curl_setopt($ch, CURLOPT_FOLLOWLOCATION, true);

        $response = curl_exec($ch);
        $httpCode = curl_getinfo($ch, CURLINFO_HTTP_CODE);
        curl_close($ch);

        return json_decode($response, true);
    }

    public function getProducts() {
        return $this->sendRequest('Get_Products');
    }

    public function createAccount($params) {
        return $this->sendRequest('CreateAccount', $params);
    }

    public function suspendAccount($serviceId) {
        return $this->sendRequest('SuspendAccount', [
            'serviceid' => $serviceId
        ]);
    }

    public function unsuspendAccount($serviceId) {
        return $this->sendRequest('UnsuspendAccount', [
            'serviceid' => $serviceId
        ]);
    }

    public function terminateAccount($serviceId) {
        return $this->sendRequest('TerminateAccount', [
            'serviceid' => $serviceId
        ]);
    }

    public function changePassword($serviceId, $newPassword) {
        return $this->sendRequest('ChangePassword', [
            'serviceid' => $serviceId,
            'password' => $newPassword
        ]);
    }
}

// Usage Example
$api = new ProductResellerAPI('hostmenow.org/backstage', 'your-api-key-here');

// Get all available products
$products = $api->getProducts();
print_r($products);

// Create a new account
$result = $api->createAccount([
    'serviceid' => '123',
    'billingcycle' => 'monthly',
    'qty' => '1',
    'domain' => 'customer.com',
    'username' => 'customer123',
    'password' => 'SecurePass123!',
    'selected_product' => '456'
]);

if ($result['status'] === 'success') {
    echo "Account created successfully!\n";
    echo "Order ID: " . $result['data']['order_id'] . "\n";
} else {
    echo "Error: " . $result['message'] . "\n";
}

?>
```

---

### Python Example

```python
import requests
import json

class ProductResellerAPI:

    def __init__(self, hostname, api_key):
        self.api_endpoint = f"https://{hostname}/modules/addons/products_reseller/api.php"
        self.api_key = api_key

    def send_request(self, action, params=None):
        if params is None:
            params = {}

        payload = {
            'api_key': self.api_key,
            'action': action,
            'params': params
        }

        headers = {
            'Content-Type': 'application/json'
        }

        try:
            response = requests.post(
                self.api_endpoint,
                json=payload,
                headers=headers,
                timeout=60
            )
            return response.json()
        except Exception as e:
            return {
                'status': 'error',
                'message': str(e)
            }

    def get_products(self):
        return self.send_request('Get_Products')

    def create_account(self, params):
        return self.send_request('CreateAccount', params)

    def suspend_account(self, service_id):
        return self.send_request('SuspendAccount', {
            'serviceid': service_id
        })

    def unsuspend_account(self, service_id):
        return self.send_request('UnsuspendAccount', {
            'serviceid': service_id
        })

    def terminate_account(self, service_id):
        return self.send_request('TerminateAccount', {
            'serviceid': service_id
        })

    def change_password(self, service_id, new_password):
        return self.send_request('ChangePassword', {
            'serviceid': service_id,
            'password': new_password
        })

# Usage Example
api = ProductResellerAPI('hostmenow.org/backstage', 'your-api-key-here')

# Get all available products
products = api.get_products()
print(json.dumps(products, indent=2))

# Create a new account
result = api.create_account({
    'serviceid': '123',
    'billingcycle': 'monthly',
    'qty': '1',
    'domain': 'customer.com',
    'username': 'customer123',
    'password': 'SecurePass123!',
    'selected_product': '456'
})

if result['status'] == 'success':
    print("Account created successfully!")
    print(f"Order ID: {result['data']['order_id']}")
else:
    print(f"Error: {result['message']}")
```

---

### Node.js Example

```javascript
const axios = require('axios');

class ProductResellerAPI {
    constructor(hostname, apiKey) {
        this.apiEndpoint = `https://${hostname}/modules/addons/products_reseller/api.php`;
        this.apiKey = apiKey;
    }

    async sendRequest(action, params = {}) {
        try {
            const response = await axios.post(this.apiEndpoint, {
                api_key: this.apiKey,
                action: action,
                params: params
            }, {
                headers: {
                    'Content-Type': 'application/json'
                },
                timeout: 60000
            });

            return response.data;
        } catch (error) {
            return {
                status: 'error',
                message: error.message
            };
        }
    }

    async getProducts() {
        return await this.sendRequest('Get_Products');
    }

    async createAccount(params) {
        return await this.sendRequest('CreateAccount', params);
    }

    async suspendAccount(serviceId) {
        return await this.sendRequest('SuspendAccount', {
            serviceid: serviceId
        });
    }

    async unsuspendAccount(serviceId) {
        return await this.sendRequest('UnsuspendAccount', {
            serviceid: serviceId
        });
    }

    async terminateAccount(serviceId) {
        return await this.sendRequest('TerminateAccount', {
            serviceid: serviceId
        });
    }

    async changePassword(serviceId, newPassword) {
        return await this.sendRequest('ChangePassword', {
            serviceid: serviceId,
            password: newPassword
        });
    }
}

// Usage Example
(async () => {
    const api = new ProductResellerAPI('hostmenow.org/backstage', 'your-api-key-here');

    // Get all available products
    const products = await api.getProducts();
    console.log(JSON.stringify(products, null, 2));

    // Create a new account
    const result = await api.createAccount({
        serviceid: '123',
        billingcycle: 'monthly',
        qty: '1',
        domain: 'customer.com',
        username: 'customer123',
        password: 'SecurePass123!',
        selected_product: '456'
    });

    if (result.status === 'success') {
        console.log('Account created successfully!');
        console.log(`Order ID: ${result.data.order_id}`);
    } else {
        console.log(`Error: ${result.message}`);
    }
})();
```

---

## Security Best Practices

1. **Use HTTPS Only**
   - All API calls must use HTTPS to encrypt data in transit
   - Never send API keys over unencrypted HTTP connections

2. **Implement IP Whitelisting**
   - Enable IP restrictions in your reseller group settings
   - Only allow requests from your known server IPs

3. **Validate Responses**
   - Always check the `status` field in API responses
   - Handle errors gracefully in your application
   - Log failed requests for debugging

4. **Rate Limiting**
   - Implement request throttling in your application
   - Avoid excessive API calls that could trigger rate limits

---

## License

This module and API are proprietary software. Usage is governed by your reseller agreement with the provider.
