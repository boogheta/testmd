# API documentation

Hyphe relies on a [JsonRPC](http://www.jsonrpc.org/) API that can be controlled easily through the web interface or called directly from a JsonRPC client.

_Note:_ as it relies on the JSON-RPC protocol, it is not quite easy to test the API methods from a browser (having to send arguments through POST), but you can test directly from the command-line using the dedicated tools, see the [Developpers' documentation](dev.md).


## Default API commands (no namespace)
### CORPUS HANDLING

- __`test_corpus`:__
 + _`corpus`_ (optional, default: `"--hyphe--"`)
 + _`_msg`_ (optional, default: `null`)

 Returns the current status of a `corpus`: "ready"/"starting"/"stopped"/"error".


- __`list_corpus`:__

 Returns the list of all existing corpora with metas.


- __`get_corpus_options`:__
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns detailed settings of a `corpus`.


- __`set_corpus_options`:__
 + _`corpus`_ (optional, default: `"--hyphe--"`)
 + _`options`_ (optional, default: `null`)

 Updates the settings of a `corpus` according to the keys/values provided in `options` as a json object respecting the settings schema visible by querying `get_corpus_options`. Returns the detailed settings.


- __`create_corpus`:__
 + _`name`_ (optional, default: `"--hyphe--"`)
 + _`password`_ (optional, default: `""`)
 + _`options`_ (optional, default: `{}`)
 + _`_noloop`_ (optional, default: `false`)
 + _`_quiet`_ (optional, default: `false`)

 Creates a corpus with the chosen `name` and optional `password` and `options` (as a json object see `set/get_corpus_options`). Returns the corpus generated id and status.


- __`start_corpus`:__
 + _`corpus`_ (optional, default: `"--hyphe--"`)
 + _`password`_ (optional, default: `""`)
 + _`_noloop`_ (optional, default: `false`)
 + _`_quiet`_ (optional, default: `false`)
 + _`_create_if_missing`_ (optional, default: `false`)

 Starts an existing `corpus` possibly `password`-protected. Returns the new corpus status.


- __`stop_corpus`:__
 + _`corpus`_ (optional, default: `"--hyphe--"`)
 + _`_quiet`_ (optional, default: `false`)

 Stops an existing and running `corpus`. Returns the new corpus status.


- __`ping`:__
 + _`corpus`_ (optional, default: `null`)
 + _`timeout`_ (optional, default: `3`)

 Tests during `timeout` seconds whether an existing `corpus` is started. Returns "pong" on success or the corpus status otherwise.


- __`reinitialize`:__
 + _`corpus`_ (optional, default: `"--hyphe--"`)
 + _`_noloop`_ (optional, default: `false`)
 + _`_quiet`_ (optional, default: `false`)

 Resets completely a `corpus` by cancelling all crawls and emptying the MemoryStructure and Mongo data.


- __`destroy_corpus`:__
 + _`corpus`_ (optional, default: `"--hyphe--"`)
 + _`_quiet`_ (optional, default: `false`)

 Resets a `corpus` then definitely deletes anything associated with it.


- __`clear_all`:__

 Resets Hyphe completely: starts then resets and destroys all existing corpora one by one.

### CORE & CORPUS STATUS

- __`get_status`:__
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns global metadata on Hyphe's status and specific information on a `corpus`.

### BASIC PAGE DECLARATION (AND WEBENTITY CREATION)

- __`declare_page`:__
 + `_`url`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Indexes a `url` into a `corpus`. Returns the (newly created or not) associated WebEntity.


- __`declare_pages`:__
 + `_`list_urls`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Indexes a bunch of urls given as an array in `list_urls` into a `corpus`. Returns the (newly created or not) associated WebEntities.

### BASIC CRAWL METHODS

- __`listjobs`:__
 + _`list_ids`_ (optional, default: `null`)
 + _`from_ts`_ (optional, default: `null`)
 + _`to_ts`_ (optional, default: `null`)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns the list and details of all "finished"/"running"/"pending" crawl jobs of a `corpus`. Optionally returns only the jobs whose id is given in an array of `list_ids` and/or that was created after timestamp `from_ts` or before `to_ts`.


- __`crawl_webentity`:__
 + `_`webentity_id`_ (mandatory)
 + _`depth`_ (optional, default: `null`)
 + _`phantom_crawl`_ (optional, default: `false`)
 + _`status`_ (optional, default: `"IN"`)
 + _`startpages`_ (optional, default: `"startpages"`)
 + _`phantom_timeouts`_ (optional, default: `{}`)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Schedules a crawl for a `corpus` for an existing WebEntity defined by its `webentity_id` with a specific crawl `depth [int]`. Optionally use PhantomJS by setting `phantom_crawl` to "true" and adjust specific `phantom_timeouts` as a json object with possible keys `timeout`/`ajax_timeout`/`idle_timeout`. Sets simultaneously the WebEntity's status to "IN" or optionally to another valid `status` ("undecided"/"out"/"discovered)". Optionally defines the `startpages` strategy by starting the crawl either from the WebEntity's preset "startpages" or "prefixes" or already seen "pages".


- __`get_webentity_logs`:__
 + `_`webentity_id`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` crawl activity logs on a specific WebEntity defined by its `webentity_id`.

### HTTP LOOKUP METHODS

- __`lookup_httpstatus`:__
 + `_`url`_ (mandatory)
 + _`timeout`_ (optional, default: `30`)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Tests a `url` for `timeout` seconds using a `corpus` specific connection (possible proxy for instance). Returns the url's HTTP code.


- __`lookup`:__
 + `_`url`_ (mandatory)
 + _`timeout`_ (optional, default: `30`)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Tests a `url` for `timeout` seconds using a `corpus` specific connection (possible proxy for instance). Returns a boolean indicating whether `lookup_httpstatus` returned HTTP code 200 or a redirection code (301/302/...).



## Commands for namespace: "crawl."

- __`deploy_crawler`:__
 + _`corpus`_ (optional, default: `"--hyphe--"`)
 + _`_quiet`_ (optional, default: `false`)

 Prepares and deploys on the ScrapyD server a spider (crawler) for a `corpus`.


- __`delete_crawler`:__
 + _`corpus`_ (optional, default: `"--hyphe--"`)
 + _`_quiet`_ (optional, default: `false`)

 Removes from the ScrapyD server an existing spider (crawler) for a `corpus`.


- __`cancel_all`:__
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Stops all "running" and "pending" crawl jobs for a `corpus`.

 Cancels all current crawl jobs running or planned for a `corpus` and empty related mongo data.


- __`start`:__
 + `_`webentity_id`_ (mandatory)
 + `_`starts`_ (mandatory)
 + `_`follow_prefixes`_ (mandatory)
 + `_`nofollow_prefixes`_ (mandatory)
 + _`follow_redirects`_ (optional, default: `null`)
 + _`depth`_ (optional, default: `null`)
 + _`phantom_crawl`_ (optional, default: `false`)
 + _`phantom_timeouts`_ (optional, default: `{}`)
 + _`download_delay`_ (optional, default: `1`)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Starts a crawl for a `corpus` defining finely the crawl options (mainly for debug purposes):
  * a `webentity_id` associated with the crawl a list of `starts` urls to start from
  * a list of `follow_prefixes` to know which links to follow
  * a list of `nofollow_prefixes` to know which links to avoid
  * a `depth` corresponding to the maximum number of clicks done from the start pages
  * `phantom_crawl` set to "true" to use PhantomJS for this crawl and optional `phantom_timeouts` as an object with keys among `timeout`/`ajax_timeout`/`idle_timeout`
  * a `download_delay` corresponding to the time in seconds spent between two requests by the crawler.


- __`cancel`:__
 + `_`job_id`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Cancels a crawl of id `job_id` for a `corpus`.


- __`get_job_logs`:__
 + `_`job_id`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` activity logs of a specific crawl with id `job_id`.



## Commands for namespace: "store."
### DEFINE WEBENTITIES

- __`get_lru_definedprefixes`:__
 + `_`lru`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` a list of all possible LRU prefixes shorter than `lru` and already attached to WebEntities.


- __`declare_webentity_by_lruprefix_as_url`:__
 + `_`url`_ (mandatory)
 + _`name`_ (optional, default: `null`)
 + _`status`_ (optional, default: `null`)
 + _`startPages`_ (optional, default: `[]`)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Creates for a `corpus` a WebEntity defined for the LRU prefix given as a `url`. Optionally set the newly created WebEntity's `name` `status` ("in"/"ou"/"undecided"/"discovered") and list of `startPages`. Returns the newly created WebEntity.


- __`declare_webentity_by_lru`:__
 + `_`lru_prefix`_ (mandatory)
 + _`name`_ (optional, default: `null`)
 + _`status`_ (optional, default: `null`)
 + _`startPages`_ (optional, default: `[]`)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Creates for a `corpus` a WebEntity defined for a `lru_prefix`. Optionally set the newly created WebEntity's `name` `status` ("in"/"ou"/"undecided"/"discovered") and list of `startPages`. Returns the newly created WebEntity.


- __`declare_webentity_by_lrus`:__
 + `_`list_lrus`_ (mandatory)
 + _`name`_ (optional, default: `null`)
 + _`status`_ (optional, default: `null`)
 + _`startPages`_ (optional, default: `[]`)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Creates for a `corpus` a WebEntity defined for a set of LRU prefixes given as `list_lrus`. Optionally set the newly created WebEntity's `name` `status` ("in"/"ou"/"undecided"/"discovered") and list of `startPages`. Returns the newly created WebEntity.

### EDIT WEBENTITIES

- __`rename_webentity`:__
 + `_`webentity_id`_ (mandatory)
 + `_`new_name`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Changes for a `corpus` the name of a WebEntity defined by `webentity_id` to `new_name`.


- __`change_webentity_id`:__
 + `_`webentity_old_id`_ (mandatory)
 + `_`webentity_new_id`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Changes for a `corpus` the id of a WebEntity defined by `webentity_old_id` to `webenetity_new_id` (mainly for advanced debug use).


- __`set_webentity_status`:__
 + `_`webentity_id`_ (mandatory)
 + `_`status`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Changes for a `corpus` the status of a WebEntity defined by `webentity_id` to `status` (one of "in"/"out"/"undecided"/"discovered").


- __`set_webentities_status`:__
 + `_`webentity_ids`_ (mandatory)
 + `_`status`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Changes for a `corpus` the status of a set of WebEntities defined by a list of `webentity_ids` to `status` (one of "in"/"out"/"undecided"/"discovered").


- __`set_webentity_homepage`:__
 + `_`webentity_id`_ (mandatory)
 + `_`homepage`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Changes for a `corpus` the homepage of a WebEntity defined by `webentity_id` to `homepage`.


- __`add_webentity_lruprefixes`:__
 + `_`webentity_id`_ (mandatory)
 + `_`lru_prefixes`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Adds for a `corpus` a list of `lru_prefixes` (or a single one) to a WebEntity defined by `webentity_id`.


- __`rm_webentity_lruprefix`:__
 + `_`webentity_id`_ (mandatory)
 + `_`lru_prefix`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Removes for a `corpus` a `lru_prefix` from the list of prefixes of a WebEntity defined by `webentity_id. Will delete the WebEntity if it ends up with no LRU prefix left.


- __`add_webentity_startpage`:__
 + `_`webentity_id`_ (mandatory)
 + `_`startpage_url`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Adds for a `corpus` a list of `lru_prefixes` to a WebEntity defined by `webentity_id`.


- __`rm_webentity_startpage`:__
 + `_`webentity_id`_ (mandatory)
 + `_`startpage_url`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Removes for a `corpus` a `startpage_url` from the list of startpages of a WebEntity defined by `webentity_id.


- __`merge_webentity_into_another`:__
 + `_`old_webentity_id`_ (mandatory)
 + `_`good_webentity_id`_ (mandatory)
 + _`include_tags`_ (optional, default: `false`)
 + _`include_home_and_startpages_as_startpages`_ (optional, default: `false`)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Assembles for a `corpus` 2 WebEntities by deleting WebEntity defined by `old_webentity_id` and adding all of its LRU prefixes to the one defined by `good_webentity_id`. Optionally set `include_tags` and/or `include_home_and_startpages_as_startpages` to "true" to also add the tags and/or startpages to the merged resulting WebEntity.


- __`merge_webentities_into_another`:__
 + `_`old_webentity_ids`_ (mandatory)
 + `_`good_webentity_id`_ (mandatory)
 + _`include_tags`_ (optional, default: `false`)
 + _`include_home_and_startpages_as_startpages`_ (optional, default: `false`)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Assembles for a `corpus` a bunch of WebEntities by deleting WebEntities defined by a list of `old_webentity_ids` and adding all of their LRU prefixes to the one defined by `good_webentity_id`. Optionally set `include_tags` and/or `include_home_and_startpages_as_startpages` to "true" to also add the tags and/or startpages to the merged resulting WebEntity.


- __`delete_webentity`:__
 + `_`webentity_id`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Removes from a `corpus` a WebEntity defined by `webentity_id` (mainly for advanced debug use).

### RETRIEVE & SEARCH WEBENTITIES

- __`get_webentity`:__
 + `_`webentity_id`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` a WebEntity defined by its `webentity_id`.


- __`get_webentity_by_lruprefix`:__
 + `_`lru_prefix`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` the WebEntity having `lru_prefix` as one of its LRU prefixes.


- __`get_webentity_by_lruprefix_as_url`:__
 + `_`url`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` the WebEntity having one of its LRU prefixes corresponding to the LRU fiven under the form of a `url`.


- __`get_webentity_for_url`:__
 + `_`url`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` the WebEntity to which a `url` belongs (meaning starting with one of the WebEntity's prefix and not another).


- __`get_webentity_for_url_as_lru`:__
 + `_`lru`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` the WebEntity to which a url given under the form of a `lru` belongs (meaning starting with one of the WebEntity's prefix and not another).


- __`get_webentities`:__
 + _`list_ids`_ (optional, default: `null`)
 + _`sort`_ (optional, default: `null`)
 + _`count`_ (optional, default: `100`)
 + _`page`_ (optional, default: `0`)
 + _`light`_ (optional, default: `false`)
 + _`semilight`_ (optional, default: `false`)
 + _`light_for_csv`_ (optional, default: `false`)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` all existing WebEntities or only the WebEntities whose id is among `list_ids.
 Results will be paginated with a total number of returned results of `count` and `page` the number of the desired page of results. Results will include metadata on the request including the total number of results and a `token` to be reused to collect the other pages via `get_webentities_page`.
 Other possible options include:
  * order the results with `sort` by inputting a field or list of fields as named in the WebEntities returned objects; optionally prefix a sort field with a "-" to revert the sorting on it; for instance: `["-indegree", "name"]` will order by maximum indegree first then by alphabetic order of names
  * set `light` or `semilight` or `light_for_csv` to "true" to collect lighter data with less WebEntities fields.


- __`advanced_search_webentities`:__
 + _`allFieldsKeywords`_ (optional, default: `[]`)
 + _`fieldKeywords`_ (optional, default: `[]`)
 + _`sort`_ (optional, default: `null`)
 + _`count`_ (optional, default: `100`)
 + _`page`_ (optional, default: `0`)
 + _`autoescape_query`_ (optional, default: `true`)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` all WebEntities matching a specific search using the `allFieldsKeywords` and `fieldKeywords` arguments. Searched keywords will automatically be escaped: set `autoescape_query` to "false" to allow input of special Lucene queries.
 Results will be paginated with a total number of returned results of `count` and `page` the number of the desired page of results. Results will include metadata on the request including the total number of results and a `token` to be reused to collect the other pages via `get_webentities_page`.
  * `allFieldsKeywords` should be a string or list of strings to search in all textual fields of the WebEntities ("name"/"status"/"lruset"/"startpages"/...). For instance `["hyphe", "www"]`
  * `fieldKeywords` should be a list of 2-elements arrays giving first the field to search into then the searched value or optionally for the field "indegree" an array of a minimum and maximum values to search into. For instance: `[["name", "hyphe"], ["indegree", [3, 1000]]]`
  * see description of `sort` in `get_webentities` above.


- __`exact_search_webentities`:__
 + `_`query`_ (mandatory)
 + _`field`_ (optional, default: `null`)
 + _`sort`_ (optional, default: `null`)
 + _`count`_ (optional, default: `100`)
 + _`page`_ (optional, default: `0`)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` all WebEntities having one textual field or optional specific `field` exactly equal to the value given as `query`. Searched query will automatically be escaped of Lucene special characters.
 Results are paginated and will include a `token` to be reused to collect the other pages via `get_webentities_page`: see `advanced_search_webentities` for explanations on `sort` `count` and `page`.


- __`prefixed_search_webentities`:__
 + `_`query`_ (mandatory)
 + _`field`_ (optional, default: `null`)
 + _`sort`_ (optional, default: `null`)
 + _`count`_ (optional, default: `100`)
 + _`page`_ (optional, default: `0`)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` all WebEntities having one textual field or optional specific `field` beginning with the value given as `query`. Searched query will automatically be escaped of Lucene special characters.
 Results are paginated and will include a `token` to be reused to collect the other pages via `get_webentities_page`: see `advanced_search_webentities` for explanations on `sort` `count` and `page`.


- __`postfixed_search_webentities`:__
 + `_`query`_ (mandatory)
 + _`field`_ (optional, default: `null`)
 + _`sort`_ (optional, default: `null`)
 + _`count`_ (optional, default: `100`)
 + _`page`_ (optional, default: `0`)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` all WebEntities having one textual field or optional specific `field` finishing with the value given as `query`. Searched query will automatically be escaped of Lucene special characters.
 Results are paginated and will include a `token` to be reused to collect the other pages via `get_webentities_page`: see `advanced_search_webentities` for explanations on `sort` `count` and `page`.


- __`free_search_webentities`:__
 + `_`query`_ (mandatory)
 + _`field`_ (optional, default: `null`)
 + _`sort`_ (optional, default: `null`)
 + _`count`_ (optional, default: `100`)
 + _`page`_ (optional, default: `0`)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` all WebEntities having one textual field or optional specific `field` containing the value given as `query`. Searched query will automatically be escaped of Lucene special characters.
 Results are paginated and will include a `token` to be reused to collect the other pages via `get_webentities_page`: see `advanced_search_webentities` for explanations on `sort` `count` and `page`.


- __`get_webentities_by_status`:__
 + `_`status`_ (mandatory)
 + _`sort`_ (optional, default: `null`)
 + _`count`_ (optional, default: `100`)
 + _`page`_ (optional, default: `0`)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` all WebEntities having their status equal to `status` (one of "in"/"out"/"undecided"/"discovered").
 Results are paginated and will include a `token` to be reused to collect the other pages via `get_webentities_page`: see `advanced_search_webentities` for explanations on `sort` `count` and `page`.


- __`get_webentities_by_name`:__
 + `_`name`_ (mandatory)
 + _`sort`_ (optional, default: `null`)
 + _`count`_ (optional, default: `100`)
 + _`page`_ (optional, default: `0`)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` all WebEntities having their name equal to `name`.
 Results are paginated and will include a `token` to be reused to collect the other pages via `get_webentities_page`: see `advanced_search_webentities` for explanations on `sort` `count` and `page`.


- __`get_webentities_by_tag_value`:__
 + `_`value`_ (mandatory)
 + _`sort`_ (optional, default: `null`)
 + _`count`_ (optional, default: `100`)
 + _`page`_ (optional, default: `0`)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` all WebEntities having at least one tag in any namespace/category equal to `value`.
 Results are paginated and will include a `token` to be reused to collect the other pages via `get_webentities_page`: see `advanced_search_webentities` for explanations on `sort` `count` and `page`.


- __`get_webentities_by_tag_category`:__
 + `_`category`_ (mandatory)
 + _`sort`_ (optional, default: `null`)
 + _`count`_ (optional, default: `100`)
 + _`page`_ (optional, default: `0`)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` all WebEntities having at least one tag in a specific `category`.
 Results are paginated and will include a `token` to be reused to collect the other pages via `get_webentities_page`: see `advanced_search_webentities` for explanations on `sort` `count` and `page`.


- __`get_webentities_by_user_tag`:__
 + `_`category`_ (mandatory)
 + `_`value`_ (mandatory)
 + _`sort`_ (optional, default: `null`)
 + _`count`_ (optional, default: `100`)
 + _`page`_ (optional, default: `0`)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` all WebEntities having at least one tag in any category of the namespace "USER" equal to `value`.
 Results are paginated and will include a `token` to be reused to collect the other pages via `get_webentities_page`: see `advanced_search_webentities` for explanations on `sort` `count` and `page`.


- __`get_webentities_page`:__
 + `_`pagination_token`_ (mandatory)
 + `_`n_page`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` the page number `n_page` of WebEntities corresponding to the results of a previous query ran using any of the `get_webentities` or `search_webentities` methods using the returned `pagination_token`.


- __`get_webentities_ranking_stats`:__
 + `_`pagination_token`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)
 + _`_ranking_field`_ (optional, default: `"indegree"`)

 Returns for a `corpus` histogram data on the indegrees of all WebEntities matching a previous query ran using any of the `get_webentities` or `search_webentities` methods using the return `pagination_token`.

### TAGS

- __`add_webentity_tag_value`:__
 + `_`webentity_id`_ (mandatory)
 + `_`namespace`_ (mandatory)
 + `_`category`_ (mandatory)
 + `_`value`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Adds for a `corpus` a tag `namespace:category`_ (optional, default: `value` to a WebEntity defined by `webentity_id`.`)


- __`add_webentities_tag_value`:__
 + `_`webentity_ids`_ (mandatory)
 + `_`namespace`_ (mandatory)
 + `_`category`_ (mandatory)
 + `_`value`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Adds for a `corpus` a tag `namespace:category`_ (optional, default: `value` to a bunch of WebEntities defined by a list of `webentity_ids`.`)


- __`rm_webentity_tag_key`:__
 + `_`webentity_id`_ (mandatory)
 + `_`namespace`_ (mandatory)
 + `_`category`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Removes for a `corpus` all tags within `namespace:category` associated with a WebEntity defined by `webentity_id` if it is set.


- __`rm_webentity_tag_value`:__
 + `_`webentity_id`_ (mandatory)
 + `_`namespace`_ (mandatory)
 + `_`category`_ (mandatory)
 + `_`value`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Removes for a `corpus` a tag `namespace:category`_ (optional, default: `value` associated with a WebEntity defined by `webentity_id` if it is set.`)


- __`set_webentity_tag_values`:__
 + `_`webentity_id`_ (mandatory)
 + `_`namespace`_ (mandatory)
 + `_`category`_ (mandatory)
 + `_`values`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Replaces for a `corpus` all existing tags of a WebEntity defined by `webentity_id` for a specific `namespace` and `category` by a list of `values` or a single tag.


- __`get_tags`:__
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` a tree of all existing tags of the webentities hierarchised by namespaces and categories.


- __`get_tag_namespaces`:__
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` a list of all existing namespaces of the webentities tags.


- __`get_tag_categories`:__
 + _`namespace`_ (optional, default: `null`)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` a list of all existing categories of the webentities tags. Optionally limits to a specific `namespace`.


- __`get_tag_values`:__
 + _`namespace`_ (optional, default: `null`)
 + _`category`_ (optional, default: `null`)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` a list of all existing values in the webentities tags. Optionally limits to a specific `namespace` and/or `category`.

### PAGES
 + _`LINKS & NETWORKS

- __`get_webentity_pages`:__
 + `_`webentity_id`_ (mandatory)
 + _`onlyCrawled`_ (optional, default: `true`)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` all indexed Pages fitting within the WebEntity defined by `webentity_id`. Optionally limits the results to Pages which were actually crawled setting `onlyCrawled` to "true".


- __`get_webentity_subwebentities`:__
 + `_`webentity_id`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` all sub-webentities of a WebEntity defined by `webentity_id` (meaning webentities having at least one LRU prefix starting with one of the WebEntity's prefixes).


- __`get_webentity_parentwebentities`:__
 + `_`webentity_id`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` all parent-webentities of a WebEntity defined by `webentity_id` (meaning webentities having at least one LRU prefix starting like one of the WebEntity's prefixes).


- __`get_webentity_nodelinks_network`:__
 + _`webentity_id`_ (optional, default: `null`)
 + _`include_external_links`_ (optional, default: `false`)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` the list of all internal NodeLinks of a WebEntity defined by `webentity_id`. Optionally add external NodeLinks (the frontier) by setting `include_external_links` to "true".


- __`get_webentities_network`:__
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` the list of all agregated weighted links between WebEntities.

### CREATION RULES

- __`get_webentity_creationrules`:__
 + _`lru_prefix`_ (optional, default: `null`)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` all existing WebEntityCreationRules or only one set for a specific `lru_prefix`.


- __`delete_webentity_creationrule`:__
 + `_`lru_prefix`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Removes from a `corpus` an existing WebEntityCreationRule set for a specific `lru_prefix`.


- __`add_webentity_creationrule`:__
 + `_`lru_prefix`_ (mandatory)
 + `_`regexp`_ (mandatory)
 + _`apply_to_existing_pages`_ (optional, default: `false`)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Adds to a `corpus` a new WebEntityCreationRule set for a `lru_prefix` to a specific `regexp` or one of "subdomain"/"subdomain-N"/"domain"/"path-N"/"prefix+N"/"page" N being an integer. Optionally set `apply_to_existing_pages` to "true" to apply it immediately to past crawls.


- __`simulate_creationrules_for_urls`:__
 + `_`pageURLs`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns an object giving for each URL of `pageURLs` (single string or array) the prefix of the theoretical WebEntity the URL would be attached to within a `corpus` following its specific WebEntityCreationRules.


- __`simulate_creationrules_for_lrus`:__
 + `_`pageLRUs`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns an object giving for each LRU of `pageLRUs` (single string or array) the prefix of the theoretical WebEntity the LRU would be attached to within a `corpus` following its specific WebEntityCreationRules.

### PRECISION EXCEPTIONS

- __`get_precision_exceptions`:__
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a `corpus` the list of all existing PrecisionExceptions.


- __`delete_precision_exceptions`:__
 + `_`list_lru_exceptions`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Removes from a `corpus` a set of existing PrecisionExceptions listed as `list_lru_exceptions`.


- __`add_precision_exception`:__
 + `_`lru_prefix`_ (mandatory)
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Adds to a `corpus` a new PrecisionException for `lru_prefix`.

### VARIOUS

- __`trigger_links_reset`:__
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Will initiate a whole reset and regeneration of all WebEntityLinks of a `corpus`. Can take a while.


- __`get_webentities_stats`:__
 + _`corpus`_ (optional, default: `"--hyphe--"`)

 Returns for a corpus a set of statistics on the WebEntities status repartition of a `corpus` each 5 minutes.
