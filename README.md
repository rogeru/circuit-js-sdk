Circuit JavaScript SDK
======================

[![GitHub release](https://img.shields.io/github/release/circuit/circuit-js-sdk.svg)](https://github.com/circuit/circuit-js-sdk)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

> For Node.js SDK see [circuit-node-sdk](https://github.com/circuit/circuit-node-sdk)



### Prerequisites
* Developer account on circuitsandbox.net. Get it for free at [developer registration](https://www.circuit.com/web/developers/registration).
* OAuth 2.0 `client_id`. Get if for free at [circuit.github.com/oauth](https://circuit.github.com/oauth).

### API Reference
https://circuitsandbox.net/sdk/ with most APIs described in the [Client](https://circuitsandbox.net/sdk/classes/Client.html) class.


### Usage
Include circuit.js in your app by adding the line below to your HTML file.
> It is recommended to use circuit.js from circuitsandbox.net to stay current with the latest version and guarantee compatibility with the Circuit backend.

```html
<script type="text/javascript" src="https://circuitsandbox.net/circuit.js"></script>`
```

### Examples
Examples are located at [/examples](/examples). Here are some snippets to get an idea:

#### Logon (OAuth 2.0)
```javascript
// Create Circuit client instance for OAuth 2.0 (Implicit Grant) on the sandbox system, asking for all permission scopes
let client = new Circuit.Client({
  client_id: '<your client_id>',
  scope: 'ALL',
  domain: 'circuitsandbox.net'
});

client.logon()
  .then(user => console.log('Logged on as ' + user.displayName))
  .catch(console.error);
```

#### Get Conversations
Get 10 newest conversations of logged on user.
```javascript
client.getConversations({numberOfConversations: 10})
  .then(conversations => console.log(`Retrieved ${conversations.length} conversations`))
```

#### Video call with another Circuit user
Start a video call with another user. Create conversation if not yet existing.
```javascript
client.makeCall('bob@company.com', {audio: true, video: true}, true)
  .then(call => console.log('New call: ', call));
```

#### Listen for events
```javascript
client.addEventListener('itemAdded', item => console.log('itemAdded event received:', item));
```

#### Create an injector
Injectors allow extending the conversation, item and user objects within the SDK.
In this example a new attribute `teaser` is created on the item object. This `teaser` will be added to item objects in any reponse or event from the SDK.
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

### Supported Browsers
Chrome and Firefox are officially supported.

### Help us improve the SDK
Help us improve the SDK or examples by sending us a pull-request or opening a [GitHub Issue](https://github.com/circuit/circuit-js-sdk/issues/new).
