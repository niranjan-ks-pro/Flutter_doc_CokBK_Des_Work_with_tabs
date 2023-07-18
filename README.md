# Flutter_doc_CokBK_Des_Work_with_tabs
 https://docs.flutter.dev/cookbook/design/tabs
Use themes to share colors and font styles
==========================================

1.  [Cookbook](https://docs.flutter.dev/cookbook)
2.  [Design](https://docs.flutter.dev/cookbook/design)
3.  [Themes](https://docs.flutter.dev/cookbook/design/themes)

To share colors and font styles throughout an app, use themes. You can either define app-wide themes, or use `Theme` widgets that define the colors and font styles for a particular part of the application. In fact, app-wide themes are just `Theme` widgets created at the root of an app by the `MaterialApp`.

After defining a Theme, use it within your own widgets. Flutter's Material widgets also use your Theme to set the background colors and font styles for AppBars, Buttons, Checkboxes, and more.

[](https://docs.flutter.dev/cookbook/design/themes#creating-an-app-theme)Creating an app theme
----------------------------------------------------------------------------------------------

To share a Theme across an entire app, provide a [`ThemeData`](https://api.flutter.dev/flutter/material/ThemeData-class.html) to the `MaterialApp` constructor.

If no `theme` is provided, Flutter creates a default theme for you.

content_copy

```
MaterialApp(
  title: appName,
  theme: ThemeData(
    // Define the default brightness and colors.
    brightness: Brightness.dark,
    primaryColor: Colors.lightBlue[800],

    // Define the default font family.
    fontFamily: 'Georgia',

    // Define the default `TextTheme`. Use this to specify the default
    // text styling for headlines, titles, bodies of text, and more.
    textTheme: const TextTheme(
      displayLarge: TextStyle(fontSize: 72, fontWeight: FontWeight.bold),
      titleLarge: TextStyle(fontSize: 36, fontStyle: FontStyle.italic),
      bodyMedium: TextStyle(fontSize: 14, fontFamily: 'Hind'),
    ),
  ),
  home: const MyHomePage(
    title: appName,
  ),
);
```

See the [`ThemeData`](https://api.flutter.dev/flutter/material/ThemeData-class.html) documentation to see all of the colors and fonts you can define.

[](https://docs.flutter.dev/cookbook/design/themes#themes-for-part-of-an-application)Themes for part of an application
----------------------------------------------------------------------------------------------------------------------

To override the app-wide theme in part of an application, wrap a section of the app in a `Theme` widget.

There are two ways to approach this: creating a unique `ThemeData`, or extending the parent theme.

info Note: To learn more, watch this short Widget of the Week video on the Theme widget:

### [](https://docs.flutter.dev/cookbook/design/themes#creating-unique-themedata)Creating unique `ThemeData`

If you don't want to inherit any application colors or font styles, create a `ThemeData()` instance and pass that to the `Theme` widget.

content_copy

```
Theme(
  // Create a unique theme with `ThemeData`
  data: ThemeData(
    splashColor: Colors.yellow,
  ),
  child: FloatingActionButton(
    onPressed: () {},
    child: const Icon(Icons.add),
  ),
);
```

### [](https://docs.flutter.dev/cookbook/design/themes#extending-the-parent-theme)Extending the parent theme

Rather than overriding everything, it often makes sense to extend the parent theme. You can handle this by using the [`copyWith()`](https://api.flutter.dev/flutter/material/ThemeData/copyWith.html) method.

content_copy

```
Theme(
  // Find and extend the parent theme using `copyWith`. See the next
  // section for more info on `Theme.of`.
  data: Theme.of(context).copyWith(splashColor: Colors.yellow),
  child: const FloatingActionButton(
    onPressed: null,
    child: Icon(Icons.add),
  ),
);
```

[](https://docs.flutter.dev/cookbook/design/themes#using-a-theme)Using a Theme
------------------------------------------------------------------------------

Now that you've defined a theme, use it within the widgets' `build()` methods by using the `Theme.of(context)` method.

The `Theme.of(context)` method looks up the widget tree and returns the nearest `Theme` in the tree. If you have a standalone `Theme` defined above your widget, that's returned. If not, the app's theme is returned.

In fact, the `FloatingActionButton` uses this technique to find the `accentColor`.

content_copy

```
Container(
  color: Theme.of(context).colorScheme.secondary,
  child: Text(
    'Text with a background color',
    style: Theme.of(context).textTheme.titleLarge,
  ),
),
```

`\
`Work with tabs
==============

1.  [Cookbook](https://docs.flutter.dev/cookbook)
2.  [Design](https://docs.flutter.dev/cookbook/design)
3.  [Work with tabs](https://docs.flutter.dev/cookbook/design/tabs)

Working with tabs is a common pattern in apps that follow the Material Design guidelines. Flutter includes a convenient way to create tab layouts as part of the [material library](https://api.flutter.dev/flutter/material/material-library.html).

info Note: To create tabs in a Cupertino app, see the [Building a Cupertino app with Flutter](https://codelabs.developers.google.com/codelabs/flutter-cupertino) codelab.

This recipe creates a tabbed example using the following steps;

1.  Create a `TabController`.
2.  Create the tabs.
3.  Create content for each tab.

[](https://docs.flutter.dev/cookbook/design/tabs#1-create-a-tabcontroller)1\. Create a `TabController`
------------------------------------------------------------------------------------------------------

For tabs to work, you need to keep the selected tab and content sections in sync. This is the job of the [`TabController`](https://api.flutter.dev/flutter/material/TabController-class.html).

Either create a `TabController` manually, or automatically by using a [`DefaultTabController`](https://api.flutter.dev/flutter/material/DefaultTabController-class.html) widget.

Using `DefaultTabController` is the simplest option, since it creates a `TabController` and makes it available to all descendant widgets.

content_copy

```
return MaterialApp(
  home: DefaultTabController(
    length: 3,
    child: Scaffold(),
  ),
);
```

[](https://docs.flutter.dev/cookbook/design/tabs#2-create-the-tabs)2\. Create the tabs
--------------------------------------------------------------------------------------

When a tab is selected, it needs to display content. You can create tabs using the [`TabBar`](https://api.flutter.dev/flutter/material/TabBar-class.html) widget. In this example, create a `TabBar` with three [`Tab`](https://api.flutter.dev/flutter/material/Tab-class.html) widgets and place it within an [`AppBar`](https://api.flutter.dev/flutter/material/AppBar-class.html).

content_copy

```
return MaterialApp(
  home: DefaultTabController(
    length: 3,
    child: Scaffold(
      appBar: AppBar(
        bottom: const TabBar(
          tabs: [
            Tab(icon: Icon(Icons.directions_car)),
            Tab(icon: Icon(Icons.directions_transit)),
            Tab(icon: Icon(Icons.directions_bike)),
          ],
        ),
      ),
    ),
  ),
);
```

By default, the `TabBar` looks up the widget tree for the nearest `DefaultTabController`. If you're manually creating a `TabController`, pass it to the `TabBar`.

[](https://docs.flutter.dev/cookbook/design/tabs#3-create-content-for-each-tab)3\. Create content for each tab
--------------------------------------------------------------------------------------------------------------

Now that you have tabs, display content when a tab is selected. For this purpose, use the [`TabBarView`](https://api.flutter.dev/flutter/material/TabBarView-class.html) widget.

info Note: Order is important and must correspond to the order of the tabs in the `TabBar`.

content_copy

```
body: const TabBarView(
  children: [
    Icon(Icons.directions_car),
    Icon(Icons.directions_transit),
    Icon(Icons.directions_bike),
  ],
),
```

`\
`

[](https://docs.flutter.dev/cookbook/design/tabs#interactive-example)
---------------------------------------------------------------------
