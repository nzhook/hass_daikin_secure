
# Notice
The BRP072C42 WIFI controller now works in the [Daikin integration](https://www.home-assistant.io/integrations/daikin/) built into Home Assistant and includes additional features such config flow and energy monitoring which were not available when this integration was copied. 
--

## hass_daikin_secure
A copy of the Home Assistant Daikin component that works with a Daikin BRP072C42 WIFI controller (over https)

Original component code:
https://github.com/home-assistant/home-assistant/tree/dev/homeassistant/components/daikin

### Usage

1. Download the custom_component/daikin directory and place it into your custom_component folder - note: this will override the existing
component.

2. Grab the 13-digit *KEY* from the sticker on the controller (Lists SSID, KEY, MAC and S.N). Will look like: 0103056789012

3. Register the hardcoded UUID (or change self.uuid in appliance.py) with your controller using that key as the pass phrase
```
curl --insecure -H "X-Daikin-uuid: d53b108a0e5e4fb5ab94a343b7d4b74a" --head "https://[IP]/common/register_terminal?key=[KEY]"
```

eg.
```
curl --insecure -H "X-Daikin-uuid: d53b108a0e5e4fb5ab94a343b7d4b74a" --head "https://192.168.1.20/common/register_terminal?key=0103056789012"
```

You should see output similar to this:
```
HTTP/1.0 200 OK
Content-Length: 6
Content-Type: text/plain
```

(if you have the wrong KEY you will get 403 HTTP_FORBIDDEN instead)


4. Make sure you get output when running this (this will confirm Home Assistant will be registered with the controller):
``
curl --insecure -H "X-Daikin-uuid: d53b108a0e5e4fb5ab94a343b7d4b74a" --head "https://[IP]/common/basic_info"
``
(if you get no output the registration did not work)

5. Restart home-assistant and the climate devices should now show up (See Configuration -> Integrations), if not you may need to
  add:
```
daikin:
```
to the configuration.yaml

Refer to the Home-Assistant Daikin component docs for detail on how to use the component
