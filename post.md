> *Who is this for?*
> 
> * Individual developers and teams that aren't using Continuous Integration with static analysis already
> * Project owners that want to produce high quality stable software products
> * Developers that want to get started with Travis and/or Scrutinizer

> **TL;DR**
>
> We'll talk about the real benefits of high code quality and a practical example


If you like to invest in yourself or your team, you're always looking at ways to improve. Everything in the tech world is a moving target, but some things are foundational. One of these is the quality of code. The quality of code could mean the difference between being able to move with the business market and having satisfied customers or delivering a half-baked product that lets them down and go elsewhere. As a developer it could mean your job application getting deleted if you can't write good clean code.

## Why does Higher Code Quality matter?

Having high code quality is not just something "shiny" to add to your toolbox; it has real implications in the work you do. There are many benefits to paying attention to code quality especially if you're not the only one maintaining the code. Lets take a view at why high quality code matters:

*   Makes it easier to read for you and others
*   Easier to maintain
*   Catch bugs before others do
*   Encourages best practices while you're coding
*   Your value as a programmer will go up
	* Technical employers care about quality
	* Helps your career
*   Low quality code could handicap a business

We'll go through some of these more in detail.

If you want to make it easier for others to work on and/or use the code you've written than you _should_ be paying attention to how you write it. It doesn't matter if it makes sense _to you_. You need to ask yourself, "Does my code makes sense to _everyone else_ that's reading it?" After all, unless you're writing in assembly language or binary, you're writing code _for humans_.

The reason some applications grow into a mess and get hard to maintain is because of low code quality. That normally translates to applications that are fragile, slow to add additional features to, and frequently contain bugs that make it to your end users. If you've outsourced software development for cheap labor you are well-acquainted with this problem. Companies are careful investing into developing software that's not stable and can't align with business goals.

Some developers aren't aware how much influence the way you write code has on getting employed. If the one in charge of hiring is technical, especially if they're a programmer such as CTOs and Senior Leads your code will be highly scrutinized. If the code quality is low, your chances of getting the job is dramatically reduced. Exceptions probably exist for Junior Developers that are fast learners or if the one hiring is not technical at all. That might be the case, but if you truly care about your career you should invest in learning how to write better code.

## Take Precautions

By now you should see how much I advocate high quality code, but you should take precautions. If **too much** time is spent trying perfect your code, you may be "spinning your wheels" and _not adding value_ to your code.  At that point you're just being pedantic. If, for instance, you're building an MVP or open source software you may never get to the point of "shipping" your product or missing opportunities for not shipping fast enough.

Another situation where I wouldn't advise over perfecting code quality is when working with legacy software. You may feel the urge of "fixing" all of the mistakes and making the code quality higher, but unless you know what you're doing it's best not to go that route. You may end up introducing more problems, perhaps because of unfamiliarity with the code you've inherited. I'm not saying _don't_ improve legacy software. What I'm saying is be _highly precautious_. You can't just go in there all willy-nilly, change code to your preferences, and expect the code to work fine.

## Why Continuous Integration?

So we've been talking about code quality and why high quality code matters, but what does Continuous Integration have to do in this mix? For those that aren't familiar with Continuous Integration (CI), it's "a development practice that requires developers to integrate code into a shared repository several times a day. Each check-in is then verified by an automated build, allowing teams to detect problems early." <sup>[1](http://www.thoughtworks.com/continuous-integration)</sup>

Your shared repository will be a shared repository that could be publicly accessible such as a public project on GitHub (Bitbucket, etc.) or even running on a private server. The key is that it's a central repo that can be "polled" or "monitored" for changes so that a separate server could start an automated build that will run to make sure your code is running properly.

Perhaps you're thinking that sounds like a lot of work, but fortunately there's plenty of services out there that make those steps much more simple. One of the services we'll talk about is called Travis and I'll go more into detail later.

We really haven't touched up on the "Why", yet but here's a few of the reasons implementing Continuous Integration rocks:

*   Builds confidence
	*  You get automatic emails if a build failed or not
*   Builds trust
	* For open source projects, the fact that you can show your code works on the platforms you say it does makes them more open to using your project
*   Doesn't interrupt developer workflow
	*  You don't have to wait until the CI build finishes because it runs asynchronously
	*  You also don't have to worry about setting up multiple build environments if you use a service like Travis
*   Set and forget
	*  Set up the CI server and you only need to update it when necessary
*   Reduces bugs in the wild
	*  Static analysis (e.g. Scrutinizer) can be done to find errors
	*  Unit tests can be run in different runtime environments
	* Errors can be set up to be specific
*   Better visibility within a team
	*  Everyone involved in the project will have visibility on what's going on with the code base. If something's wrong it's easier to fix earlier in the process.



## What is Travis

[Travis CI](http://www.travis-ci.org) is a Continuous Integration service that's hugely popular in the open source community. It's just so easy you'll see why everyone likes to use it. The eagle's eye view on how to set it up is very simple.

**Pre-requisites**: You have an open source package on GitHub already.

Here are the steps: you create an account on [travis-ci.org](http://www.travis-ci.org) with your GitHub account, you choose your project, then add a .travis.yml file to your GitHub repository and commit it. It should start the build automatically on Travis.

For the sake of learning we're going to use Laravel as the test subject.

### How to Configure

1. Go to [https://github.com/laravel/framework](https://github.com/laravel/framework) and fork it
2. Move over to [Travis CI](http://www.travis-ci.org) and login with Github
3. Click on your username at the top right, then click the "Sync" button
4. Switch on the repo for Laravel: [GITHUB_USERNAME]/framework
	* This essentially associates the project on Travis to the one on GitHub
5. Remove the .travis.yml file. We'll replace it.
6. Add the following .travis.yml file to your GitHub repository and commit it

	```
	language: php

	php:
	    - 5.5
	    - 5.6
	    - 7.0
	    - hhvm

	  install:
	    - composer install --no-interaction --prefer-source
	    
	  script: vendor/bin/phpunit```
	  
	This is your config file for Travis. Here we've specified which versions of PHP we want to build our project on and also install `composer` libraries. By default, Travis calls `phpunit` for php projects. For each language there are default build steps (i.e. ruby projects call rake). Everything is optional except the `language` key.

7. **(optional)** Click on the build image to get a snippet that you could put in your repository's README as a badge. This image will be green or red automatically depending if the build was successful or not. It's one of the first things people usually see when they go to your project's GitHub repository.

### Triggering a Build

There's different ways of triggering a build.

* Pushing to the repo
* Adding a pull request to the main repo
* You can also re-run a job, but I could never find a case for it though. Seems like this would only be used occasionally (i.e. intermittent network problems with external dependencies).

### Go green or Go Home

Builds either fail or succeed. You should do what you can to get the build to green, especially if it's a branch that others rely on or is already deployed.

Travis splits up builds per language's runtime versions. This allows you to check in which environment there are issues. Just click on a build and you'll get results similar to what you'd see in your command line.

### Preparing your CI Environment

For some environments, your projects are so simple that the .travis.yml ends up with just a few lines. In most cases you'll have to set up your config file to get your project working correctly on the build server.

Here's how you'd do several common tasks. Unless noted otherwise, the examples are sections that you would add to your .travis.yml file.

#### Setting Environment Variables

```
env:
  global:
    - key=value
```

In case some of the environment variables contain sensitive data you could set them manually through the Travis GUI under "Settings" tab for your repo.

#### Installing Programs

If you're trying to install system packages, you'd use the `before_install` hook. The Travis VMs are on Ubuntu so we would be calling `apt` to install packages

```
before_install:
	- sudo apt-get update -qq
	- sudo apt-get install -qq [packages list]
```

For project level dependencies you'd use the `install` hook to call your package manager's `install` command or similar. Note, that for some languages such as python, this is already done by default, so the extra step can usually be disregarded. Here's how your .travis.yml would look if you have to install software for PHP with `composer`.

```
install: 
  - composer install --no-interaction --prefer-source
```

#### Running Unit Tests

Running tests completely depend on the language that you set up in your .travis.yml config. 

For PHP, `phpunit ` is called by default so you don't have to call it manually if your PHPUnit config file is named properly and set at the root of the project. 

For python, tests aren't called automatically. In that case you would use the `script` key hook. This is called after the `install` key hook. Keep in mind that both are single line shell commands, so any valid command will work.

If, for instance, you were installing `phpunit` with `composer` your .travis.yml file could like this:

```
language: php

php:
  - 5.5
  - 5.6
  
install: 
  - composer install --no-interaction --prefer-source
  
script: vendor/bin/phpunit
```

By default `phpunit` is installed for PHP projects, but in this case we used the `composer` version of `phpunit` which would allow us to specific a specific version of `phpunit` to use.

#### Setting Language Versions

Travis supports over 25 languages and several runtimes for each. To specify which languages and runtime versions to use, set the following in your .travis.yml file:

```[THE_LANGUAGE]: [VERSION]```

Example:

```
php: 5.6
```

Or multiple versions:

```
php:
  - 5.5
  - 5.6
  - 7
```

## What is Scrutinizer

Scrutinizer is a Static Analysis service that checks your code's quality and neatly gives you a rating. If you have messy code it will detect, mark, and even fix some of it for you which comes really handy sometimes. It will also find things that can be improved and give you solid suggestions.

### How to Configure

1. Login with Github
2. Click "Add Repository"
3. Enter your unique repo name: [GITHUB_USER]/framework (your Laravel fork)
4. Click "Add Repository"
5. An analysis will start right away
6. Wait for a status
7. **(optional)** Scrutinizer has a badge too that you could put in your README
	
### How to Trigger

*  Push to repo
*  Manually run
*  Scheduled

### Highly Configurable

Scrutinizer allows a number of different configuration values if you're happy with the defaults. To view these different settings look at: [https://scrutinizer-ci.com/docs/](https://scrutinizer-ci.com/docs/)


## Code Quality _Does_ matter

We've talked about why code quality matters for both developers and businesses, what Continuous Integration is and its benefits, and how to get started with Travis and Scrutinizer. If you truly care about your project and/or your skills moving with a CI solution that will automatically check your code will be a step in the right direction.

### References

[1] http://www.thoughtworks.com/continuous-integration