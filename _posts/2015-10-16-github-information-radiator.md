---
layout: post
title: "Github Information Radiator"
date: 2015-10-16
summary: Communication doesn’t scale well, so in order to broadcast status information to everyone we hacked together a Raspberry Pi and a programmable Philips Hue Go light into an automated, real-time information radiator.
---

Our [CI](https://en.wikipedia.org/wiki/Continuous_integration)/[CD](https://en.wikipedia.org/wiki/Continuous_delivery) pipeline relies heavily on [Github](https://github.com/), [among](https://jenkins-ci.org/) [other](https://www.docker.com/) [things](http://aws.amazon.com/).


We've got proper monitoring using [New Relic](https://newrelic.com/), [AWS Cloudwatch](https://aws.amazon.com/cloudwatch/) and [Pingdom](https://www.pingdom.com/) and alerting via [PagerDuty](https://www.pagerduty.com/) and [Slack](https://slack.com/) and all that.


But I decided to go one step further and build an [information radiator](https://www.atlassian.com/wallboards/information-radiators.jsp):


![Github Information Radiator](/i/monitoring-github/monitoring-github-256.gif)


## Ingredients

5” Octocat Figurine
[![5" Octocat Figurine](/i/monitoring-github/github-figurine.jpg)](http://github.myshopify.com/products/octocat-figurine)


Philips Hue Go
[![Philips Hue Go](/i/monitoring-github/hue-lux-go.jpg)](http://www.amazon.com/Philips-798835-Personal-Wireless-Lighting/dp/B00UVHAC1O)


Philips Hue Hub Starter Kit
[![Philips Hue Hub Starter Kit](/i/monitoring-github/hue-lux-hub-kit.jpg)](http://www.amazon.com/Philips-426353-White-Starter-Generation/dp/B00A4EUUO8)


Raspberry Pi
[![Raspberry Pi](/i/monitoring-github/rpi.png)](http://www.amazon.com/Raspberry-Pi-Model-Project-Board/dp/B00T2U7R7I)


`$ cat poll_github.sh`

```sh
#!/bin/bash

# poll global github status api

HUE_HOST=192.168.1.3
export HUE_HOST
lights="4"

status=$(curl --silent https://status.github.com/api/status.json | grep status | egrep -o '(good|minor|major)')
case "$status" in
good)  /bin/bash hue_set_ok.sh     $lights ;;
minor) /bin/bash hue_set_orange.sh $lights ;;
major) /bin/bash hue_set_error.sh  $lights ;;
*)     /bin/bash hue_set_other.sh  $lights ;;
esac
```


`$ cat hue_set_ok.sh`

```sh
#!/bin/bash

HOST=${HUE_HOST:-192.168.1.3}
USER=${HUE_USER:-beautifulhuetest}

for n in $@
do
    curl -X PUT http://$HOST/api/$USER/lights/$n/state --data '{"bri":60,"hue":25500,"sat":254,"alert":"none","effect":"none"}'
done
```


## Next Step

Replace the simplistic polling implementation by [monitoring our Slack channel for Jenkins messages](https://github.com/slackhq/python-slackclient).


## References

* [information radiator](https://www.atlassian.com/wallboards/information-radiators.jsp)
* [status.github.com](https://status.github.com/)

