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
  path:"./assets/view"
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

Все картинки из директории /assets/view и субдиректорий первого уровня соберутся в /public/img/sprites/global_block_sprite  
затем соберутся в один спрайт, который расположиться в /public/img/sprites/global_block_sprite_{hash}.png  
также сгенерируется файл /public/img/sprites/global_block_sprite.json в котором будут перечислены попавшие в спрайт картинки  

```
...
{
  "name": "ad_up",
  "filename": "ad_up.png",
  "checksum": "ab95fec780bdc65f674a7cdb7d747254",
  "width": 62,
  "height": 62,
  "positionX": 0,
  "positionY": 0
}
...
```

В файлах стилуса прописать:

\#id_element  
  sprite global_block_sprite 'ad_up'

Здесь:  
  sprite - хелпер стилуса, который раскрывается автоматически при генерироввнии css  
  global_block_sprite - название сгенерированного спрайта, пока нет настройки для изменения этого названия  
  ad_up - параметр name из файла global_block_sprite.json, в данном случае фон будет из картинки /assets/view/ad/up.png

При раскрытии стилус прописывает свойства CSS указанному элементу  

width  
height  
background  
