Simple Android RecyclerView
===========================

This is a collection of basic implementations for Android's RecyclerView that
allows for easy implementation on the most simple cases. It is intended to be
used for instances where there is a single collection being bound to a custom
view implementation.

Intallation
-----------

This project is a work in progress and does not yet have an installation path
through maven/gradle. To be updated.

Usage
-----

### Item Binder

In a well written, simple, recycler view there are only two required operations.
We need to define how to create new views, and we need to be able to bind an 
item to one of those views.
The item binder is the class that does this work. To accomplish this we just
implement the ItemViewBinder interface, providing the view type that we're 
binding to, and the collection data type that our items are.

In this example, we're binding a collection of `Guest` models to a custom
view `GuestWidgetView`.

```java
public class GuestIndexBinder implements ItemViewBinder<GuestWidgetView, Guest>
{
    final private Context context;

    public GuestIndexBinder(Context context)
    {
        this.context = context;
    }

    @Override
    public GuestWidgetView createView()
    {
        return new GuestWidgetView(context);
    }

    @Override
    public void bindView(final Guest guest, GuestWidgetView view)
    {
        view.setName(guest.getFullName());
    }
}
```

### Initialization

Once we have our item binder, we can attach it to our `SimpleRecyclerView`.

The view needs our collection of data, and our binder.
To attach these, the `SimpleRecyclerView` provides an `init` method that accepts
the collection and the binder.

```java
        this.guestList.init(
            new ArrayList<Guest>(),
            new GuestIndexBinder(this.getActivity())
        );
```

The initialization method will also assign the view a LayoutManager if none has
been defined. Since it will be the case most of the time, it will default to the
standard Android `LinearLayoutManager`.

### Updating Data

The data for a `SimpleRecyclerView` is stored inside an `ItemAdapter` class, 
which is created for you automatically when the `SimpleRecyclerView.init` method
is invoked.

When you need to update the data in the view, this adapter is accessible through
the `SimpleRecylcerView.getItemAdapter()` method.

Alternatively, a custom `ItemAdapter` can be made and set through the class's
`setItemAdapter` method, instead of using the `init` method. When doing this
the user MUST also define the layout manager to be used.
