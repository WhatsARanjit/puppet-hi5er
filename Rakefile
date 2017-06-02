require 'puppet'
require File.expand_path(File.dirname(__FILE__) + '/lib/hi5er.rb')

Puppet.initialize_settings

task :default => :convert

desc 'Convert a Hiera 3 file into Hiera 5 format'
task :convert, [:hiera_config] do |t, args|
  config_file = args[:hiera_config] || Puppet.settings['hiera_config']
  conversion  = HieraFiver.new(config_file)
  conversion.parse_config3
  conversion.print5
end
