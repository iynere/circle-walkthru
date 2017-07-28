# Encyclopedia Brown and the Case of Getting Tests to Pass on Circle

Welcome, Sales and Success folks! Today’s adventure involves the mystery of best-of-breed, cloud-based, continuous integration and delivery. Specifically, what is all of that stuff? And why do people care about it?

Let’s break this down into a few steps to go from zero to continuous delivery:

* [Testing](#testing)
* [Automated Testing](#automated-testing)
* [Continuous Integration](#continuous-integration)
* [Enter CircleCI](#enter-circleci)
* [Continuous Delivery](#continuous-delivery)

## <a name="testing">Testing</a>

I’m going to assume that most of you are familiar with the basic benefits of software: it can do tedious tasks much faster and more reliably than people can. Well, one task that goes hand-in-hand with creating software is testing software. Historically, this has involved teams of software developers writing a bunch of complicated code that does a bunch of complicated stuff, all to satisfy the exacting demands of some "requirements document". After they’re convinced everything works, they "throw it over the wall" to the test team, who clicks buttons and enter data and make sure the software does what the requirements document says it should. (An example would be: go to google.com, search for "apples", and ensure that you get relevant results about apples.) The testers find a bunch of problems, which they throw back "over the wall" to the developers, and the process is repeated until the bugs are small enough that some manager decides it’s good enough to ship.

This is obviously not ideal for a few reasons, such as:

* It’s slow, because humans have to do the testing AND because of all the back-and-forth
* It’s imprecise, because english language is a vague way to capture software requirements

## <a name="automated-testing">Automated Testing</a>

It turns out that software is a really good tool for testing software, that fixes the issues above. It still isn’t perfect, and it introduces some new challenges (like requiring developers to add test-writing into their coding process), but it is definitely an improvement.

Countless books, blog posts, podcasts, and zunecasts exist to cover the finer points of automated software testing, but it basically boils down to this: there should be one (or a few) simple software-based test processes that can be run, without any human involvement, to thoroughly test the main, production software (the "code under test") that has been developed.

## <a name="continuous-integration">Continuous Integration</a>

![ci pic](https://raw.githubusercontent.com/iynere/circle-walkthru/master/ci.png)

I’m going to defer to an external resource to provide a pretty good introduction of the idea of Continuous Integration: 

*[Continuous Integration – Martin Fowler](http://martinfowler.com/articles/continuousIntegration.html)*

Martin Fowler has historically been a pretty significant thought leader in this space.

## <a name="enter-circleci">Enter CircleCI</a>

(**Note**: If you get stuck on anything below, please bug @kevin or @rose in the "**circle-101**" channel in Slack. Scrollback in that channel is likely also a good resource/FAQ.)

The Martin Fowler article introduced the concept of software version control systems (VCS). Before you can use CircleCI, you’ll need to learn a bit about GitHub and Bitbucket, which are the two version control systems that work with CircleCI. Both are web-based version of a free, open source "**version control system**" called [Git](https://en.wikipedia.org/wiki/Git), but you don’t need to know too much about that right now.

You can learn the basics of GitHub in this guide: [https://guides.github.com/activities/hello-world](https://guides.github.com/activities/hello-world)

And here are two similar guides for Bitbucket:

- *[Tutorial: Learn Git with Bitbucket Cloud](https://confluence.atlassian.com/bitbucket/tutorial-learn-git-with-bitbucket-cloud-759857287.html)*
- *[Tutorial: Learn about pull requests in Bitbucket Cloud](https://confluence.atlassian.com/bitbucket/tutorial-learn-about-pull-requests-in-bitbucket-cloud-774243385.html)*

These guides go into a bit more detail than is necessary for this walk-through, particularly the Bitbucket guides, however they may be helpful to you in the future. Whether or not you choose to complete them now, the following steps assume that you have created a GitHub or Bitbucket account, so please create one now if you haven't already done so.

### Creating your repo

First, create a new repo, named anything you like, using whichever VCS you'd like. On GitHub, make sure to check the "**Initialize this repository with a README**" box. If you're on Bitbucket, make sure "**Git**" is selected as your Repository type.

The basis of a CircleCI build is a `.yml` file that tells Circle exactly what to do with your repo—called a "**project**" on Circle. (`.yml` is the extension for YAML files; [YAML](https://en.wikipedia.org/wiki/YAML) (rhymes with "mammal") is a "data-serialization language" that is often used for configuration files like this.)

On CircleCI 2.0, this file must be called `config.yml` and must be in a hidden folder called `.circleci` (on Mac, Linux, and Windows systems, files and folders whose names start with a period are treated as system files that are hidden from users by default).

To create the file and folder on GitHub, click the "**Create new file**" button the repo page and type `.circleci/config.yml`.

On Bitbucket, first create a README (click the "Create a README" button, then click "**Commit**," and then "**Commit**" again), then navigate to the "**Source**" page using the sidebar, click the "**New file**" button, and type `.circleci/config.yml`.

On whichever VCS you are using, you should now have in front of you a blank `config.yml` file in a `.circleci` folder.

For the simplest possible `config.yml`, navigate to [CircleCI's Hello World guide](https://circleci.com/docs/2.0/hello-world) and copy the text in step 2 (see below) into the file editing window on Bitbucket or GitHub:

```
version: 2
jobs:
  build:
    docker:
      - image: circleci/<language>:<version TAG>
    steps:
      - checkout
      - run: echo "hello world"
```

The `<language>:<version TAG>` text tells CircleCI what Docker image to use when it builds your project. Circle will use the image to boot up a "**container**"—a virtual computing environment where it will install any languages, system utilities, dependencies, web browsers, etc., that your project might need in order to run.

For this example, replace the `<language>:<version TAG>` text with `ruby:2.3-node-browsers`. This would typically be used for a web application built with Ruby on Rails and Node.js. Then commit your new file.

### Setting up your build on CircleCI

For this step, you will need a CircleCI account. Make sure you are logged into your GitHub or Bitbucket account, then visit [https://circleci.com/signup](https://circleci.com/signup) and click either the "**Start with GitHub**" or "**Start with Bitbucket**" button. You will need to give CircleCI access to your GitHub or Bitbucket account in order to run your builds.

Next, you will be given the option of "**following**" any projects you have access to that are already building on CircleCI (this would typically apply to developers connected to a company or organization's GitHub/Bitbucket account). Since this probably doesn't apply to you, click "**Skip - I don't want to follow any projects.**" On the next screen, you'll be able to add the repo you just created as a new project on Circle.

To add your new repo, find your GitHub or Bitbucket account on the left side of the page, under the "**1) Choose an organization that you are a member of**" text. When you click on your account, you should see your repo appear in the window on the right. Click the "**Setup project**" button next to it.

On the next screen, you're given some options for configuring your project on CircleCI. Leave everything as-is for now and just click the "**Start building**" button a bit down the page on the right.

### Running your first build!

You should see your build start to run automatically—and pass! So, what just happened? Click on the green button and let's investigate.

**1. Spin up environment:** CircleCI used the `ruby:2.3-node-browsers` Docker image to launch a virtual computing environment with Ruby, Node.js, and web browsers pre-installed.

**2. Checkout code:** Circle checked out your GitHub/Bitbucket repository and "cloned" it into the virtual environment launched in step 1.

**3. echo "hello world":** this was the only other instruction in your `config.yml` file: Circle ran the `echo` command with the input "hello world" ([`echo`](https://linux.die.net/man/1/echo) does exactly what you'd think it should do)

Because there was no actual source code in your repo, and no actual tests configured in your `config.yml`, Circle considers your build to have "succeeded." Most customers' projects are far more complicated, oftentimes with multiple Docker images and multiple steps, including a large number of tests—[here's an example](https://circleci.com/docs/2.0/configuration-reference/#full-example). You can learn more about all the possible steps one might put in a `config.yml` file here:

*[Writing Jobs with Steps](https://circleci.com/docs/2.0/configuration-reference)*

--------------------


### Bonus: why a build with no tests 'succeeds'

See if you can do a little outside research to figure out why a command like `echo "hello world"` results in a "passing," rather than failing, build. Try opening Terminal on your Mac or using [this web-based terminal](https://www.tutorialspoint.com/unix_terminal_online.php) if you don't have a Mac. Take a look at [this article](http://stackoverflow.com/questions/6834487/what-is-the-variable-in-shell-scripting). 

Then try running these commands:

```
$ echo Hi
$ echo $?
$ asdfjaka
$ echo $?
```

On a Continuous Integration server an "exit code" of "0" means the tests passed.

## <a name="continuous-delivery">Continuous Delivery</a>

Okay, that’s the basics of automated testing and how to do it on CircleCI. I’ll leave this article to plant the seeds of Continuous Delivery in your brains:

*[Continuous Delivery – Martin Fowler](http://martinfowler.com/bliki/ContinuousDelivery.html)*
