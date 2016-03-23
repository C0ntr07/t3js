---
layout: documentation
title: API - Context
permalink: /docs/api/context/
---

# Context
The object type that modules use to interact with the environment.
Used exclusively within Box.Application, but exposed publicly for easy testing.

<div class="anchor" id="getElement"></div>

## getElement

### Description
Returns the element that represents the module.

### Returns
An HTMLElement object.

### Example
{% highlight javascript %}
Box.Application.addModule('abc', function(context) {
    return {
        init: function() {
            // outputs HTMLElement with id 'mod-test-module'
            console.log(context.getElement());
        }
    };
});
{% endhighlight %}

<hr class="separator">

<div class="anchor" id="getConfig"></div>

## getConfig

### Description
Retrieves a module's configuration data from embedded JSON in a 'text/x-config' script tag.
This method is a proxy to <a href="../application/#getModuleConfig">Box.Application.getModuleConfig</a> but with a shorter name.

### Usage
<table class="table table-striped">
    <thead>
        <tr>
            <th>Parameter</th>
            <th>Type</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td class="optional">name</td>
            <td>string</td>
            <td>Specific configuration value to retrieve.</td>
        </tr>
    </tbody>
</table>

### Example
{% highlight html %}
<div id="mod-test-module" data-module="test-module">
    <script type="text/x-config">{"foo": "bar"}</script>
    ...
</div>
{% endhighlight %}

{% highlight javascript %}
Box.Application.addModule('abc', function(context) {
    return {
        init: function() {
            // Outputs "bar"
            console.log(context.getConfig('foo'));
        }
    };
});
{% endhighlight %}

<hr class="separator">

# Proxy Methods

<hr class="separator">

<div class="anchor" id="broadcast"></div>

## broadcast

### Description
Broadcasts a message to all registered listeners. A proxy to <a href="../application/#broadcast">Box.Application.broadcast</a>

### Usage
<table class="table table-striped">
    <thead>
        <tr>
            <th>Parameter</th>
            <th>Type</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td class="required">name</td>
            <td>string</td>
            <td>Name of the message</td>
        </tr>
        <tr>
            <td class="optional">data</td>
            <td>any</td>
            <td>Custom parameters for the message</td>
        </tr>
    </tbody>
</table>

### Example
{% highlight html %}
<div id="mod-search-bar" data-module="search-bar">
    ...
</div>
{% endhighlight %}

{% highlight javascript %}
Box.Application.addModule('search-bar', function(context) {
    return {
        search: function() {
            context.broadcast('searchcomplete', {
                numResults: 100
            });
        }
    };
});
{% endhighlight %}

<hr class="separator">

<div class="anchor" id="getService"></div>

## getService

### Description
Retrieves an instance of a registered service.
A proxy to <a href="../application/#getService">Box.Application.getService</a>

### Usage
<table class="table table-striped">
    <thead>
        <tr>
            <th>Parameter</th>
            <th>Type</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td class="required">service</td>
            <td>string</td>
            <td>Name of service.</td>
        </tr>
    </tbody>
</table>

### Returns
T3 Service or null.

### Example
{% highlight javascript %}
Box.Application.addModule('abc', function(context) {
    var dom;

    return {
        init: function() {
            dom = context.getService('dom');
        }
    };
});
{% endhighlight %}

<hr class="separator">

<div class="anchor" id="getGlobal"></div>

## getGlobal

### Description
Returns a global variable. This function exists to make accessing globals more explicit.

### Usage
<table class="table table-striped">
    <thead>
        <tr>
            <th>Parameter</th>
            <th>Type</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td class="required">name</td>
            <td>string</td>
            <td>Name of global.</td>
        </tr>
    </tbody>
</table>

### Returns
The global variable if it exists or null.


### Example
{% highlight javascript %}
var navigator = context.getGlobal('navigator');
console.log(navigator.userAgent);
{% endhighlight %}

<hr class="separator">

<div class="anchor" id="getGlobalConfig"></div>

## getGlobalConfig

### Description
Retrieves a configuration value that was passed through init.
A proxy to  <a href="../application/#getGlobalConfig">Box.Application.getGlobalConfig</a>

### Usage
<table class="table table-striped">
    <thead>
        <tr>
            <th>Parameter</th>
            <th>Type</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td class="required">key</td>
            <td>string</td>
            <td>Config key.</td>
        </tr>
    </tbody>
</table>

### Returns
Object.

### Example
{% highlight javascript %}
Box.Application.init({
    username: 'bob'
});

Box.Application.addModule('abc', function(context) {
    return {
        init: function() {
            // Outputs "bob"
            console.log(context.getGlobalConfig('username'));
        }
    };
});
{% endhighlight %}
