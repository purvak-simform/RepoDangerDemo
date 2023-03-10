# Danger Tool Integration with CircleCI

This setUp is for the public repo

1.  Add the Danger gem to your Gemfile:  

    ` gem 'danger' `

2.  Run `bundle install` to install the gem. 

3.  Initialize Danger by running danger init in the root directory of your project.
    This will generate a *Dangerfile* in your project's root directory.

4. Generate Github personal access token from [https://github.com/settings/tokens/new](https://github.com/settings/tokens/new)

5. Under "Select scopes", check the boxes for "repo" and "read:org" and **Generate Token**
  
   > Note: that Danger needs this access token to access the GitHub API.
   
6. After generating GitHub personal access token, add it as a secret environment variable in your CircleCI

    **Project Repo -> Project Settings -> Environment Variable.**


7. Create .circleci/config.yml and configure it as in below step

8. Example configuration for CircleCI .circleci/config.yml . 
 

```ruby
# .circleci/config.yml 
version: 2.1 
jobs: 
  build: 
    docker: 
      - image: circleci/ruby:3.0.2-node-browsers 
    steps: 
      - checkout 
      - run: gem install bundler 
      - run: bundle install --jobs=4 --retry=3 
      - run: bundle exec rspec 
      - run: 
          name: Run Danger 
	        command: |
            DANGER_GITHUB_API_TOKEN=$DANGER_GITHUB_API_TOKEN bundle exec danger
``` 

In this example, we're running Danger after the tests have completed. We're using the circleci/ruby Docker image to run our build environment. We're checking out the code, installing Bundler, running bundle install, running the tests with bundle exec rspec if needed, and then finally running Danger with **bundle exec danger** with github access token set as env variable in circleci. 

 
For more detail understanding you can also follow some reference:  

1. https://blog.appcircle.io/article/danger-in-ci-automate-your-mobile-code-reviews 

2. https://www.rubydoc.info/gems/danger/0.8.5#adding-a-new-ci 

3. https://danger.systems/guides/getting_started.html#setting-up-danger-to-run-on-your-ci

4. https://evilmartians.com/chronicles/danger-on-rails-make-robots-do-some-code-review-for-you#changelog

