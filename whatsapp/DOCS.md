# Whatsapp web HA add-on

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

Note: Replace <your_ha_ip> with your Home Assistant IP address

```yaml
rest_command:

    ###############################
    ############ Chats ############
    ###############################

    # Delete a chat
    whatsapp_delete_chat:
        url: 'http://<your_ha_ip>:3000/api/chats/{{ chat_id }}'
        method: DELETE

    # Archive a chat
    whatsapp_archive_chat:
        url: 'http://<your_ha_ip>:3000/api/chats/{{ chat_id }}/archive'
        method: PUT

    # Unarchive chat
    whatsapp_unarchive_chat:
        url: 'http://<your_ha_ip>:3000/api/chats/{{ chat_id }}/unarchive'
        method: PUT

    # Pin a chat
    whatsapp_pin_chat:
        url: 'http://<your_ha_ip>:3000/api/chats/{{ chat_id }}/pin'
        method: PUT

    # Unpin a chat
    whatsapp_unpin_chat:
        url: 'http://<your_ha_ip>:3000/api/chats/{{ chat_id }}/unpin'
        method: PUT

    # mark as read
    whatsapp_mark_chat_as_read:
        url: 'http://<your_ha_ip>:3000/api/chats/{{ chat_id }}/read'
        method: PUT

    # mark a chat as unread
    whatsapp_mark_chat_as_unread:
        url: 'http://<your_ha_ip>:3000/api/chats/{{ chat_id }}/unread'
        method: PUT

    # send typing status
    whatsapp_send_typing_status:
        url: 'http://<your_ha_ip>:3000/api/chats/{{ chat_id }}/send-typing'
        method: PUT

    # send recording status
    whatsapp_send_recording_status:
        url: 'http://<your_ha_ip>:3000/api/chats/{{ chat_id }}/send-recording'
        method: PUT

    # send clear command to stop typing or recording
    whatsapp_send_clear_status:
        url: 'http://<your_ha_ip>:3000/api/chats/{{ chat_id }}/clear-status'
        method: PUT


    ###############################
    ########### Messages ##########
    ###############################

    # send a message to a chat
    whatsapp_send_text_message:
        url: 'http://<your_ha_ip>:3000/api/chats/{{ chat_id }}/messages'
        method: POST
        headers:
            Content-Type: application/json
        payload: '{"msg": "{{ message }}"}'

    # Send a file, e.g. a picture
    whatsapp_send_media_message:
        url: 'http://<your_ha_ip>:3000/api/chats/{{ chat_id }}/messages'
        method: POST
        headers:
            Content-Type: application/json
        payload: '{"url": "{{ url }}"}'

    # Star a message
    whatsapp_star_message:
        url: 'http://<your_ha_ip>:3000/api/chats/{{ chat_id }}/messages/{{ message_id }}/star'
        method: PUT

    # Unstar a message
    whatsapp_unstar_message:
        url: 'http://<your_ha_ip>:3000/api/chats/{{ chat_id }}/messages/{{ message_id }}/unstar'
        method: PUT

    # react to a message with a emoji
    whatsapp_react_message:
        url: 'http://<your_ha_ip>:3000/api/chats/{{ chat_id }}/messages/{{ message_id }}/react'
        method: PUT
        headers:
            Content-Type: application/json
        #reaction must be a emoji like: 😂
        payload: '{"reaction": "{{ reaction }}"}'

    # Unreact to a message
    whatsapp_unreact_message:
        url: 'http://<your_ha_ip>:3000/api/chats/{{ chat_id }}/messages/{{ message_id }}/unreact'
        method: PUT

    # Delete a message
    whatsapp_delete_message:
        url: 'http://<your_ha_ip>:3000/api/chats/{{ chat_id }}/messages/{{ message_id }}'
        method: DELETE

  ###############################
  ############ Groups ###########
  ###############################

    # Create a group
    # participants must be a list of user ids
    # yaml example:
    #   data:
    #     name: "My group"
    #     participants:
    #       - 12345679@c.us
    #       - 98765432@c.us
    whatsapp_create_group:
        url: 'http://<your_ha_ip>:3000/api/groups'
        method: POST
        headers:
            Content-Type: application/json
        payload: '{"name": "{{ name }}", "participants": {{ participants }}}'

    # update group info
    whatsapp_update_group:
        url: 'http://<your_ha_ip>:3000/api/groups/{{ group_id }}'
        method: PUT
        headers:
            Content-Type: application/json
        payload: '{"name": "{{ name }}", "description": "{{ description }}"}'
    
    # Add participants to a group
    whatsapp_add_participants_to_group:
        url: 'http://<your_ha_ip>:3000/api/groups/{{ group_id }}/participants'
        method: POST
        headers:
            Content-Type: application/json
        payload: '{"participants": {{ participants }}}'

    # Remove participants from a group
    whatsapp_remove_participants_from_group:
        url: 'http://<your_ha_ip>:3000/api/groups/{{ group_id }}/participants'
        method: DELETE
        headers:
            Content-Type: application/json
        payload: '{"participants": {{ participants }}}'

    # promote participants to admins
    whatsapp_promote_participants_to_admins:
        url: 'http://<your_ha_ip>:3000/api/groups/{{ group_id }}/promote'
        method: PUT
        headers:
            Content-Type: application/json
        payload: '{"participants": {{ participants }}}'

    # demote admins to participants
    whatsapp_demote_admins_to_participants:
        url: 'http://<your_ha_ip>:3000/api/groups/{{ group_id }}/demote'
        method: PUT
        headers:
            Content-Type: application/json
        payload: '{"participants": {{ participants }}}'

    # leave a group
    whatsapp_leave_group:
        url: 'http://<your_ha_ip>:3000/api/groups/{{ group_id }}'
        method: DELETE


  ###############################
  ########### Contacts ##########
  ###############################

    #block a contact
    whatsapp_block_contact:
        url: 'http://<your_ha_ip>:3000/api/contacts/{{ contact_id }}/block'
        method: PUT

    #unblock a contact
    whatsapp_unblock_contact:
        url: 'http://<your_ha_ip>:3000/api/contacts/{{ contact_id }}/unblock'
        method: PUT

  ###############################
  ########### User ##############
  ###############################

    # send online status
    whatsapp_send_available_presence:
        url: 'http://<your_ha_ip>:3000/api/me/presence/available'
        method: PUT

    # send offline status
    whatsapp_send_unavailable_presence:
        url: 'http://<your_ha_ip>:3000/api/me/presence/unavailable'
        method: PUT

    # update display name
    whatsapp_update_display_name:
        url: 'http://<your_ha_ip>:3000/api/me/display-name'
        method: PUT
        headers:
            Content-Type: application/json
        payload: '{"name": "{{ name }}"}'

    # update text status
    whatsapp_update_status:
        url: 'http://<your_ha_ip>:3000/api/me/text-status'
        method: PUT
        headers:
            Content-Type: application/json
        payload: '{"status": "{{ status }}"}'
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
          to: "122222222@c.us"
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

