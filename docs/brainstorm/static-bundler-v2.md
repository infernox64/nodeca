Static Bundler
--------------

### Stage 1. Prepare dynamic data for assets

Once API tree of the application was fully populated, and i18n being loaded,
we generate API tree (with stubbed server methods) and i18n bundles:

```
.
└─ system/
    ├─ <namespace>/
    │   ├─ api-tree.js    # nodeca.<namespace>.* (server, shared, client)
    │   └─ i18n/                                                           
    │       └─ <lang>.js  # nodeca.<namespace>.i18n.<lang>
    └─ ...
```

`nodeca.<namespace>.server.*` contains methods with signatures similar to
original ones, but bodies ready to be used on the client and call corresponding
server methods transparently.


### Stage 2. Prepare views

##### 2.1. Combine "generic" views

```
.
├─ <namespace>/
│   └─ views/
│       └─ *.jade
└─ ...
```


##### 2.2. Prepare skins

```
.
├─ theme-<name>/
│   ├─ <namespace>
│   │   └─ views/
│   │       └─ *.jade
│   └─ ...
└─ ...
```


##### 2.3. Localize JADE views

We can replace `__('foo.bar')` calls with strings for each language if
translations has no macros, so we are building localized JADEs here...

```
.
├─ theme-<name>/
│   ├─ <namespace>
│   │   └─ views/
│   │       └─ *.<lang>.jade
│   └─ ...
└─ ...
```


##### 2.4. Compile JADE views

Views are compiled into one file (per language).

```
.
├─ theme-<name>/
│   ├─ <namespace>
│   │   └─ views/
│   │       └─ <lang>.js
│   └─ ...
└─ ...
```


### Stage 3. Compile static assets

Patch and merge static files, compile stylus for each skin using generic static
files as base structure.

```
.
├─ theme-<name>/
│   ├─ <namespace>
│   │   └─ *.*
│   └─ ...
└─ ...
```


### Stage 4. Put alltogether

Combine API tree, translations, skinz with views and static data in one place.

```
.
├─ system/
│   ├─ <namespace>/
│   │   ├─ api-tree.js
│   │   └─ i18n/
│   │       └─ <lang>.js
│   └─ ...
│
├─ theme-<name>/
│   ├─ <namespace>
│   │   ├─ views/
│   │   │   └─ <lang>.js
│   │   └─ *.*
│   └─ ...
│
└─ ...
```