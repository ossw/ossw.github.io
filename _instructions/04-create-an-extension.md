---
layout: subpage
title: Create an extension
description: how to write custom extension to OSSW android application.
permalink: instructions/create-an-extension/index.html
---

##What is extension?

Extension (or plugin) is an android application that integrates with OSSW android application and exposes some properties or allows to invoke some functions.

You can check a source code of sample extension that is available on [github](https://github.com/ossw/ossw-android-sample-plugin)

##Extension definition
One of the first decisions is to choose a package name for the extension, in the sample plugin it's:
{% highlight text %}
com.althink.android.ossw.plugins.sample
{% endhighlight %}
I will reference this name with %PACKAGE_NAME% placeholder.

Every extension's AndroidManifest.xml should look like this:
{% highlight xml linenos %}
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="%PACKAGE_NAME%">

    <application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme">
        <activity
            android:name=".SamplePluginSettingsActivity"
            android:label="@string/plugin_name">
            <intent-filter>
                <action android:name="%PACKAGE_NAME%.config" />
                <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
        </activity>

        <provider
            android:name=".SamplePluginContentProvider"
            android:authorities="%PACKAGE_NAME%"
            android:exported="true"
            android:syncable="false"
            android:enabled="true"
            android:label="@string/plugin_name"
            ><meta-data android:name="com.althink.android.ossw.plugin" android:value="true"></meta-data>
        </provider>

        <service
            android:name=".SamplePluginService"
            android:exported="true">
            <intent-filter>
                <action android:name="%PACKAGE_NAME%" />
            </intent-filter>
        </service>
    </application>
</manifest>
{% endhighlight %}

There are three important definitions:

- extension service (required)
- extension content provider (required)
- extension settings activity (optional)

##Extension Service

Extension service is a backend service that is responsible for updating extension properties and handling extension function invocations.

The simplest extension service implementation will look like this:

{% highlight java linenos %}
public class SamplePluginService extends Service {
    private final Messenger mMessenger = new Messenger(new OperationHandler());

    @Override
    public IBinder onBind(Intent intent) {
        // perform some initialization
        return mMessenger.getBinder();
    }
    
    private class OperationHandler extends Handler {

        public OperationHandler() {
        }

        @Override
        public void handleMessage(Message msg) {
            super.handleMessage(msg);

            switch (msg.what) {
                // handle function invocation, msg.what contains function identifier
                
                // optional function parameter can be accessed by invoking:
                // String parameter = msg.getData().getString("parameter");
            }
        }
    }
}
{% endhighlight %}

##Extension Content Provider

Extension content provider is a content provider that expose extension API and extension properties (if any). Extension content provider should handle given URIs:

####Extension properties

	content://%PACKAGE_NAME%/api/properties
	
Table with definition of extension properties, every row represents single property, table columns are as follows:

- _id (int) - property identifier,
- name (String) - property technical name (e.g. heartRate)
- description (String) - property description (e.g. Heart rate)
- type (String) - property type:
    - INTEGER
    - FLOAT
    - STRING
    
####Extension functions

	content://%PACKAGE_NAME%/api/functions
	
Table with definition of extension functions, every row represents single function, table columns are as follows:

- _id (int) - function identifier,
- name (String) - function technical name (e.g. nextTrack)
- description (String) - property description (e.g. Next track)

####Extension property values
	content://%PACKAGE_NAME%/properties
	
Table with extension property values, table columns are extension property names and first row values are extension property values. Only one row should be returned.

##Extension Settings Activity

Extension settings activity is a standard android PreferenceActivity that is responsible for extension configuration.

Extension settings activity should handle "%PACKAGE_NAME%.config" intent action, for sample extension it's 

	com.althink.android.ossw.plugins.sample.config
	
Intent filter definition should look like this:

{% highlight xml linenos %}
<intent-filter>
    <action android:name="%PACKAGE_NAME%.config" />
    <category android:name="android.intent.category.DEFAULT" />
</intent-filter>
{% endhighlight %}