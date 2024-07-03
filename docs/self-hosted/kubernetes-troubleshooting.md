# Troubleshooting
## Private Packagist Self-Hosted (Kubernetes)

#### Private Packagist Self-Hosted update requires KOTS upgrade
Please refer to the "[Updates](./kubernetes-maintenance.md#updates)" section in our maintenance guide.

#### Generating a support bundle

You can generate a support bundle from the Replicated Management Console on port
8800 by navigating to the Troubleshoot tab, clicking "Generate a support bundle",
and selecting "Analyze".
Once the analysis is done, either download the bundle and manually send it to us
or click on the send icon which will send us the bundle. Please always notify us
if you send us a support bundle!

#### Issues with Multi-factor Authentication

If you are having problems setting up MFA, or are unable to log in via MFA, with
your generated codes, there may be a time-drift issue with either the
Self-hosted Private Packagist server or the device you are using to generate the
codes.

To make sure that the Self-hosted Private Packagist server is synchronized to
the correct time, you should check that both the current server time and
timezone are set to the correct values. If you can enable Network Time Protocol
(NTP) for the server, we also recommend doing that.

The methods for doing so will vary depending on the underlying server Operating
System.

> Please be aware that offline-based TOTP hardware can drift up to a few minutes
> a year. As Private Packagist only allows a time-drift of up to one (1) minute, we
> recommend using TOTP devices that have the ability to stay synchronized with
> the correct time (such as a phone, or re-programmable TOTP hardware devices).
