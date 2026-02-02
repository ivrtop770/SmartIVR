# User Guide - SmartIVR Embedding

## Loading the SDK

Add the following script tag to your HTML:

```html
<script src="https://smart-ivr.co.il/embedding.js"></script>

```

## Basic Usage

### 1. Connecting to a System (Recommended)

This method checks if the system already exists. If it does, it provides a connection link; otherwise, it initiates the embedding process.

```javascript
// Once the script is loaded, SmartIVREmbedding is available on the window object

const systemData = {
  username: '0770000000',
  password: 'password123',
  apiKey: null,
  systemId: '507f1f77bcf86cd799439011', 
  name: 'MyIvr'
};

// Connect to system - checks for existence, returns link if found, otherwise opens embedding
SmartIVREmbedding.connect(systemData)
  .then(result => {
    if (result) {
      if (result.connected) {
        // System already exists - connection link provided
        console.log('✅ System exists!', result.connectionLink);
        // You can open the link automatically
        window.open(result.connectionLink, '_blank');
      } else {
        // This is a new embedding
        console.log('Embedding successful!', result);
        console.log('System ID:', result.systemId);
        console.log('Connection Link:', result.connectionLink);
      }
    } else {
      console.log('User cancelled the embedding');
    }
  })
  .catch(error => {
    console.error('Error:', error);
  });

```

## API Reference

### `SmartIVREmbedding.connect(systemData, options)`

**Recommended Use Case**: Checks if a system exists based on the `systemId`. If it exists, it returns a login link; otherwise, it opens the embedding popup.

**Parameters:**

* `systemData` (Object): System details
* `username` (string): System phone number/identifier.
* `password` (string, optional): Password.
* `apiKey` (string, optional): API Key.
* `systemId` (string, **Required**): The system identifier from your project.
* `name` (string, optional): System name.


* `options` (Object, optional):
* `encryptionKey` (string): Encryption key (default: automatically generated).
* `width` (number): Popup width (default: 550).
* `height` (number): Popup height (default: 700).



**Returns:** `Promise<Object|null>`

* `success` (boolean): Whether the operation was successful.
* `connected` (boolean): **true** if the system already existed, **false** if it was a new embedding.
* `systemIndex` (number): The system index.
* `systemId` (string): The system identifier.
* `connectionLink` (string): Direct login link to the system.
* `system` (Object): Full system details.