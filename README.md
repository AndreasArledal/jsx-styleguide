jsx-styleguide
==============

A small set of neat styles for JSX derived from working with React.

Annotate files
only required with React 0.11.2 or if you want to safe guard.

```js
//Bad
/**
  * @jsx React.DOM
*/

//Good
/** @jsx React.DOM */

//Good
/**
  * @jsx React.DOM
  * Other documentation...
*/
```

___

###Initial data
Your component probably needs some initial data, this is good, you're allowed two different types of loading of data.
    
Fetching from DOM.

```js
//Bad
<div id="first-name">
  Henrik
</div>
var name = $('#first-name').val();
React.renderComponent(<MyComponent name={name} />);
 
//Good
<div id="first-name" data-name="Henrik">Henrik</div>
var div = document.querySelector('#first-name);
React.renderComponent(<MyComponent data={div.dataset} />);
```

___

Fetching from an API

```js
//Bad
React.createClass({
    componentWillMount: function () {
        this.loadTranslations();
        this.loadData();
    },
    loadData / loadTranslations: function () {
        $.ajax(
            //LOTS OF JQUERY ARARRRGH!
        );
    }
});

//Good
var Api = require('../_api_client');
React.createClass({
  componentWillMount: function () {
    this.loadData(); //only one call to API
  },
  loadData: function () {
    var myPromise = Api.get/put/post/destroy('endpoint/12', someData);
    //use .bind() if you want to use `this.setState()` within your callbacks.
    //or put success/error handlers on your React class to get around using .bind();
    var success = function () {};
    var error = function () {}; 
 
    myPromise.then(success, error);
  }
});
```

___

###Referencing props or state in a components render function
When referencing props or state in the render function, there should be no spaces around the curlies.

```js
//Bad
render: function () {
    return (
      <div>
        { this.props.porcupine }
      </div>
    );
} 

//Good
render: function () {
    return (
      <div>
        {this.props.porcupine}
      </div>
    )
}
```

___

###Empty state variables in `getInitialState`

```js
//Bad
getInitialState: function () {
  return {
    age: 12, 
    name: undefined
  }; 
}
 
//Good - always prefer null to undefined.
getInitialState: function () {
  return {
    age: 12,
    name: null
  };
}
```

___

###Check state in render

```js
//Bad
render: function () {
  if(this.state.age === undefined) {
    return (<div>No data found</div>);
  } else {
    return (<div>Your age is {this.state.age}</div>);
  }
}
 
//Good
render: function () {
  if(this.state.age) {
    return (
      <div>Your age is {this.state.age}</div>
    );
  } else {
    return (
      <div>No data to render</div>
    );
  }
}
```

___

###Render multiple new React components

```js
//Bad
render: function () {
  return (
    <ul>
    {this.state.listItems.map(function(item) {
        return (<ListItem data={item.title} />)
      }) 
    }
    </ul>
  );
}

//Good
render: function () {
  var listItems = _.map(this.state.listItems, function(item) {
    return (<ListItem data={item.title} key={item.id} />);
  });
  return (
    <ul>
      {listItems}
    </ul>
  );
}
```

___

###Formatting of tags

Shorter and simpler tags

```js
//Bad
render: function () {
  var someHref = 'http://www.github.com/';
  var classes = cx({
    'active': true
  });
  var someId = "github-link";
  return (
    <div>
      <a href={someHref}
         id={someId}
         className={someClasses}
      > 
      {this.props.myLinkText}
      </a>
    </div>
  );
}

//Good
render: function () {
  var someHref = 'http://www.github.com/';
  var classes = cx({
    'active': true
  });
  var someId = "github-link";
  return (
    <div>
      <a href={someHref} className={classes} id={someId}>{this.props.myLinkText}</a>
    </div>
  )
}
```
