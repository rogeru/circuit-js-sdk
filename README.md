Circuit JavaScript SDK
======================

[![GitHub release](https://img.shields.io/github/release/qubyte/rubidium.svg)](https://github.com/circuit/circuit-js-sdk)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

> For Node.js see [circuit-nodejs-sdk](https://github.com/circuit/circuit-nodejs-sdk)


### Usage ###
Include the circuit.js file in your app by adding the line below to your HTML file.

```javascript
<script type="text/javascript" src="https://circuitsandbox.net/circuit.js"></script>`
```

Examples are located at [/examples](/examples).

### Prerequisites ###
To use the SDK you need to get free developer account. Get yours today at the [developer portal](https://developers.circuit.com).

### Supported Browsers ###
Chrome and Firefox are officially supported.

### Logon ###
OAuth 2.0 is used for authentication. More info at [circuit.github.com/oauth](https://circuit.github.com/oauth)
```javascript
// Create a client instance and optionally pass config options
var client = new Circuit.Client();

// Create Circuit client instance for OAuth 2.0 (Implicit Grant) on the sandbox system
 var client = new Circuit.Client({
   client_id: '<your client_id>',
   scope: 'ALL',
   domain: 'circuitsandbox.net'
 });

 client.logon()
   .then(user => console.log('Logged on as ' + user.displayName))
   .catch(console.error);
```

### Get Conversations ###
Options allow retrieving a speific number conversations, their starting timestamp and whether to get conversation before or after that timstamp. This allows paging.
```javascript
 client.getConversations({direction: Circuit.Enums.SearchDirection.BEFORE, numberOfConversations: 10})
   .then(conversations => console.log(`Count ${conversations.length}`))
```

### Listen for events ###
```javascript
client.addEventListener('itemAdded', item => console.log('itemAdded event received:', item));
```

### Create an injector ###
Injectors allow extending the conversation, item and user objects within the SDK.
In this example a new attribute `teaser` is created on the item object.
```javascript
Circuit.Injectors.itemInjector = function (item) {
  if (item.type === Circuit.Enums.ConversationItemType.TEXT) {
    // Create item.teaser attribute with replacing br and hr tags with a space
    item.teaser = item.text.content.replace(/<(br[\/]?|\/li|hr[\/]?)>/gi, ' ');
  } else {
    item.teaser = item.type;
  }
};
```

