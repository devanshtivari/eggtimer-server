eggtimer-server
===============

[![Build Status](https://circleci.com/gh/jessamynsmith/eggtimer-server.svg?style=shield)](https://circleci.com/gh/jessamynsmith/eggtimer-server)
[![Coverage Status](https://coveralls.io/repos/jessamynsmith/eggtimer-server/badge.svg?branch=master)](https://coveralls.io/r/jessamynsmith/eggtimer-server?branch=master)

Open-source tracker for menstrual periods. Check out the live app:
https://eggtimer.herokuapp.com/

Note: the front end for this app is being ported to Ionic (see https://github.com/jessamynsmith/eggtimer-ui)

Development
-----------

Fork the project on github and git clone your fork, e.g.:

    git clone https://github.com/<username>/eggtimer-server.git

Create a virtualenv using Python 3 and install dependencies. I recommend getting python3 via [homebrew](http://brew.sh/), then installing [virtualenv](https://virtualenv.pypa.io/en/latest/installation.html) and [virtualenvwrapper](https://virtualenvwrapper.readthedocs.org/en/latest/install.html#basic-installation) to that python. NOTE! You must change 'path/to/python3'
to be the actual path to python3 on your system.

    mkvirtualenv eggtimer --python=/path/to/python3
    pip install -r requirements/development.txt

Set environment variables as needed (check settings.py for values).

Set up db:

    python manage.py syncdb
    python manage.py migrate

Run tests and view coverage:

    coverage run manage.py test
    coverage report -m

Check code style:

    flake8
    
Generate graph of data models, e.g.:

    python manage.py graph_models --pygraphviz -a -g -o all_models.png  # all models
    python manage.py graph_models periods --pygraphviz -g -o period_models.png  # period models

Run server:

    python manage.py runserver
    
The javascript linter and tests require you to install node, then:

    npm install -g jshint mocha blanket moment moment-timezone

Set up your environment to know about node. NOTE! You must change 'path/to/node_modules' to be
the actual path to node modules on your system.

    export NODE_PATH=/path/to/node_modules/

Lint JavaScript:

    jshint */static/*/js
    
Run JavaScript tests:

    mocha -R html-cov */tests/static/*/js/* > ~/eggtimer_javascript_coverage.html
    
    
Continuous Integration and Deployment
-------------------------------------

This project is already set up for continuous integration and deployment using circleci, coveralls,
and Heroku.

Make a new Heroku app, and add the following addons:

    Heroku Postgres
	  Mailgun
	  New Relic APM
	  Papertrail
	  Heroku Scheduler

Enable the project on coveralls.io, and copy the repo token

Enable the project on circleci.io, and under Project Settings -> Environment variables, add:

    COVERALLS_REPO_TOKEN <value_copied_from_coveralls>
    
On circleci.io, under Project Settings -> Heroku Deployment, follow the steps to enable
Heroku builds. At this point, you may need to cancel any currently running builds, then run
a new build.

Once your app is deployed successfully, you can add the Scheduler task on Heroku:

    python manage.py notify_upcoming_period --settings=eggtimer.settings


Thank you to:
Emily Strickland (github.com/emilyst) for the name
