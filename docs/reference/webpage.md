# The WebPage Class

The `WebPage` class is used to instantiate web pages. A JustPy request handler must always return an instance of this class or of a class that inherits from this class.

## Attributes

### components

 * Type: `list`
 * Default: `[]`

List of components on the page. Only direct children are on the list.

### use_websockets
 
 * Type: `boolean`
 * Default: `True`
 
Example:
 
```python
import justpy as jp
wp = jp.WebPage()
wp.use_websockets = False
```

Disables Websockets and uses Ajax instead for the page. 

!!! tip "You can disable Websockets by default for all pages by setting the `websockets` keyword argument of `justpy` to `False`. [examples](/tutorial/ajax)"
 
### delete_flag
 
 * Type: `boolean`
 * Default: `True`
 * Usage:
 
```python
import justpy as jp
wp = jp.WebPage()
wp.delete_flag = False
```

When `delete_flag` is `False` the `WebPage` instance is not deleted when the page closes. Furthermore, none of the elements on the page are deleted either.

[example](/tutorial/basic_concepts/#creating-web-pages-once)
 
 
### highcharts_theme
 
 * Type: `string` or `None`
 * Default: `None`
 
 Sets the theme for the Higcharts charts on the page.
 
 Can be one of:  
 `['avocado', 'dark-blue', 'dark-green', 'dark-unica', 'gray', 'grid-light', 'grid', 'high-contrast-dark', 'high-contrast-light', 'sand-signika', 'skies', 'sunset']`
 
### page_id
 
 * Type: `int`
 * Default: Automatically generated by JustPy
 
 When a `WebPage` instance is created (instantiated), an id is automatically generated for it. The id is the key in the dictionary that holds all `WebPage`s. 
 
 When an event occurs in the browser, the id of the page is sent together with other information about the event. You usually do not need to use this attribute as the WebPage instance itself (along with the id) is provided to the event handlers.
 
!!! danger "Do not change this attribute." 
 
### template_file
 
 * Type: `string`
 * Default: 'tailwind.html'
 
 Can be either 'tailwind.html' or 'quasar.html'.
 

### title
 
 * Type: `string`
 * Default: `JustPy`
 
The title of the page and the label on the browser tab.
 
```python
import justpy as jp

input_classes = "m-2 bg-gray-200 border-2 border-gray-200 rounded w-64 py-2 px-4 text-gray-700 focus:outline-none focus:bg-white focus:border-purple-500"

async def input_demo(request):
    wp = jp.WebPage()

    def change_title_and_url(self, msg):
        msg.page.display_url = self.value
        msg.page.title = self.value

    in1 = jp.Input(a=wp, classes=input_classes, placeholder='Type here to change title and url', input=change_title_and_url)
    return wp

jp.justpy(input_demo)
```
 
### display_url
 
 * Type: `string`
 * Default: `None`
 
The URL to display in the browser address, see example above. If `None`, url not set
 
### redirect
 
 * Type: `string`
 * Default: `None`
 
Url to redirect to. If `None`, no redirection is attempted
 
### open
 
 * Type: `string`
 * Default: `None`
 
Url to open in new tab. If `None`, `False` or the empty string, no new tab is opened.
 
 
### favicon
 
 * Type: `string`
 * Default: `None`
 
Url to favicon. If `None`, `False` or the empty string, the default favicon is used.
 
### css
 
 * Type: `string`
 * Default: `None`
 
CSS to inject into the page. The string is inserted in style tag in the head of the document.
 
 
### head_html and body_html
 
 * Type: `string`
 * Default: `''`
 
 From version 0.0.7
 
A string of any extra HTML to put in the head section or body section of the template. Can be additional JavaScript for example.
```python
wp.head_html ="""
<script src="/my_scripts.js"></script>
"""
```

Simple example:
```python
import justpy as jp

def test_head():
    wp = jp.WebPage()
    wp.head_html = '<script>console.log("Hello there from head");</script>'
    wp.body_html = '<script>console.log("Hello there from body");</script>'
    jp.Div(text='Testing...', a=wp)
    return wp

jp.justpy(test_head)
```
 
### html
 
 * Type: `string`
 * Default: `''` (the empty string)
 
If not the empty string, overrides everything else and sets the HTML on the page to this value.
 
### body_style
 
 * Type: `string`
 * Default: `''` (the empty string)
 
Sets the style attribute for the body html element of the page
 
### body_classes
 
 * Type: `string`
 * Default: `''` (the empty string)
 
Sets the classes for the body html element of the page
 
### dark
 
 * Type: `boolean`
 * Default: `False`
 
Only applicable currently with the Quasar pages. Set to true for Quasar's dark mode. 

 
### data
 
 * Type: `dictionary`
 * Default: `{}` (the empty dictionary)
 
See [Tutorial link](/tutorial/model_and_data/#introduction-and-examples)
 
### reload_interval
 
 * Type: `float`
 * Default: `None`

See [Tutorial link](/tutorial/ajax/#ajax-page-reload)
    
    
### use_cache

* Type: `boolean`
* Default: `False`

If `True` the rendering process does not call the page's `build_list` method and uses the value of the `cache` attribute instead.

### cache

Place the cache content in this attribute   
    

## Methods

### `add_component(self, child, position=None)`  
Adds a child component at the specified position. If position is `None` or not specified, adds component as the last child.

### `add(self, *args)`  
Adds all arguments as child elements at last position.

### `remove_component(self, component)` or `remove(self, component)`  
Removes a component from the page if it is there. If not, raises an exception.

### `get_components(self)`  
Returns a list of all the elements on the page. 

### `last(self)`  
Returns the last element on the page (`self.components[-1]`)

### `__len__(self)`  
`len(wp)` returns the number of direct children elements the page has.

### `delete_components(self)`
Deletes all the components on the page. This calls all the components' `delete` functions. This removes any reference to them from the internal JustPy data structures and allows garbage collection

### `remove_page(self)`  
Removes the reference to the page for the page directory. If there was no other reference to the page, the Python garbage collector will eventually reclaim the memory used by the page.

### `to_html(self, indent=0, indent_step=0, format=True)`  
Returns an HTML string representing the page. Set `indent_step` to an integer larger than 0 to make output human readable.

### `build_list(self)`  
This is the method JustPy calls to render the page. This method converts all elements on the page to dictionaries and creates a list with these dictionaries. The result of `build_list` is the input to the Vue app that renders the page in the browser.

### `react(self)`  
This method does nothing in the original class. It is is called by `build_list` just before the list is actually built. If you want to preform some action just before the page is rendered, write a new page class that inherits from WebPage and overrides `react`.

### `async def update(self)`  
This method updates all browser tabs that the page is rendered on.  JustPy keeps track of all the websocket connections to the rendered pages and uses them to update the page.

### `async def delayed_update(self, delay)`  
Same as `update` but with a delay. The argument specifies the delay in seconds.

### `on_disconnect(self, websocket=None)`  
Override if you want to do something special when one of the tabs the page is rendered on closes.

```python
import justpy as jp

class MyPage(jp.WebPage):

    async def on_disconnect(self, websocket=None):
        await super().on_disconnect()
        # Do what you want to do on disconnect here
        print('Page disconnected')


def close_test():
    wp = MyPage()
    for i in range(10):
        jp.Div(text=f'Div {i}', a=wp, classes='bg-blue-500 text-white m-2 p-2 w-32')
    return wp

jp.justpy(close_test)

```

### `async def run_javascript(self, javascript_string, request_id='', send=True)`

Runs JavaScript code on the page.
The method has two keyword arguments:

- `request_id`: A user supplied identification for the run request. Can be used in combination with the `result_ready` page event to obtain the result of the computation

- `send`: By default, the result of the command is sent to the server. In some cases, you may need to execute a promise and handle sending to the server from within your code (see second example below). In those cases, set `send` to `False` to stop the result being sent twice.

Simple example:

```python
import justpy as jp

async def my_click(self, msg):
    await msg.page.run_javascript("""
    for (var i = 0; i<10; i++) {
        console.log('Line: ' + i);
        }
    """)

def javascript_test():
    wp = jp.WebPage()
    d = jp.Div(a=wp, classes='m-2')
    b = jp.Button(text='Run Script', a=d, classes=jp.Styles.button_simple, click=my_click)
    return wp

jp.justpy(javascript_test)

```

Getting the browser location:

```python
import justpy as jp


javascript_string = """
var options = {
  enableHighAccuracy: true,
  timeout: 5000,
  maximumAge: 0
};

function success(pos) {
  var crd = pos.coords;

  console.log('Your current position is:');
  console.log(`Latitude : ${crd.latitude}`);
  console.log(`Longitude: ${crd.longitude}`);
  console.log(`More or less ${crd.accuracy} meters.`);
  e.result = {latitude: crd.latitude, longitude: crd.longitude, accuracy: crd.accuracy};
  send_to_server(e, 'page_event', false);
}

function error(err) {
  console.warn(`ERROR(${err.code}): ${err.message}`);
  e.result = 'Error';
  send_to_server(e, 'page_event', false);
}

navigator.geolocation.getCurrentPosition(success, error, options);
"""

async def result_ready(self, msg):
    if msg.request_id == 'geo_location':
        msg.page.add(jp.Div(text=f'longitude: {msg.result.longitude} Latitude: {msg.result.latitude} Accuracy: {msg.result.accuracy}',
                        classes='m-2 p-2 text-lg border'))


async def page_ready(self, msg):
    jp.run_task(self.run_javascript(javascript_string, request_id='geo_location', send=False))


def result_ready_test():
    wp = jp.WebPage()
    wp.on('page_ready', page_ready)
    wp.on('result_ready', result_ready)
    # Some arbitrary content
    wp.d = jp.Div(classes='flex flex-wrap', a=wp)
    for i in range(1, 31):
        jp.Div(text=f'Div {i}', a=wp.d, classes='border m-2 p-2 text-xs')
    return wp


jp.justpy(result_ready_test)
```



### `async def reload(self)`

Forces a page reload.

```python
await wp.reload()
```
