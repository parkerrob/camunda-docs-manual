---

title: 'Migrating a Cockpit Plugin'
category: 'Migrate from Camunda BPM 7.2 to 7.3'

---

As of version 7.3, the use of [ngDefine][ng-define] in Cockpit and Admin Plugins is deprecated. You are encouraged to use [requireJS][requirejs] instead. For information about the use of requireJS in plugins, see the [How to develop a Cockpit Plugin][howto-cockpit-plugin] Tutorial or the migration information below.

ngDefine remains part of the Cockpit and Admin app for backwards compatability, but may be removed in the future. ngDefine is not part of the Tasklist app. Tasklist plugins must be written using requireJS.

This change only affects the client side of the plugins. The server side does not have to be changed.

### Replace ngDefine with define call

With ngDefine, you could create an angular module with its dependencies using the ngDefine call:

```javascript
ngDefine('cockpit.plugin.myPlugin', [
  'jquery',
  'angular',
  'http://some-url/some-library.js',
  'module:some.other.angularModule:./someOtherModule.js'
], function(ngModule, $, angular) {
  // ...
});
```

From 7.3 onwards, you have to load dependencies using a define call and create and return the angular module in the callback:

```javascript
define([
  'jquery',
  'angular',
  'http://some-url/some-library.js',
  './someOtherModule.js'
], function($, angular) {

  var ngModule = angular.module('cockpit.plugin.myPlugin', ['some.other.angularModule']);

  // ...

  return ngModule;
});
```


### angular-ui 
In the 7.3 release of the [Admin][admin] and [Cockpit][cockpit] UIs, the [angular-ui][angular-ui] which is __not supported anymore__ has been, partially, replaced by [angular-bootstrap][angular-bootstrap].

Custom Cockpit plugins might have use directives or filters provided by [angular-ui][angular-ui] and therefore need to be reviewed.

### Hints
Typically, you can skip this if you do not have custom plugins, otherwise you might want to have a look at the templates of your custom plugins (because it is where filters and directives are expected to be used).

Directives which are __not available anymore__:

- `ui-animate`
- `ui-calendar`
- `ui-codemirror`
- `ui-currency`
- `ui-date`
- `ui-event`
- `ui-if`
- `ui-jq`
- `ui-keypress`
- `ui-map`
- `ui-mask`
- `ui-reset`
- `ui-route`
- `ui-scrollfix`
- `ui-select2`
- `ui-showhide`
- `ui-sortable`
- `ui-tinymce`
- `ui-validate`

Filters which are __not availabe anymore__:

- `format`
- `highlight`
- `inflector`
- `unique`


[ng-define]: http://nikku.github.io/requirejs-angular-define
[requirejs]: http://requirejs.org
[howto-cockpit-plugin]: ref:/real-life/how-to/#cockpit-how-to-develop-a-cockpit-plugin
[admin]: https://github.com/camunda-/camunda-admin-ui
[cockpit]: https://github.com/camunda-/camunda-cockpit-ui
[angular-ui]: https://github.com/angular-ui/angular-ui-OLDREPO
[angular-bootstrap]: https://github.com/angular-ui/bootstrap