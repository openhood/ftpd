# FTPD

ftpd is a pure Ruby FTP server library.  It supports implicit and
explicit TLS, and is suitable for use by a program such as a test
fixture or small FTP daemon.

## HELLO WORLD

This is examples/hello_world.rb, a bare minimum FTP server.  It allows
any user/password, and serves files in the diretory '/tmp/ftp'.  It
binds to an ephemeral port on the local interface:

    require 'ftpd'
    require 'tmpdir'

    class Driver

      def initialize(temp_dir)
        @temp_dir = temp_dir
      end

      def authenticate(user, password)
        true
      end

      def file_system(user)
        Ftpd::DiskFileSystem.new(@temp_dir)
      end

    end

    Dir.mktmpdir do |temp_dir|
      driver = Driver.new(temp_dir)
      server = Ftpd::FtpServer.new(driver)
      server.start
      puts "Server listening on port #{server.bound_port}"
      gets
    end

A more full-featured example that allows TLS and takes options is in
examples/example.rb

## LIMITATIONS

TLS is only supported in passive mode, not active, but I don't know
why.  Either the FTPS client used by the test doesn't work in active
mode, or this server doesn't work in FTPS active mode (or both).

The DiskFileSystem class only works in Linux.  This is because it
shells out to the "ls" command.  This affects the example, which uses
the DiskFileSystem.

## DEVELOPMENT

### TESTS

To run the cucumber (functional) tests:

    $ rake test:features

To run the rspec (unit) tests:

    $ rake test:spec

To run all tests:

    $ rake test

or just:

    $ rake

To run the stand-alone example:

    $ examples/example.rb

The example prints its port, username and password to the console.
You can connect to the stand-alone example with any FTP client.  This
is useful when testing how the server responds to a given FTP client.

## REFERENCES

(This list of references comes from the README of the em-ftpd gem,
which is licensed under the same MIT license as this gem, and is
Copyright (c) 2008 James Healy)

There are a range of RFCs that together specify the FTP protocol. In
chronological order, the more useful ones are:

* <http://tools.ietf.org/rfc/rfc959.txt>
* <http://tools.ietf.org/rfc/rfc1123.txt>
* <http://tools.ietf.org/rfc/rfc2228.txt>
* <http://tools.ietf.org/rfc/rfc2389.txt>
* <http://tools.ietf.org/rfc/rfc2428.txt>
* <http://tools.ietf.org/rfc/rfc3659.txt>
* <http://tools.ietf.org/rfc/rfc4217.txt>

For an english summary that's somewhat more legible than the RFCs, and
provides some commentary on what features are actually useful or
relevant 24 years after RFC959 was published:

* <http://cr.yp.to/ftp.html>

For a history lesson, check out Appendix III of RCF959. It lists the
preceding (obsolete) RFC documents that relate to file transfers,
including the ye old RFC114 from 1971, "A File Transfer Protocol"

## ORIGIN

I created ftpd to support the test framework I wrote for Databill,
LLC, which has given its kind permission to donate it to the
community.

## WHOAMI

Wayne Conrad <wconrad@yagni.com>

## CREDITS

Thanks to Databill, LLC, which supported the creation of this library,
and granted permission to donate it to the community.
