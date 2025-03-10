---
id: material-top-tab-navigator
title: createMaterialTopTabNavigator
sidebar_label: createMaterialTopTabNavigator
---

A material-design themed tab bar on the top of the screen that lets you switch between different routes by tapping the route or swiping horizontally. Transitions are animated by default. Screen components for each route are mounted immediately.

This wraps [`react-native-tab-view`](https://github.com/react-native-community/react-native-tab-view).

To use this navigator, you need to install [`@react-navigation/material-top-tabs`](https://github.com/react-navigation/navigation-ex/tree/master/packages/material-top-tabs):

```sh
yarn add @react-navigation/core@next @react-navigation/material-top-tabs@next react-native-tab-view
```

Now we need to install [`react-native-gesture-handler`](https://github.com/kmagiera/react-native-gesture-handler) and [`react-native-reanimated`](https://github.com/kmagiera/react-native-reanimated).

If you are using Expo, to ensure that you get the compatible versions of the libraries, run:

```sh
expo install react-native-gesture-handler react-native-reanimated
```

If you are not using Expo, run the following:

```sh
yarn add react-native-reanimated react-native-gesture-handler
```

If you are using Expo, you are done. Otherwise, continue to the next steps.

To complete the linking on iOS, make sure you have [Cocoapods](https://cocoapods.org/) installed. Then run:

```sh
cd ios
pod install
cd ..
```

To finalize installation of `react-native-gesture-handler` for Android, be sure to make the necessary modifications to `MainActivity.java`:

```diff
package com.reactnavigation.example;

import com.facebook.react.ReactActivity;
+ import com.facebook.react.ReactActivityDelegate;
+ import com.facebook.react.ReactRootView;
+ import com.swmansion.gesturehandler.react.RNGestureHandlerEnabledRootView;

public class MainActivity extends ReactActivity {

  @Override
  protected String getMainComponentName() {
    return "Example";
  }

+  @Override
+  protected ReactActivityDelegate createReactActivityDelegate() {
+    return new ReactActivityDelegate(this, getMainComponentName()) {
+      @Override
+      protected ReactRootView createRootView() {
+       return new RNGestureHandlerEnabledRootView(MainActivity.this);
+      }
+    };
+  }
}
```

Finally, run `react-native run-android` or `react-native run-ios` to launch the app on your device/simulator.

## API Definition

To use this tab navigator, import it from `@react-navigation/material-top-tabs`:

```js
import { createMaterialTopTabNavigator } from '@react-navigation/material-top-tabs';

const Tab = createMaterialTopTabNavigator();

function App() {
  return (
    <Tab.Navigator>
      <Tab.Screen name="Feed" component={Feed} />
      <Tab.Screen name="Article" component={Article} />
    </Tab.Navigator>
  );
}
```

> For a complete usage guide please visit [Tab Navigation](https://reactnavigation.org/docs/tab-based-navigation.html)

### Props

The `Tab.Navigator` component accepts following props:

#### `initialRouteName`

The name of the route to render on first load of the navigator.

#### `screenOptions`

Default options to use for the screens in the navigator.

#### `backBehavior`

Behavior of back button handling.

- `initialRoute` to return to initial tab
- `order` to return to previous tab (in the order they are shown in the tab bar)
- `history` to return to last visited tab
- `none` to not handle back button

#### `tabBarPosition`

Position of the tab bar in the tab view. Possible values are `'top'` and `'bottom'`. Defaults to `'top'`.

#### `lazy`

Boolean indicating whether to lazily render the scenes. By default all scenes are rendered to provide a smoother swipe experience. But you might want to defer the rendering of unfocused scenes until the user sees them. To enable lazy rendering, set `lazy` to `true`.

When you enable `lazy`, the unfocused screens will usually take some time to render when they come into focus. You can use the `renderLazyPlaceholder` prop to customize what the user sees during this short period.

#### `lazyPreloadDistance`

When `lazy` is enabled, you can specify how many adjacent routes should be preloaded with this prop. This value defaults to `0` which means lazy pages are loaded as they come into the viewport.

#### `lazyPlaceholderComponent`

React component to render for routes that haven't been rendered yet. Receives an object containing the route as the argument. The `lazy` prop also needs to be enabled.

This view is usually only shown for a split second. Keep it lightweight.

By default, this renders `null`.

#### `removeClippedSubviews`

Boolean indicating whether to remove invisible views (such as unfocused screens) from the native view hierarchy to improve memory usage. Defaults to `false`.

**Note**: Don't enable this on iOS where this is buggy and views don't re-appear.

#### `keyboardDismissMode`

String indicating whether the keyboard gets dismissed in response to a drag gesture. Possible values are:

- `'auto'` (default): the keyboard is dismissed when the index changes.
- `'on-drag'`: the keyboard is dismissed when a drag begins.
- `'none'`: drags do not dismiss the keyboard.

#### `swipeEnabled`

Boolean indicating whether to enable swipe gestures. Swipe gestures are enabled by default. Passing `false` will disable swipe gestures, but the user can still switch tabs by pressing the tab bar.

#### `swipeVelocityImpact`

Determines how relevant is a velocity while calculating next position while swiping. Defaults to `0.2`.

#### `timingConfig`

Configuration object for the timing animation which occurs when tapping on tabs. Supported properties are:

- `duration` (`number`)

#### `springConfig`

Configuration object for the spring animation which occurs after swiping. Supported properties are:

- `damping` (`number`)
- `mass` (`number`)
- `stiffness` (`number`)
- `restSpeedThreshold` (`number`)
- `restDisplacementThreshold` (`number`)

#### `springVelocityScale`

Number for determining how meaningful is gesture velocity for calculating initial velocity of spring animation. Defaults to `0`.

#### `initialLayout`

Object containing the initial height and width of the screens. Passing this will improve the initial rendering performance. For most apps, this is a good default:

```js
{ width: Dimensions.get('window').width }}
```

#### `position`

Animated (from `react-native-reanimated`) value to listen to the position updates. The passed position value will be kept in sync with the current position of the tabs. It's useful for accessing the animated value outside the tab view.

#### `sceneContainerStyle`

Style to apply to the view wrapping each screen. You can pass this to override some default styles such as overflow clipping:

#### `style`

Style to apply to the tab view container.

#### `gestureHandlerProps`

An object with props to be passed to underlying [`PanGestureHandler`](https://kmagiera.github.io/react-native-gesture-handler/docs/handler-pan.html#properties). For example:

```js
gestureHandlerProps={{
  maxPointers: 1,
  waitFor: [someRef]
}}
```

#### `tabBarComponent`

Override component to use as the tab bar.

#### `tabBarOptions`

An object containing the props for the tab bar component. It can contain the following properties:

- `activeTintColor` - Label and icon color of the active tab.
- `inactiveTintColor` - Label and icon color of the inactive tab.
- `showIcon` - Whether to show icon for tab, default is false.
- `showLabel` - Whether to show label for tab, default is true.
- `upperCaseLabel` - Whether to make label uppercase, default is true.
- `pressColor` - Color for material ripple (Android >= 5.0 only).
- `pressOpacity` - Opacity for pressed tab (iOS and Android < 5.0 only).
- `scrollEnabled` - Whether to enable scrollable tabs.
- `tabStyle` - Style object for the tab.
- `indicatorStyle` - Style object for the tab indicator (line at the bottom of the tab).
- `labelStyle` - Style object for the tab label.
- `iconStyle` - Style object for the tab icon.
- `style` - Style object for the tab bar.
- `allowFontScaling` - Whether label font should scale to respect Text Size accessibility settings, default is true.
- `renderIndicator` - Function which takes an object with the current route and returns a custom React Element to be used as a tab indicator.

Example:

```js
<Tab.Navigator
  tabBarOptions={{
    labelStyle: { fontSize: 12 },
    tabStyle: { width: 100 },
    style: { backgroundColor: 'blue' },
  }}
>
  {/* ... */}
</Tab.Navigator>
```

### Options for `Tab.Screen`

The `options` prop can be used to configure individual screens inside the navigator. Supported options are:

#### `title`

Generic title that can be used as a fallback for `headerTitle` and `tabBarLabel`.

#### `tabBarIcon`

Function that given `{ focused: boolean, horizontal: boolean, tintColor: string }` returns a React.Node, to display in the tab bar. `horizontal` is `true` when the device is in landscape and `false` when portrait. The icon is re-rendered whenever the device orientation changes.

#### `tabBarLabel`

Title string of a tab displayed in the tab bar or a function that given `{ focused: boolean, tintColor: string }` returns a React.Node, to display in tab bar. When undefined, scene `title` is used. To hide, see `tabBarOptions.showLabel` in the previous section.

#### `tabBarAccessibilityLabel`

Accessibility label for the tab button. This is read by the screen reader when the user taps the tab. It's recommended to set this if you don't have a label for the tab.

#### `tabBarTestID`

ID to locate this tab button in tests.

### Events

The navigator can fire events on certain actions. Supported events are:

#### `tabPress`

This event is fired when the user presses the tab button for the current screen in the tab bar. By default a tab press does several things:

- If the tab is not focused, tab press will focus that tab
- If the tab is already focused:
  - If the screen for the tab renders a scroll view, scroll to top is performed by `useScrollToTop`
  - If the screen for the tab renders a stack navigator, a `popToTop` action is performed on the stack

To prevent the default behavior, you can call `eventPreventDefault`:

```js
navigation.addListener('tabPress', e => {
  // Prevent default behavior
  e.preventDefault();

  // Do something manually
  // ...
});
```

#### `tabLongPress`

This event is fired when the user presses the tab button for the current screen in the tab bar for an extended period.

## Example

```js
import { createMaterialTopTabNavigator } from '@react-navigation/material-top-tabs';

const Tab = createMaterialTopTabNavigator();

function App() {
  return (
    <Tab.Navigator
      initialRouteName="Feed"
      activeTintColor="#e91e63"
      labelStyle={{ fontSize: 12 }}
      style={{ backgroundColor: 'tomato' }}
    >
      <Tab.Screen
        name="Feed"
        component={Feed}
        options={{ tabBarLabel: 'Home' }}
      />
      <Tab.Screen
        name="Notifications"
        component={Notifications}
        options={{ tabBarLabel: 'Updates' }}
      />
      <Tab.Screen
        name="Profile"
        component={Profile}
        options={{ tabBarLabel: 'Profile' }}
      />
    </Tab.Navigator>
  );
}
```
