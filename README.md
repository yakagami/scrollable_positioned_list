# scrollable_positioned_list

## What is this?

This is a fork of  [ScrollablePositionedList](https://pub.dev/packages/scrollable_positioned_list) with `ScrollPosition` exposed in the `ScrollOffsetController`. A `ScrollablePositionedList` works much like the builder version of `ListView` except that the list can be scrolled or jumped to a specific item.

Aside from the original features of `ScrollablePositionedList`, this can be used to programmatically move the `ScrollablePositionedList` using methods like `jumpTo`. Currently the only change from upstream is the following line in `ScrollOffsetController`:

```dart

  /// ScrollPosition of the current ScrollablePositionedList. Values and methods
  /// such as pixels, maxScrollExtent and jumpTo are not necessarily defined to
  /// start from the beginning of the list. Whenever itemScrollController.jumpTo
  /// is called, the ScrollPosition will begin from the offset that index.
  /// position.jumpTo will cause the application to freeze at very large values
  /// as it must build all the widgets between the starting offset and the ending
  /// offset.
  ScrollPosition get position => _scrollableListState!.primary.scrollController.position;

```

You can use this package by adding the following lines to your `pubspec.yaml`:

```yml

  scrollable_positioned_list:
    git: https://github.com/yakagami/scrollable_positioned_list

```

## Usage

### Example

A `ScrollablePositionedList` can be created with:

```dart
final ItemScrollController itemScrollController = ItemScrollController();
final ScrollOffsetController scrollOffsetController = ScrollOffsetController();
final ItemPositionsListener itemPositionsListener = ItemPositionsListener.create();
final ScrollOffsetListener scrollOffsetListener = ScrollOffsetListener.create()

ScrollablePositionedList.builder(
  itemCount: 500,
  itemBuilder: (context, index) => Text('Item $index'),
  itemScrollController: itemScrollController,
  scrollOffsetController: scrollOffsetController,
  itemPositionsListener: itemPositionsListener,
  scrollOffsetListener: scrollOffsetListener,
);
```

One then can scroll to a particular item with:

```dart
itemScrollController.scrollTo(
  index: 150,
  duration: Duration(seconds: 2),
  curve: Curves.easeInOutCubic);
```

or jump to a particular item with:

```dart
itemScrollController.jumpTo(index: 150);
```

One can monitor what items are visible on screen with:

```dart
itemPositionsListener.itemPositions.addListener(() => ...);
```

### Experimental APIs (subject to bugs and changes)

Changes in scroll position can be monitored with:

```dart
scrollOffsetListener.changes.listen((event) => ...)
```

see `ScrollSum` in [this test](test/scroll_offset_listener_test.dart) for an example of how the current offset can be 
calculated from the stream of scroll change deltas.  This feature is new and experimental.

Changes in scroll position in pixels, relative to the current scroll position, can be made with:

```dart
scrollOffsetController.animateScroll(offset: 100, duration: Duration(seconds: 1));
```

A full example can be found in the example folder.

--------------------------------------------------------------------------------

This is not an officially supported Google product.
