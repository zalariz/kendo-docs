---
title: Overview
meta_title: Summary of Kendo UI Notification functionality
meta_description: Find out how to use the Kendo UI Notification widget
slug: gs-web-notification-overview
relatedDocs: api-web-notification
tags: getting-started,web,notification
publish: true
---

# Notification Overview

The **Kendo UI Notification** provides a styled UI widget with arbitrary content, which can provide information to the user on various occasions.

*It is assumed that the reader of this page is familiar with the [fundamental Kendo UI widget concepts](/kendo-ui/getting-started/widgets).*

## Getting Started

Generally, the **Notification** widget can be initialized from any element. If the widget will be used with popup notifications,
or if the notifications will not be appended to the Notification element, then the widget element **will be hidden** as it is assumed that it is not needed for anything particular.

If the **Notification** element will be used to contain static (non-popup) notifications, then its tag is recommended to be one, which allows nesting the elements inside the notifications' template. For example,
inline elements (`span`, `a`, `em`, etc.) cannot contain block elements (`div`, `p`, `ul`, `li`, headings, etc.).

The **Notification** widget provides several built-in notification types - "info", "success", "warning" and "error". The purpose of having different notification types is the ability to
use different templates and looks for each type. The built-in types provide ready-to-use shorthand methods for displaying, as well as built-in templates and styling. The shorthand method names match the listed notification types.
If no type is specified when a notification is shown, "info" is assumed. An unlimited amount of custom notification types and corresponding templates can be defined. See [Templates](#templates).

### Example - Notification initialization

    <span id="notification"></span>
	
	<script>
	$(function(){
		var notificationElement = $("#notification");
        
        // initialize the widget
        notificationElement.kendoNotification();
        
        // get the widget object
        var notificationWidget = notificationElement.data("kendoNotification");
        
        // display a "foo" warning message
        notificationWidget.show("foo", "warning");
        // the above line is equivalent to
        notificationWidget.warning("foo");
        
        // display a "bar" info message
        notificationWidget.show("bar", "info");
        // the above line is equivalent to
        notificationWidget.show("bar");
        // and also to
        notificationWidget.info("bar");
	});
	</script>

## Hiding the notifications

By default, the notifications remain visible for 5 seconds and then disappear. Clicking anywhere on them removes them right away. If this is not intuitive enough for the users, a visible close button can be displayed.
Hide delay is configurable in milliseconds. A zero value prevents automatic hiding.

If needed, automatic hiding by clicking anywhere on the notifications can be disabled. In this case the notifications can be dismissed only with the button, if it is present.
In addition, the ability to hide a notification manually can be postponed. The benefit of this feature is to prevent accidental hiding of notifications, which have just appeared. By default, postponing is disabled.

### Example - managing hiding settings

    <span id="notification"></span>
	
	<script>
	$(function(){
		$("#notification").kendoNotification({
            // hide automatically after 7 seconds
            autoHideAfter: 7000,
            // prevent accidental hiding for 1 second
            allowHideAfter: 1000,
            // show a hide button
            button: true,
            // prevent hiding by clicking on the notification content
            hideOnClick: false
        });
	});
	</script>

## Positioning and stacking

By default, the **Notification** widget creates popups, which overlay the other page content. If no position settings are defined, the first popup will be displayed near the bottom-right corner of the browser viewport.
In this case, subsequent popups will stack upwards. Positioning and stacking can be controlled independently. If no stacking setting is defined, the popups will stack according to the positioning settings
by using common sense (e.g. popups which start at the top of the viewport will stack downwards).

By default, popups are *pinned*, i.e. when the page is scrolled, they do not move. This is achieved by applying a `position:fixed` style to the popups. When the popups are not pinned, they use `position:absolute`.

If the popup content will vary and stacking is likely to occur, it is a good idea to define explicit dimensions, so that the popups are aligned and look better when stacked next to one another.

### Example - position, stacking and size configuration

    <span id="notification"></span>
	
	<script>
	$(function(){
		$("#notification").kendoNotification({
            position: {
                // notification popup will scroll together with the other content
                pinned: false,
                // the first notification popup will appear 30px from the viewport's top and right edge
                top: 30,
                right: 30
            },
            // new notifications will appear below old ones
            stacking: "down",
            // set appropriate size
            width: 300,
            height: 50
        });
	});
	</script>

In addition to popups, the **Notification** widget can also display "static" notifications, which do not overlay other elements, but instead take part in the so-called *normal flow* of the page content. In this case
positioning settings do not make sense and are ignored. Stacking can be downwards (default) or upwards. Static notifications are displayed, if a target container is specified.

### Example - static configuration

    <div id="notification"></div>
	
	<script>
	$(function(){
		$("#notification").kendoNotification({
            // insert all notifications to the widget's originating element
            appendTo: "#notification",
            // new notifications will appear above old ones
            stacking: "up"
        });
	});
	</script>

A given **Notification** instance can display either popup or static notifications, not both at the same time. The positioning and stacking settings are also assumed to remain unchanged after widget initialization.
If any of these must be changed on the fly, the `setOptions` method must be used, which all Kendo UI widgets have.

There may be cases when the popup notifications appear too quickly or become too much on the screen and there is no available space for more. In this case the subsequent popups will appear outside of the visible
viewport area and will be inaccessible (if pinned). If such scenarios are likely to occur, it is recommended to consider using shorter hide delay or static notifications, for better usability.

## Templates

*This documentation section assumes that you are familiar with [Kendo UI templates](/kendo-ui/getting-started/framework/templates/overview)*.

The **Notification** widget allows configuring multiple templates. Each template will be used together with its corresponding notification type (either build-in or custom).

### Example - using templates

    <span id="notification"></span>
	
	<script>
	$(function(){
        var notificationElement = $("#notification");
        
		notificationElement.kendoNotification({
            templates: {
                // define a custom template for the built-in "warning" notification type
                warning: "<div class='myWarning'>Warning: #= myMessage #</div>",
                // define a template for the custom "timeAlert" notification type
                timeAlert: "<div class='myAlert'>System alert generated at #= time # : #= myMessage #</div>"
            }
        });
        
        var n = notificationElement.data("kendoNotification");
        
        // show a warning message using the built-in shorthand method
        n.warning({
            myMessage: "some warning message here"
        });
        
        // show a "myType" message using the default show() method
        n.show({
            time: new Date().toLocaleTimeString(),
            myMessage: "Server will be restarted."
        }, "timeAlert");
	});
	</script>

## Accessing the Notification instance

Similar to all other Kendo UI widgets, an existing **Notification** instance is accessed via the `.data("kendoNotification")` jQuery method, executed by the jQuery object of the originating element.
See [Getting reference to a Kendo UI widget](/kendo-ui/getting-started/widgets#getting-reference-to-a-kendo-ui-widget).

For further reading and related information, please refer to the [Notification API](/kendo-ui/api/web/notification/).