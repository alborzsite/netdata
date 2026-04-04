# WhatsApp via 360messenger

![](https://netdata.cloud/img/whatsapp.svg)

Send notifications to WhatsApp using [360messenger.com](https://360messenger.com) via Netdata's Agent alert notification feature, which supports dozens of endpoints, user roles, and more.

![](https://img.shields.io/badge/maintained%20by-Community-%2300ab44)

## Setup

### Prerequisites

- A 360messenger account. Register at [360messenger.com](https://360messenger.com).
- An API token from your 360messenger dashboard.
- The phone number(s) you want to send notifications to. Phone numbers must be in international format without the `+` sign (e.g., `447488888888`).
- Terminal access to the Agent you wish to configure.

### Configuration

#### Options

The following options can be defined for this notification:

| Option | Description | Default | Required |
| --- | --- | --- | --- |
| `SEND_WHATSAPP_360MESSENGER` | Set to `YES` to enable notifications | `YES` | yes |
| `WHATSAPP_360MESSENGER_API_TOKEN` | Your 360messenger API token | | yes |
| [`DEFAULT_RECIPIENT_WHATSAPP_360MESSENGER`](#option-default-recipient-whatsapp-360messenger) | The phone number to send notifications to. Separate multiple recipients with spaces. | | yes |

##### DEFAULT_RECIPIENT_WHATSAPP_360MESSENGER

All roles will default to this variable if left unconfigured.

The `DEFAULT_RECIPIENT_WHATSAPP_360MESSENGER` can be overridden per role at the bottom of the same file:

```
role_recipients_whatsapp_360messenger[sysadmin]="447488888888"
role_recipients_whatsapp_360messenger[domainadmin]="447488888888"
role_recipients_whatsapp_360messenger[dba]="120363422685876942"
role_recipients_whatsapp_360messenger[webmaster]="447488888888"
role_recipients_whatsapp_360messenger[proxyadmin]="447488888888"
role_recipients_whatsapp_360messenger[sitemgr]="447488888888"
```

#### via File

The configuration file name for this integration is `health_alarm_notify.conf`.

You can edit the configuration file using the [`edit-config`](https://learn.netdata.cloud/docs/netdata-agent/configuration#edit-configuration-files) script from the Netdata [config directory](https://learn.netdata.cloud/docs/netdata-agent/configuration#locate-your-config-directory).

```bash
cd /etc/netdata 2>/dev/null || cd /opt/netdata/etc/netdata
sudo ./edit-config health_alarm_notify.conf
```

##### Basic Configuration

```bash
#------------------------------------------------------------------------------
# whatsapp_360messenger (360messenger.com) global notification options

SEND_WHATSAPP_360MESSENGER="YES"
WHATSAPP_360MESSENGER_API_TOKEN="your_api_token_here"
DEFAULT_RECIPIENT_WHATSAPP_360MESSENGER="447488888888"
```


##### Multiple Recipients

To send notifications to multiple recipients, separate them with spaces:

```bash
DEFAULT_RECIPIENT_WHATSAPP_360MESSENGER="447488888888"
```

## Troubleshooting

### Test Notification

You can run the following command by hand to test your alerts configuration:

```bash
# become user netdata
sudo su -s /bin/bash netdata

# enable debugging info on the console
export NETDATA_ALARM_NOTIFY_DEBUG=1

# send test alarms to sysadmin
/usr/libexec/netdata/plugins.d/alarm-notify.sh test

# send test alarms to whatsapp_360messenger role specifically
/usr/libexec/netdata/plugins.d/alarm-notify.sh test "whatsapp_360messenger"
```

Note that this will test all alert mechanisms for the selected role.

### Getting your API Token

1. Log in to your [360messenger dashboard](https://360messenger.com)
2. Navigate to **API Settings**
3. Generate a new API token
4. Copy the token and paste it into `WHATSAPP_360MESSENGER_API_TOKEN`
