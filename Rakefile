require 'yaml'
require 'httparty'
require 'json'

# ElasticSearch location
HOST = 'localhost'
PORT = 9200

BASE_URL = "http://#{HOST}:#{PORT}"

# Index and document type name
INDEX = 'elasticsearch-by-example'
TYPE  = 'example'

# URLs
base_url  = "http://#{HOST}:#{PORT}"
index_url = "#{base_url}/#{INDEX}"
type_url  = "#{index_url}/#{TYPE}"

# Endpoints
type_mapping_endpoint  = "#{type_url}/_mapping"
index_refresh_endpoint = "#{index_url}/_refresh"
type_search_endpoint   = "#{type_url}/_search"

# Path to examples directory read from console parameter
path = "examples/#{ARGV[0]}"


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
  system("open #{BASE_URL}/_plugin/head/")
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
end
