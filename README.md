# librenms-random-thoughts
Random thoughts about different LibreNMS integrations as of Aug 2025.

## Pollers

Getting LibreNMS in Grafana for pretty graphs is quite a task because there are nearly no resources/guides online. Mostly uncharted territories. Here are my observations:
- Prometheus integration sucks because it wants you to use PushGateway and then it still keeps all the data inside the thing which is supposed to be used for temporary, quick tasks. So the ball grows big very quickly and it becomes disk-heavy. Also it increases polling time quite a bit, other people on the forums have pointed it out, no clue why and how that affects SNMP polls between devices, but yeah it sucks, don't use it.
- InfluxDB on the other hand works like a charm. There is even a [premade Grafana dashboard](https://grafana.com/grafana/dashboards/2556-librenms-interfaces/) made by some kind soul. Use that and then filter and edit according to your needs. Those metrics are leagues better than whatever it throws into Prometheus PushGateway, trust me.

## Alerts

Getting an alert message into WhatsApp is a whole nightmare, regular SMS and stuff like Telegram take no time to setup to get up and running, but for WhatsApp, you have to go through hoops to get it working. Right now, this is my setup:
```
Transport type: API
API Method: POST
Send as form: ON
API URL: https://api.twilio.com/2010-04-01/Accounts/ACcXXX/Messages.json
To=whatsapp:<your_phone_number>
From=whatsapp:<whatsapp_business_api_phone_number>
ContentSid=HXXXX
ContentVariables={"1":{{ $msg|json_encode }}}
Auth Username: ACcXXX
Auth Password: <token>
```

whereas the message template contains a variable named `{1}`. Otherwise, it sends the test alert just fine but with a real alert it hangs up because it doesn't escape characters properly, and everything gets ruined.

Hopefully someone trying to get this to work through Twilio will stumple upon this repo and save themselves hours of headache :)
