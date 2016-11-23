<pre>
  Title: QR Authorization spec
  Author: Jeremy Johnson <j-dog@j-dog.net>
  Status: Draft
  Type: Informational
  Created: 2016-11-20
</pre>

This extends the [counterparty url scheme](https://github.com/CounterpartyXCP/cips/blob/master/cip-0002.md) to add support for message signing and a callback

`counterparty:<address>?[message=<message>][action=<action>][icon=<icon_url>][callback=<url>]`

### Additional Query keys ###
* `action`: action to perform [sign, broadcast, bet]
* `callback`: URL to do form POST callback to
* `icon`: URL to an 48x48 icon file (48x48, gif/jpg/png)


## Callback ##
A callback to the specified url will be called once the action is performed, and any data will be passed via a POST request. 

The callback URL must be a valid URL beginning with http: or https:

When passing querystring parameters for a callback url, the URL must be a URL encoded value.

URL:

`https://www.domain.com/folder/script.php?api_key=keyhere&foo=bar`

Encoded URL:

`https%3A%2F%2Fwww.domain.com%2Ffolder%2Fscript.php%3Fapi_key%3Dkeyhere%26foo%3Dbar`

### `sign` Callback Data ###
When the action is `sign` the callback data will contain :

* `address`: address that signed the message
* `message`: message that was signed
* `signature`: signature of `message` signed with `address`

The callback data will also include any querystring parameters specified in the callback url.

## Examples ##
Sign message with address and return data to callback url:

`counterparty:1FwkKA9cqpNRFTpVaokdRjT9Xamvebrwcu?action=sign&message=Authparty+Login+BJybUUzYzVuHaUd&icon=https://domain.com/logo.png&callback=https://domain.com/script.php`

Sign message with ANY address and return data to callback url:

`counterparty:?action=sign&message=Authparty+Login+BJybUUzYzVuHaUd&icon=https://domain.com/logo.png&callback=https://domain.com/script.php`

Broadcast message from address:

`counterparty:1FwkKA9cqpNRFTpVaokdRjT9Xamvebrwcu?action=broadcast&message=AUTHPARTY+VERIFY-ADDRESS+UMIXIJAnvwbZDax`

## UI Suggestions ##
If `action` is specified, verify action with user to ensure action is truly desired.

If no address is provided (counterparty:?), prompt user to select address, or use current/default address.

If action is `sign` and no `callback` is specified, display the sign message dialog to the user.

If icon is provided, display icon to user when confirming signing action.