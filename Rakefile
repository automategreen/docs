require 'html-proofer'

task :test do
  sh "bundle exec middleman build"
  HTMLProofer.check_directory("./build", {
    :allow_hash_href => true,
    :url_swap => {%r{https://docs.automategreen.com(/.*)} => "\\1"},
    :check_html => false
  }).run
  # HTML::Proofer.new("./_site", {:href_ignore => ["#"], :href_swap => {%r{https://www.automategreen.com(/.*)} => "\\1"}}).run
end