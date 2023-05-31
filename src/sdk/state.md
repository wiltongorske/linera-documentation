# Creating the Application State

The `struct` which defines your application's state can be found in `state.rs`.

To represent our Counter, all we're going to need a single `u64`. To persist
the counter we'll be using Linera's [view](../advanced_topics/views.md) paradigm.

Views are a little like an [ORM](https://en.wikipedia.org/wiki/Object%E2%80%93relational_mapping),
however instead of mapping your datastructures to a relational database like Postgres, they are
instead mapped onto key-value stores like [RocksDB](https://rocksdb.org/).

In vanilla Rust, we might represent our Counter as so:

```rust,ignore
// do not use this
struct Counter {
  value: u64
}
```

However, to persist your data, you'll need to replace the existing `Application` state struct in `state.rs`
with the following view:

```rust,ignore
/// The application state.
#[derive(RootView, GraphQLView)]
#[view(context = "ViewStorageContext")]
pub struct Counter {
    pub value: RegisterView<u64>,
}
```

and all other occurences of `Application` in your app.

The `RegisterView` supports modifying a single value of type `T`. There are different types of
views for different use-cases, but the majority of common data structures have already been implemented:

- A `Vec` maps to a `CollectionView`
- A `HashMap` maps to a `MapView`
- A `Queue` maps to a `QueueView`

For an exhaustive list refer to the Views [documentation](../advanced_topics/views.md).

Finally, run `cargo build` to ensure that your changes compile.