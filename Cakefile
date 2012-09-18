fs     = require 'fs'
{exec} = require 'child_process'
util   = require 'util'

coffeedir = 'coffee'
jsdir = 'js'

lessdir = 'less'
cssdir = 'css'

coffeeOptions = "--bare --output #{jsdir} "
lesscOptions = ""


task 'watch', 'Watch for coffeescript and less changes', ->
  util.log "Watching for code changes to:"
  
  cFiles = fs.readdir("#{coffeedir}", (err, files)->
    for file in files
      fn = (file) -> 
        parts = file.split('.', 1)
        if(/(.*)\.(coffee)/i.test(file))
          compileCoffee(parts[0])
          fileRef = file
          util.log " -> #{coffeedir}/#{file}"
          fs.watchFile "#{coffeedir}/#{file}", (curr, prev) ->
              if +curr.mtime isnt +prev.mtime
                  util.log "Saw change in #{coffeedir}/#{fileRef}"
                  compileCoffee(parts[0])
      fn(file)
  )
  
  lFiles = fs.readdir("#{lessdir}", (err, files)->
    for file in files
      fn = (file) ->
        parts = file.split('.', 1)
        if(/(.*)\.(less)/i.test(file))
          util.log " -> #{lessdir}/#{file}"
          compileLess(parts[0])
          fileRef = file
          fs.watchFile "#{lessdir}/#{file}", (curr, prev) ->
              if +curr.mtime isnt +prev.mtime
                  util.log "Saw change in #{lessdir}/#{fileRef}"
                  compileLess(parts[0])
      fn(file)
  )
  
  util.log "-----------------------------------"
  
  util.log "Performed initial compile. Ready and waiting for changes"

# if you wanted to run these alone then you might as well do it from the command line
compileCoffee = (file) ->
  exec "coffee #{coffeeOptions} #{coffeedir}/#{file}.coffee", (error, stdout, stderr) ->
    util.log(stdout) if stdout
    util.log(stderr) if stderr
  
compileLess = (file) ->
  exec "lessc #{lesscOptions} #{lessdir}/#{file}.less #{cssdir}/#{file}.css", (error, stdout, stderr) ->
    util.log(stdout) if stdout
    util.log(stderr) if stderr
