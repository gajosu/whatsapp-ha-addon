# Whatsapp web HA add-on

_A WhatsApp API client that connects through the WhatsApp Web browser app with Home Assistant._

![Supports aarch64 Architecture][aarch64-shield]
![Supports amd64 Architecture][amd64-shield]
![Supports armhf Architecture][armhf-shield]
![Supports armv7 Architecture][armv7-shield]
![Supports i386 Architecture][i386-shield]

## What is this add-on?
This add-on is a wrapper for the [whatsapp-web.js](https://github.com/pedroslopez/whatsapp-web.js) library. It allows you to send and receive messages from WhatsApp through Home Assistant.

## How to use
1. Add repository to Home Assistant ([Instructions here](../../../#readme))
2. Install the add-on
3. Start the add-on
5. Open the web interface http://<your_ha_ip>:3000
6. Scan the QR code
7. Add the rest commands to Home Assistant configuration.yaml

### **How to get a User ID**

The user id is made from three parts:

- Country code (Example `34` for Spain)
- Phone number (Example `123456789`)
- And a static part: @c.us (for users) @g.us (for groups)

So, if you want to send a message to a user with number `123456789` and country code `34`, the user id will be `34123456789@c.us` (the `@c.us` part is static)

<br>

## Configuration
Add the following to your `configuration.yaml` file:

```yaml
rest_command:
    whatsapp_send_text_message:
        url: http://<your_ha_ip>:3000/api/messages
        method: POST
        headers:
            Content-Type: application/json
        payload: '{"to": "{{ to }}", "msg": "{{ message }}"}'

    # Send a file, e.g. a picture
    whatsapp_send_media_message:
        url: http://<your_ha_ip>:3000/api/messages
        method: POST
        headers:
            Content-Type: application/json
        payload: '{"to": "{{ to }}", "url": "{{ url }}"}'
```

## Example usage
```yaml
automation:
  - alias: Send a message
    trigger:
      - platform: state
        entity_id: binary_sensor.motion_sensor_158d0001c0b2b2
        to: 'on'
    action:
      - service: rest_command.whatsapp_send_text_message
        data:
          #  sending to +1 22222222
          to: "122222222"
          message: "Someone is at the door!"
```

## Reciving messages
The add-on will send events to Home Assistant when a message is received.


### Events
<!-- Table -->
| Event | Description | Data |
| --- | --- | --- |
| `whatsapp_authenticated` | Triggered when the connection is authenticated. | None |
| `whatsapp_disconnected` | Triggered when the connection is disconnected. | None |
| `whatsapp_message_received` | Triggered when a message is received. | { [Message Schema](#message-schema) } |
| `whatsapp_message_sent` | Triggered when a message is sent. | { [Message Schema](#message-schema) } |
| `whatsapp_message_ack` | Triggered when a message is acknowledged. | { message: [Message Schema](#message-schema), ack : [Ack Schema](#ack-schema-enum) } |
| `whatsapp_message_revoke_for_everyone` | Triggered when a message is revoked for everyone. | { message: [Message Schema](#message-schema), revokedMessage:  [Message Schema](#message-schema)} |
| `whatsapp_message_revoke_for_me` | Triggered when a message is revoked for you. | { message: [Message Schema](#message-schema)} |
| `whatsapp_message_group_join` | Triggered when a user joins a group. | { [GroupNotification Schema](#groupnotification-schema) } |
| `whatsapp_message_group_leave` | Triggered when a user leaves a group. | { [GroupNotification Schema](#groupnotification-schema) } |
| `whatsapp_message_group_update` | Triggered when a group is updated, such as subject, description or picture. | { [GroupNotification Schema](#groupnotification-schema) } |
| `whatsapp_message_call` | Triggered when a call is received. | { [Call Schema](#call-schema) } |
| `whatsapp_state` | Triggered when the state of the connection changes. | { state : [WA State Schema Schema](#wa-state-schema-enum) } |


### Message Schema
<!-- Table -->
| Key | Description | Type |
| --- | --- | --- |
| `ack` | Acknowledgement of the message. | `number` |
| `author` | Author of the message. | `string` |
| `body` | Body of the message. | `string` |
| `broadcast` | Whether the message is a broadcast. | `boolean` |
| `deviceType` | Device type of the message. | `string` |
| `duration` | Duration of the message in seconds. | `number` |
| `forwardingScore` | Indicates how many times the message was forwarded. The maximum value is 127.. | `number` |
| `from` | Sender of the message. | `string` |
| `fromMe` | Whether the message was sent by the user. | `boolean` |
| `hasMedia` | Whether the message has media. | `boolean` |
| `hasQuotedMsg` | Whether the message has a quoted message. | `boolean` |
| `id` | ID of the message. | `object` |
| `inviteV4` | Invite V4 of the message. | `string` |
| `isEphemeral` | Whether the message is ephemeral. | `boolean` |
| `isForwarded` | Whether the message is forwarded. | `boolean` |
| `isGif` | Whether the message is a GIF. | `boolean` |
| `isStarred` | Whether the message is starred. | `boolean` |
| `isStatus` | Whether the message is a status. | `boolean` |
| `links` | Links in the message. | `Array of {link: string, isSuspicious: boolean}` |
| `to` | Recipient of the message. | `string` |
| `type` | Type of the message. | `MessageTypes` |

For more information about the message schema, see the [Message class](https://docs.wwebjs.dev/Message.html).

<br />

### Ack Schema (ENUM)
<!-- Table -->
| Name | Description | Type |
| --- | --- | --- |
| `ACK_ERROR` | Error. | `string` |
| `ACK_PENDING` | Pending. | `string` |
| `ACK_SERVER` | Message received by the server. | `string` |
| `ACK_DEVICE` | Message received by the device. | `string` |
| `ACK_READ` | Message read by the recipient. | `string` |
| `ACK_PLAYED` | Message played by the recipient. | `string` |

<br />

## WA State Schema (ENUM)
<!-- Table -->
| Name | Description | Type |
| --- | --- | --- |
| `CONFLICT` | Connection conflict. | `string` |
| `CONNECTED` | Connected to WA servers. | `string` |
| `DEPRECATED_VERSION` | Deprecated version. | `string` |
| `OPENING` | Opening connection. | `string` |
| `PAIRING` | Pairing. | `string` |
| `SMB_TOS_BLOCK` | SMB TOS block. | `string` |
| `TIMEOUT` | Connection timeout. | `string` |
| `TOS_BLOCK` | TOS block. | `string` |
| `UNLAUNCHED` | Unlaunched. | `string` |
| `UNPAIRED` | Unpaired. | `string` |
| `UNPAIRED_IDLE` | Unpaired idle. | `string` |

### GroupNotification Schema
<!-- Table -->
| Field | Type | Description |
| --- | --- | --- |
| `id` | `object` | The id of the group. |
| `author` | `string` | The ContactId of the author. |
| `body` | `string` | The body of the message. |
| `chatId` | `string` | The id of the chat. |
| `recipientIds` | `string[]` | The ContactIds of the recipients. |
| `timestamp` | `number` | The timestamp of the message. |
| `type` | `enum('add', 'invite',, 'remove', 'leave', 'subject', 'description', 'picture', 'announce', 'restrict')` | The type of the group notification. |

### Call Schema
<!-- Table -->
| Field | Type | Description |
| --- | --- | --- |
| `id` | `string` | The id of the call. |
| `from` | `string` | The ContactId of the caller. |
| `timestamp` | `number` | The timestamp of the call. |
| `isVideo` | `boolean` | Whether the call is a video call. |
| `isGroup` | `boolean` | Whether the call is a group call. |
| `canHandleLocally` | `boolean` | Whether the call can be handled in waweb |
| `webClientShouldHandle` | `boolean` | Whether the call should be handled in waweb |
| `participants` | `object` | The participants of the call. |


For more information about the message schema, see the [Message class](https://docs.wwebjs.dev/Message.html).

<br />

### Ack Schema (ENUM)
<!-- Table -->
| Name | Description | Type |
| --- | --- | --- |
| `ACK_ERROR` | Error. | `string` |
| `ACK_PENDING` | Pending. | `string` |
| `ACK_SERVER` | Message received by the server. | `string` |
| `ACK_DEVICE` | Message received by the device. | `string` |
| `ACK_READ` | Message read by the recipient. | `string` |
| `ACK_PLAYED` | Message played by the recipient. | `string` |

<br />

## WA State Schema (ENUM)
<!-- Table -->
| Name | Description | Type |
| --- | --- | --- |
| `CONFLICT` | Connection conflict. | `string` |
| `CONNECTED` | Connected to WA servers. | `string` |
| `DEPRECATED_VERSION` | Deprecated version. | `string` |
| `OPENING` | Opening connection. | `string` |
| `PAIRING` | Pairing. | `string` |
| `SMB_TOS_BLOCK` | SMB TOS block. | `string` |
| `TIMEOUT` | Connection timeout. | `string` |
| `TOS_BLOCK` | TOS block. | `string` |
| `UNLAUNCHED` | Unlaunched. | `string` |
| `UNPAIRED` | Unpaired. | `string` |
| `UNPAIRED_IDLE` | Unpaired idle. | `string` |


<br >

## Example usage
```yaml
automation:
  - alias: Respond to a message
    trigger:
      - platform: event
        event_type: whatsapp_message_received
    action:
      - service: rest_command.whatsapp_send_text_message
        data:
          to: "{{ trigger.event.data.from }}"
          message: "Hello!"
```

## More details
For more details about the API, see the [documentation](https://docs.wwebjs.dev/) of the underlying library.

## Support
If you have any questions, please open an issue on the [gajosu/whatsapp-web-rest-api](https://github.com/gajosu/whatsapp-web-rest-api) repository.

## Contributing
If you want to contribute to this add-on, please open a pull request on the [gajosu/whatsapp-web-rest-api](https://github.com/gajosu/whatsapp-web-rest-api)


## Credits
- Pedro S Lopez ([@pedroslopez](https://github.com/pedroslopez)) for the [whatsapp-web.js](https://github.com/pedroslopez/whatsapp-web.js)
- Gabriel González ([@gajosu](https://github.com/gajosu?)) for the [whatsapp-web-rest-api](https://github.com/gajosu/whatsapp-web-rest-api)

## Disclaimer
This project is not affiliated, associated, authorized, endorsed by, or in any way officially connected with WhatsApp or any of its subsidiaries or its affiliates. The official WhatsApp website can be found at https://whatsapp.com. "WhatsApp" as well as related names, marks, emblems and images are registered trademarks of their respective owners.


[aarch64-shield]: https://img.shields.io/badge/aarch64-yes-green.svg
[amd64-shield]: https://img.shields.io/badge/amd64-yes-green.svg
[armhf-shield]: https://img.shields.io/badge/armhf-yes-green.svg
[armv7-shield]: https://img.shields.io/badge/armv7-yes-green.svg
[i386-shield]: https://img.shields.io/badge/i386-yes-green.svg


## License

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this project except in compliance with the License.
You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0.

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.