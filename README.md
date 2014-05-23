# toDOM : fast, light and simple JavaScript DOM generator
---------------------------------------------------------
toDOM is a simple library allowing you to generate DOM element on the fly with JavaScript, using a straightforward JSON input.

It is designed to be fast and flexible. You can build any DOM element you want using toDOM.

## When to use?
You might want to use it when you make a lot of (costly) DOM operations, like :
* adding / removing elements from the document ;
* handling events (clicks, mouseups, mousemoves, ...) ;
* interacting with elements (change of style, change of content, ...).

## Why use it?
Since you build your elements directly from your JavaScript code, you don't have to make any additional DOM call in order to get a handle of them. It will result in a boost in performance and will make your life a lot easier when you have complex elements to build on the fly.

## Installing toDOM
Just include toDOM.min.js or toDOM.js somewhere in your < body > and you are ready to go!

## Usage
### Basic
```javascript
var element = toDOM({
  tag : 'div',
  innerHTML : 'Hello World!'
});

document.body.appendChild(element);
```

This will result in :
```html
<body>
    <!-- ... -->
    <div>
        Hello World!
    </div>
</body>
```

### Keeping references
``` javascript
var scope = {};

document.body.appendChild(
  toDOM({
    tag : 'div',
    as : 'myReference'
  }, scope)
);

scope.myReference; // is a reference to the created element
scope.myReference.innerHTML = 'Hello World!';
```

### Setting attributes
```javascript
document.body.appendChild(
    toDOM({
        tag : 'input',
        attr : {
            id : 'randy',
            type : 'password'
            // or any attribute that this type of element can have
        }
    })
);
```

### Adding style
```javascript
document.body.appendChild(
    toDOM({
      style : {
          width : '10px',
          height : '10px',
          backgroundColor : 'red'
          // or any CSS property specified in CamelCase
      }
    })
);
```
You might have seen that I haven't given any tag this time. It's OK, trust me... by default the tag is set to "div".

### Adding events
```javascript
document.body.appendChild(
    toDOM({
        innerHTML : 'Click me!',
        events : {
            click : function() {
                console.log('clicked!');
            },
            mousedown : function() {
                console.log('mousedowned!');
            }
            // or any DOM event that you want to use, you just need to remove the "on" prefix
        }
    })
);
```

### With children
toDOM is actually recursive! So you can use it like this :
```javascript
var scope = {};
document.body.appendChild(
    toDOM({
        tag : 'table',
        children : [
            { tag : 'tr',
              children : [
                { tag : 'td I guess' },
                { tag : 'td this is' },
                { tag : 'td ...',
                  as : 'missingText'
                }
              ]
            },
            { tag : 'tr',
              children : [
                { tag : 'td Don\'t' },
                'td you', // simplified notation
                'td think?'
                ]
            }
        ]
    }, scope)
);

setTimeout(function() {
  scope.missingText.innerHTML = (toDOM === 'cool') ? 'awesome!' : 'awesome!';
}.bind(this), 2000);
```

### Shortcuts
One of toDOM's purpose is to help you write less code without losing readability. It exposes some shortcuts to avoid the loss of being too explicit. You can set classes and ids without using the "attr" key :

```javascript
document.body.appendChild(
    toDOM({
        tag : '#my-id.my-class.my-other-class My login form',
        children : [
            { tag : 'input#login' },
            { tag : 'input#password',
              attr : {
                type : 'password'
              }
            }
        ]
    })
);
```

## sandjs version
There is also a sand.js version of toDOM, which is exactly the same except it uses the [sand.js](https://github.com/KspR/sandjs) module manager, so that it is properly defined and usable as a module (it's the right way to use it if you are building a big app).

## Who uses toDOM?
* [Whibo](http://whibo.com) (collaborative real-time whiteboard)

## Authors 
* [Sam Ton That](https://github.com/KspR)
* [Pierre Cole](https://github.com/piercus)