jsx-styleguide
==============

A small set of neat styles for JSX derived from working with React.

Annotate files
only required with React 0.11.2 or if you want to safe guard.
    
    //Bad
    /**
      * @jsx React.DOM
    */
    //Good
    /** @jsx React.DOM */

Initial data
Your component probably needs some initial data, this is good, you're allowed two different types of loading of data.
    
    Fetching from DOM.
    //Bad
    <div id="first-name">
      Henrik
    </div>
    var name = $('#first-name').val();
    React.renderComponent(<MyComponent name={name} />);
     
    //Good
    <div id="first-name" data-name="Henrik"></div>
    var div = document.querySelector('#first-name);
    React.renderComponent(<MyComponent data={div.dataset} />);
 

    Fetching from an API
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
        var success = function () {};
        var error = function () {}; //use .bind() if you want to use `this.setState()` within your callbacks.
     
        myPromise.then(success, error);
      }
    });


Empty state variables in `getInitialState`

    //Bad
    getInitialState: function () {
      return {age: 12, name: undefined}; || return {age: 12, name: null}; 
    }
     
    //Good
    getInitialState: function () {
      return {
        age: 12
      };
    }

Check state in render

    //Bad
    render: function () {
      if(this.state.age !== undefined) {
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
    
Render multiple new React components

    //Bad
    render: function () {
      return (
        <ul>
        {_.map(this.state.listItems, function(item) {
            return (<ListItem data={item.title} />)
          }) 
        }
        </ul>
      );
    }
     
    //Good
    render: function () {
      var listItems = _.map(this.state.listItems, function(item) {
        return (<ListItem data={item.title} />);
      });
      return (
        <ul>
          {listItems}
        </ul>
      );
    }