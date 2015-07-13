---
layout: subpage
title: Create a watchset
description: how to create custom screens and assign actions on them.
permalink: instructions/create-a-watchset/index.html
---

##What is watchset?

Watchset is a JSON file with definition of screens and actions available on them. You can define up to 255 screens in one watchset.
Every screen contains controls and actions assigned to the events. 

Here you can see the simplest watchset that contains only one screen. This screen doesn't have any controls and handles only one event - closes the watchset when back button is pressed.

{% highlight json linenos %}
{
	"type": "watchset",
	"name": "sample watchset",
	"apiVersion": 1,
	"data": {
		"screens": [
			{
				"id": "sample screen",
				"controls": [
				],
				"actions": {
					"button_back_short": {"action": "close"}
				}
			}
		]
	}
}
{% endhighlight %}

##File properties

Every file with watchset definition should contain those parameters:

- type - should be set to "watchset"
- name - user defined name that will show on a list when watchset is imported
- apiVersion - reserved for future use, should be set to "1"
- data - watchset properties

##Watchset properties

At this stage watchset data contains only list of screens. In future versions there will be also possibility to define static content (images, fonts) that will be used on the screens. To define multiple screens you have to separate them with a comma.

{% highlight json linenos %}
{
	"type": "watchset",
	"name": "sample watchset",
	"apiVersion": 1,
	"data": {
		"screens": [
			{
				"id": "first screen",
				"controls": [
				],
				"actions": {
					"button_back_short": {"action": "close"}
				}
			},
			{
				"id": "second screen",
				"controls": [
				],
				"actions": {
					"button_back_short": {"action": "close"}
				}
			}
		]
	}
}
{% endhighlight %}

##Screen properties

Every screen should contain:

- id - identifier of the screen (used in places where some event triggers screen change action)
- controls - list of controls that should be shown on this screen
- actions - list of event to action mapping

One screen may contain up to 255 controls and unlimited number of unique actions (but you can assign only one action to one event). Here you can see simple screen that contains two controls (one that shows hour and the second one with minutes). There are three event to action mappings: 

- the first one toggles screen colors when the up button is long pressed
- the second one toggles backlight when the back button is long pressed
- the last one close the watchset when the back button is short pressed

{% highlight json linenos %}
{
	"type": "watchset",
	"name": "sample watchset",
	"apiVersion": 1,
	"data": {
		"screens": [
			{
			    "id": "sample watchface",
			    "controls": [
			        {
			    		"type": "number",
			    		"numberRange": "0-99",
			    		"position": {"x": 4, "y": 4},
			    		"style": {"type": "generated", "thickness": 8, "width": 66, "height": 76, "space": 4, "leftPadded": true},
			    		"source": {"type": "internal", "property": "hour"}
			    	},
			    	{
			    		"type": "number",
			    		"numberRange": "0-99",
			    		"position": {"x": 4, "y": 85},
			    		"style": {"type": "generated", "thickness": 6, "width": 66, "height": 76, "space": 4, "leftPadded": true},
			    		"source": {"type": "internal", "property": "minutes"}
			    	}
			    ],
				"actions": {
					"button_up_long": {"action": "toggleColors"},
					"button_back_long": {"action": "toggleBacklight"},
					"button_back_short": {"action": "close"}
				}
			}
		]
	}
}
{% endhighlight %}

##Screen controls

There are few types of controls that can be added to the screen. Every type has it's own properties.

###Number

Number control shows a numeric value. This value can be integer or decimal number. Sample definition of number control looks like this:

{% highlight json linenos %}
{
	"type": "number",
	"numberRange": "0-99",
	"position": {"x": 4, "y": 4},
	"style": {"type": "generated", "thickness": 8, "width": 66, "height": 76, "space": 4, "leftPadded": true},
	"source": {"type": "internal", "property": "hour"}
}
{% endhighlight %}

Number control properties are as follows:

- type - control type, should be set to "number"
- numberRange - number range, possible values are:
	- 0-9
	- 0-19
	- 0-99
	- 0-199
	- 0-999
	- 0-1999
	- 0-9999
	- 0-19999
	- 0-99999
	- 0-9.9
	- 0-19.9
	- 0-99.9
	- 0-199.9
	- 0-999.9
	- 0-1999.9
	- 0-9999.9
	- 0-19999.9
	- 0-99999.9
	- 0-9.99
	- 0-19.99
	- 0-99.99
	- 0-199.99
	- 0-999.99
	- 0-1999.99
	- 0-9999.99
	- 0-19999.99
	- 0-99999.99
- position - x and y coordinates of top left corner of this control
- style - some properties that define how this control should look like, at this stage only generated digits are supported.
	- thickness - digit thickness in pixels
	- width - single digit width
	- height - single digit height
	- space - space in pixels between digits
	- leftPadded - flag that informs if number should be zero padded (1.9 when false or 001.9 when true for 0-999.9 range), this property is skipped for ranges that start with one like 0-199.
- source - data source (check data source section)

###Text

Text control renders a text using given font. 

{% highlight json linenos %}
{
	"type": "text",
	"position": {"x": 1, "y": 10},
	"size": {"width": 143, "height": 27},
	"font": {"type": "builtin", "name": "normalRegular"},
	"style": {"horizontalAlign": "center"},
	"source": {"type": "extension", "extensionId": "com.althink.android.ossw.plugins.sample", "property": "stringParam"}
}
{% endhighlight %}

Text control properties are as follows:

- type - control type, should be set to "text"
- position - x and y coordinates of top left corner of this control
- size - width and height of this control (text is not rendered outside of this range)
- font - font time, currently only builtin fonts are supported:
	- normalRegular
	- normalBold
	- optionNormal
	- optionBig
- style - text style:
	- horizontalAlign - text horizontal align:
		- left
		- right
		- center
- source - data source (check data source section)

###Progress

Progress control renders a horizontal progress bar. 

{% highlight json linenos %}
{
	"type": "progress",
	"maxValue": 60,
	"position": {"x": 0, "y": 165},
	"size": {"width": 144, "height": 2},
	"style": {"border": 0},
	"source": {"type": "internal", "property": "seconds"}
}
{% endhighlight %}

Progress control properties are as follows:

- type - control type, should be set to "progress"
- maxValue - maximum value that represents 100% of progress
- position - x and y coordinates of top left corner of this control
- size - width and height of this control
- style - control style:
	- border - renders border of given number of pixels around the progress bar
- source - data source (check data source section)

##Data source

Every control definition contains a "source" property that defines what value should be shown. There are three types of data source:

- internal - internal data, available in every mode (peripheral, airplane, central)
- extension - data from an android application extension (only for peripheral mode)
- sensor - data from sensor connected over BLE (only for central mode)


###Internal data source

Internal data source is available in every mode. Sample definition looks like this:

{% highlight json linenos %}
"source": {"type": "internal", "property": "hour"}
{% endhighlight %}

Extension data source properties are as follows:

- type - should be set to "internal"
- property - internal property name

Available internal properties:

- hour - current hour
- minutes - current minutes
- seconds - current seconds
- dayOfMonth - current day of month
- month - current month
- year - current year
- batteryLevel - current battery level (100 is a max)


###Extension data source

Extension data source is available only in peripheral mode. Sample definition looks like this:

{% highlight json linenos %}
"source": {"type": "extension", "extensionId": "com.althink.android.ossw.plugins.musicplayer", "property": "artist"}
{% endhighlight %}

Extension data source properties are as follows:

- type - should be set to "extension"
- extensionId - extension identifier
- property - extension property name (for available names check extension documentation)


###Sensor data source

Sensor data source is available only in central mode. Sample definition looks like this:

{% highlight json linenos %}
"source": {"type": "sensor", "property": "heartRate"}
{% endhighlight %}

Sensor data source properties are as follows:

- type - should be set to "sensor"
- property - sensor property name


Available sensor properties:

- heartRate - current heart rate from BLE HR sensor

##Event to action mapping

You can assign actions to given events. Sample actions definition looks like this:

{% highlight json linenos %}
{
"actions": {
	"button_up_long": {"action": "toggleColors"},
	"button_back_long": {"action": "toggleBacklight"},
	"button_back_short": {"action": "close"}
}
{% endhighlight %}

Available events:

- button_up_short - UP button short pressed
- button_select_short - SELECT button short pressed
- button_down_short - DOWN button short pressed
- button_back_short - BACK button short pressed
- button_up_long - UP button long pressed
- button_select_long - SELECT button long pressed
- button_down_long - DOWN button long pressed
- button_back_long - BACK button long pressed

Available actions:

- toggleBacklight - toggle backlight
- toggleColors - toggle colors
- showScreen - show watchset screen identified by screenId property
{% highlight json linenos %}
{"action": "showScreen", "screenId": "second screen"}
{% endhighlight %}
- extensionFunction - invoke function from android application extension, check extension documentation for available functions
{% highlight json linenos %}
{"action": "extensionFunction",  "extensionId": "com.althink.android.ossw.plugins.musicplayer", "function": "nextTrack"}
{% endhighlight %}
- settings - show settings
- close - close a watchset
