#[Grid](https://github.com/digitalvapor/grid)
by [Tom Spalding](https://github.com/digitalvapor)

A [Pelican](https://github.com/getpelican/pelican) plugin that can easily include your Etsy shop in a page or post. Supported in the [icosahedron](https://github.com/digitalvapor/icosahedron) theme.

This is *only* meant for a few most recent items to display. There are some major to-do items, such as adding more than just Etsy support in the future.

##Settings
In `pelicanconf.py` set the following:
```
ETSY_STORE = 0000000                      #your store id
PLUGINS = ['grid']                        #clone this repo into your plugin folder
ETSY_API_KEY = 'aaaaaaaa1aaaaa22222bbbb3' #apply for an Etsy API key
ETSY_SHOPNAME = 'storename'               #as in http://storename.etsy.com
```

##Screenshots
###Desktop
![large](screenshot0.png 'largish example')
###Mobile
![small](screenshot1.png 'smallish example')

##Metadata
You can call the following metadata:
```
page.etsy_shopname
page.etsy_store
page.store_url
page.num_items
page.item_title[i]
page.item_url[i]
page.item_desc[i]
page.item_quantity[i]
page.item_tags[i]
page.item_id[i]
page.item_img0[i]
page.item_img1[i]
page.item_img2[i]
page.item_img3[i]
```
This plugin truncates the item's description at the first newline. If you format your descriptions with newlines then you can easily keep things brief. The multiple item images are great for the responsive designer within.

##Template Examples
This sample code below uses the [Grid Items](http://refills.bourbon.io/#grid-items) Bourbon Refill. Use my [theme](https://github.com/digitalvapor/icosahedron) if you want it to display the same.

###page.html
Set `Shoptype: etsy` in your page or post markdown to use the plugin.

Include this in your `page.html` template.

```
{% if ETSY_API_KEY %}
    {% include 'shop.html' %}
{% endif %}
```

####shop.html
Use this as your `shop.html` template.

```
{% if page.shoptype == 'etsy' %}
<div class="grid-items-lines">
  {% for item in page.item_id %}
  	{% set i = loop.index0 %}
    <a href="{{ page.item_url[i] }}" class="grid-item{% if i % 5 == 0 %} grid-item-two{% endif %}">
      <img class="thumb" src="{{ page.item_img1[i] }}" alt="{{ page.item_title[i] }}">
      <img class="full" src="{{ page.item_img2[i] }}" alt="{{ page.item_title[i] }}">
      <h1>{{ page.item_title[i] }}</h1>
      <p>{{ page.item_desc[i] }}</p>
    </a>
  {% endfor %}
  <div class="right-cover"></div>
  <div class="bottom-cover"></div>
</div>
<nav class="actions"><a href="{{ page.store_url }}" title="{{ page.etsy_shopname }}">Visit our Shop!</a></nav>
{% endif %}
```

###article.html
Set specific items to show off in a post with the following frontmatter. Each of the item numbers corresponds to your listed Etsy item.

That is, `https://www.etsy.com/listing/187423319/polypachyderm-9-x-12` means you use `187423319`. Setting `Shop: etsy` is a flag for the plugin to include the listed items.

```
Shop: etsy
Items: 187009679,187423319
```

Include this in your `article.html` template.

```
{% if ETSY_API_KEY %}
    {% include 'item.html' %}
{% endif %}
```

####item.html
Use this as your `item.html` template.
```
{% if article.shop == 'etsy' %}
<div class="grid-items-lines">
  {% for item in article.item_id %}
    {% set i = loop.index0 %}
    <a href="{{ article.item_url[i] }}" class="grid-item grid-item-three">
      <img class="thumb" src="{{ article.item_img1[i] }}" alt="{{ article.item_title[i] }}">
      <img class="full" src="{{ article.item_img2[i] }}" alt="{{ article.item_title[i] }}">
      <h1>{{ article.item_title[i] }}</h1>
      <p>{{ article.item_desc[i] }}</p>
    </a>
  {% endfor %}
  <div class="right-cover"></div>
  <div class="bottom-cover"></div>
</div>
{% endif %}
```

##License
This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](https://creativecommons.org/licenses/by-sa/4.0/).
