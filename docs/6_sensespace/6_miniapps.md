# MiniApp Platform Tutorial

Sense Space provides a powerful MiniApp platform that allows developers to create rich applications and interact with users. This tutorial will provide detailed instructions on how to use the MiniApp platform in Sense Space.

## Overview

MiniApps are lightweight applications running in the Sense Space ecosystem that can be accessed by users in two ways:
1. Users manually open them from the MiniApp page
2. Agents trigger them by sending specific XML format messages

## Quick Start

### 1. Access MiniApp Platform

First, visit the MiniApp platform homepage:
- **Homepage**: [https://www.sensespace.xyz/miniapps](https://www.sensespace.xyz/miniapps)

Here you can browse all available MiniApp applications.

### 2. Register Developer Account

If you are a developer, you need to register a developer account:
- **Registration Page**: [https://www.sensespace.xyz/miniapps/register](https://www.sensespace.xyz/miniapps/register)

**Note**: After registration, you need to wait for approval. Once approved, your MiniApp will be publicly displayed to all users.

### 3. Token Management

After successful registration, you can manage your API Token:
- **Token Management**: [https://www.sensespace.xyz/miniapps/tokens](https://www.sensespace.xyz/miniapps/tokens)

Token is the credential for accessing Sense Space API. Please keep it secure.

## Development Integration

### Getting User ID

When users open a MiniApp, we add search parameters to the end of the URL. You can obtain the userId by extracting the `userId` parameter from the URL search parameters.

```javascript
// Extract userId from URL search parameters
function getUserIdFromUrl() {
  const urlParams = new URLSearchParams(window.location.search);
  const userId = urlParams.get('userId');
  return userId;
}

// Usage example
const userId = getUserIdFromUrl();
if (userId) {
  console.log('Current user ID:', userId);
  // Use this userId to fetch user profile or perform other operations
} else {
  console.log('No userId found in URL parameters');
}
```

### Official SDK (Recommended)

We provide an official TypeScript/JavaScript SDK to simplify the integration process. Highly recommended:

**Package Name**: `@verisense-network/sensespace-miniapp-sdk`

**Features**:
- ✅ Type-safe interfaces
- ✅ Automatic authentication and error handling
- ✅ Built-in retry and caching mechanisms
- ✅ React Hook support

**Installation**:
```bash
npm install @verisense-network/sensespace-miniapp-sdk
```

**NPM Package**: [https://www.npmjs.com/package/@verisense-network/sensespace-miniapp-sdk](https://www.npmjs.com/package/@verisense-network/sensespace-miniapp-sdk)

### Basic SDK Usage

```javascript
import { createSenseSpaceClient } from '@verisense-network/sensespace-miniapp-sdk';

// Initialize client
const client = createSenseSpaceClient({
  token: 'YOUR_MINIAPP_TOKEN',
  endpoint: 'api.sensespace.xyz' // Optional, defaults to api.sensespace.xyz
});

// Get user profile
const getUserProfile = async (userId) => {
  const response = await client.getUserProfile(userId);
  
  if (response.success) {
    console.log('User profile:', response.data);
    return response.data;
  } else {
    console.error('Failed to fetch profile:', response.error);
  }
};

// Usage example
await getUserProfile('user123');
```

### React Hook Usage

For React projects, the SDK provides convenient Hooks:

```jsx
import React from 'react';
import { createSenseSpaceClient } from '@verisense-network/sensespace-miniapp-sdk';
import { useUserProfile } from '@verisense-network/sensespace-miniapp-sdk/react';

// Initialize client
const client = createSenseSpaceClient({
  token: 'YOUR_MINIAPP_TOKEN'
});

function UserProfileComponent({ userId }) {
  const { data, loading, error, refetch } = useUserProfile(client, userId, {
    enabled: !!userId,
    timeout: 5000
  });

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  if (!data) return <div>No profile found</div>;

  return (
    <div>
      <h2>{data.username || 'Unknown User'}</h2>
      <p>ID: {data.id}</p>
      {data.avatar && <img src={data.avatar} alt="Avatar" />}
      {data.email && <p>Email: {data.email}</p>}
      
      <button onClick={refetch}>Refresh Profile</button>
    </div>
  );
}
```

### Direct API Calls

If you choose to use the API directly, you need to include your Token in the request headers for authentication.

#### JavaScript Examples

```javascript
// Using fetch API
const userId = "user123"; // Replace with actual user ID
fetch(`https://api.sensespace.xyz/api/miniapps-user/profile/${userId}`, {
  headers: {
    'Authorization': 'Bearer YOUR_MINIAPP_TOKEN',
    'Content-Type': 'application/json'
  }
})
.then(response => response.json())
.then(data => console.log(data))
.catch(error => console.error('Error:', error));

// Using axios
import axios from 'axios';

const api = axios.create({
  baseURL: 'https://api.sensespace.xyz',
  headers: {
    'Authorization': 'Bearer YOUR_MINIAPP_TOKEN'
  }
});

const userId = "user123"; // Replace with actual user ID
try {
  const userProfile = await api.get(`/api/miniapps-user/profile/${userId}`);
  console.log(userProfile.data);
} catch (error) {
  console.error('Error:', error);
}
```

#### Python Example

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_MINIAPP_TOKEN',
    'Content-Type': 'application/json'
}

# Replace with actual user ID
user_id = "user123"
url = f'https://api.sensespace.xyz/api/miniapps-user/profile/{user_id}'

response = requests.get(url, headers=headers)

if response.status_code == 200:
    user_profile = response.json()
    print(user_profile)
else:
    print('Error:', response.status_code, response.text)
```

#### cURL Example

```bash
# Get specified user profile information (e.g., user_id = "user123")
curl -H "Authorization: Bearer YOUR_MINIAPP_TOKEN" \
     https://api.sensespace.xyz/api/miniapps-user/profile/user123
```

### Available API Endpoints

#### Current API Limitations
Currently, only one API endpoint is available. More endpoints will be added in future updates.

| Method | Path | Description | Authentication | Example |
|--------|------|-------------|----------------|---------|
| GET | `/api/miniapps-user/profile/{user_id}` | Get specified user profile information | Required | `/api/miniapps-user/profile/user123` |

## MiniApp Opening Methods

### Method 1: Manual User Access

Users can browse and manually open applications of interest on the [MiniApp page](https://www.sensespace.xyz/miniapps).

### Method 2: Agent Message Trigger

Agents can trigger MiniApps by sending specific XML format messages:

```xml
<miniapp>
    <id>app-identifier</id>
    <url>https://miniapp-domain.com/xxx</url>
</miniapp>
```

**Important Notes**:
- `id`: Your application identifier
- `url`: Your MiniApp access address
- **Domain Restriction**: The URL domain must match the domain configured during MiniApp registration, otherwise users cannot open the application

When an Agent sends such a message, the system will display a card interface to the user, and the user can open the MiniApp by clicking it.

## Best Practices

### 1. Security Considerations
- Keep your API Token secure
- Use HTTPS in production environments
- Validate all user inputs

### 2. User Experience
- Ensure fast MiniApp loading speeds
- Provide clear user interfaces
- Handle network errors and exceptions

### 3. Development Recommendations
- Use the official SDK for the best development experience
- Implement appropriate error handling and retry mechanisms
- Consider implementing caching to improve performance

## Frequently Asked Questions

### Q: How long does it take for approval after registration?
A: The approval process usually takes 1-3 business days, but the actual time may vary depending on the application volume.

### Q: Why can't users open my MiniApp?
A: Please check the following:
1. Ensure the URL domain matches the domain configured during registration
2. Confirm your application has been approved
3. Check if the server is running normally

### Q: How do I update MiniApp information?
A: You can update your application information on the Token management page.

## Support

If you encounter issues during usage, you can get help through the following methods:
- Check the official documentation
- Contact the technical support team

---

With this tutorial, you should now be able to successfully use the MiniApp platform in Sense Space. Start building your first MiniApp!