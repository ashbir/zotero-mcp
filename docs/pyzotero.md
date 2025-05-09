Title: Description — Pyzotero 1.6.12.dev7+gc3f6eb2 documentation

URL Source: https://pyzotero.readthedocs.io/en/latest/

Markdown Content:
Pyzotero is a Python wrapper for the [Zotero API (v3)](https://www.zotero.org/support/dev/web_api/v3/start).

Getting started (short version)[](pyzotero_readthedocs_io_en_latest.md#getting-started-short-version "Link to this heading")
--------------------------------------------------------------------------------------------------------------------------------

1.  In a shell / prompt: `pip install pyzotero` or `conda config --add channels conda-forge && conda install pyzotero`
    
2.  You’ll need the ID of the personal or group library you want to access:
    

> *   Your **personal library ID** is available [here](https://www.zotero.org/settings/keys), in the section `Your userID for use in API calls`
>     
> *   For **group libraries**, the ID can be found by opening the group’s page: `https://www.zotero.org/groups/groupname`, and hovering over the `group settings` link. The ID is the integer after `/groups/`
>     

3.  You’ll also need [\[\*\]](pyzotero_readthedocs_io_en_latest.md#id3) to get an **API key** from the Zotero [site](https://www.zotero.org/settings/keys/new)
    
4.  Are you accessing your own Zotero library? Set\`\`library\_type\`\` to `'user'`
    
5.  Are you accessing a shared group library? Set\`\`library\_type\`\` to `'group'`
    
6.  **Read-only** access to your local Zotero library is available: set `local=True`
    

>     from pyzotero import zotero    zot \= zotero.Zotero(library\_id, library\_type, api\_key)    items \= zot.top(limit\=5)    \# we've retrieved the latest five top-level items in our library
>     \# we can print each item's item type and ID
>     for item in items:
>     print('Item Type: %s | Key: %s' % (item\['data'\]\['itemType'\], item\['data'\]\['key'\]))

Refer to the [Errors](pyzotero_readthedocs_io_en_latest.md#read) and [Write API Methods](pyzotero_readthedocs_io_en_latest.md#write).

Installation, testing, usage (longer version)[](pyzotero_readthedocs_io_en_latest.md#installation-testing-usage-longer-version "Link to this heading")
--------------------------------------------------------------------------------------------------------------------------------

Installation[](pyzotero_readthedocs_io_en_latest.md#installation "Link to this heading")
-----------------------------------------------------------------------------------------------

Using [pip](http://www.pip-installer.org/en/latest/index.html): `pip install pyzotero`

Using [Anaconda](https://www.anaconda.com/distribution/): `conda config --add channels conda-forge && conda install pyzotero`

From a local clone, if you wish to install Pyzotero from a specific branch:

> git clone git://github.com/urschrei/pyzotero.git
> cd pyzotero
> git checkout main
> pip install .

The Pyzotero source tarball is also available from [PyPI](http://pypi.python.org/pypi/Pyzotero)

Installing development versions[](pyzotero_readthedocs_io_en_latest.md#installing-development-versions "Link to this heading")
--------------------------------------------------------------------------------------------------------------------------------

Pyzotero remains in development as of November 2024. Unstable development versions can be found on the [Github main branch](https://github.com/urschrei/pyzotero/tree/main), and installed directly from a checked-out `main` branch on a local clone, as in the example above.

Testing[](pyzotero_readthedocs_io_en_latest.md#testing "Link to this heading")
-------------------------------------------------------------------------------------

Testing requires the `HTTPretty`, and `Python-Dateutil` packages.

Run `pytest .` from the top-level directory.

Building Documentation[](pyzotero_readthedocs_io_en_latest.md#building-documentation "Link to this heading")
-------------------------------------------------------------------------------------------------------------------

If you wish to build Pyzotero’s documentation for offline use, it can be built from the `doc` directory of a local git repo by running `make` followed by the desired output format(s) (`html`, `epub`, `latexpdf` etc.)

This functionality requires Sphinx. See the [Sphinx documentation](http://sphinx.pocoo.org/tutorial.html#running-the-build) for full details.

Reporting issues[](pyzotero_readthedocs_io_en_latest.md#reporting-issues "Link to this heading")
-------------------------------------------------------------------------------------------------------

If you encounter an error while using Pyzotero, please open an issue on its [Github issues page](https://github.com/urschrei/pyzotero/issues).

General Usage[](pyzotero_readthedocs_io_en_latest.md#general-usage "Link to this heading")
-------------------------------------------------------------------------------------------------

Important

A `Zotero` instance is bound to the library or group used to create it. Thus, if you create a `Zotero` instance with a `library_id` of `67` and a `library_type` of `group`, its item methods will only operate upon that group. Similarly, if you create a `Zotero` instance with your own `library_id` and a `library_type` of `user`, the instance will be bound to your Zotero library.

First, create a new Zotero instance:

> _class_ zotero.Zotero(_library\_id_, _library\_type_\[, _api\_key_, _preserve\_json\_order_, _locale_, _local_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero "Link to this definition")
> 
> Parameters:
> 
> *   **library\_id** (_str_) – a valid Zotero API user ID
>     
> *   **library\_type** (_str_) – a valid Zotero API library type: **user** or **group**
>     
> *   **api\_key** (_str_) – a valid Zotero API user key
>     
> *   **preserve\_json\_order** (_bool_) – Load JSON returns with OrderedDict to preserve their order
>     
> *   **locale** (_str_) – Set the [locale](https://www.zotero.org/support/dev/web_api/v3/types_and_fields#zotero_web_api_item_typefield_requests), allowing retrieval of localised item types, field types, and creator types. Defaults to “en-US”.
>     
> *   **local** (_str_) – use the local Zotero http server instead of the remote API. Note that the local server currently (November 2024) only allows **read** requests
>     

Example:

> from pyzotero import zotero
> zot \= zotero.Zotero('123', 'user', 'ABC1234XYZ')
> \# we now have a Zotero object, zot, and access to all its methods
> first\_ten \= zot.items(limit\=10)
> \# a list containing dicts of the ten most recently modified library items

Errors[](pyzotero_readthedocs_io_en_latest.md#errors "Link to this heading")
-----------------------------------------------------------------------------------

Where possible, any `ZoteroError` which is raised will preserve the underlying error in its `__cause__` and `__context__` properties, should you wish to work with these directly.

Read API Methods[](pyzotero_readthedocs_io_en_latest.md#read-api-methods "Link to this heading")
-------------------------------------------------------------------------------------------------------

Note

All search/request parameters inside square brackets are **optional**. Methods such as [`Zotero.top()`](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.top "zotero.Zotero.top"), [`Zotero.items()`](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.items "zotero.Zotero.items") etc. can be called with no additional parameters if you wish.

Tip

The Read API returns 25 results by default (the API documentation claims 50). In the interests of usability, Pyzotero returns 100 items by default, by setting the API `limit` parameter to 100, unless it’s set by the user. If you wish to retrieve e.g. all top-level items without specifying a `limit` parameter, you’ll have to wrap your call with [`Zotero.everything()`](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.everything "zotero.Zotero.everything"): `results = zot.everything(zot.top())`.

Zotero.key\_info()[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.key_info "Link to this definition")

Returns info about the user and group library permissions associated with the current `Zotero` instance, based on the API key. Together with [`Zotero.groups()`](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.groups "zotero.Zotero.groups"), this allows all accessible resources to be determined.

Return type:

dict

Retrieving Items[](pyzotero_readthedocs_io_en_latest.md#retrieving-items "Link to this heading")
-------------------------------------------------------------------------------------------------------

Tip

In contrast to the v1 API, a great deal of additional metadata is now returned. In most cases, simply accessing items by referring to their `item['data']` key will suffice.

The following methods will retrieve either user or group items, depending on the value (`user` or `group`) used to create the `Zotero` instance:

> Zotero.items(\[_search/request parameters_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.items "Link to this definition")
> 
> Returns Zotero library items
> 
> Return type:
> 
> list of dicts
> 
> Zotero.settings(\[_since_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.settings "Link to this definition")
> 
> Returns synced Zotero user library settings such as feeds and PDF reading progress. Use the optional `since` parameter to specify the retrieval of changes since a known previous library version.
> 
> Return type:
> 
> list of dicts
> 
> Zotero.count\_items()[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.count_items "Link to this definition")
> 
> Returns a count of all items in a library / group
> 
> Return type:
> 
> int
> 
> Zotero.top(\[_search/request parameters_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.top "Link to this definition")
> 
> Returns top-level Zotero library items
> 
> Return type:
> 
> list of dicts
> 
> Zotero.publications(\[_search/request parameters_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.publications "Link to this definition")
> 
> Returns the publications from the “My Publications” collection of a user’s library. Only available on `user` libraries.
> 
> Return type:
> 
> list of dicts
> 
> Zotero.trash(\[_search/request parameters_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.trash "Link to this definition")
> 
> Returns library items from the library’s trash
> 
> Return type:
> 
> list of dicts
> 
> Zotero.deleted(\[_search/request parameters_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.deleted "Link to this definition")
> 
> Returns deleted collections, library items, tags, searches and settings (requires “since=” parameter)
> 
> Return type:
> 
> list of dicts
> 
> Zotero.item(_itemID_\[, _search/request parameters_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.item "Link to this definition")
> 
> Returns a specific item
> 
> Parameters:
> 
> **itemID** (_str_) – a zotero item ID
> 
> Return type:
> 
> list of dicts
> 
> Zotero.children(_itemID_\[, _search/request parameters_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.children "Link to this definition")
> 
> Returns the child items of a specific item
> 
> Parameters:
> 
> **itemID** (_str_) – a zotero item ID
> 
> Return type:
> 
> list of dicts
> 
> Zotero.collection\_items(_collectionID_\[, _search/request parameters_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.collection_items "Link to this definition")
> 
> Returns items from the specified collection. This does not include items in sub-collections
> 
> Parameters:
> 
> **collectionID** (_str_) – a Zotero collection ID
> 
> Return type:
> 
> list of dicts
> 
> Zotero.collection\_items\_top(_collectionID_\[, _search/request parameters_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.collection_items_top "Link to this definition")
> 
> Returns top-level items from the specified collection.
> 
> Parameters:
> 
> **collectionID** (_str_) – a Zotero collection ID
> 
> Return type:
> 
> list of dicts
> 
> Zotero.get\_subset(_itemIDs_\[, _search/request parameters_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.get_subset "Link to this definition")
> 
> Retrieve an arbitrary set of non-adjacent items. Limited to 50 items per call.
> 
> Parameters:
> 
> **itemIDs** (_list_) – a list of Zotero Item IDs
> 
> Return type:
> 
> list of dicts

Example of returned item data:

> \[{u'data': {u'ISBN': u'0810116820',
>            u'abstractNote': u'',
>            u'accessDate': u'',
>            u'archive': u'',
>            u'archiveLocation': u'',
>            u'callNumber': u'HIB 828.912 BEC:3g N9',
>            u'collections': \[u'2UNGXMU9'\],
>            u'creators': \[{u'creatorType': u'author',
>                           u'firstName': u'Daniel',
>                           u'lastName': u'Katz'}\],
>            u'date': u'1999',
>            u'dateAdded': u'2010-01-04T14:50:40Z',
>            u'dateModified': u'2014-08-06T11:28:41Z',
>            u'edition': u'',
>            u'extra': u'',
>            u'itemType': u'book',
>            u'key': u'VDNIEAPH',
>            u'language': u'',
>            u'libraryCatalog': u'library.catalogue.tcd.ie Library Catalog',
>            u'numPages': u'',
>            u'numberOfVolumes': u'',
>            u'place': u'Evanston, Ill',
>            u'publisher': u'Northwestern University Press',
>            u'relations': {u'dc:replaces': u'http://zotero.org/users/436/items/9TXN8QUD'},
>            u'rights': u'',
>            u'series': u'',
>            u'seriesNumber': u'',
>            u'shortTitle': u'Saying I No More',
>            u'tags': \[{u'tag': u'Beckett, Samuel', u'type': 1},
>                      {u'tag': u'Consciousness in literature', u'type': 1},
>                      {u'tag': u'English prose literature', u'type': 1},
>                      {u'tag': u'Ireland', u'type': 1},
>                      {u'tag': u'Irish authors', u'type': 1},
>                      {u'tag': u'Modernism (Literature)', u'type': 1},
>                      {u'tag': u'Prose', u'type': 1},
>                      {u'tag': u'Self in literature', u'type': 1},
>                      {u'tag': u'Subjectivity in literature', u'type': 1}\],
>            u'title': u'Saying I No More: Subjectivity and Consciousness in The Prose of Samuel Beckett',
>            u'url': u'',
>            u'version': 792,
>            u'volume': u''},
>  u'key': u'VDNIEAPH',
>  u'library': {u'id': 436,
>               u'links': {u'alternate': {u'href': u'https://www.zotero.org/urschrei',
>                                         u'type': u'text/html'}},
>               u'name': u'urschrei',
>               u'type': u'user'},
>  u'links': {u'alternate': {u'href': u'https://www.zotero.org/urschrei/items/VDNIEAPH',
>                            u'type': u'text/html'},
>             u'self': {u'href': u'https://api.zotero.org/users/436/items/VDNIEAPH',
>                       u'type': u'application/json'}},
>  u'meta': {u'creatorSummary': u'Katz',
>            u'numChildren': 0,
>            u'parsedDate': u'1999-00-00'},
>  u'version': 792}\]

See [‘Hello World’](pyzotero_readthedocs_io_en_latest.md#hello-world) example, above

Retrieving Files[](pyzotero_readthedocs_io_en_latest.md#retrieving-files "Link to this heading")
-------------------------------------------------------------------------------------------------------

> Zotero.file(_itemID_\[, _search/request parameters_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.file "Link to this definition")
> 
> Returns the raw file content of an item. This can be dumped like so:
> 
> with open('article.pdf', 'wb') as f:
>   f.write(zot.file('BM8MZJBB'))
> 
> Parameters:
> 
> **itemID** (_str_) – a zotero item ID
> 
> Return type:
> 
> binary string
> 
> Zotero.dump(_itemID_\[, _filename_, _path_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.dump "Link to this definition")
> 
> A convenient wrapper around [`Zotero.file()`](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.file "zotero.Zotero.file"). Writes an attachment to disk using the optional path and filename. If neither are supplied, the file is written to the current working directory, and a [`Zotero.item()`](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.item "zotero.Zotero.item") call is first made to determine the attachment filename. No error checking is done regarding the path. If successful, the full path including the file name is returned.
> 
> Note
> 
> HTML snapshots will be dumped as zip files. These will be named with their API item key, and a .zip extension.
> 
> \# write a file to the current working directory using the stored filename
> zot.dump('BM8MZJBB')
> \# write the same file to a different path, with a new name
> zot.dump('BM8MZJBB', 'article\_1.pdf', '/home/beckett/pdfs')
> 
> Parameters:
> 
> *   **itemID** (_str_) – a zotero item ID
>     
> *   **filename** (_str_) – (optional) an alternate filename
>     
> *   **path** (_str_) – (optional) a valid path for the file
>     
> 
> Return type:
> 
> String

File retrieval and dumping should work for most common document, audio and video file formats. If you encounter an error, [please open an issue](https://github.com/urschrei/pyzotero/issues).

Retrieving Collections[](pyzotero_readthedocs_io_en_latest.md#retrieving-collections "Link to this heading")
-------------------------------------------------------------------------------------------------------------------

> Zotero.collections(\[_search/request parameters_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.collections "Link to this definition")
> 
> Returns a library’s collections. **This includes subcollections**.
> 
> Return type:
> 
> list of dicts
> 
> Zotero.collections\_top(\[_search/request parameters_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.collections_top "Link to this definition")
> 
> Returns a library’s top-level collections.
> 
> Return type:
> 
> list of dicts
> 
> Zotero.collection(_collectionID_\[, _search/request parameters_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.collection "Link to this definition")
> 
> Returns a specific collection
> 
> Parameters:
> 
> **collectionID** (_str_) – a Zotero library collection ID
> 
> Return type:
> 
> dict
> 
> Zotero.collections\_sub(_collectionID_\[, _search/request parameters_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.collections_sub "Link to this definition")
> 
> Returns the sub-collections of a specific collection
> 
> Parameters:
> 
> **collectionID** (_str_) – a Zotero library collection ID
> 
> Return type:
> 
> list of dicts
> 
> Zotero.all\_collections(\[_collectionID_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.all_collections "Link to this definition")
> 
> Returns either all collections and sub-collections in a flat list, or, if a collection ID is specified, that collection and all of its sub-collections. This method can be called at any collection “depth”.
> 
> Parameters:
> 
> **collectionID** (_str_) – a Zotero library collection ID (optional)
> 
> Return type:
> 
> list of dicts

Example of returned collection data:

> \[{u'data': {u'key': u'5TSDXJG6',
>             u'name': u'Critical GIS',
>             u'parentCollection': False,
>             u'relations': {},
>             u'version': 778},
>   u'key': u'5TSDXJG6',
>   u'library': {u'id': 436,
>                u'links': {u'alternate': {u'href': u'https://www.zotero.org/urschrei',
>                                          u'type': u'text/html'}},
>                u'name': u'urschrei',
>                u'type': u'user'},
>   u'links': {u'alternate': {u'href': u'https://www.zotero.org/urschrei/collections/5TSDXJG6',
>                             u'type': u'text/html'},
>              u'self': {u'href': u'https://api.zotero.org/users/436/collections/5TSDXJG6',
>                        u'type': u'application/json'}},
>   u'meta': {u'numCollections': 0, u'numItems': 1},
>   u'version': 778}\]

Retrieving groups[](pyzotero_readthedocs_io_en_latest.md#retrieving-groups "Link to this heading")
---------------------------------------------------------------------------------------------------------

> Zotero.groups(\[_search/request parameters_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.groups "Link to this definition")
> 
> Retrieve the Zotero group data to which the current library\_id and api\_key has access
> 
> Return type:
> 
> list of dicts

Example of returned group data:

> \[{u'data': {u'description': u'',
>             u'fileEditing': u'admins',
>             u'hasImage': 1,
>             u'id': 169947,
>             u'libraryEditing': u'admins',
>             u'libraryReading': u'members',
>             u'members': \[1177919, 1408658\],
>             u'name': u'smart\_cities',
>             u'owner': 436,
>             u'type': u'Private',
>             u'url': u'',
>             u'version': 0},
>   u'id': 169947,
>   u'links': {u'alternate': {u'href': u'https://www.zotero.org/groups/169947',
>                             u'type': u'text/html'},
>              u'self': {u'href': u'https://api.zotero.org/groups/169947',
>                        u'type': u'application/json'}},
>   u'meta': {u'created': u'2013-05-22T11:22:46Z',
>             u'lastModified': u'2013-05-22T11:26:50Z',
>             u'numItems': 817},
>   u'version': 0}\]

Retrieving Tags[](pyzotero_readthedocs_io_en_latest.md#retrieving-tags "Link to this heading")
-----------------------------------------------------------------------------------------------------

> Zotero.tags(\[_search/request parameters_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.tags "Link to this definition")
> 
> Returns a library’s tags
> 
> Return type:
> 
> list of strings
> 
> Zotero.item\_tags(_itemID_\[, _search/request parameters_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.item_tags "Link to this definition")
> 
> Returns tags from a specific item
> 
> Parameters:
> 
> **itemID** (_str_) – a valid Zotero library Item ID
> 
> Return type:
> 
> list of strings

Example of returned tag data:

> \['Authority in literature', 'Errata'\]

Retrieving Version Information[](pyzotero_readthedocs_io_en_latest.md#retrieving-version-information "Link to this heading")
--------------------------------------------------------------------------------------------------------------------------------

The [Zotero API](https://www.zotero.org/support/dev/web_api/v3/syncing) recommends requesting version information for all (or all changed) items and collections when implementing syncing. The following convenience methods (which by default return an unlimited number of responses) simplify this process.

The return values of these methods associate each item / collection with the last version (or greater) at which the item / collection was modified. By passing the keyword argument `since=versionNum` only items / collections which have been modified since `versionNum` are included in the response. Thus, an application which previously sucessfully synced with the server at `versionNum` can use these methods to determine which items / collections need to be retrieved from the server.

> Zotero.item\_versions(\[_search/request parameters_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.item_versions "Link to this definition")
> 
> Returns a dict containing version information for items in the library
> 
> Return type:
> 
> dict: string -\> integer
> 
> Zotero.collection\_versions(_itemID_\[, _search/request parameters_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.collection_versions "Link to this definition")
> 
> Returns a dict containing version information for collections in the library
> 
> Return type:
> 
> dict: string -\> integer

Example of returned version data:

> {'C9KW275P': 3915, 'IB489TKM': 4025 }

Full–Text Content[](pyzotero_readthedocs_io_en_latest.md#fulltext-content "Link to this heading")
--------------------------------------------------------------------------------------------------------

These methods allow the retrieval of full–text content for given library items

> Zotero.new\_fulltext(_since_)[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.new_fulltext "Link to this definition")
> 
> Returns a dict containing item keys and library versions newer than `since` (a library version string, e.g. `"1085"`)
> 
> rtype:
> 
> dict: string -\> integer

Example of returned data:

> {
>     u'229QED6I': 747,
>     u'22TGJFS2': 769,
>     u'23SZWREM': 764
> }
> 
> Zotero.fulltext\_item(_itemID_\[, _search/request parameters_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.fulltext_item "Link to this definition")
> 
> Returns a dict containing full-text data for the given attachment item. `indexedChars` and `totalChars` are used for text documents, while `indexedPages` and `totalPages` are used for PDFs.

Example of returned data:

> {
> "content": "This is full-text content.",
> "indexedPages": 50,
> "totalPages": 50
> }
> 
> Zotero.set\_fulltext(_itemID_, _payload_)[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.set_fulltext "Link to this definition")
> 
> Set full-text data for an item
> 
> rtype:
> 
> boolean
> 
> `itemID` should correspond to an existing attachment item.
> 
> `payload`: a dict containing three keys:
> 
> > `content`: the full-text content, and either
> > 
> > For text documents, `indexedChars` and `totalChars` OR
> > 
> > For PDFs, `indexedPages` and `totalPages`.

Example payload:

> {
> "content": "This is full-text content.",
> "indexedPages": 50,
> "totalPages": 50
> }

The `follow()`, and `everything()` methods[](pyzotero_readthedocs_io_en_latest.md#the-follow-and-everything-methods "Link to this heading")
--------------------------------------------------------------------------------------------------------------------------------

These methods (currently experimental) aim to make Pyzotero a little more RESTful. Following any Read API call which can retrieve **multiple items**, calling `follow()` will repeat that call, but for the next _x_ number of items, where _x_ is either a number set by the user for the original call, or 50 by default. Each subsequent call to `follow()` will extend the offset.

Zotero.follow()[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.follow "Link to this definition")

Example:

> from pyzotero import zotero
> zot \= zotero.Zotero(library\_id, library\_type, api\_key)
> \# only retrieve a single item
> \# this will retrieve the most recently added/modified top-level item
> first\_item \= zot.top(limit\=1)
> \# now we can start retrieving subsequent items
> next\_item \= zot.follow()
> third\_item \= zot.follow()

Zotero.everything()[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.everything "Link to this definition")

Example:

> from pyzotero import zotero
> zot \= zotero.Zotero(library\_id, library\_type, api\_key)
> \# retrieve all top-level items
> toplevel \= zot.everything(zot.top())

The `everything()` method should work with all Pyzotero Read API calls which can return multiple items, but has not yet been extensively tested. [Feedback is welcomed](https://github.com/urschrei/pyzotero/issues).

Retrieving item counts[](pyzotero_readthedocs_io_en_latest.md#retrieving-item-counts "Link to this heading")
-------------------------------------------------------------------------------------------------------------------

If you wish to retrieve item counts for subsets of a library, you can use the following methods:

Zotero.num\_items()[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.num_items "Link to this definition")

Returns the count of top-level items in the library

Return type:

int

Zotero.num\_collectionitems(_collectionID_)[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.num_collectionitems "Link to this definition")

Returns the count of items in the specified collection

Return type:

int

Retrieving last modified version[](pyzotero_readthedocs_io_en_latest.md#retrieving-last-modified-version "Link to this heading")
--------------------------------------------------------------------------------------------------------------------------------

If you wish to retrieve the last modified version of a user or group library, you can use the following method:

Zotero.last\_modified\_version()[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.last_modified_version "Link to this definition")

Returns the last modified version of the library

Return type:

int

Search / Request Parameters for Read API calls[](pyzotero_readthedocs_io_en_latest.md#search-request-parameters-for-read-api-calls "Link to this heading")
--------------------------------------------------------------------------------------------------------------------------------

Additional parameters may be set on Read API methods **following any required parameters**, or set using the [`Zotero.add_parameters()`](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.add_parameters "zotero.Zotero.add_parameters") method detailed below.

The following two examples produce the same result:

> \# set parameters on the call itself
> z \= zot.top(limit\=7, start\=3)
> 
> \# set parameters using explicit method
> zot.add\_parameters(limit\=7, start\=3)
> z \= zot.top()

The following parameters are **optional**.

**You may also set a search term here, using the ‘itemType’, ‘q’, ‘qmode’, or ‘tag’ parameters**.

This area of the Zotero Read API is under development, and may change frequently. See [the API documentation](https://www.zotero.org/support/dev/web_api/v3/basics#read_requests) for the most up-to-date details of search syntax usage and export format details.

> Zotero.add\_parameters(\[_format=None_, _itemKey=None_, _itemType=None_, _q=None_, _qmode=None_, _since=None_, _tag=None_, _sort=None_, _direction=None_, _limit=None_, _start=None_\[, _content=None_\[, _style=None_\]\]\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.add_parameters "Link to this definition")
> 
> Parameters:
> 
> **format** (_str_) – “atom”, “bib”, “bibtex”, json”, “keys”, “versions”. Pyzotero retrieves and decodes JSON responses by default
> 
> Attention
> 
> Setting `format='bib'` will remove the `limit` parameter if it’s been set, as **the API does not allow a limit on bibliography output**; it instead enforces a limit of 150 items, and if the set of items you are trying to generate a bibliography for exceeds 150, an error will be raised.
> 
> Parameters:
> 
> **itemKey** (_str_) – A comma-separated list of item keys. Valid only for item requests. Up to 50 items can be specified in a single request
> 
> Search parameters:
> 
> Parameters:
> 
> *   **itemType** (_str_) – item type search. See the [Search Syntax](https://www.zotero.org/support/dev/web_api/v3/basics#search_syntax) for details
>     
> *   **q** (_str_) – Quick search. Searches titles and individual creator fields by default. Use the `qmode` parameter to change the mode. Currently supports phrase searching only
>     
> *   **qmode** (_str_) – Quick search mode. To include full-text content in the search, use `everything`. Defaults to `titleCreatorYear`. Searching of other fields will be possible in the future
>     
> *   **since** (_int_) – default `0`. Return only objects modified after the specified library version
>     
> *   **tag** (_str_) –
>     
>     tag search. See the [Search Syntax](https://www.zotero.org/support/dev/web_api/v3/basics#search_syntax) for details. More than one tag may be passed by passing a list of strings – These are treated as `AND` search terms, meaning only items which include all of the specified tags are returned. You can search for items matching any tag in a list by using `OR`: `"tag1 OR tag2"`, and all items which exclude a tag: `"-tag"`.
>     
> 
> The following parameters can be used for search requests:
> 
> Parameters:
> 
> *   **sort** (_str_) – The name of the field by which entries are sorted: (`dateAdded`, `dateModified`, `title`, `creator`, `type`, `date`, `publisher`, `publicationTitle`, `journalAbbreviation`, `language`, `accessDate`, `libraryCatalog`, `callNumber`, `rights`, `addedBy`, `numItems`, `tags`)
>     
> *   **direction** (_str_) – `asc` or `desc`
>     
> *   **limit** (_int_) – 1 – 100 or None
>     
> *   **start** (_int_) – 1 – total number of items in your library or None
>     
> 
> If you wish to retrieve citation or bibliography entries, use the following parameters:
> 
> Parameters:
> 
> *   **content** (_str_) – ‘bib’, ‘html’, or one of the export formats (see below). If ‘bib’ is passed, you may **also** pass:
>     
> *   **style** (_str_) – Any valid CSL style in the Zotero style repository
>     
> *   **linkwrap** (_str_) – Set this to “1” to have URLs in bibliography entries (see below) wrapped in `<a>` tags.
>     
> 
> Return type:
> 
> list of HTML strings or None.

Note

Any parameters you set will be valid **for the next call only**. Any parameters set using `add_parameters()` will be overridden by parameters you pass in the call itself.

A note on the `content` and `style` parameters:

Example:

> zot.add\_parameters(content\='bib', style\='mla')

If these are set, the return value is a list of UTF-8 formatted HTML `div` elements, each containing an item:

`['<div class="csl-entry">(content)</div>']`.

You may also set `content='citation'` if you wish to retrieve citations. Similar to `bib`, the result will be a list of one or more HTML `span` elements.

If you select one of the available [export formats](https://www.zotero.org/support/dev/web_api/v3/basics#export_formats) as the `content` parameter, pyzotero will in most cases return a list of unicode strings in the format you specified. The exception is the `csljson` format, which is parsed into a list of dicts. Please note that you must provide a `limit` parameter if you specify one of these export formats. Multiple simultaneous retrieval of particular formats, e.g. `content="json,coins"` is not currently supported.

If you set `format='keys'`, a newline-delimited string containing item keys will be returned

If you set `format='bibtex'`, a [bibtexparser](https://bibtexparser.readthedocs.io/en/v0.6.2/bibtexparser.html#bibdatabase.BibDatabase.entries) object containing citations will be returned. You can access the citations as a list of dicts using the `.entries` property. The bibtexparser object also implements a [dump method](https://bibtexparser.readthedocs.io/en/v0.6.2/bibtexparser.html#bibtexparser.dump), if you’d like to write your citations to a `.bib` file.

Write API Methods[](pyzotero_readthedocs_io_en_latest.md#write-api-methods "Link to this heading")
---------------------------------------------------------------------------------------------------------

Saved Searches[](pyzotero_readthedocs_io_en_latest.md#saved-searches "Link to this heading")
---------------------------------------------------------------------------------------------------

Pyzotero allows you to retrieve, delete, or modify saved searches:

> Zotero.searches()[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.searches "Link to this definition")
> 
> Retrieve all saved searches. Note that this retrieves saved search _metadata_, as opposed to _content_; saved searches cannot currently (January 2019) be run using the API.
> 
> Return type:
> 
> list of dicts
> 
> Zotero.saved\_search(_name_, _conditions_)[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.saved_search "Link to this definition")
> 
> Create a new saved search. conditions is a list of one or more dicts, each of which must contain the following three string keys: condition, operator, value. See the [documentation](https://www.zotero.org/support/dev/web_api/v3/write_requests#saved_search_requests) for an example.
> 
> Parameters:
> 
> *   **name** (_str_) – the name of the search
>     
> *   **conditions** (_list_) – one or more dicts containing search conditions and operators
>     
> 
> Return type:
> 
> dict showing creation success status
> 
> Zotero.delete\_saved\_search(_search\_keys_)[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.delete_saved_search "Link to this definition")
> 
> Delete one or more saved searches. search\_keys is a list of one or more search keys. These can be retrievd using [`Zotero.searches()`](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.searches "zotero.Zotero.searches")
> 
> Parameters:
> 
> **search\_keys** (_list_) – list of unique saved search keys
> 
> Return type:
> 
> None
> 
> Zotero.show\_operators()[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.show_operators "Link to this definition")
> 
> Show available saved search operators
> 
> Return type:
> 
> list
> 
> Zotero.show\_conditions()[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.show_conditions "Link to this definition")
> 
> Show available saved search conditions
> 
> Return type:
> 
> list
> 
> Zotero.show\_condition\_operators(_condition_)[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.show_condition_operators "Link to this definition")
> 
> Show available operators for a given saved search condition
> 
> Parameters:
> 
> **condition** (_str_) – a valid saved search condition
> 
> Return type:
> 
> list

Item Methods[](pyzotero_readthedocs_io_en_latest.md#item-methods "Link to this heading")
-----------------------------------------------------------------------------------------------

> Zotero.item\_types()[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.item_types "Link to this definition")
> 
> Returns a dict containing all available item types
> 
> Return type:
> 
> dict
> 
> Zotero.item\_fields()[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.item_fields "Link to this definition")
> 
> Returns a dict of all available item fields
> 
> Return type:
> 
> dict
> 
> Zotero.item\_creator\_types(_itemtype_)[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.item_creator_types "Link to this definition")
> 
> Returns a dict of all valid creator types for the specified item type
> 
> Parameters:
> 
> **itemtype** (_str_) – a valid Zotero item type. A list of available item types can be obtained by the use of [`item_types()`](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.item_types "zotero.Zotero.item_types")
> 
> Return type:
> 
> dict
> 
> Zotero.creator\_fields()[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.creator_fields "Link to this definition")
> 
> Returns a dict containing all localised creator fields
> 
> Return type:
> 
> dict
> 
> Zotero.item\_type\_fields(_itemtype_)[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.item_type_fields "Link to this definition")
> 
> Returns all valid fields for the specified item type
> 
> Parameters:
> 
> **itemtype** (_str_) – a valid Zotero item type. A list of available item types can be obtained by the use of [`item_types()`](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.item_types "zotero.Zotero.item_types")
> 
> Return type:
> 
> list of dicts
> 
> Zotero.item\_template(_itemtype_, _linkmode_)[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.item_template "Link to this definition")
> 
> Returns an item creation template for the specified item type
> 
> Parameters:
> 
> *   **itemtype** (_str_) – a valid Zotero item type. A list of available item types can be obtained by the use of [`item_types()`](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.item_types "zotero.Zotero.item_types")
>     
> *   **linkmode** (_str_) – either None (default) or a valid Zotero linkMode value required when itemtype is attachment. A list of available link modes can be obtained by the use of `item_attachment_link_modes()`
>     
> 
> Return type:
> 
> dict

### Creating and Updating Items[](pyzotero_readthedocs_io_en_latest.md#creating-and-updating-items "Link to this heading")

> Zotero.create\_items(_items_\[, _parentid_, _last\_modified_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.create_items "Link to this definition")
> 
> Create Zotero library items
> 
> Parameters:
> 
> *   **items** (_list_) – one or more dicts containing item data
>     
> *   **parentid** (_str_) – A Parent item ID. This will cause the item(s) to become the child items of the given parent ID
>     
> *   **last\_modified** (_str/int_) – If not None will set the value of the If-Unmodified-Since-Version header.
>     
> 
> Return type:
> 
> list of dicts
> 
> Returns a copy of the created item(s), if successful. Use of [`item_template()`](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.item_template "zotero.Zotero.item_template") is recommended in order to first obtain a dict with a structure which is known to be valid.
> 
> Before calling this method, the use of [`check_items()`](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.check_items "zotero.Zotero.check_items") is encouraged, in order to confirm that the item to be created contains only valid fields.
> 
> Note that if any items contain a key field matching an existing item on the server it will be updated (any properties not in the dict will be left unmodified).

Example:

> template \= zot.item\_template('book')
> template\['creators'\]\[0\]\['firstName'\] \= 'Monty'
> template\['creators'\]\[0\]\['lastName'\] \= 'Cantsin'
> template\['title'\] \= 'Maris Kundzins: A Life'
> resp \= zot.create\_items(\[template\])

If successful, `resp` will be a dict containing the creation status of each item:

> {'failed': {}, 'success': {'0': 'ABC123'}, 'unchanged': {}}
> 
> Zotero.update\_item(_item_\[, _last\_modified_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.update_item "Link to this definition")
> 
> Update an item in your library
> 
> Parameters:
> 
> *   **item** (_dict_) – a dict containing item data. Fields not in item will be left unmodified.
>     
> *   **last\_modified** (_str/int_) – If not `None`, will set the value of the If-Unmodified-Since-Version header. If unspecified/None then If-Unmodified-Since-Version will be set to the version property of item.
>     
> 
> Return type:
> 
> Boolean
> 
> Will return `True` if the request was successful, or will raise an error.

Example:

> > i \= zot.items()
> > \# see above for example of returned item structure
> > \# modify the latest item which was added to your library
> > i\[0\]\['data'\]\['title'\] \= 'The Sheltering Sky'
> > i\[0\]\['data'\]\['creators'\]\[0\]\['firstName'\] \= 'Paul'
> > i\[0\]\['data'\]\['creators'\]\[0\]\['lastName'\] \= 'Bowles'
> > zot.update\_item(i\[0\])
> 
> Zotero.update\_items(_items_)[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.update_items "Link to this definition")
> 
> Update items in your library. The API only accepts 50 items per call, so longer updates are chunked
> 
> Parameters:
> 
> **items** (_list_) – a list of dicts containing Item data. Fields not in item will be left unmodified.
> 
> Return type:
> 
> Boolean
> 
> Will return `True` if the request was successful, or will raise an error.
> 
> Zotero.check\_items(_items_)[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.check_items "Link to this definition")
> 
> Check whether items to be created on the server contain only valid keys. This method first creates a set of valid keys by calling [`item_fields()`](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.item_fields "zotero.Zotero.item_fields"), then compares the user-created dicts to it. If any keys in the user-created dicts are unknown, a `InvalidItemFields` exception is raised, detailing the invalid fields.
> 
> Parameters:
> 
> **items** (_list_) – one or more dicts containing item data
> 
> Return type:
> 
> List. Each list item is a valid dict containing item data.

### Uploading files[](pyzotero_readthedocs_io_en_latest.md#uploading-files "Link to this heading")

> Warning
> 
> Attachment methods are in beta.
> 
> Zotero.attachment\_simple(_files_\[, _parentid_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.attachment_simple "Link to this definition")
> 
> Create one or more file attachment items.
> 
> Parameters:
> 
> *   **files** (_list_) – a list containing one or more file paths: `['/path/to/file/file.pdf', … ]`
>     
> *   **parentid** (_string_) – a library Item ID. If this is specified, attachments will be created as child items of this ID.
>     
> 
> Return type:
> 
> Dict. Showing status of each requested upload.
> 
> Zotero.attachment\_both(_files_\[, _parentid_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.attachment_both "Link to this definition")
> 
> Create one or more file attachment items, specifying names for uploaded files
> 
> Parameters:
> 
> *   **files** (_list_) – a list containing one or more lists or tuples in the following format: `(file name, file path)`
>     
> *   **parentid** (_string_) – a library Item ID. If this is specified, attachments will be created as child items of this ID.
>     
> 
> Return type:
> 
> Dict. Showing status of each requested upload.
> 
> Zotero.upload\_attachments(_attachments_\[, _parentid_, _basedir=None_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.upload_attachments "Link to this definition")
> 
> Upload files to their corresponding attachments. If the attachments lack the `key` property they are assumed not to exist and will be created. The `parentid` parameter is **not compatible** with existing attachments. In order for uploads to succeed, the filename parameter of each attachment must resolve.
> 
> This method is really only required in cases where a sync has been interrupted, leaving your library with attachment items that don’t have corresponding files attached. It _may_ also work for uploading modified files, though **this is untested**.
> 
> Parameters:
> 
> *   **attachments** (_list_) – A list of dicts representing zotero imported files which may or may not already have their key fields filled in.
>     
> *   **parentid** (_string_) – a library Item ID. If this is specified and key fields are not included, attachments will be created as child items of this ID.
>     
> *   **basedir** (_string/path_) – A string or path object to which the filenames specified in attachments will be evaluated relative to. If unspecified the filenames are evaluated as they are.
>     
> 
> Return type:
> 
> Dict. Showing status of each requested upload.
> 
> \# example of the return type
> {
>     'success': \[attach1, attach2...\],
>     'failure': \[attach3, attach4...\],
>     'unchanged': \[attach4, attach5...\]
> }
> 
> Note
> 
> unlike the space-saving responses from the server, the return value here eschews the complex index / key lookup and simply passes back the `imported_file` item template populated with keys (if created successfully or passed in) corresponding to each result. This is the return type for all of these methods.

### Deleting items[](pyzotero_readthedocs_io_en_latest.md#deleting-items "Link to this heading")

> Zotero.delete\_item(_item_\[, _last\_modified_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.delete_item "Link to this definition")
> 
> Delete one or more items from your library
> 
> Parameters:
> 
> *   **item** (_list_) – a list of one or more dicts containing item data. You must first retrieve the item(s) you wish to delete, as `version` data is required.
>     
> *   **last\_modified** (_str/int_) – If not `None`, will set the value of the If-Unmodified-Since-Version header.
>     

### Deleting tags[](pyzotero_readthedocs_io_en_latest.md#deleting-tags "Link to this heading")

> Zotero.delete\_tags(_tag\_a_\[, _tag …_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.delete_tags "Link to this definition")
> 
> Delete one or more tags from your library
> 
> Parameters:
> 
> **tag** (_string_) – the tag(s) you’d like to delete
> 
> You may also pass a list using `zot.delete_tags(*[tag_list])`

Adding tags[](pyzotero_readthedocs_io_en_latest.md#adding-tags "Link to this heading")
---------------------------------------------------------------------------------------------

> Zotero.add\_tags(_item_, _tag_\[, _tag …_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.add_tags "Link to this definition")
> 
> Add one or more tags to an item, and update it on the server
> 
> Parameters:
> 
> *   **item** (_dict_) – a dict containing item data
>     
> *   **tag** (_string_) – the tag(s) you’d like to add to the item
>     
> 
> Return type:
> 
> list of dicts
> 
> You may also pass a list using `zot.add_tags(item, *[tag_list])`

Example:

> z \= zot.top(limit\=1)
> \# we've now retrieved the most recent top-level item
> updated \= zot.add\_tags(z\[0\], 'tag1', 'tag2', 'tag3')
> \# updated now contains a representation of the updated server item

Collection Methods[](pyzotero_readthedocs_io_en_latest.md#collection-methods "Link to this heading")
-----------------------------------------------------------------------------------------------------------

> Zotero.create\_collections(_dicts_\[, _last\_modified_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.create_collections "Link to this definition")
> 
> Create a new collection in the Zotero library
> 
> Parameters:
> 
> *   **dicts** (_list_) – list of dicts each containing the key `name`, with each value being a new collection name you wish to create. Each dict may optionally contain a `parentCollection` key, the value of which is the ID of an existing collection. If this is set, the collection will be created as a child of that collection.
>     
> *   **last\_modified** (_str/int_) – If not None will set the value of the If-Unmodified-Since-Version header.
>     
> 
> Return type:
> 
> list of dicts
> 
> Return type:
> 
> Boolean
> 
> Zotero.create\_collection(_dicts_\[, _last\_modified_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.create_collection "Link to this definition")
> 
> Alias for [`Zotero.create_collections()`](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.create_collections "zotero.Zotero.create_collections") to preserve backward compatibility
> 
> Zotero.addto\_collection(_collection_, _item_)[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.addto_collection "Link to this definition")
> 
> Add the specified item(s) to the specified collection
> 
> Parameters:
> 
> *   **collection** (_str_) – a collection key
>     
> *   **item** (_dict_) – an item dict retrieved using an API call
>     
> 
> Return type:
> 
> Boolean
> 
> Collection keys can be obtained by a call to [`collections()`](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.collections "zotero.Zotero.collections") (see details above).
> 
> Zotero.deletefrom\_collection(_collection_, _item_)[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.deletefrom_collection "Link to this definition")
> 
> Remove the specified item from the specified collection
> 
> Parameters:
> 
> *   **collection** (_str_) – a collection key
>     
> *   **item** (_dict_) – a dict containing item data
>     
> 
> Return type:
> 
> Boolean
> 
> See the [`delete_item()`](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.delete_item "zotero.Zotero.delete_item") example for multiple-item removal.
> 
> Zotero.update\_collection(_collection , last\_modified\]_)[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.update_collection "Link to this definition")
> 
> Update existing collection metadata (name etc.)
> 
> Parameters:
> 
> **collection** (_dict_) – a dict containing collection data, previously retrieved using one of the Collections calls (e.g. [`collections()`](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.collections "zotero.Zotero.collections"))
> 
> Return type:
> 
> Boolean
> 
> Zotero.update\_collections(_collection\_items_)[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.update_collections "Link to this definition")
> 
> Update multiple existing collection metadata. The API only accepts 50 collections per call, so longer updates are chunked
> 
> Parameters:
> 
> **collection\_items** (_list_) – a list of dicts containing Collection data. Fields not in collection\_item will be left unmodified.
> 
> Return type:
> 
> Boolean
> 
> Will return `True` if the request was successful, or will raise an error.
> 
> Zotero.collection\_tags(_collectionID_\[, _search/request parameters_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.collection_tags "Link to this definition")
> 
> Retrieve all tags for a given collection
> 
> Parameters:
> 
> **collectionID** (_str_) – a collection ID
> 
> Return type:
> 
> list of strings

Examples:

> \# get existing collections, which will return a list of dicts
> c \= zot.collections()
> \# rename the last collection created in the library
> c\[0\]\['name'\] \= 'Whither Digital Humanities?'
> \# update collection name on the server
> zot.update\_collection(c\[0\])
> 
> Zotero.delete\_collection(_collection_\[, _last\_modified_\])[](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.delete_collection "Link to this definition")
> 
> Delete a collection from the Zotero library
> 
> Parameters:
> 
> *   **collection** (_dict_) – a dict containing collection data, previously retrieved using one of the Collections calls (e.g. [`collections()`](pyzotero_readthedocs_io_en_latest.md#zotero.Zotero.collections "zotero.Zotero.collections")). Alternatively, you may pass a **list** of collection dicts.
>     
> *   **last\_modified** (_str/int_) – If not None will set the value of the If-Unmodified-Since-Version header.
>     
> 
> Return type:
> 
> Boolean

Notes[](pyzotero_readthedocs_io_en_latest.md#notes "Link to this heading")
---------------------------------------------------------------------------------

Most Read API methods return **lists** of **dicts** or, in the case of tag methods, **lists** of **strings**. Most Write API methods return either `True` if successful, or raise an error. See `zotero_errors.py` for a full listing of these.

Warning

URL parameters will supersede API calls which should return e.g. a single item: `https://api.zotero.org/users/436/items/ABC?start=50&limit=10` will return 10 items beginning at position 50, even though `ABC` does not exist. Be aware of this, and don’t pass URL parameters which do not apply to a given API method.

License[](pyzotero_readthedocs_io_en_latest.md#license "Link to this heading")
-------------------------------------------------------------------------------------

Pyzotero is licensed under the [Blue Oak Model Licence](https://opensource.org/license/blue-oak-model-license) license.

Cat Picture[](pyzotero_readthedocs_io_en_latest.md#cat-picture "Link to this heading")
---------------------------------------------------------------------------------------------

This is The Grinch.

![Image 1: _images/cat.png](https://pyzotero.readthedocs.io/en/latest/_images/cat.png)

_Orangecat_[](pyzotero_readthedocs_io_en_latest.md#id4 "Link to this image")
