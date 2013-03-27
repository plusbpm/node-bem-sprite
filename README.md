This is a fork of https://github.com/naltatis/node-sprite

We use it as a code base for https://github.com/droganov/node-bem-espresso

Because original module doesn't suit our needs.

# Install
```
sudo npm install https://github.com/plusbpm/node-bem-sprite.git
```

# Usage
```
sprite          = require "node-bem-sprite"
sprite.blocks 
  path:"./public/img/block"
  output:"./public/img/sprites"
  httpPath: "/img/sprites"
  watch:true
  (err, gsprite, stylus_helper) ->

    assets.cssCompilers.styl.compileSync = (sourcePath, source) ->
      result = ''
      callback = (err, js) ->
        throw err if err
        result = js
      libs = {}
      libs.nib or= try require 'nib' catch e then (-> ->)
      options = 
        filename: sourcePath
      stylus(source, options)
        .use(libs.nib())
        .define('sprite', stylus_helper.fn)
        .render callback
      result

    console.log "Global sprite is ready."
    return
```