Implementing an efficient LRU cache in JavaScript
-------------------------------------------------

Let's say we need to process a very large – hundreds of gigabytes large
– csv file containing urls we will need to download. To ensure we are
not going to run out of memory while parsing the whole file, we read the
file line by line:

    csv.forEachLine(line => {  download(line.url);});

Now let's say the person that created our file forgot to deduplicate the
urls.I am aware that this could easily be solved by using `sort -u` but
that's not the point.Deduping urls is not as straigthforward as it may
seem. Check the [ural](https://github.com/medialab/ural#readme) library
in python, for instance, for some examples of what can be achieved in
this regard.This is an issue because we don't want to fetch the same url
more than once: grabbing resources from the web is time-consuming &
should be done parcimoniously not to flood the sites we are grabbing
those resources from.

An obvious solution could be to remember urls we already fetched by
caching them into a map:

    const done = new Map();
    csv.forEachLine(
        line => {  
                    if (done.has(line.url))    
                        return done.get(line.url);  
                    const result = download(line.url);  
                    done.set(line.url, result);
                }
    );

At this point, the astute reader will have noticed that we just defeated
the purpose of reading the csv file line by line since we now need to
commit all its urls to memory.

And herein lies the issue: we want to avoid fetching the same url more
than once while also making sure we don't run out of memory. We have to
find a way to compromise.

Fortunately for us, it seems that only a tiny
fraction of the urls contained in our file are repeated very often while
the vast majority of others only appears one or two times. We can
leverage this fact by designing a policy to throw away urls from our map
if we have a good intuition they are unlikely to appear again. Thus, we
won't allow our map to exceed a predetermined amount of memory.

And one of the most commonly used eviction policy is called “LRU”:
**L**east **R**ecently **U**sed.

#### The LRU cache

Hence, we understand that a LRU cache is a fixed-capacity map able to
bind values to keys with the following twist: if the cache is full and
we still need to insert a new item, we will make some place by evicting
the least recently used one.

To do so, the cache will need to store given items in order of their
last access. Then, each time someone tries to set a new key, or access a
key, we need to modify the underlying list to ensure the needed order is
maintained.

LFU (Least Frequently Used) is a perfectly valid cache eviction
policy. But why is this order relevant? Wouldn't it be better to record the number
of times each item was accessed so we can evict the **L**east
**F**requently **U**sed instead?

Not necessarily. Here are some reasons why:

-   LRU is a actually a good proxy of LFU since the more frequently a
    pair is accessed, the less chance it has to be evicted.
-   You will need to store integers in addition to everything else to
    keep track of the number of times the items were accessed.
-   Ordering items on their last access is very straightforward to do
    since it can be synchronized with operations on the cache.
-   LFU will often force you to make an arbitrary choice of item to
    evict: for instance if all your pairs have been accessed only once.
    With LRU, you don't have such choice to make: you just evict the
    least recently used. No ambiguity here.

#### Implementing a LRU cache

There are many ways to implement a working LRU cache but I will only
focus on the way you are most likely to encounter in the wild when
developing for high-level languages.

Usually, to implement a proper LRU cache, we need the two following
ingredients:

1.  A [hashmap](https://en.wikipedia.org/wiki/Hash_table)-like data
    structure able to retrieve values associated to arbitrary keys –
    such as strings – efficiently. In JavaScript we can either use the
    ES6
    `Map` or any plain object `{}`; remember
    that our cache is no different from a fixed-capacity key-value
    store.
2.  A way to store our items in the order of their last access. What's
    more, we will need to move items around efficiently. That's why
    people naturally lean toward a [doubly-linked
    list](https://en.wikipedia.org/wiki/Doubly_linked_list) for the job. 
3.  But there could be other more efficient ways, per example by leveraging the power 
    of Javascript Arrays & the rich family of functions that is built in already.

Minimally, our implementation needs to be able to run the two following
operations:

-   `#.set`: associating a value to the given key, while
    evicting the least recently used item if the cache is already full.
-   `#.get`: retrieving the value associated to the given
    key if this one exists at all in the cache, while updating the
    underlying list to keep LRU order.

And here is how we could use such a cache:

    // Let's create a cache able to contain 3 itemsconst 
    cache = new LRUCache(3);

    // Let's add items
    cache.set(1, 'one');
    cache.set(2, 'two');
    cache.set(3, 'three');
    // Up until now, nothing was evicted from the cache
    cache.has(2);
    >>> true
    // Oh no! we need to add a new item
    cache.set(4, 'four');
    // `1` was evicted because it was the LRU key
    cache.has(1);
    >>> false
    // If we get `2`, it won't be the LRU key anymore
    cache.get(2);
    >>> 'two'
    // Which means that `3` that will get evicted now!
    cache.set(5, 'five');
    cache.has(3);
    >>> false
    // Thus we never store more than 3 items
    cache.size
    >>> 3
    cache.items()
    >>> ['five', 'two', 'four']

