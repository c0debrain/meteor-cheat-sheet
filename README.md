# Meteor Cheat sheet

## Meteor Core:

**Anywhere:**
```
Meteor.isClient
Meteor.isServer
Meteor.startup(func)
Meteor.absoluteUrl([path], [options])
Meteor.settings
Meteor.release
```

## Publish and Subscribe:

Server: ``Meteor.publish(name, func)``
```
  this.userId
  this.added(collection, id, fields)
  this.changed(collection, id, fields)
  this.removed(collection, id)
  this.ready()
  this.onStop(func)
  this.error(error)
  this.stop()
  this.connection
```

Client: 
```
Meteor.subscribe(name [, arg1, arg2, arg3][, callbacks])
```

## Methods:

Anywhere: ``Meteor.methods(methods)``
```
  this.userId
  this.setUserId  @only server side
  this.isSimulation
  this.unblock()
  this.connection  @only server side
```

**Anywhere:**
```
new Meteor.Error(error, reason, details)
Meteor.call(name, param1, param2, â€¦ [, asyncCallback])
Meteor.apply(name, params[, options][, asyncCallback])  
```

options: ``wait``, ``onResultReceived``

## Server Connections:

**Client:**
```
Meteor.status()
Meteor.reconnect()
Meteor.disconnect()
```

**Server:** ``Meteor.onConnection(callback)``

**Anywhere:** ``DDP.connect(url)``


## Collections:

**Anywhere:** 
```
new Meteor.Collection(name, [options])  @options: connection, idGeneration, transform
collection.find(selector, [options])  @options: sort, skip, limit, fields, reactive, transform
collection.findOne(selector, [options])  @options: sort, skip, fields, reactive, transform
collection.insert(doc, [callback])
collection.update(selector, modifier, [options], [callback])  @options: multi, upsert
collection.upsert(selector, modifier, [options], [callback])  @options: multi
collection.remove(selector, [callback])
collection.allow(options)  @options: insert, update, remove, fetch, transform
collection.deny(options)  @options: insert, update, remove, fetch, transform
cursor.forEach(callback, [thisArg])
cursor.map(callback, [thisArg])
cursor.fetch()
cursor.count()
cursor.rewind()
cursor.observe(callbacks)  ## callbacks: added(At), changed(At), removed(At), movedTo
cursor.observeChanges(callbacks)  ## callbacks: added(Before), changed, removed, movedBefore
new Meteor.Collection.ObjectID(hexString)
```

## Session:

**Client:**
```
Session.set(key, value)
Session.setDefault(key, value)
Session.get(key)
Session.equals(key, value)
```

## Accounts:

**Anywhere but publish functions:**
```
Meteor.user()
Meteor.userId()
```
**Anywhere:** ``Meteor.users``

**Client:**
```
Meteor.loggingIn()
Meteor.logout([callback])
Meteor.logoutOtherClients([callback])
Meteor.loginWithPassword(user, password, [callback])
Meteor.loginWithExternalService([options], [callback])  @options: requestPermissions, requestOfflineToken, forceApprovalPrompt
```

**Template:**
```
{{currentUser}}
{{loggingIn}}
```

**Anywhere:** 
```
Accounts.config(options)  @options: sendVerificationEmail, forbidClientAccountCreation, restrictCreationByEmailDomain, loginExpirationInDays
```

**Client:** 
```
Accounts.ui.config(options)  @options: requestPermissions, requestOfflineToken, passwordSignupFields
```

**Server:**
```
Accounts.validateNewUser(func)
Accounts.onCreateUser(func)
Accounts.validateLoginAttempt(func)
Accounts.onLogin(func)
Accounts.onLoginFailu(func)
```

## Passwords:

**Anywhere:** ``Accounts.createUser(options, [callback])  @options: username, email, password, profile``

**Client:** 
```
Accounts.changePassword(oldPassword, newPassword, [callback])
Accounts.forgotPassword(options, [callback])``  @options: ema
Accounts.resetPassword(token, newPassword, [callback])
Accounts.verifyEmail(token, [callback])
```

**Server**
```
Accounts.setPassword(userId, newPassword)
Accounts.sendResetPasswordEmail(userId, [email])
Accounts.sendEnrollmentEmail(userId, [email])
Accounts.sendVerificationEmail(userId, [email])
```

**Anywhere:** ``Accounts.emailTemplates``

## Templates:

**Client:**
``Template.myTemplate([data])``
``Template.myTemplate.events(eventMap)``

**events:** 
```
click, dblclick, focus, blur, change, mouseenter, mouseleave, mousedown, mouseup, keydown, keypress, keyup
```
**eventMap attributes:** 
```
type, target, currentTarget, which
```
**eventMap methods:** 
```
stopPropagation(), stopImmediatePropagation(), preventDefault(), isPropagationStopped(), isImmediatePropagationStopped(), isDefaultPrevented()
```

**Client:** 
```
Template.myTemplate.helpers(helpers)
Template.myTemplate.rendered = function ( ) { ... }
Template.myTemplate.created = function ( ) { ... }
Template.myTemplate.destroyed = function ( ) { ... }
Template.instance()
Template.getParentData(n)
UI.registerHelper(name, function)
UI.body
UI.render(Template.myTemplate)
UI.renderWithData(Template.myTemplate, data)
UI.insert(instantiatedComponent, parentNode[, nextNode])
```
```
  this.findAll(selector)
  this.find(selector)
  this.firstNode
  this.lastNode
  this.data
```

## Match:

**Anywhere:** 
```
check(value, pattern)
Match.test(value, pattern)
```

**patterns:** ``Match.Any, String, Number, Boolean, undefined, null, Match.Integer, [pattern],``
``{key1: pattern1, key2: pattern2, ...}, Match.ObjectIncluding({key1: pattern1, key2: pattern2, ...}),``
``Object, Match.Optional(pattern), Match.OneOf(pattern1, pattern2, ...),``

### Any constructor 

``function (eg, Date)``, ``Match.Where(condition)``

## Timers:

**Anywhere:** 
```
Meteor.setTimeout(func, delay)
Meteor.setInterval(func, delay)
Meteor.clearTimeout(id)
Meteor.clearInterval(id)
```

## Tracker:

**Client:** 
```
Tracker.autorun(runFunc)
Tracker.flush()
Tracker.nonreactive(func)
Tracker.active
Tracker.currentComputation
Tracker.onInvalidate(callback)
Tracker.afterFlush(callback)
```

``Tracker.Computation  ## Computation objects``

**Client:**
```
computation.stop()
computation.invalidate()
computation.onInvalidate(callback)
computation.stopped
computation.invalidated
computation.firstRun
```

``Tracker.Dependency  ### Dependency objects``

**Client:**
```
dependency.changed()
dependency.depend([fromComputation])
dependency.hasDependents()
```

## EJSON:

**Anywhere:** 
```
EJSON.parse(str)
EJSON.stringify(val, [options])``  @options: ``indent, canonical
EJSON.fromJSONValue(val)
EJSON.toJSONValue(val)
EJSON.equals(a, b, [options])``  @options: ``keyOrderSensitive
EJSON.clone(val)
EJSON.newBinary(size)
EJSON.isBinary(x)
EJSON.addType(name, factory)
```
```
  instance.clone()
  instance.equals(other)
  instance.typeName()
  instance.toJSONValue()
```

## HTTP:
**Anywhere:** 
```
HTTP.call(method, url [, options] [, asyncCallback])``  @options: ``content, data, query, params, auth, headers, timeout, followRedirects
HTTP.get(url, [options], [asyncCallback])
HTTP.post(url, [options], [asyncCallback])
HTTP.put(url, [options], [asyncCallback])
HTTP.del(url, [options], [asyncCallback])
```

## Email:

**Anywhere:** 
```
Email.send(options)``  @options: ``from, to, cc, bcc, replyTo, subject, text, html, headers
```

## Assets:

**Server:** 
```
Assets.getText(assetPath, [asyncCallback])
Assets.getBinary(assetPath, [asyncCallback])
```

## Commands:

```
$ kill -9 `ps ax | grep node | grep meteor | awk '{print $1}'`  ## to kill meteor

$ meteor help
$ meteor run [--port] [--production] [--raw-logs] [--settings] [--release] [--program]
$ meteor create [--release <release>] <name> [--example] [--list]
$ meteor update [--release <release>]
$ meteor add <package> [package] [package..]
$ meteor remove <package> [package] [package..]
$ meteor list [--using]
$ meteor bundle <output_file.tar.gz> [--debug]
$ meteor mongo [--url] [site]
$ meteor reset
$ meteor deploy <site> [--settings settings.json] [--debug] [--delete] [--star]
$ meteor logs <site>
$ meteor authorized <site> [--list] [--add <username>] [--remove <username>]
$ meteor claim <site>
$ meteor login [--email]
$ meteor logout
$ meteor whoami
$ meteor test-packages [--release <release>] [--port] [--deploy] [--production] [--settings] [package...]
```
