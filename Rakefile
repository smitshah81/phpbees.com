desc 'Re-generate tag pages.'
task :tags do
  Dir['tags/*.md'].each { |f| File.delete(f) }
  tags = {}
  Dir['**/*.markdown'].each do |file|
    tagline = File.read(file).split("\n").detect { |line| line.start_with?('tags: ') }
    next unless tagline
    tagline.tr('[', '').tr(']', '').split(':').last.split(',').map(&:strip).each do |tag|
      next if tag.empty?
      tags[tag] ||= 0
      tags[tag] += 1
    end
  end
  tags.delete_if { |_k, v| v < 5 }
  tags.keys.each do |tag|
    filename = "tags/#{tag}.md"
    puts filename
    File.write filename, <<-EOS
---
layout: tag
tag: #{tag}
permalink: /tags/#{tag}/
---
    EOS
  end
  File.write '_data/tags.yml', tags.keys.sort.map { |tag| "#{tag}:\n  name: #{tag}\n  count: #{tags[tag]}" }.join("\n")
end

desc 'Check for broken links and such.'
task :check do
  require 'html-proofer'
  sh 'bundle exec jekyll build'
  HTMLProofer.check_directory(
    './_site',
    alt_ignore: [/.*/],
    http_status_ignore: [999]
  ).run
end