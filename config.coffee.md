    pkg = require './package.json'
    Promise = require 'bluebird'
    supervisord = require 'supervisord'
    fs = require 'fs'

    config = ->

dispatchers: space-separated list of dispatchers
passport: e.g. O:Kwaoo

      """
        [Relay]
        dispatchers = #{process.env.DISPATCHERS}
        passport = #{process.env.PASSPORT ? 'None'}
        ;relay_ip =
        ;advertised_ip =
        port_range = 49152:65534
        log_level = DEBUG
        [TLS]
        cert_paths = local

      """

    run = ->
      supervisor = Promise.promisifyAll supervisord.connect 'http://127.0.0.1:5710'

Configure MediaProxy-relay

      mp_config = config()
      fs.writeFileSync "/opt/mediaproxy/local/mediaproxy-#{pkg.mediaproxy.version}/config.ini", mp_config
      supervisor.startProcessAsync 'relay'

    module.exports = {run,config}
    if require.main is module
      run()
