require 'yaml'
require 'httparty'
require 'json'
require 'pp'

settings = YAML::load(File.open('config/settings.yml'))

# elasticsearch location
host = settings['elasticsearch']['host']
port = settings['elasticsearch']['port']

# Index and document type name
index = settings['elasticsearch']['index']
type  = settings['elasticsearch']['type']

# URLs
base_url  = "http://#{host}:#{port}"
index_url = "#{base_url}/#{index}"
type_url  = "#{index_url}/#{type}"

# Endpoints
type_mapping_endpoint  = "#{type_url}/_mapping"
index_refresh_endpoint = "#{index_url}/_refresh"
type_search_endpoint   = "#{type_url}/_search"


##################
# Helper methods #
##################

def cluster_node_available? base_url
  index_health_endpoint = "#{base_url}/_cluster/health"
  begin
    HTTParty.get(index_health_endpoint)
  rescue Errno::ECONNREFUSED
    return false
  end

  return true
end

until cluster_node_available? base_url
  puts "The cluster node #{base_url} has refused the connection... Probably offline."
  exit
end


#########
# Tasks #
#########

desc 'List available examples'
task :list do
  Dir.glob("**/query.json") do |example|
    puts example.split('/')[1..-2].join('/')
  end
end

desc 'Open ElasticSearch Head in browser (Mac only!)'
task :head do
  system("open #{base_url}/_plugin/head/")
end

desc 'Run complete example'
task :run, :example do |t, args|
  Rake::Task['index:delete'].invoke
  Rake::Task['index:create'].invoke
  Rake::Task['index:mapping'].invoke(args[:example])
  Rake::Task['index:documents'].invoke(args[:example])
  Rake::Task['index:refresh'].invoke
  Rake::Task['index:query'].invoke(args[:example])
end

namespace :run do
  desc 'Run only query of an example'
  task :query, :example do |t, args|
    Rake::Task['index:query'].invoke(args[:example])
  end
end

namespace :index do
  desc 'Delete index'
  task :delete do
    index_delete_response_code = HTTParty.delete(index_url).response.code
    index_delete_successful = index_delete_response_code == '200'
    puts "Delete index : #{index_url}" if index_delete_successful
  end
  desc 'Create index'
  task :create do
    # Explicit index creation is especially important when applying a custom mapping.
    HTTParty.put(index_url)
  end
  desc 'Put mapping'
  task :mapping, :example do |t, args|
    mapping_file_path = "examples/#{args[:example]}/mapping.json"
    if File.exists?(mapping_file_path)
      mapping = File.read(mapping_file_path)
      HTTParty.put(type_mapping_endpoint, {:body => mapping})
      puts "Put mapping  : #{type_mapping_endpoint}"
    end
  end
  desc 'Index documents'
  task :documents, :example do |t, args|
    docs_file_path = "examples/#{args[:example]}/docs.json"
    docs = JSON.parse(File.read(docs_file_path))
    docs.each_with_index do |doc, i|
      document_url = "#{type_url}/#{i}"
      HTTParty.put(document_url, {:body => doc.to_json})
      puts "Put document : #{document_url}"
    end
  end
  desc 'Refresh index'
  task :refresh do
    HTTParty.post(index_refresh_endpoint)
    puts "Post refresh : #{index_refresh_endpoint}"
  end
  desc 'Query index'
  task :query, :example do |t, args|
    query_file_path = "examples/#{args[:example]}/query.json"
    query = File.read(query_file_path)
    response = HTTParty.post(type_search_endpoint, {:body => query})
    puts "Post search  : #{type_search_endpoint}"
    puts "Result"
    puts "------"
    pp response.parsed_response
  end
end

namespace :cluster do
  desc 'Check cluster health'
  task :health do
    index_health_endpoint = "#{base_url}/_cluster/health"
    index_health_parsed_response = HTTParty.get(index_health_endpoint).parsed_response
    puts "Status                : #{index_health_parsed_response['status']}"
    puts "Cluster name          : #{index_health_parsed_response['cluster_name']}"
    puts "Timed out             : #{index_health_parsed_response['timed_out']}"
    puts "Number of nodes       : #{index_health_parsed_response['number_of_nodes']}"
    puts "Number of data nodes  : #{index_health_parsed_response['number_of_data_nodes']}"
    puts "Active primary shards : #{index_health_parsed_response['active_primary_shards']}"
    puts "Active shards         : #{index_health_parsed_response['active_shards']}"
    puts "Relocating shards     : #{index_health_parsed_response['relocating_shards']}"
    puts "Initializing shards   : #{index_health_parsed_response['initializing_shards']}"
    puts "Unassigned shards     : #{index_health_parsed_response['unassigned_shards']}"
  end
end
