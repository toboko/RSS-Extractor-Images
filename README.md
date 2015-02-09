RSS - Extractor images
===

Sometimes in your website, you need to print news about other website or make re-link for advertising or other types.
With this PHP script you can extract: images,titles,posts,details,... all the references that you can find inside a **RSS/Feed** file.

You only need to know the url to RSS/Feed file.

>Setup

```php
global $text, $maxchar, $end;
$first_img = '';
$rss = new DOMDocument();
$rss->load('http://yourwebsite.org/?feed=rss2');
```

> First Step: push the element inside the array
         
```php     
$feed = array();
foreach ($rss->getElementsByTagName('item') as $node) {
    $item = array (
        'title'=> $node->getElementsByTagName('title')->item(0)->nodeValue,
        'link' => $node->getElementsByTagName('link')->item(0)->nodeValue,
        'date' => $node->getElementsByTagName('pubDate')->item(0)->nodeValue,
        'content' => $node->getElementsByTagName('encoded')->item(0)->nodeValue,
    );
    array_push($feed, $item);
}
```

> Second Step: extract the src from img tag

```php
$title       = str_replace(' & ', ' &amp; ', $feed[$x]['title']);
$link        = $feed[$x]['link'];
$description = $feed[$x]['desc'];
$content     = $feed[$x]['content'];
$description = substr($description, 0, 100);
$pubDate     = date('l F d, Y', strtotime($feed[$x]['date']));

preg_match_all('/src=([\'"])?(.*?)\\1/', $content, $matches);
$first_img   = $matches[0][0];
```

> Check the complete code inside **engine.php**