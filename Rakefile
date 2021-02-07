#encoding: utf-8 

require 'html-proofer'
require 'html/pipeline'
require 'find'

desc "Run Mardown validation for the repository"
task :validate_markdown do
  # make an out dir
  Dir.mkdir("out") unless File.exist?("out")

  pipeline = HTML::Pipeline.new [
    HTML::Pipeline::MarkdownFilter,
    HTML::Pipeline::TableOfContentsFilter
  ], :gfm => true

  # iterate over files, and generate HTML from Markdown
  Find.find("README.md") do |path|
    if File.extname(path) == ".md"
      contents = File.read(path)
      result = pipeline.call(contents)

      File.open("out/#{path.split("/").pop.sub('.md', '.html')}", 'w') { |file| file.write(result[:output].to_s) }
    end
  end
  # test your out dir!
  options = {
    :url_ignore => [
      "https://www.tcell.io/",
      "https://www.twistlock.com/",
      "https://www.cloudpassage.com/cloud-computing-security/",
      "https://twitter.com/LMaccherone/status/843644744538427392",
    ],
    :http_status_ignore => [429]
  }
  HTMLProofer.check_directory("./out", options).run
end

task default: :validate_markdown
