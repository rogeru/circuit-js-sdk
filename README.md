Circuit JavaScript SDK
======================

[![GitHub release](https://img.shields.io/github/release/circuit/circuit-js-sdk.svg)](https://github.com/circuit/circuit-js-sdk)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

> For Node.js SDK see [circuit-nodejs-sdk](https://github.com/circuit/circuit-nodejs-sdk)



### Prerequisites
* Developer account on circuitsandbox.net. Get it for free at [Developer registration](https://www.circuit.com/web/developers/registration).
* Register your OAuth 2.0 app at [circuit.github.com/oauth](https://circuit.github.com/oauth) to get a `client_id`.

### API Reference
https://circuitsandbox.net/sdk/ with most APIs described at in the [Client](https://circuitsandbox.net/sdk/classes/Client.html) class.


### Usage
Include circuit.min.js in your app by adding the line below to your HTML file.

```html
<script type="text/javascript" src="https://circuitsandbox.net/circuit.min.js"></script>`
```

### Examples
Examples are located at [/examples](/examples). Try them live [here](https://rawgit.com/circuit/js-sdk/master/index.html).

Here are some snippets to get an idea:

#### Logon
OAuth 2.0 is used for authentication.
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

#### Get Conversations
Options allow retrieving a speific number conversations, their starting timestamp and whether to get conversation before or after that timstamp. This allows paging.
```javascript
client.getConversations({numberOfConversations: 10})
  .then(conversations => console.log(`Retrieved ${conversations.length} conversations`))
```

#### Call Conversations
Start a video call with another user. Create conversation is not yet existing.
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

### Supported Browsers
Chrome and Firefox are officially supported.

### Help us improve the SDK
Help us improve the SDK or examples by sending us a pull-request or opening a [GitHub Issue](https://github.com/circuit/circuit-js-sdk/issues/new).
