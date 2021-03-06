# OpenShift AeroGear Push Server Cartridge

**DEPRECATED:** This is based on OpenShift 2 and JBoss AS7, and is no longer supported. Users are asked to check out the AeroGear community template for Openshift 3: https://github.com/aerogear/aerogear-unifiedpush-server/tree/master/openshift  

Provides the _AeroGear UnifiedPush Server_ running on top of JBoss Application Server on OpenShift and embeds the _AeroGear SimplePush Server_ within JBoss Application Server on OpenShift. 

|                 | Project Info  |
| --------------- | ------------- |
| License:        | Apache License, Version 2.0  |
| Build:          | Openshift  |
| Documentation:  | https://aerogear.org/push/  |
| Issue tracker:  | https://issues.jboss.org/browse/AGPUSH  |
| Mailing lists:  | [aerogear-users](http://aerogear-users.1116366.n5.nabble.com/) ([subscribe](https://lists.jboss.org/mailman/listinfo/aerogear-users))  |
|                 | [aerogear-dev](http://aerogear-dev.1069024.n5.nabble.com/) ([subscribe](https://lists.jboss.org/mailman/listinfo/aerogear-dev))  |

The [AeroGear UnifiedPush Server](https://github.com/aerogear/aerogear-unified-push-server) is a server that allows sending push notifications to different (mobile) platforms. The initial version of the server supports [Apple’s APNs](http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9), [Google Cloud Messaging](http://developer.android.com/google/gcm/index.html) and [Mozilla’s SimplePush](https://wiki.mozilla.org/WebAPI/SimplePush).

The [AeroGear SimplePush Server](https://github.com/aerogear/aerogear-simplepush-server) is a Java server side implementation of Mozilla's [SimplePush Protocol](https://wiki.mozilla.org/WebAPI/SimplePush/Protocol) that describes a JavaScript API and a protocol which allows backend/application developers to send notification messages to their web applications. 

### Installation
The AeroGear Push Server cartridge defaults to using MySQL. When creating your application, you'll also want to add the MySQL cartridge:

```
rhc app create --no-git <APP> https://cartreflect-claytondev.rhcloud.com/reflect?github=aerogear/openshift-origin-cartridge-aerogear-push-jboss7
```

### Getting started with the AeroGear UnifiedPush Server

#### Administration Console

Once the server is running access it via ```https://{APP}-{NAMESPACE}.rhcloud.com/ag-push```. Check the Administration console [user guide](http://aerogear.org/docs/guides/AdminConsoleGuide/) for more information on using the console.

#### Login

Temporarily, there is an "admin:123" user.  On _first_ login,  you will need to change the password.

### Getting started with the AeroGear SimplePush Server

#### Client connections

For _secured_ connections, client applications should connect to the _AeroGear SimplePush Server_ via ```https://{APP}-{NAMESPACE}.rhcloud.com:8443/simplepush```.

For _unsecured_ connections, client applications can connect to the _AeroGear SimplePush Server_ via ```http://{APP}-{NAMESPACE}.rhcloud.com:8000/simplepush```.

**NOTE:** It is recommended that you always use _secured_ connections.

#### Known issue with an idled OpenShift application and WebSocket requests

Currently, if your AeroGear Push Server application is [idled by OpenShift](https://www.openshift.com/faq/what-happens-if-my-application-is-not-used-for-a-long-time), attempts to establish a WebSocket connection to the _AeroGear SimplePush Server_ will not wake up your idled application. This is a known issue (see [AEROGEAR-1296](https://issues.jboss.org/browse/AEROGEAR-1296)) and will be fixed in a future release of OpenShift Online. Note that requests to your application on port 80 (i.e., ```http://{APP}-{NAMESPACE}.rhcloud.com```) will wake up your idled application.


### Template Repository Layout

    .openshift/        Location for OpenShift specific files
      action_hooks/    See the Action Hooks documentation [1]
      markers/         See the Markers section [2]

\[1\] [Action Hooks documentation](https://github.com/openshift/origin-server/blob/master/node/README.writing_applications.md#action-hooks)
\[2\] [Markers](#markers)


### Environment Variables

The `aerogear-push` cartridge provides several environment variables to reference for ease
of use:

    OPENSHIFT_AEROGEAR_PUSH_IP                         The IP address used to bind JBossAS
    OPENSHIFT_AEROGEAR_PUSH_HTTP_PORT                  The JBossAS listening port
    OPENSHIFT_AEROGEAR_PUSH_TOKEN_KEY                  The token key for the SimplePush Server
    OPENSHIFT_AEROGEAR_PUSH_CLUSTER_PORT               
    OPENSHIFT_AEROGEAR_PUSH_MESSAGING_PORT             
    OPENSHIFT_AEROGEAR_PUSH_MESSAGING_THROUGHPUT_PORT  
    OPENSHIFT_AEROGEAR_PUSH_REMOTING_PORT              
    JAVA_OPTS_EXT                                      Appended to JAVA_OPTS prior to invoking the Java VM

For more information about environment variables, consult the
[OpenShift Application Author Guide](https://github.com/openshift/origin-server/blob/master/node/README.writing_applications.md).

### Markers

Adding marker files to `.openshift/markers` will have the following effects:

    enable_jpda          Will enable the JPDA socket based transport on the java virtual
                         machine running the JBoss AS 7 application server. This enables
                         you to remotely debug code running inside the JBoss AS 7
                         application server.

    java7                Will run JBossAS with Java7 if present. If no marker is present
                         then the baseline Java version will be used (currently Java6)

## Documentation

For more details about the current release, please consult [our documentation](https://aerogear.org/push/).

## Development

If you would like to help develop AeroGear you can join our [developer's mailing list](https://lists.jboss.org/mailman/listinfo/aerogear-dev), join #aerogear on Freenode, or shout at us on Twitter @aerogears.

Also takes some time and skim the [contributor guide](http://aerogear.org/docs/guides/Contributing/)

## Questions?

Join our [user mailing list](https://lists.jboss.org/mailman/listinfo/aerogear-users) for any questions or help! We really hope you enjoy app development with AeroGear!

## Found a bug?

If you found a bug please create a ticket for us on [Jira](https://issues.jboss.org/browse/AGPUSH) with some steps to reproduce it.
