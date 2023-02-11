# rapt-pill-HA
Idiots guide to getting the rapt pill to work with HA using webhooks


## Requirements:
- RAPT Pill
- Home Assistant install
- Cloud access (Nebu Casa)


## Method:

### Rapt portal

In the [Rapt portal](https://app.rapt.io/integration/webhooks/list) create a custom webhook




Name the webhook whatever.
for the url, make sure this is web accessable. i.e. using a cloud service. this wont work if you are trying to send webhooks to a local instance of Home Assistant
eg https://this-should-be-a-web-accessible-address-eg-nabucasa/api/webhook/rapt-hydrometer-pill
make sure after the web address you inclide:  /api/webhook/rapt-hydrometer-pill





In the payload section, add the follow text
```
{
"device_id": "@device_id",
"device_name": "@device_name",
"temperature": "@temperature",
"gravity": "@gravity",
"battery": "@battery",
"rssi": "@rssi"
}
```


### Home assistant
In the ```configuration.yaml``` add the following

```
template:        
  - trigger:
      - platform: webhook
        webhook_id: rapt-hydrometer-pill
    sensor:
      - name: "RAPT Hydrometer Name"
        unique_id: rapt-pill-name
        state: "{{ trigger.json.device_name }}"
      - name: "RAPT Device ID"
        unique_id: rapt-pill-id
        state: "{{ trigger.json.device_id }}"
      - name: "RAPT Hydrometer Temperature"
        unique_id: rapt-pill-temp
        state: "{{ trigger.json.temperature }}"
        unit_of_measurement: °C
        icon: mdi:water-thermometer
      - name: "RAPT Hydrometer Battery"
        unique_id: rapt-pill-battery
        state: "{{ trigger.json.battery }}"
        unit_of_measurement: "%"
        icon: mdi:battery-bluetooth-variant
      - name: "RAPT Hydrometer rssi"
        unique_id: rapt-pill-rssi
        state: "{{ trigger.json.rssi }}"
      - name: "RAPT Hydrometer Gravity"
        unique_id: rapt-pill-gravity
        state: "{{ trigger.json.gravity }}"
        unit_of_measurement: SG
        icon: mdi:spirit-level
```


