    assert = require 'assert'
    describe 'The configuration', ->
      {config} = require '../config'
      it 'should process properly', ->
        process.env.DISPATCHERS = 'dispatch.example.net'
        assert.strictEqual config(), '''
          [Relay]
          dispatchers = dispatch.example.net
          passport = None
          ;relay_ip =
          ;advertised_ip =
          port_range = 49152:65534
          log_level = DEBUG
          [TLS]
          cert_paths = local

        '''
