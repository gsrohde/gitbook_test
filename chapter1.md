# GitBook Bugs

1. The backtic quotes should allow unescaped angle brackets to be displayed as such.  But:


```bash
cd /usr/local/ebi # or to the Rails root of the copy you are upgrading

# Check the git status to be sure the copy is "clean".
# Generally there shouldn't be any modified files and you should be 
# on the master branch.
#
git status

# Get the latest code from Github.
#
git pull

# Usually we deploy the code at the HEAD of the master branch.  But if there has 
# been code checked in to the master branch since the version to be released, 
# you will have to back up to it.
# Note that we tag releases AFTER deploying them, so we can't use the release tag
# as the git reference.
#
# [If necessary:]
# git checkout <git reference to version being deployed>

# Check the gem bundle.
#
bundle check

# If the the bundle contains uninstalled Gems, run this:
#
bundle install
# [This assumes no special flags must be used to install the new gems.]

# Check for pending migrations.
#
RAILS_ENV=production bundle exec rake db:migrate:status
# The RAILS_ENV setting can be omitted by individual developers who only want
# to migrate their development databases.

# If there ARE pending migrations, run them; but as a precaution, dump the database first:
#
pg_dump ebi_production > [some suitable directory]/ebi_production.psql
# (Generally, after the dump file has been created, I rename it to include the timestamp 
# information in the file name: ebi_production_YYYYMMDDhhmm.psql.  This way multiple dump 
# files can be stored in the same directory.)

# Now it is safe to run the migration:
RAILS_ENV=production bundle exec rake db:migrate

# Run the automated test suite:
bundle exec rake db:test:prepare
RAILS_ENV=production bundle exec rake db:fixtures:load
# For the actual running of the tests, I generally run them in two batches: first,
# the tests that don't require a JavaScript driver and then those that do.  (The
# latter are slower and more problematic.)
bundle exec rspec -t ~js
bundle exec rspec -t js

# Restart the Rails server:
#
touch tmp/restart.txt

# Now the new code should be in effect.  Do manual testing of new features and bug fixes, if desired.
```

[Note: If you are using RVM version 1.11 or later, you can omit the "bundle exec" portion of all rake and rspec commands.]
