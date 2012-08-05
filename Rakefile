desc 'List available examples'
task :list do
  Dir.glob("**/query.json") do |example|
    puts example.split('/')[1..-2].join('/')
  end
end
