EggTimer - Final Code 
============================================================================

Solution code for Advanced Android with Kotlin Codelab 

Introduction
------------

EggTimer is a timer app for cooking eggs.
You can start and stop the timer, choose different cooking intervals.. 

In this codelab, working from this starter app, you:

* Add notitications to the eggtimer app.
* Use channels and importance for the app notifications. 
* Customize and style the notifications.


Pre-requisites
--------------

You should be familiar with:

* Services, AlarmManager, Broadcast Receivers.


Getting Started
---------------

1. Download
2. Swtich to start branch
3. Run the app.


Solution implementation
------------------------
Inside an extension function like this
`fun NotificationManager.sendNotification(messageBody: String, applicationContext: Context) {...}`
1. get an instance of NotificationCompat.Builder

```
val builder = NotificationCompat.Builder(
        applicationContext,
        applicationContext.getString(R.string.egg_notification_channel_id)
)
```

2. set title, text and icon to builder

```
   .setSmallIcon(R.drawable.cooked_egg)
   .setContentTitle(applicationContext.getString(R.string.notification_title))
   .setContentText(messageBody)
```
3. Deliver the notification
`notify(NOTIFICATION_ID, builder.build())`

4. From the **viewModel** get an instance of NotificationManager and call sendNotification
```
val notificationManager = ContextCompat.getSystemService(
    app, 
    NotificationManager::class.java
) as NotificationManager
                notificationManager.sendNotification(app.getString(R.string.timer_running), app)
```

Notification Channels
---------------------
We need to create notification channels for the app, so we can group and clasify the notifications according to their importance. The developer set the initial configuration and the user overrite it if necessary.
1. We can create channel in the fragment or activity class as follows; `channelId` and `channelName` are Strings
```
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
        val notificationChannel = NotificationChannel(
            channelId,
            channelName,
            // TODO: Step 2.4 change importance
            NotificationManager.IMPORTANCE_LOW
        )
        // TODO: Step 2.6 disable badges for this channel

        notificationChannel.enableLights(true)
        notificationChannel.lightColor = Color.RED
        notificationChannel.enableVibration(true)
        notificationChannel.description = "Time for breakfast"

        val notificationManager = requireActivity().getSystemService(
            NotificationManager::class.java
        )
        notificationManager.createNotificationChannel(notificationChannel)
    }
```
    

