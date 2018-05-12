{spawn, exec, execSync} = require 'child_process'
fs                        = require 'fs'
os                        = require 'os'
path                      = require 'path'

_                         = require 'lodash'


NPM_DIR = 'node_modules'
NPM_BIN_DIR = path.join NPM_DIR, '.bin'

spawnNodeProcess = (args, output = 'stderr', callback) ->
  relayOutput = (buffer) -> console.log buffer.toString()
  proc =         spawn 'node', args
  proc.stdout.on 'data', relayOutput if output is 'both' or output is 'stdout'
  proc.stderr.on 'data', relayOutput if output is 'both' or output is 'stderr'
  proc.on        'exit', (status) -> callback(status) if typeof callback is
  'function'

COFFEE_EXE = path.join NPM_BIN_DIR, 'coffee'

run = (args, callback) ->
  spawnNodeProcess [COFFEE_EXE].concat(args), 'stderr', (status) ->
    process.exit(1) if status isnt 0
    callback() if typeof callback is 'function'

buildJS = (callback) ->
  files = fs.readdirSync('.').filter (file) -> file.match ///\.(lit)?coffee$///
  run ['-c'].concat(files), callback

task 'build', 'build the coffeescript source', buildJS
