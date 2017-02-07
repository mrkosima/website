require 'html-proofer'

task :build do
  sh "bundle exec jekyll build"
end

task :check_site do
  HTMLProofer.check_directory("./_site", {
    :allow_hash_href => true,
    :alt_ignore => [/.+/],
    :assume_extension => true,
    :check_html => true,
    :disable_external => true,
    :empty_alt_ignore => true,
    :file_ignore => [
      %r{/blog/2015/},  # Lots of old/deprecated links, maybe fix/redirect later?
      %r{/docs/setup/}, # Duplicate ID generated by headings
    ],
    :internal_domains => [
      "babeljs.io",
    ],
    :only_4xx => true,
    :url_swap => {
      /#.*$/ => "",
    },
  }).run
end

task :check_users_links do
  HTMLProofer.check_file("./_site/users/index.html", {
    :checks_to_ignore => ["ScriptCheck", "ImageCheck"],
    :external_only => true,
    :hydra => {
      :max_concurrency => 5,
    },
    :typhoeus => {
      :followlocation => true,
      :timeout => 20,
    },
    :url_ignore => [
      "http://www.ticketmaster.com/", # Always returns as timeout
    ]
  }).run
end

task :default => ["build", "check_site", "check_users_links"]
