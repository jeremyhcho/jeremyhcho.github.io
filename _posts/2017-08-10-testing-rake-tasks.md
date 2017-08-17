---
layout: post
title: Testing Rake Tasks
---

You can test rake tasks in RSpec!

{% highlight ruby %}
require 'rails_helper'
require 'rake'

describe 'my_rake_task' do
  let(:run_rake_task) do
    Rake::Task['my_rake_task'].reenable
    Rake.application.invoke_task 'my_rake_task'
  end

  before do
    Rake.application.rake_require 'tasks/path/to/my/task'
    Rake::Task.define_task :environment
  end

  context 'testing rake tasks' do
    it 'runs the rake task' do
      expect { run_rake_task }.to change { Something }
    end
  end
end
{% endhighlight %}
