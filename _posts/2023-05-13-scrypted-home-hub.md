---
layout: post
title: "Meet Scrypted - Stream ANY Camera to ANY Home Hub"
date: 2023-05-13 10:00:00 -0500
categories: self-hosted
tags: scrypted smart-home iot unifi apple google amazon alexa home-assistant home-security open-source
image:
  path: /assets/img/headers/smart-home-hub.jpg
---

Meet Scrypted an Open Source app that will let you connect almost any camera to any home hub, certified or not!  You can connect popular devices from UniFi, Amcrest, Hikvision, Nest & Google, Tuya, Reolink, and many others to your home hub of choice, whether that be Apple’s HomeKit, Google Home, Alexa, or even Home Assistant.  This lets you choose and reuse your own devices and take advantage of the automation and integration you get with your Smart Home Hub.  

{% include embed/youtube.html id='x1xsFRQYSv0' %}
📺 [Watch Video](https://www.youtube.com/watch?v=x1xsFRQYSv0)

## Where to Buy

UniFi UDM SE - <https://amzn.to/44TWJYJ>

UniFi Cameras - <https://amzn.to/3M3FPyp>

Apple TV 4k - <https://amzn.to/3M1ZFtz>

Apple iPad - <https://amzn.to/3VX1k8z>

Echo Dot - <https://amzn.to/44YmpTW>

Echo Show 5 - <https://amzn.to/3MmFdoN>

Echo Show 15 - <https://amzn.to/3Mo8ZJM>

See the whole kit here!

<https://kit.co/TechnoTim/smart-home-hubs-devices>

(Affiliate links are included in this description. I may receive a small commission at no cost to you.)

## Smart Home Camera Fragmentation

I have cameras in and around my house.  I have cameras on the front door, cameras inside my house, cameras that point outside of my house, cameras in my garage, cameras in my server room, and even cameras in my server rack….

![Camera in my server rack](/assets/img/posts/camera-in-my-server-rack.jpg)
_Don’t ask..._

All of these cameras work great and I keep all of the recorded footage in my home but what’s not great is that I am not able to tap into popular Home Hubs like HomeKit, Alexa, Google Home, or even Home Assistant.  That’s because many of these platforms require some very specific requirements for adding cameras to your home hub.  That means they might have to be certified, have to be compatible, and both manufacturers have to get along…  and we know how that story goes.  

![Getting ecosystems to play nice together](/assets/img/posts/ecosystem meme.jpg)
_Getting ecosystems to play nice together_

So this left me with using one app to check my video, while leaving a whole host of features that my home hub provides on the table.  Things like notifications within my ecosystem, the ability to trigger automation based on my eco system, cool features like picture in a picture on other devices, and all the things that make home hubs, well, hubs.  And while I do like my home security choice, I don’t like that it doesn’t integrate with my home hub of choice.  It’s not my fault these two companies don’t get along and I am not going to buy all new cameras just to be compatible with my home hub. That’s where Scrypted comes in.

## What is Scrypted

[Scrypted](https://scrypted.app) is open source software that you host on your own machine that allows you to connect almost any camera to any hub, that’s right, certified or not.

You can connect popular devices from UniFi, Amcrest, Hikvision, Nest & Google, Tuya, Reolink, and many others to your home hub of choice, whether that be Apple’s HomeKit, Google Home, Alexa, or even Home Assistant.  This lets you choose and reuse your own devices and take advantage of the automation and integration you get with your home hub.  That’s right, something the big players aren’t offering you, and that’s choice.  This means that you don’t have to pay for that subscription if you want video outside of your home, you can use scrypted to connect to one of the major hubs or even an open source one like Home Assistant.  

![Scrypted integrations](/assets/img/posts/scrypted-integrations-and-hubs.jpg)
_Scrypted supports many different camera integrations and many Home Hubs!_

Here’s where it gets really cool...

Scrypted is pluggable, so it allows developers to create and update plugins within Scrypted, giving them and you lots of flexibility.  Want to connect a Google Nest Camera to Alexa? Sure!  Want to connect a Reolink camera to Google Home, absolutely!  Want to connect your UniFi Cameras to HomeKit?  No problem!  What about connecting some named or no name camera that only supports RTSP or ONVIF?  Scrypted has you covered!

## Scrypted Requirements

You’re probably wondering, what does all of this cost?  Well, if you already have the hardware it costs nothing but a little bit of your time.

The next thing you’re probably asking what hardware you need to get started, or maybe you're not even asking that, but I will tell you that you will need some hardware to get started 😀

The [requirements are actually pretty low](https://github.com/koush/scrypted#installation) and you can run it on the latest raspberry pi, or Windows, Mac, or Linux, and even Docker, either standalone or on many NAS devices like Unraid or Synology.  It’s easy to set up and after you’ve connected your cameras to your home hub, you’ll be able to take advantage of all of the integrations your camera offers as well as automation your hub offers.

So that’s what I am going after today, setting up Scrypted to connect my UniFi cameras to my HomeKit hub, which is one of my Apple TVs so I can use all of my cameras as if they are HomeKit Certified. Now wait, even if you don’t want or have this combination of devices and hubs, you can still follow along to set this up with any camera or any hub.

## Installing Scrypted with Docker

So I first created a Linux machine and then installed Docker, which I highly recommend using, but if you don’t feel comfortable you can install it any other way you like.  If you are using Docker, I recommend doing this on Ubuntu, but Windows Mac, or any other version of Linux will work just fine.  If you’re using a Linux machine I recommend using Docker and Portainer.  [Portainer](https://www.portainer.io/) is a great container management system  for Docker that has a great UI.  The [install](https://docs.portainer.io/start/install-ce?hsCtaTracking=a66b69bb-4970-46b7-bc31-cfc8022c7eb2%7C0d5be9a2-9dac-4ab1-9498-4b07566effd3) is fast and painless and makes managing Docker really easy.

Once you’re in portainer, all you need to do is connect to your Docker instance, add a container,  and then set a few properties like the name of the container, the image name and tag, and then you’ll need to map your data volume from the container to the local machine.  This should be somewhere your Portainer machine can read and write to, for me it’s just a simple path to a folder on the machine.  The last thing they recommend is setting the network to host mode which means it will use the networking on the host instead of Docker networking.  Once all that’s set, just deploy the container and you’re good to go.

![Scrypted integrations](/assets/img/posts/scrypted-portainer-install.jpg)
_Installing Scrypted with Portainer is simple!_

Oh, and if you want to use Docker compose, you can use this to get started quickly!  Note, this will also include `watchtower` to updated your stack automatically.  If you don't want to use `watchtower` just comment out that section.

```yaml
version: "3.5"

# The Scrypted docker-compose.yml file typically resides at:
# ~/.scrypted/docker-compose.yml

# Example volumes SMB (CIFS) and NFS.
# Uncomment only one.

# volumes:
#     nvr:
#         driver_opts:
#             type: cifs
#             o: username=[username],password=[password],vers=3.0,file_mode=0777,dir_mode=0777 
#             device: //[ip-address]/[path-to-directory]
#     nvr:
#         driver_opts:
#             type: "nfs"
#             o: "addr=[ip-address],nolock,soft,rw"
#             device: ":[path-to-directory]"

services:
    scrypted:
        image: koush/scrypted
        environment:
            - SCRYPTED_WEBHOOK_UPDATE_AUTHORIZATION=Bearer SET_THIS_TO_SOME_RANDOM_TEXT
            - SCRYPTED_WEBHOOK_UPDATE=http://localhost:10444/v1/update
            # nvidia support
            # - NVIDIA_VISIBLE_DEVICES=all
            # - NVIDIA_DRIVER_CAPABILITIES=all
        # runtime: nvidia
        container_name: scrypted
        restart: unless-stopped
        network_mode: host

        devices:
            # hardware accelerated video decoding, opencl, etc.
            - /dev/dri:/dev/dri
            # uncomment below as necessary.
            # zwave usb serial device
            #   - /dev/ttyACM0:/dev/ttyACM0
            # all usb devices, such as coral tpu
            #   - /dev/bus/usb:/dev/bus/usb

        volumes:
            - ~/.scrypted/volume:/server/volume
            # modify and add the additional volume for Scrypted NVR
            # the following example would mount the /mnt/sda/video path on the host
            # to the /nvr path inside the docker container.
            # - /mnt/sda/video:/nvr

            # or use a network mount from one of the examples above
            # - type: volume
            #   source: nvr
            #   target: /nvr
            #   volume:
            #     nocopy: true

            # uncomment the following lines to expose Avahi, an mDNS advertiser.
            # make sure Avahi is running on the host machine, otherwise this will not work.
            # - /var/run/dbus:/var/run/dbus
            # - /var/run/avahi-daemon/socket:/var/run/avahi-daemon/socket
        # logging is noisy and will unnecessarily wear on flash storage.
        # scrypted has per device in memory logging that is preferred.
        logging:
            driver: "json-file"
            options:
                max-size: "10m"
                max-file: "10"
        labels:
            - "com.centurylinklabs.watchtower.scope=scrypted"

    # watchtower manages updates for Scrypted.
    watchtower:
        environment:
            - WATCHTOWER_HTTP_API_TOKEN=SET_THIS_TO_SOME_RANDOM_TEXT
            - WATCHTOWER_HTTP_API_UPDATE=true
            - WATCHTOWER_SCOPE=scrypted
            # remove the following line to never allow docker to auto update.
            # this is not recommended.
            - WATCHTOWER_HTTP_API_PERIODIC_POLLS=true
        image: containrrr/watchtower
        container_name: scrypted-watchtower
        restart: unless-stopped
        volumes:
            - /var/run/docker.sock:/var/run/docker.sock
        labels:
            - "com.centurylinklabs.watchtower.scope=scrypted"
        ports:
            # The auto update port 10444 can be configured
            # Must match the port in the auto update url above.
            - 10444:8080
        # check for updates once an hour (interval is in seconds)
        command: --interval 3600 --cleanup --scope scrypted
```

## Configuring Scrypted

Once the container is running, you’ll want to go to the machines IP address on port 10443

Once you get there you will be greeted with a sign in page where you will create an account and password.  Once signing in you will see the scripted homepage!

The first thing I did was turn on dark mode, of course, and then went into the plugins page and clicked install plugins.

Here you’ll want to install the plugin for the platform you want to support.  Scripted supports many different cameras and many different hub platforms, for instance you can search for Alex and see the plugins for Amazon Alexa or Google home and see the integrations for Google home, or even Amcrest if you want to find the amcrest camera plugin!  There are lots of supported cameras but  for me this is going to be unifi so I searched for unifi and installed it.  Once it’s installed it will ask for a username and password for your unifi device.  We need to create one in UniFi protect, but you’ll want to create a new local account and not provide yours!

![Scrypted plugins](/assets/img/posts/scrypted-plugin-install.jpg)
_Installing the uniFi Protect Plugin for Scrypted_

## Create UniFi  Protect User

So we need to go into UniFi and create a new user.  We’ll have to do this in the UniFi console and what I did was create a new Role first that has Full Management access to Protect.  The documentation says that you might be able to drop this down later to a Read Only user, which I may do, but I will create the user and give it admin access to Protect only.  Again, be sure to create a local account and set the permissions appropriately.

Once that user was created I then added the user and the password as well as the IP address of my UDM SE.

Once I saved my credentials I could then see all of my cameras and you can view them now, however since I am going to use HomeKit, I need to add that plugin as well.

![Scrypted plugins](/assets/img/posts/scrypted-unifi-protect-user.jpg)
_You'll need to create an account in UniFi Protect for Scrypted to use_

## Adding Cameras to HomeKit

So I searched for the HomeKit plugin and installed it.  We don’t need to change anything in the plugin, I just reloaded the unifi plugin and went back into my cameras.  Now you’ll see some additional options, one being HomeKit.  You’ll want to be sure that this is enabled.  I also made sure that the SnapSho tplugin was enabled too.

Once this was set up, all I had to do now was just add the cameras to HomeKit.

I did this by navigating to each individual camera in Scrypted and then clicked on HomeKit and then Clicked on pairing.  It will then show you a QR code which you can scan and it will add it to your home in HomeKitI did this for all of my cameras, accepted the message about it not being an officially certified accessory, and then chose the default settings of stream while I am home and while I am away.  The reason I didn’t change any of this is because I still still use my UniFi Protect for camera storage, rather than store it on my docker container. And for those counting, I have nine cameras…. Yes, 9 cameras…

## Awesome Integration & Automation

Now that we have this set up, what can we do with it?

Well, now in the home app I can see all of my cameras at a glance.  I can see the latest snapshot of each camera and drill in further to see a live view.  I can pin the camera too so that I can multitask.

![Scrypted Apple TV Integration](/assets/img/posts/scrypted-homekit-apple-tv.jpg)
_Finally!  Notifications on my Apple TV!_

I get notifications if someone presses the doorbell and I can talk with them too.  Not only do I get notifications on my phone, but I also get them on my macbook, ipad, and even AppleTV -  so just in case I am “super busy” watching something that’s “super important” I can decide whether or not to get my lazy butt off the couch.

Here’s the other cool thing: Since I have an  Apple TV, I can even say “Hey Siri” show me my cameras and it will show me all of my connected cameras.  From there I can browse them, pick one to watch and even listen to, and even pin it to the screen so I can keep an eye on things or if I am expecting a delivery.  If they are in the same zone as other devices, I can interact with these devices too, like toggling on and off the lights on my porch.

![Scrypted Apple TV Picture in a Picture](/assets/img/posts/scrypted-pip-tv-game.jpg)
Picture in a Picture on my TV!_

## More Scrypted Compatibility

One of the things I like most about  Scrypted is that I can use almost any camera I want and I can connect it to one of many platforms.  Now, I obviously connected UniFi cameras to HomeKit, but you can connect almost any camera to any platform.  Want to connect some old PoE cameras to Alex or Google Home Hub, or even Home Assistant?  No problem.  That’s the beauty of Scrypted, is that it’s pluggable and can connect almost any camera to any Home Hub.  Well, I learned a lot about Scrypted, HomeKit, and Unifi Protect and I hope you learned something too.  And remember if you found anything in this video helpful, don’t forget to like and subscribe.  Thanks for reading and watching!

## Join the conversation

<blockquote class="twitter-tweet" data-dnt="true" data-theme="dark"><p lang="en" dir="ltr">What a week! I found this awesome open source software that let&#39;s me connect and stream almost ANY camera to ANY Smart Home Hub. No more vendor lock! It&#39;s called Scrypted and it&#39;s awesome!<br><br>Check it out!👉<a href="https://t.co/NdvoQydUEo">https://t.co/NdvoQydUEo</a> <a href="https://t.co/RIbFIKNJgp">pic.twitter.com/RIbFIKNJgp</a></p>&mdash; Techno Tim (@TechnoTimLive) <a href="https://twitter.com/TechnoTimLive/status/1657405981456752641?ref_src=twsrc%5Etfw">May 13, 2023</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

## Links

⚙️ See all the hardware I recommend at <https://l.technotim.live/gear>

🚀 Don't forget to check out the [🚀Launchpad repo](https://l.technotim.live/quick-start) with all of the quick start source files
