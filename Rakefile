# ElasticSearch location
ELASTICSEARCH_HOST = 'localhost'
ELASTICSEARCH_PORT = 9200

BASE_URL = "http://#{ELASTICSEARCH_HOST}:#{ELASTICSEARCH_PORT}"

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
