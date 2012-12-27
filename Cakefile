{exec} = require 'child_process'
path = require 'path'
fs = require 'fs'
{log, error} = console
{minify} = require './node_modules/uglify-js'

task 'uglify', 'Uglify js file', ->
  ugly = minify path.resolve('./timezonedetect.js')
  fs.writeFile path.resolve('./timezonedetect.min.js'), ugly.code, (err)->
    throw err if err
    log 'Written!'

task 'doc', 'Generate doc in gh-pages branch', ->
  dir = path.resolve './'
  
  # Create a tmp directory
  exec 'mktemp -d', (err, stdout, stderr)->
    error err if err?
    tmp = stdout.replace(/^\s*|\s*$/g, '') 
    
    # clone the git in that temp dir
    exec "git clone #{dir} #{tmp}", (err, stdout, stderr)->
      error err if err?
      log stdout
      
      # Change branch to gh-pages
      exec "git checkout gh-pages", {cwd:tmp}, (err, stdout, stderr)->
        error err if err?
        log stdout
        
        # Create doc        
        exec 'docco-husky timezonedetect.js', (err, stdout, stderr)->
          error err if err?
          log stdout
          
          # Move the doc to the tmp dir
          exec "cp #{dir}/docco-husky/* #{tmp} -r", (err, stdout, stderr)->
            error err if err?
            log stdout
            
            # Commit and push to gh-pages
            exec "git commit -am 'Generated automatically'", {cwd:tmp}, (err, stdout, stderr)->
              error err if err?
              log stdout
