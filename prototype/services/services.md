# Services

## Development

Connect zigbee2mqtt to lidl

- [see **Silicon Labs EZSP v8** support](https://www.zigbee2mqtt.io/guide/adapters/#not-recommended)
- setup `configuration.yaml`

  ```yaml
  permit_join: false
  frontend: true
  mqtt:
    base_topic: zigbee2mqtt
    server: mqtt://mosquitto:1883
    client_id: test_m2q_client
  serial:
    port: tcp://<adapter_ip>:8888
    adapter: ezsp
  advanced:
    log_level: debug
  devices:
    '0x00158d00038bf31e':
      friendly_name: test_remote
    '0x84fd27fffe2586f0':
      friendly_name: test_ikea_remote
    '0xb4e3f9fffec900c7':
      friendly_name: test_light
    '0xbc33acfffe4f6299':
      friendly_name: test_lr_pir_sensor
    '0x680ae2fffe01b47a':
      friendly_name: test_plug
    '0xb4e3f9fffe2152ee':
      friendly_name: test_ikea_pir_sensor
    '0x00158d000448d06b':
      friendly_name: test_paulmann_led
    '0xb4e3f9fffeb9250b':
      friendly_name: test_ikea_switch
    '0xbc33acfffe11f829':
      friendly_name: '0xbc33acfffe11f829'
  ```

- Run MQTT broker: `docker-compose up mosquitto`
- Run zigbee2mqtt service: `docker-compose pull zigbee2mqtt && docker-compose up zigbee2mqtt --force-recreate --build`
- Capture messages: `mqtt sub --topic "zigbee2mqtt/test_remote/#" 2>&1 | tee .dev-out/first_dump/test-z2m-test_remote.json`
- Send message: `mqtt pub --topic "zigbee2mqtt/test_paulmann_led/set" --message '{"state": "ON"}'`
- Update state: `mqtt pub --topic "zigbee2mqtt/test_paulmann_led/get" --message '{"state": ""}'`
`mqtt sub --topic "zigbee2mqtt/bridge/devices" 2>&1 | tee .dev-out/test_dump/test-z2m-devices.json`

## Firmware update

https://github.com/grobasoz/zigbee-firmware/issues/16