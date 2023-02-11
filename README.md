# rapt-pill-HA
Idiot's guide to getting the rapt pill to send data to HA using webhooks.


## Requirements:
- RAPT Pill
- Home Assistant install
- Cloud access HA instance (Nebu Casa)


## Method:

### Rapt portal

In the [Rapt portal](https://app.rapt.io/integration/webhooks/list) create a custom webhook

![Custom-webhook](https://user-images.githubusercontent.com/52124037/218245590-95ddaa8b-5d8c-4855-b673-c2a16b0895c6.png)



#### Rapt Portal Webhook Details
![Custom-webhook-details](https://user-images.githubusercontent.com/52124037/218245740-678fa470-907f-4a2f-97cb-2880a24c4985.png)
Name the webhook whatever.


IMPORTANT!! Make sure your HA instance has a web accessible URL i.e. Using a cloud service. This won't work if you are trying to send webhooks to a local instance of Home Assistant (e.g. 192.168.1.1:8123)

Copy you HA URL from your browser and add the the following ```/api/webhook/rapt-hydrometer-pill```

Your URL should look something like the following ```https://this-should-be-a-web-accessible-address-eg-nabucasa/api/webhook/rapt-hydrometer-pill```

Select Method: Post



#### Rapt Portal Webhook Payload
![Custom-webhook-payload](https://user-images.githubusercontent.com/52124037/218245797-ad54f6ba-7a0d-448d-9770-2ba86f123357.png)

Copy the following into the payload section:

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

#### Rapt Portal Webhook Payload
![Custom-webhook-devices](https://user-images.githubusercontent.com/52124037/218245844-1438fd28-303c-491e-9825-1fd24f3f649c.png)

IMPORTANT!!! Click the ✕ symbol to turn it into a ✓ to start the Rapt portal sending data.


Click save.

### Home assistant
In the ```configuration.yaml``` add the following:

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
Restart Home Assistant.

Done.
