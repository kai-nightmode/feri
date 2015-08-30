# Feri - Watch

Watch is all about watching the source and destination folders for changes. Initiating the proper clean or build tasks in response to file system events.

The watch module lives in the file [code/7 - watch.js](../../code/7 - watch.js)

## Watch Object

* [buildOne](#watchbuildone)
* [emitterDest](#watchemitterdest)
* [emitterSource](#watchemittersource)
* [notTooRecent](#watchnottoorecent)
* [processWatch](#watchprocesswatch)
* [updateLiveReloadServer](#watchupdatelivereloadserver)
* [watchDest](#watchwatchdest)
* [watchSource](#watchwatchsource)

## watch.buildOne

Type: `function`

Figure out which files should be processed after receiving an add or change event from the source directory watcher.

```js
@param   {String}   fileName  File path like '/source/js/combined.js'
@return  {Promise}
```

## watch.emitterDest

Type: `object`

An instance of [nodejs.org/api/events.html](https://nodejs.org/api/events.html) for the destination folder. Can emit the following events.

* add
* change
* error
* ready

Example

```js
return feri.action.watch().then(function() {
    feri.watch.emitterDest.on('add', function(file) {
        console.log('detected add event for ' + file)
    })
})

// pretend a file called 'sample.txt' was just added to the destination folder

// display 'detected add event for /destination/sample.txt'
```

## watch.emitterSource

Type: `object`

An instance of [nodejs.org/api/events.html](https://nodejs.org/api/events.html) for the source folder. Can emit the following events.

* add
* addDir
* change
* error
* ready
* unlink
* unlinkDir

Example

```js
return feri.action.watch().then(function() {
    feri.watch.emitterSource.on('unlinkDir', function(file) {
        console.log('A directory called ' + file + ' was just removed.')
    })
})

// pretend a directory called 'css' was just removed from the source folder

// display 'A directory called /source/css was just removed.'
```

## watch.notTooRecent

Type: `function`

Suppress subsequent file change notifications if they happen within 300 ms of a previous event.

```
@param   {String}   file  File path like '/path/readme.txt'
@return  {Boolean}        True if a file was not active recently.
```

## watch.processWatch

Type: `function`

Watch both source and destination folders for activity.

```
@return  {Promise}
```

Example

```js
config.option.watch = true
config.option.livereload = true

return watch.processWatch().then(function() {
    // we are now watching
}
```

Note: This function is also aliased as `feri.action.watch`.

## watch.updateLiveReloadServer

Type: `function`

Update the LiveReload server with a list of changed files.

```
@param   {Boolean}  now  True meaning we have already waited 300 ms for events to settle
@return  {Promise}       Promise that returns true if everything is ok otherwise an error.
```

## watch.watchDest

Type: `function`

Watch the destination directory for changes in order to update our LiveReload server as needed.

```
@return  {Promise}
```

## watch.watchSource

Type: `function`

Watch source directory for changes and kick off the appropriate response tasks as needed.

```
@return  {Promise}
```

## License

MIT © [Daniel Gagan](https://forestmist.org)