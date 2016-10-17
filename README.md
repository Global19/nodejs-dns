# @google-cloud/dns
> Google Cloud DNS Client Library for Node.js

*Looking for more Google APIs than just DNS? You might want to check out [`google-cloud`][google-cloud].*

- [API Documentation][gcloud-dns-docs]
- [Official Documentation][cloud-dns-docs]


```sh
$ npm install --save @google-cloud/dns
```
```js
var dns = require('@google-cloud/dns')({
  projectId: 'grape-spaceship-123',
  keyFilename: '/path/to/keyfile.json'
});

// Create a managed zone.
dns.createZone('my-new-zone', {
  dnsName: 'my-domain.com.'
}, function(err, zone) {});

// Reference an existing zone.
var zone = dns.zone('my-existing-zone');

// Create an NS record.
var nsRecord = zone.record('ns', {
  ttl: 86400,
  name: 'my-domain.com.',
  data: 'ns-cloud1.googledomains.com.'
});

zone.addRecord(nsRecord, function(err, change) {});

// Create a zonefile from the records in your zone.
zone.export('/zonefile.zone', function(err) {});

// Promises are also supported by omitting callbacks.
zone.addRecords(nsRecord).then(function(data) {
  var change = data[0];
});

// It's also possible to integrate with third-party Promise libraries.
var dns = require('@google-cloud/dns')({
  promise: require('bluebird')
});
```


## Authentication

It's incredibly easy to get authenticated and start using Google's APIs. You can set your credentials on a global basis as well as on a per-API basis. See each individual API section below to see how you can auth on a per-API-basis. This is useful if you want to use different accounts for different Google Cloud services.

### On Google Compute Engine

If you are running this client on Google Compute Engine, we handle authentication for you with no configuration. You just need to make sure that when you [set up the GCE instance][gce-how-to], you add the correct scopes for the APIs you want to access.

``` js
// Authenticating on a global basis.
var projectId = process.env.GCLOUD_PROJECT; // E.g. 'grape-spaceship-123'

var dns = require('@google-cloud/dns')({
  projectId: projectId
});

// ...you're good to go!
```

### Elsewhere

If you are not running this client on Google Compute Engine, you need a Google Developers service account. To create a service account:

1. Visit the [Google Developers Console][dev-console].
2. Create a new project or click on an existing project.
3. Navigate to  **APIs & auth** > **APIs section** and turn on the following APIs (you may need to enable billing in order to use these services):
  * Google Cloud DNS API
4. Navigate to **APIs & auth** >  **Credentials** and then:
  * If you want to use a new service account, click on **Create new Client ID** and select **Service account**. After the account is created, you will be prompted to download the JSON key file that the library uses to authenticate your requests.
  * If you want to generate a new key for an existing service account, click on **Generate new JSON key** and download the JSON key file.

``` js
var projectId = process.env.GCLOUD_PROJECT; // E.g. 'grape-spaceship-123'

var dns = require('@google-cloud/dns')({
  projectId: projectId,

  // The path to your key file:
  keyFilename: '/path/to/keyfile.json'

  // Or the contents of the key file:
  credentials: require('./path/to/keyfile.json')
});

// ...you're good to go!
```


[google-cloud]: https://github.com/GoogleCloudPlatform/google-cloud-node
[gce-how-to]: https://cloud.google.com/compute/docs/authentication#using
[dev-console]: https://console.developers.google.com/project
[gcloud-dns-docs]: https://googlecloudplatform.github.io/google-cloud-node/#/docs/dns
[cloud-dns-docs]: https://cloud.google.com/dns/docs
