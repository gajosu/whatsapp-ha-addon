# Whatsapp web HA add-on

## How to use
1. Add repository to Home Assistant ([Instructions here](../../../#readme))
2. Install the add-on
3. Start the add-on
5. Open the web interface http://<your_ha_ip>:3000
6. Scan the QR code
7. Add the rest commands to Home Assistant configuration.yaml

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

