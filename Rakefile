require 'io/console'
    
PROJECT_DIRECTORY = 'projects-to-build'
MODEL_DIRECTORY = 'enkeli-model-to-build'
UI_DIRECTORY = 'enkeli-ui-to-build'

task default: [
  :create_working_directory,
  :clone_source_code,
  :compile_model,
  :set_tapestry_production_mode,
  :compile_ui,
  :move_war_package,
  :delete_working_directory ]

task :create_working_directory do |t|
  mkdir PROJECT_DIRECTORY
end

task :clone_source_code do |t|
  cd PROJECT_DIRECTORY
  print 'Username for Github: '
  user = gets.chomp
  print "Password for #{user}: "
  pass = STDIN.noecho(&:gets).chomp
  puts 'Downloading model...'
  puts `git clone https://#{user}:#{pass}@github.com/teseractos/enkeli-model.git #{MODEL_DIRECTORY}`
  puts 'Downloading user interface...'
  puts `git clone https://#{user}:#{pass}@github.com/teseractos/enkeli-ui-1.0.git #{UI_DIRECTORY}`
  cd '..'
end

task :compile_model do |t|
  cd "#{PROJECT_DIRECTORY}/#{MODEL_DIRECTORY}"
  puts 'Building model...'
  puts `mvn clean install`
  cd '../..'
end

task :compile_ui do |t|
  cd "#{PROJECT_DIRECTORY}/#{UI_DIRECTORY}"
  puts 'Building user interface...'
  puts `mvn clean package`
  cd '../..'
end

task :set_tapestry_production_mode do |t|
  puts 'Setting Tapestry production mode...'
  web_xml_path = "#{PROJECT_DIRECTORY}/#{UI_DIRECTORY}/src/main/webapp/WEB-INF/web.xml"
  outdata = File.read(web_xml_path).gsub(/tapestry.production-mode.*\n.*false/, "tapestry.production-mode</param-name>\n\t\t<param-value>true")
  File.open(web_xml_path, 'w') { |out| out << outdata }   
end

task :move_war_package do |t|
  Rake::FileList.new("#{PROJECT_DIRECTORY}/#{UI_DIRECTORY}/target/*.war") do |war|
    mv war, '.'
  end
end

task :delete_working_directory do |t|
  puts 'Deleting temporal data...'
  rm_rf PROJECT_DIRECTORY
end
