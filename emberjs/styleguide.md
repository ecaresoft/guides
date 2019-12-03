Guia de estilos
HBS
https://github.com/DockYard/styleguides/blob/master/engineering/handlebars.md
Orden de composicion de files
https://github.com/DockYard/styleguides/blob/master/engineering/ember.md#organizing-your-modules
Por contextos o alfabetico?
Routes:
Controllers ?? : https://github.com/DockYard/styleguides/blob/master/engineering/ember.md#organizing-your-modules
Components:
Templates ?? : https://github.com/DockYard/styleguides/blob/master/engineering/ember.md#organizing-your-modules

<b>Ordering static attributes, dynamic attributes, and action helpers for HTML elements</b>

!Make it easier for your teammates to read templates.
Order attributes and then action helpers this will provide clarity.

```hbs
{{! Bad }}

<button disabled={{isDisabled}} data-auto-id="click-me" {{action (action click)}} name="wonderful-button" class="wonderful-button">Click me</button>
```

```hbs
{{! Good }}

<button class="wonderful-button"
  data-auto-id="click-me"
  name="wonderful-button"
  disabled={{isDisabled}}
  onclick={{action click}}>
    Click me
</button>
```

Models: https://github.com/DockYard/styleguides/blob/master/engineering/ember.md#organizing-your-modules
Mixins:
Services:

Orden de llamada de propiedades de componentes en templates


Naming and contexts
Max levels
Context inherit
Standard para el get(obj, ‘properties’);
https://github.com/DockYard/styleguides/blob/master/engineering/ember.md#use-get-and-set

Cuando usar services, mixins & helpers (Differences)
Cómo manejar locales (?)
New syntaxis (Angular vs old syntaxis 2.11.2)
Alphabetical order or contexts?
Ember Data
Override init in components
https://dockyard.com/blog/2015/10/19/2015-dont-dont-override-init
Ember testing

### Naming conventions
https://guides.emberjs.com/release/object-model/classes-and-instances/
