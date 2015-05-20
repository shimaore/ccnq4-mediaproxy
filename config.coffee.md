    pkg = require './package.json'
    Promise = require 'bluebird'
    supervisord = require 'supervisord'
    fs = require 'fs'

    run = ->
      supervisor = Promise.promisifyAll supervisord.connect 'http://127.0.0.1:5708'

Configure MediaProxy-relay

      mp_config = """
        [Relay]
        # space-separated list of dispatchers
        dispatchers = #{proces.env.DISPATCHERS}
        # e.g. O:Kwaoo
        passport = #{process.env.PASSPORT ? 'None'}
        ;relay_ip =
        ;advertised_ip =
        port_range = 49152:65534
        log_level = DEBUG
        [TLS]
        cert_paths = local

      """
      fs.writeFileSync "/opt/mediaproxy/local/mediaproxy-#{pkg.mediaproxy.version}/config.ini", mp_config
      supervisor.startProcessAsync 'relay'

    module.exports = run
    if require.main is module
      run()
