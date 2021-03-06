
# Ember TML

This addon adds helpers for doing advanced localization of your ember apps with Translation Markup Language (TML) by Translation Exchange.

Translation Markup Language (TML) is a simple markup language that provides syntax for identifying dynamic data and decorations within strings. TML aims at abstracting out the decoration mechanisms of the string and instead provides its own simple, but powerful syntax. This allows for translation sharing across multiple applications and platforms.


## Installation

````javascript
ember install ember-tml
````


## Configuration

The first thing you need to do to get started with Ember TML is sign up for a free [Translation Exchange](https://translationexchange.com/) account and create your first project. 

Once you have created a project and have your project `key` and `token` you must configure the service:

````javascript
// config/environment.js
module.exports = function(environment) {
  var ENV = {
    tml: {
      key: "YOUR_PROJECT_KEY"
    }
  }
}
````


## Content Security Policy

If you're using the content security policy, you'll need to add the translationexchange domain to allow loading of remote scripts. 

In `config/environment.js`, add this to the ENV hash (modify as necessary):

````javascript
// config/environment.js
contentSecurityPolicy: {
  'default-src': "'none'",
  'script-src': "'self' *.translationexchange.com",
  'font-src': "'self'",
  'connect-src': "'self' *.translationexchange.com",
  'frame-src': "'self' *.translationexchange.com",
  'img-src': "'self' *",
  'style-src': "'self' *",
  'media-src': "'self' *"
}
````


## Usage

The Ember TML Addon provides a few helpers and a service:


#### {{tr}} Helper
`tr` is the basic translate function. It takes 3 parameters:

* `label` is the only required parameter.
* `description` is an optional parameter, but should always be used if the label by itself is not sufficient enough to provide the meaning of the phrase.
* `tokens` is an optional parameter that contains a hash of token values to be substituted in the label.

This is what it should look like in your templates:

````handlebars
<div>{{tr "Hello World" }}</div>
<div>{{tr "Invite" "Link to invite your friends"}}</div>
<div>{{tr "Welcome {user}!" user=userName}}</div>
<div>{{tr "You have {count || message, messages}" count=1}}</div>
<div>{{tr "You have {count || message, messages}" count=5}}</div>
````

yields:

````handlebars
<div>Hello World</div>
<div>Invite</div>
<div>Welcome Jane!</div>
<div>You have 1 message</div>
<div>You have 5 messages</div>
````

#### {{trl}} Helper
`trl` works the same as `tr` but should be used for attribute values.

````handlebars
<img src="..." title={{trl "Hello World"}} />
{{input placeholder=(trl "Enter email address")}}
````

yields:

````handlebars
<img src="..." title="Hello World" />
<input placeholder="Enter email address" />
````

You can find more on how to use TML in the docs at [Translation Exchange](https://translationexchange.com/docs/tml/basics)


### API

#### TML Service Api

The TML service will be injected into `Controllers`, `Routes`, `Views` and `Components`.

````javascript
Ember.get(this, 'tml').trl("Hello World");
````
and includes some handy methods for working with TML:

**`currentTranslator`**  
an object representing the current logged in translator
````javascript
Ember.get(this, 'tml.currentTranslator');
// {
//   name: "translator_username",
//   inline: true
// }
````

**`currentSource`**  
a string of the current source
````javascript
Ember.get(this, 'tml.currentSource');
// "index"
````

**`currentApplication`**  
return an object representing your Translation Exchange application
````javascript
Ember.get(this, 'tml.currentApplication');
// {
//   id: 123,
//   key: "APP_KEY",
//   name: "Dom Examples",
//   current_locale: "zh",
//   default_locale: "en-US",
//   features: {...},
//   languages: [...]
// }
````

**`currentLanguage`**  
returns the currently selected language
````javascript
Ember.get(this, 'tml.currentLanguage');
// {
//   id: 233,
//   english_name: "Russian",
//   native_name: "Русский",
//   locale: "ru",
//   right_to_left: false,
//   flag_url: "https://s3-us-west-1.amazonaws.com/trex-snapshots/flags/default/languages/16/ru.png"
// }
````
**`availableLanguages`**  
returns a list of all available languages for your project

**`translationModeEnabled`**  
returns whether or not the application is currently in translation mode

**`translate(label [,description, tokens])`**  
**`tr(label [,description, tokens])`**  
works the same as the `tr` helper

**`translateLabel(label [,description, tokens])`**  
**`trl(label [,description, tokens])`**  
works the same as the `trl` helper

**`changeLanguage(locale)`**  
sets the current language


### Building a Language Selector
````handlebars
<ul>
  {{#each tml.availableLanguages as |language|}}
    <li>
      <a {{action (action tml.changeLanguage) language.locale}}>
        <img src={{language.flag_url}} />
        {{tr language.english_name}}
      </a>
    </li>
  {{/each}}
</ul>
````






