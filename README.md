sdk-python
==========

SDK in Python for CALLR API

## Quick start
Install via PyPI

    pip install callr

Or get sources from Github

## Initialize your code

Pip

```python
import callr
```

Source

```python
import sys
sys.path.append("/path/to/callr_folder")
import callr
```

## Exception Management

```python
try:
	# Set your credentials
	api = callr.Api("login", "password")

	# This will raise an exception
	api.call("sms.send", "CALLR")

# Exceptions handler
except (callr.CallrException, callr.CallrLocalException) as e:
	print "ERROR: %s" % e.code
	print "MESSAGE: %s" % e.msg
	print "DATA: ", e.data
```

## Usage
### Sending SMS

#### Without options

```python
result = api.call('sms.send', 'CALLR', '+33123456789', 'Hello world!', None)
```

*Method*
* [sms.send](http://thecallr.com/docs/api/services/sms/#sms.send)

#### Personalized sender

> Your sender must have been authorized and respect the [sms_sender](http://thecallr.com/docs/formats/#sms_sender) format

```python
result = api.call('sms.send', 'Your Brand', '+33123456789', 'Hello world!', None)
```

*Method*
* [sms.send](http://thecallr.com/docs/api/services/sms/#sms.send)

#### If you want to receive replies, do not set a sender - we will automatically use a shortcode

```python
result = api.call('sms.send', '', '+33123456789', 'Hello world!', None)
```

*Method*
- [sms.send](http://thecallr.com/docs/api/services/sms/#sms.send)

#### Force GSM encoding

```python
optionSMS = { 'force_encoding': 'GSM' };

api.call('sms.send', '', '+33123456789', 'Hello world!', optionSMS);
```

*Method*
* [sms.send](http://thecallr.com/docs/api/services/sms/#sms.send)

*Objects*
* [SMS.Options](http://thecallr.com/docs/objects/#SMS.Options)

#### Long SMS (availability depends on carrier)

```python
text = ('Some super mega ultra long text to test message longer than 160 characters ' +
       'Some super mega ultra long text to test message longer than 160 characters ' +
       'Some super mega ultra long text to test message longer than 160 characters')
result = api.call('sms.send', 'CALLR', '+33123456789', text, None)
```

*Method*
* [sms.send](http://thecallr.com/docs/api/services/sms/#sms.send)

#### Specify your SMS nature (alerting or marketing)

```python
optionSMS = { 'nature': 'ALERTING' };

result = api.call('sms.send', 'CALLR', '+33123456789', 'Hello world!', optionSMS)
```

*Method*
* [sms.send](http://thecallr.com/docs/api/services/sms/#sms.send)

*Objects*
* [SMS.Options](http://thecallr.com/docs/objects/#SMS.Options)

#### Custom data

```python
optionSMS = { 'user_data': '42' };

result = api.call('sms.send', 'CALLR', '+33123456789', 'Hello world!', optionSMS)
```

*Method*
* [sms.send](http://thecallr.com/docs/api/services/sms/#sms.send)

*Objects*
* [SMS.Options](http://thecallr.com/docs/objects/#SMS.Options)


#### Delivery Notification - set URL to receive notifications

```python
optionSMS = {
    'push_dlr_enabled': True,
    'push_dlr_url': 'http://yourdomain.com/push_delivery_path',
    # 'push_dlr_url_auth': 'login:password' # needed if you use Basic HTTP Authentication
}

result = api.call('sms.send', 'CALLR', '+33123456789', 'Hello world!', optionSMS)
```

*Method*
* [sms.send](http://thecallr.com/docs/api/services/sms/#sms.send)

*Objects*
* [SMS.Options](http://thecallr.com/docs/objects/#SMS.Options)


### Inbound SMS - set URL to receive inbound messages (MO) and replies

> **Do not set a sender if you want to receive replies** - we will automatically use a shortcode.

```python
optionSMS = {
    'push_mo_enabled': True,
    'push_mo_url': 'http://yourdomain.com/mo_delivery_path',
    # 'push_mo_url_auth': 'login:password' # needed if you use Basic HTTP Authentication
}

result = api.call('sms.send', '', '+33123456789', 'Hello world!', optionSMS)
```

*Method*
* [sms.send](http://thecallr.com/docs/api/services/sms/#sms.send)

*Objects*
* [SMS.Options](http://thecallr.com/docs/objects/#SMS.Options)


### Get an SMS
```python
result = api.call('sms.get', 'SMSHASH')
```

*Method*
* [sms.get](http://thecallr.com/docs/api/services/sms/#sms.get)

*Objects*
* [SMS](http://thecallr.com/docs/objects/#SMS)

### SMS Global Settings

#### Get settings
```python
result = api.call('sms.get_settings')
```

*Method*
* [sms.get_settings](http://thecallr.com/docs/api/services/sms/#sms.get_settings)

*Objects*
* [SMS.settings](http://thecallr.com/docs/objects/#SMS.Settings)


#### Set settings

> Add options that you want to change in the object

```python
settings = {
    'push_dlr_enabled': True,
    'push_dlr_url': 'http://yourdomain.com/push_delivery_path',
    'push_mo_enabled': True,
    'push_mo_url': 'http://yourdomain.com/mo_delivery_path'
}

result = api.call('sms.set_settings', settings)
```

> Returns the updated settings.

*Method*
* [sms.set_settings](http://thecallr.com/docs/api/services/sms/#sms.set_settings)

*Objects*
* [SMS.settings](http://thecallr.com/docs/objects/#SMS.Settings)

********************************************************************************

### REALTIME

#### Create a REALTIME app with a callback URL

```python
options = {
    'url': 'http://yourdomain.com/realtime_callback_url'
}

result = api.call('apps.create', 'REALTIME10', 'Your app name', options)
```

*Method*
* [apps.create](http://thecallr.com/docs/api/services/apps/#apps.create)

*Objects*
* [REALTIME10](http://thecallr.com/docs/objects/#REALTIME10)
* [App](http://thecallr.com/docs/objects/#App)

#### Start a REALTIME outbound call

```python
target = {
    'number': '+33132456789',
    'timeout': 30
}

callOptions = {
    'cdr_field': '42',
    'cli': 'BLOCKED'
}

result = api.call('dialr/call.realtime', 'appHash', target, callOptions)
```

*Method*
* [dialr/call.realtime](http://thecallr.com/docs/api/services/dialr/#call.realtime)

*Objects*
* [Target](http://thecallr.com/docs/objects/#Target)
* [REALTIME10.Call.Options](http://thecallr.com/docs/objects/#REALTIME10.Call.Options)

********************************************************************************

### DIDs

#### List available countries with DID availability
```python
result = api.call('did/areacode.countries')
```

*Method*
* [did/areacode.countries](http://thecallr.com/docs/api/services/did/areacode/#did/areacode.countries)

*Objects*
* [DID.Country](http://thecallr.com/docs/objects/#DID.Country)

#### Get area codes available for a specific country and DID type

```python
result = api.call('did/areacode.get_list', 'US', None)
```

*Method*
* [did/areacode.get_list](http://thecallr.com/docs/api/services/did/areacode/#did/areacode.get_list)

*Objects*
* [DID.AreaCode](http://thecallr.com/docs/objects/#DID.AreaCode)

#### Get DID types available for a specific country
```python
result = api.call('did/areacode.types', 'US')
```

*Method*
* [did/areacode.types](http://thecallr.com/docs/api/services/did/areacode/#did/areacode.types)

*Objects*
* [DID.Type](http://thecallr.com/docs/objects/#DID.Type)

#### Buy a DID (after a reserve)

```python
result = api.call('did/store.buy_order', 'OrderToken')
```

*Method*
* [did/store.buy_order](http://thecallr.com/docs/api/services/did/store/#did/store.buy_order)

*Objects*
* [DID.Store.BuyStatus](http://thecallr.com/docs/objects/#DID.Store.BuyStatus)

#### Cancel your order (after a reserve)

```python
result = api.call('did/store.cancel_order', 'OrderToken')
```

*Method*
* [did/store.cancel_order](http://thecallr.com/docs/api/services/did/store/#did/store.cancel_order)

#### Cancel a DID subscription

```python
result = api.call('did/store.cancel_subscription', 'DID ID')
```

*Method*
* [did/store.cancel_subscription](http://thecallr.com/docs/api/services/did/store/#did/store.cancel_subscription)

#### View your store quota status

```python
result = api.call('did/store.get_quota_status')
```

*Method*
* [did/store.get_quota_status](http://thecallr.com/docs/api/services/did/store/#did/store.get_quota_status)

*Objects*
* [DID.Store.QuotaStatus](http://thecallr.com/docs/objects/#DID.Store.QuotaStatus)

#### Get a quote without reserving a DID

```python
result = api.call('did/store.get_quote', 0, 'GOLD', 1)
```

*Method*
* [did/store.get_quote](http://thecallr.com/docs/api/services/did/store/#did/store.get_quote)

*Objects/
* [DID.Store.Quote](http://thecallr.com/docs/objects/#DID.Store.Quote)

#### Reserve a DID

```python
result = api.call('did/store.reserve', 0, 'GOLD', 1, 'RANDOM')
```

*Method*
* [did/store.reserve](http://thecallr.com/docs/api/services/did/store/#did/store.reserve)

*Objects*
* [DID.Store.Reservation](http://thecallr.com/docs/objects/#DID.Store.Reservation)

#### View your order

```python
result = api.call('did/store.view_order', 'OrderToken')
```

*Method*
* [did/store.buy_order](http://thecallr.com/docs/api/services/did/store/#did/store.view_order)

*Objects*
* [DID.Store.Reservation](http://thecallr.com/docs/objects/#DID.Store.Reservation)

********************************************************************************

### Conferencing

#### Create a conference room

```python
params = { 'open': True }
access = []

result = api.call('conference/10.create_room', 'room name', params, access)
```

*Method*
* [conference/10.create_room](http://thecallr.com/docs/api/services/conference/10/#conference/10.create_room)

*Objects*
* [CONFERENCE10](http://thecallr.com/docs/objects/#CONFERENCE10)
* [CONFERENCE10.Room.Access](http://thecallr.com/docs/objects/#CONFERENCE10.Room.Access)

#### Assign a DID to a room

```python
result = api.call('conference/10.assign_did', 'Room ID', 'DID ID');
```

*Method*
* [conference/10.assign_did](http://thecallr.com/docs/api/services/conference/10/#conference/10.assign_did)

#### Create a PIN protected conference room

```python
params = { 'open': True }
access = [
    { 'pin': '1234', 'level': 'GUEST' },
    { 'pin': '4321', 'level': 'ADMIN', 'phone_number': '+33123456789' }
]

result = api.call('conference/10.create_room', 'room name', params, access)
```

*Method*
* [conference/10.create_room](http://thecallr.com/docs/api/services/conference/10/#conference/10.create_room)

*Objects*
* [CONFERENCE10](http://thecallr.com/docs/objects/#CONFERENCE10)
* [CONFERENCE10.Room.Access](http://thecallr.com/docs/objects/#CONFERENCE10.Room.Access)

#### Call a room access

```python
result = api.call('conference/10.call_room_access', 'Room Access ID', 'BLOCKED', True)
```

*Method*
* [conference/10.call_room_access](http://thecallr.com/docs/api/services/conference/10/#conference/10.call_room_access)

********************************************************************************

### Media

#### List your medias

```python
result = api.call('media/library.get_list', None)
```

*Method*
* [media/library.get_list](http://thecallr.com/docs/api/services/media/library/#media/library.get_list)

#### Create an empty media

```python
result = api.call('media/library.create', 'name')
```

*Method*
* [media/library.create](http://thecallr.com/docs/api/services/media/library/#media/library.create)

#### Upload a media

```python
media_id = 0

result = api.call('media/library.set_content', media_id, 'text', 'base64_audio_data')
```

*Method*
* [media/library.set_content](http://thecallr.com/docs/api/services/media/library/#media/library.set_content)

#### Use Text-to-Speech

```python
media_id = 0

result = api.call('media/tts.set_content', media_id, 'Hello world!', 'TTS-EN-GB_SERENA', None)
```

*Method*
* [media/tts.set_content](http://thecallr.com/docs/api/services/media/tts/#media/tts.set_content)

********************************************************************************

### CDR

#### Get inbound or outbound CDRs
```python
from = 'YYYY-MM-DD HH:MM:SS'
to = 'YYYY-MM-DD HH:MM:SS'

result = api.call('cdr.get', 'OUT', from, to, None, None)
```

*Method*
* [cdr.get](http://thecallr.com/docs/api/services/cdr/#cdr.get)

*Objects*
* [CDR.In](http://thecallr.com/docs/objects/#CDR.In)
* [CDR.Out](http://thecallr.com/docs/objects/#CDR.Out)
