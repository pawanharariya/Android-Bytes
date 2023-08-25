## Android Bytes ##

Android Bytes is a simple app that showcases using
[Room](https://developer.android.com/topic/libraries/architecture/room) and a Repository to create
an offline cache. It fetches Android Dev Byte videos' information from the server to display and
provides an intent that launches the corresponding video on Youtube.

In addition, it also uses
[WorkManager](https://developer.android.com/topic/libraries/architecture/workmanager) for scheduling
a daily background data sync between local database and server.

## App Preview ##

<img src="https://github.com/pawanharariya/Android-Bytes/assets/43620548/8a8afcea-bc39-4c1d-b006-0bf05be43ca0" alt="Android Bytes App Preview 1" width=200/>
<img src="https://github.com/pawanharariya/Android-Bytes/assets/43620548/ed4e95d9-275d-4ff3-9698-fa49b1bb4081" alt="Android Bytes App Preview 2" width=200/>

## Cache ##

Caching is copying data and storing it closer to the place it will be used for faster access in
future uses. For example, browser caches every page web page visited on local file system, so that
it is loaded faster the next time the page is visited.

We store each on the file system. Every Android app has access to a folder it can use to store files
and data.

### Cache Invalidation ###

Cache invalidation means knowing when the cached data is no longer correct. For example, when the
data on the server is itself changed, the cached data is of no use. Network requests over HTTP have
features to deal with this and we can enable network caching using Retrofit. We can also create
offline cache using SQL database.

## Repository ##

Repository pattern is common pattern used to provide unified view of the data from several sources.
For example, data coming from network and database are combined in the repository. The users of the
repository don't have to care where the data comes from. Repository only provides APIs to the data
sources.

**Note:** Repository is also responsible for managing data in cache.

## WorkManager ##

Background works like uploading logs, process data, upload metrics or pre-fetch content may happen
when app is in background or even not running. WorkManager is a Jetpack library that helps to
perform tasks when the app is in background or even not running. It can also be used to schedule
background work to be done periodically. We don't tell WorkManager how to do work, instead under
what conditions to do the work. Constraints can be when the device is charging, device is idle or is
connected to wifi.

### Application Class ###

We schedule the background work in the Application Class. It is the main object that the operating
system uses to interact with the app. Application's `onCreate()` runs every time the app is launched
and it has to run before any screen is shown. It lot of work is done in `onCreate()` it slows the
app's loading time. We use Coroutines to move initialisation and schedule background work out
of `onCreate()`.

### Scheduling the Work ###

WorkManager schedules work with a work request. Work request can be periodic or one-time. To
schedule the work we use `enqueue` method on WorkManager. Works are differentiated by WorkName, and
we use `ExistingPeriodicWorkPolicy` to resolve when two works with same name are enqueued. We can
keep the previous work and discard the newly arrived work request or replace the previous request
with new one.