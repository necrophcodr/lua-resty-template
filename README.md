#lua-resty-template

**lua-resty-template** is a templating engine for OpenResty.

## Hello, World with lua-resty-template

##### Lua
```lua
local template = require "resty.template"
-- Using template.new
local view = template.new("view.html")
view.message  = "Hello, World!"
view.render()
-- Using template.render
template.render("view.html", { message = "Hello, World!" })
```

##### view.html
```html
<!DOCTYPE html>
<html>
<body>
  <h1>{{message}}</h1>
</body>
</html>
```

##### Output
```html
<!DOCTYPE html>
<html>
<body>
  <h1>Hello, World!</h1>
</body>
</html>
```

## Template Syntax

You may use the following tags in templates:

* `{{expression}}`, writes result of expression - html escaped
* `{*expression*}`, writes result of expression 
* `{% lua code %}`, executes Lua code
* `{( template )}`, includes `template` file

#### Example
##### Lua
```lua
template.render("view.html", {
  title   = "Testing lua-resty-template",
  message = "Hello, World!"
  names   = { "James", "Jack", "Anne" },
  jquery  = '<script src="//ajax.googleapis.com/ajax/libs/jquery/2.1.0/jquery.min.js"></script>' 
})
```

##### view.html
```html
{(header.html)}
<h1>{{message}}</h1>
<ul>
{% for _, name in ipairs(names) do %}
    <li>{{name}}</li>
{% end %}
</ul>
{(footer.html)}
```

##### header.html
```html
<!DOCTYPE html>
<html>
<head>
  <title>{{title}}</title>
  {*jquery*}
</head>
<body>
```

##### footer.html
```html
</body>
</html>
```

## Lua API
#### template.new

Creates a new template instance that is used as a context when `render`ed.

`local view = template.new("template.html")`

##### Example
```lua
local template = require "resty.template"
local view = template.new("view.html")
view.message  = "Hello, World!"
view.render()
--You may also pass additional context to render:
view.render({ title = "Testing lua-resty-template" })
```

#### template.compile

Compiles, and caches a template and returns the compiled template as a function that takes context as a parameter and returns rendered template as a string.

`local func = template.compile("template.html")`

##### Example
```lua
local template = require "resty.template"
local func     = template.compile("view.html")
local world    = func({ message = "Hello, World!" })
local universe = func({ message = "Hello, Universe!" })
print(world, universe)
```

#### template.render

Compiles, and outputs template either with `ngx.print` if available, or `print`.

`template.render("template.html", { message = "Hello, World!" })`

##### Example
```lua
local template = require "resty.template"
template.render("view.html", { message = "Hello, World!" })
template.render("view.html", { message = "Hello, Universe!" })
```

## Alternatives

You may also look at these:

* etlua (https://github.com/leafo/etlua)
* lustache (https://github.com/Olivine-Labs/lustache)
* mixlua (https://github.com/LuaDist/mixlua)
* tirtemplate (https://github.com/torhve/LuaWeb/blob/master/tirtemplate.lua)

`lua-resty-template` *was originally forked from Tor Hveem's `tirtemplate.lua` that he had extracted from Zed Shaw's Tir web framework (http://tir.mongrel2.org/).*
