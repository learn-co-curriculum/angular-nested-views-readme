# Nested views and dynamic URLs

## Overview

As mentioned earlier, `uiRouter` offers us nested views. These are extremely powerful and allow us to have views inside views!

## Objectives

- Describe dynamic views
- Write a dynamic route using ui-router
- Assign relevant template

## Nested View

Imagine that we have a normal setup for a web page - a header, a footer, and everything in between those change depending on what page we are on. This is great for most cases - we can swap between different pages such as home, about, contact, etc.

But what if we want to have a settings page, with multiple different pages for us to look at? We might want to be able to edit profile information, but also change our notification preferences or even have a "delete your account page".

We'd setup our settings route, and this would be displayed in our main section where all the routes go. What now? How do we get all these different pages rendered *inside* our settings page?

We could just create a tabs component that changes what we can see depending on what tab has been clicked, but it means our URL will always stick at `/settings` rather than changing between `/settings/notifications` and `/settings/user`.

This is where nested views come in. As mentioned before with states, a state can have multiple children states. These will then be rendered inside of our parent state - but we must make sure that we use the `ui-view` directive inside our parent state too.

Our previous setup might've looked like this:

```js
angular
	.module('app', ['ui.router'])
	.config(function ($stateProvider) {
		$stateProvider
			.state('settings', {
				url: '/settings',
				templateUrl: 'views/settings.html',
				controller: 'SettingsController'
			})
			.state('settingsUser', {
                url: '/settings/user',
                templateUrl: 'views/settings/user.html',
                controller: 'UserSettingsController'
            })
            .state('settingsNotifications', {
                url: '/settings/notifications',
                templateUrl: 'views/settings/notifications.html',
                controller: 'NotificationsSettingsController'
            })
	});
```

Let's setup our settings example in `uiRouter`.

```js
angular
	.module('app', ['ui.router'])
	.config(function ($stateProvider) {
		$stateProvider
			.state('settings', {
				url: '/settings',
				templateUrl: 'views/settings.html',
				controller: 'SettingsController'
			})
			.state('settings.user', {
                url: '/user',
                templateUrl: 'views/settings/user.html',
                controller: 'UserSettingsController'
            })
            .state('settings.notifications', {
                url: '/notifications',
                templateUrl: 'views/settings/notifications.html',
                controller: 'NotificationsSettingsController'
            })
	});
```

We're not saving much code here - but by having child states it means we can render those states *inside* the parent state - the first example of code would replace the settings state with either the notifications or user settings state.

You'll notice how we state the parent controller in the name of the state. We put the parent name first, then a dot, and then the actual state name. This tells `uiRouter` that `settings` is the parent state.

You'll also notice on our `settings.user` and `settings.notifications` states, we haven't put the URLs as `/settings/user` and `/settings/notifications` respectively - this is because they automatically inherit the URL from their parent state. We are simply adding on `/user` and `/notifications` to `/settings`.

Now, inside of our `settings.html` we need to make sure we include the `ui-view` directive. This is where the templates for our users and notifications pages will be rendered.

```html
<div class="settings">
	<h3>Settings</h3>

	<a ui-sref="settings.user">User Settings</a> - <a ui-sref="settings.notifications">Notification Settings</a>

    <div ui-view></div>
</div>
```

Now our user and notifications pages will be rendered underneath the settings navigation - neat!

Also, when we go to `/settings/user`, `uiRouter` automatically detects that our parent state is `settings`, and puts this into our main applications `<div ui-view>`. It'll then sort out the children states, so it renders the page correctly.

It's important to note that when we load a child state, such as `settings.user`, it'll resolve all the parent state's resolves. If we had a resolve on `settings` to fetch settings data, going to our `settings.user` state will load that resolve on the `settings` state.
<p data-visibility='hidden'>View <a href='https://learn.co/lessons/angular-nested-views-readme'>Angular Nested Views </a> on Learn.co and start learning to code for free.</p>

<p class='util--hide'>View <a href='https://learn.co/lessons/angular-nested-views-readme'>Angular Nested Views </a> on Learn.co and start learning to code for free.</p>
