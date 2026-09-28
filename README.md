# Practical 4: Android Alarm Application

## AIM

Create an **Android Alarm application** using **Service** and **BroadcastReceiver**.

## Practical Requirements

The application performs the following tasks:

1. Create `MainActivity` according to the given UI design.
2. Create an `AlarmBroadcastReceiver` class.
3. Create an `AlarmService` class.
4. Add `android.permission.SCHEDULE_EXACT_ALARM` permission in the `AndroidManifest.xml` file.
5. Allow the user to select an alarm time.
6. Trigger the alarm at the selected time.
7. Play an alarm sound using `MediaPlayer`.

## Concepts Studied

* BroadcastReceiver
* Service
* TextClock
* TimePickerDialog
* Calendar Class
* SimpleDateFormat Class
* PendingIntent
* AlarmManager
* `getSystemService()`
* `sendBroadcast()`
* MediaPlayer
* `startService()`
* `stopService()`
* `Intent.getStringExtra()`
* `Intent.putStringExtra()`
* MaterialCardView

## Application Components

### 1. MainActivity

`MainActivity` provides the main user interface of the alarm application.

It contains components such as:

* Current time display using `TextClock`
* Alarm time display
* Time picker
* Set Alarm button
* Stop Alarm button
* MaterialCardView for UI design

The user selects a desired alarm time using `TimePickerDialog`.

### 2. AlarmManager

`AlarmManager` is used to schedule the alarm for a specific time.

Example:

```kotlin
val alarmManager = getSystemService(ALARM_SERVICE) as AlarmManager
```

A `PendingIntent` is created to trigger the `AlarmBroadcastReceiver` when the scheduled time is reached.

### 3. PendingIntent

`PendingIntent` allows the Android system to execute an Intent at a later time.

Example:

```kotlin
val pendingIntent = PendingIntent.getBroadcast(
    this,
    0,
    intent,
    PendingIntent.FLAG_UPDATE_CURRENT or PendingIntent.FLAG_IMMUTABLE
)
```

### 4. BroadcastReceiver

`AlarmBroadcastReceiver` receives the broadcast when the alarm time is reached.

The receiver starts the alarm service.

Example:

```kotlin
override fun onReceive(context: Context, intent: Intent) {
    val serviceIntent = Intent(context, AlarmService::class.java)
    context.startService(serviceIntent)
}
```

### 5. AlarmService

`AlarmService` runs the alarm operation in the background.

It uses `MediaPlayer` to play the alarm sound.

Example:

```kotlin
val mediaPlayer = MediaPlayer.create(this, R.raw.alarm)
mediaPlayer.start()
```

The service can be stopped when the user presses the **Stop Alarm** button.

```kotlin
stopService(Intent(this, AlarmService::class.java))
```

## Setting Alarm Time

The user selects the hour and minute using `TimePickerDialog`.

```kotlin
TimePickerDialog(
    this,
    { _, hour, minute ->
        // Set selected alarm time
    },
    currentHour,
    currentMinute,
    false
).show()
```

The selected time can be stored using the `Calendar` class.

```kotlin
val calendar = Calendar.getInstance()

calendar.set(
    Calendar.HOUR_OF_DAY,
    hour
)

calendar.set(
    Calendar.MINUTE,
    minute
)

calendar.set(
    Calendar.SECOND,
    0
)
```

## Date and Time Formatting

`SimpleDateFormat` can be used to display the selected alarm time in a readable format.

```kotlin
val format = SimpleDateFormat("hh:mm a", Locale.getDefault())
val alarmTime = format.format(calendar.time)
```

## Intent Data

Data can be passed between components using Intent extras.

### Put Data

```kotlin
intent.putExtra("alarm_time", alarmTime)
```

### Get Data

```kotlin
val alarmTime = intent.getStringExtra("alarm_time")
```

## Manifest Permission

The application requires permission to schedule exact alarms.

Add the following permission to `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.SCHEDULE_EXACT_ALARM" />
```

The required components should also be declared in the manifest:

```xml
<receiver
    android:name=".AlarmBroadcastReceiver"
    android:exported="false" />

<service
    android:name=".AlarmService"
    android:exported="false" />
```

## Alarm Sound

The alarm sound file `alarm.mp3` should be placed inside:

```text
app/src/main/res/raw/
```

Example:

```text
res
└── raw
    └── alarm.mp3
```

The sound can then be accessed using:

```kotlin
R.raw.alarm
```

## Working Flow

```text
User selects alarm time
          ↓
      MainActivity
          ↓
     AlarmManager
          ↓
     PendingIntent
          ↓
AlarmBroadcastReceiver
          ↓
     AlarmService
          ↓
       MediaPlayer
          ↓
     Alarm Sound Plays
```

## Important Classes

| Class                    | Purpose                                        |
| ------------------------ | ---------------------------------------------- |
| `MainActivity`           | Provides the user interface and sets the alarm |
| `AlarmManager`           | Schedules the alarm                            |
| `PendingIntent`          | Executes the Intent at the scheduled time      |
| `AlarmBroadcastReceiver` | Receives the alarm broadcast                   |
| `AlarmService`           | Runs the alarm operation                       |
| `MediaPlayer`            | Plays the alarm sound                          |
| `Calendar`               | Sets the alarm date and time                   |
| `TimePickerDialog`       | Allows the user to select the alarm time       |
| `SimpleDateFormat`       | Formats date and time                          |

## Result

The Android Alarm application successfully schedules an alarm using **AlarmManager**, receives the scheduled broadcast using **BroadcastReceiver**, starts an **AlarmService**, and plays the alarm sound using **MediaPlayer**.

## Conclusion

This practical demonstrates how **Service**, **BroadcastReceiver**, **AlarmManager**, **PendingIntent**, and **MediaPlayer** work together to create an Android alarm application. It provides an understanding of background services, broadcast communication, alarm scheduling, and handling date and time in Android.
