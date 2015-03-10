mimetype-filter
===============

## Note
**This plugin has been merged in the trunk of the Nutch project. Starting from version 1.10 this plugin will be bundled with the Nutch distribution**

Nutch 1.x plugin that allows to filter the indexed documents by the MIME type property of the crawled web pages. Basically this will allow you to restrict the MIME type of the contents that will be stored in Solr/Elasticsearch index without the need to restrict the crawling/parsing process, so no need to use URLFilter plugins family. Also this address one particular corner case when certain URLs doesn't have any format to filter such as some RSS feeds (`http://www.awesomesite.com/feed`) and it will end in your index mixed with all your HTML pages.

Configuration
-------------

A custom configuration file that will hold the rules that can be specified using the a property `mimetype.filter.file` in your `nutch-site.xml` file:

```xml
<property>
    <name>mimetype.filter.file</name>
    <value>mimetype-filter.txt</value>
</property>
```

If no `mimetype.filter.file` key is found in your `nutch-site.xml` file an `allow` policy is used instead, so all your crawled documents will be indexed.

The rules configuration file use the same format as the `urlfilter-suffix` plugin, basically it starts with a general policy (`+` to allow everything and `-` to block everything) an it follows a list of exceptions to the general rule, so:

```
-
image
```

Will block all mimetypes except those that contain image in the MIME type string extracted by Tika, wich will allow the indexing of all kind of images.

```
+
text/html
```

On the other hand this second example allows the indexing of every document type but blocks the `text/html`.

**NOTE**: Keep in mind that the rules that follows the global policy are evaluated as regular expressions.
