require 'rake'
require 'kramdown'
require 'erb'
require 'listen'
require 'fileutils'

# Task to build HTML files from Markdown
task :build do
  # Automatically fetch all Markdown files in the current directory
  markdown_files = FileList['content/**/*.md']

  markdown_files.each do |md_file|
    FileUtils.mkdir_p('build')
    # Read the Markdown file content
    md_content = File.read(md_file)

    # Convert Markdown to HTML using Kramdown
    html_content = Kramdown::Document.new(md_content).to_html

    # Apply the ERB layout
    erb_template = ERB.new(File.read('layout/layout.erb'))
    html_with_layout = erb_template.result_with_hash(
      title: File.basename(md_file, '.md').capitalize,
      content: html_content
    )

    # Define the output HTML file name
    output_file = File.join('build', File.basename(md_file, '.md') + '.html')

    # Write the final HTML content to the output file
    File.write(output_file, html_with_layout)

    puts "#{md_file} -> #{output_file}"
  end
end
task :watch do
  stop_flag = false

  listener = Listen.to('content') do |modified, added, removed|
    unless (modified + added).empty?
      puts "Changes detected, running build task..."
      Rake::Task[:build].execute
    end
  end

  puts "Watching for changes in ./content..."
  listener.start

  # Trap Ctrl+C (SIGINT) to set the stop flag
  trap("INT") do
    puts "\nExiting watch task..."
    stop_flag = true
  end

  # Keep the task running until interrupted
  until stop_flag
    sleep 1
  end

  listener.stop
end
task :serve do
   watch_thread = Thread.new do
         Rake::Task[:watch].execute
   end
   sh 'npx sirv build --dev --no-clear'
   watch_thread.join
end
# Default task
task :default => [:build]
