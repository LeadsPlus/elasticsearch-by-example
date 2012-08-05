require 'yaml'
require 'httparty'
require 'json'
require 'pp'

settings = YAML::load(File.open('config/settings.yml'))

# ElasticSearch location
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
