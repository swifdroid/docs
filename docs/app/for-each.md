# ForEach

This element allows you to create multiple similar elements based on a collection or a range.

**`ForEach`** can be used anywhere, but it is most commonly used inside [→ VStack](classic/v-stack.md) and [→ HStack](classic/h-stack.md)

!!! warning ""
    [→ RecyclerView](x/recycler-view.md) uses a different approach for dynamic content

## Static example

```swift
let names = ["Bob", "John", "Annie"]

VStack {
    ForEach(names) { name in
        TextView(name)
    }
    ForEach(names) { index, name in
        TextView("\(index). \(name)")
    }

    // or with range

    ForEach(1...20) { item in
        TextView("\(item)")
    }
    ForEach(1...20) { index, item in
        TextView("\(index). \(item)")
    }

    // and even like this

    20.times {
        TextView("repeating")
    }
    20.times { item in
        TextView("\(item)")
    }
    20.times { index, item in
        TextView("\(index). \(item)")
    }
}
```

## Dynamic example

```swift
@State var names = ["Bob", "John", "Annie"]

VStack {
    ForEach($names) { name in
        TextView(name)
    }

    // or with index

    ForEach($names) { index, name in
        TextView("\(index). \(name)")
    }
}

Button("Change 1").onClick { [weak self] in
    // this will append new Div with name automatically
    self?.names.append("George")
}

Button("Change 2").onClick { [weak self] in
    // this will replace and update Divs with names automatically
    self?.names = ["Bob", "Peppa", "George"]
}
```

## Conveniences

```swift
VStack {
    ForEach(...) {}
}
```
can be replaced with
```swift
VForEach(...) {}
```
As well as
```swift
HStack {
    ForEach(...) {}
}
```
can be replaced with
```swift
HForEach(...) {}
```