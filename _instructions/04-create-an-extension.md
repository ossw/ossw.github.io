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
One of the first decisions is to choose a package name for the extension (in the sample plugin it's com.althink.android.ossw.plugins.sample). I will reference this name with %PACKAGE_NAME% placeholder.

Every extension AndroidManifest.xml should look like this:
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

Extension service is a backend service that is responsible for fetching extension properties and handling extension function invocations.

Extension content provider is a content provider that expose extension API and extension properties (if any). Extension content provider should be handle given URIs:

- content://%PACKAGE_NAME%/api/properties - table with definition of extension properties, table columns are as follows:
	- _id (int) - property id,
    - name (String) - property technical name (e.g. heartRate)
    - description (String) - property description (e.g. Heart rate)
    - type (String) - property type:
    	- INTEGER
    	- FLOAT
    	- STRING
- content://%PACKAGE_NAME%/api/functions - table with definition of extension functions, table columns are as follows:
	- _id (int) - property id,
    - name (String) - property technical name (e.g. heartRate)
    - description (String) - property description (e.g. Heart rate)
- content://%PACKAGE_NAME%/properties - table with extension property values, table columns are extension property names and values are extension property values.