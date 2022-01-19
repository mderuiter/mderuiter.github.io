---
layout: post
title: Introduction to Fastlane plugins
author: Milan de Ruiter
tags: ["Fastlane", "Automation", "Tips & Tricks"]
---

[Fastlane](https://Fastlane.tools/) is a tool used by a lot of mobile developers to automate signing and distribution of their applications. Next to signing and distribution Fastlane is also used for running test, creating screenshots and for other custom actions. With Fastlane you can easily configure lanes to support your team's workflow using a single command.

Your workflow is configurable with Fastlane via the fastfile. In the fastfile you can define lanes which performs a part of your workflow. You can have a task which does the building of the app and you van have a other task which does the distribution. 

> A lane is a method (block of code) you can call within Fastlane. The lanes are defined in your fastfile.

Creating a lot of lanes can make your fastfile very big and inefficient to maintain. A idea can be to separate your lanes over multiple fastfiles. Like a fastfile for deploying and a fastfile specially for testing.

Next to creating a separate fastfile, moving logic to a Fastlane plugin can also help to keep your fastfile clean and maintainable. Another advantage about plugins is that it can be shared over projects! 

# Getting started with plugins

To get started with a Fastlane plugin all you need is to have Fastlane installed. To install and get started with Fastlane there is a good webpage by Fastlane it self explaining how to get started. But since Fastlane can easiliy installed as a gem or via homebrew, you can install it via:

```
# Using RubyGems
$ sudo gem install fastlane -NV

# Alternatively using Homebrew
$ brew install fastlane
```

To create a Fastlane plugin, the only thing you need to do is to open a terminal, navigate to your favorite project directory and run the following command (where plugin_name is the name you would like to have for your plugin. 

```
$ fastlane new_plugin plugin_name
```

After running this command it will ask you some questions about your new plugin. 

- If the name of your plugin is correct
- Short summary of the plugin
- Detailed description of your plugin

 Once all questions are answered, your plugin is created. 🎉

### Customizing the plugin

To customize your plugin we can open the plugin directory with our favorite editor.  In the directory we can see that there are a lot of files created automatically for us. One important file we have to use to customize the plugin is actually a bit hidden in a folder structure.

To customize the plugin we have to modify the run method in the action file:

```
/lib/fastlane/plugin/plugin_name/actions/plugin_name_action.rb
```

When we just created our plugin our run method will look similar to this:

```ruby
def self.run(params)
   UI.message("The plugin is working!")
end
```

In the run method we can add all our code we would like to have in our plugin. But this will make the action file really big and hard to maintain. To solve this problem I like to move helper methods to a helper file. When we created our plugin next to the action file also a helper file is created. 

```ruby
/lib/fastlane/plugin/plugin_name/helper/plugin_name_helper.rb
```

Next to moving methods to the helper file to make the action file clean, it makes also more sense since the helper can be used in multiple action. To make use of the helper we can modify our run method in the action file to look similar to this (remember to replace the helper name to your plugin helper name).

```ruby
def self.run(params)
   Helper::PluginNameHelper.show_message
end
```

# Testing the plugin

When we take a look to the directory of our pluging we see that there is a directory called Fastlane. In this directory we have a fastfile with a single lane defined which we can use to test our plugin.

Open up a terminal and run the following command in your plugin directory.

```
$ fastlane test
```

# Using the plugin

Once you are done with your plugin you of course have pushed all your changes to your git repository. 

In another project where you would like to make use of your freshly created plugin, you can now add your plugin to the project's Pluginfile.

In the Pluginfile add your plugin like this:

```ruby
gem "plugin_name", git: "your-plugin-git-repo-url"
```

Now you can make use of the lanes in your plugin in your project directory.

```ruby
gem "plugin_name", path: "../plugin_name"
```

When you added the plugin to your `fastlane/Pluginfile` you are now able to make use of your plugin. In your project create a new lane and add your plugin action (the same action is also used in the lane we used to test our plugin).

```ruby
def test_my_plugin
   plugin_name_action
end
```

When we have created the new lane we can now run Fastlane and see our plugin do it's job.

```
$ fastlane test_my_plugin
```
