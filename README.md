# README


## A ruby on rails/express.js MVP for an app that connects people with work projects related to their skills. I created the backend; the frontend was created by a teammate. This is an old project. The steps to get it running are quite convoluted.

<a href="https://ibb.co/pPZQGRQ"><img src="https://i.ibb.co/8B2bTdb/Screenshot-134.png" alt="Screenshot-134" border="0"></a>

## Steps to start running the backend API ##
<br>
Ruby version: 3.3.0<br>
System: Linux/WSL<br>
PostgreSQL version: 14

### Installing RVM and Ruby ###
```
sudo apt-add-repository -y ppa:rael-gc/rvm
sudo apt-get update
sudo apt-get install rvm
sudo usermod -a -G rvm $USER
rvm install 3.3.0
```

### Database initialization ###

```
sudo apt-get install postgresql libpq-dev
sudo -u postgres psql
```

In postgres console
```
create role athena login password 'athena' createdb;
```

In `onboarding-backend`
```
bundle install
rake db:create
rake db:migrate
rake db:seed
rails s
```

## Common errors ##
### Database errors ###
1. **ActiveRecord::ConnectionNotEstablished could not connect to server: No such file or directory. Is the server running locally and accepting connections on Unix domain socket "var/run/postgresql/.s.PGSQL.5432"?** The following are different fixes. Only try one if the previous did not work.
    1. Stop the rails server if it is running. Restart PostgreSQL using `sudo service postgresql restart` and try again
    2. Verify that PostgreSQL is running on port 5432 using `psql --help`. 
        * If it isn't and port 5432 is occupied by a diferent program, add `port: <new-port>` to database.yml
        * If it isn't but port 5432 is free, change the connection port to 5432 using `sudo nano /etc/postgresql/12/main/postgresql.conf`
        * Restart PostgreSQL after changes (`sudo service postgresql restart`)
    3. Edit the configuration file like in the image below (peer -> trust) with `sudo nano /etc/postgresql/12/main/pg_hba.conf`. Then `sudo service postgresql restart` and try again.
    <a href="https://ibb.co/r3NDwNr"><img src="https://i.ibb.co/7QLs1Lm/pg-hba.png" alt="pg-hba" border="0"></a>
### Rails errors ###
1. Bundle failing on gem 'nokogiri'
    * Stop the server and run `sudo apt-get install build-essential patch ruby-dev zlib1g-dev liblzma-dev`
    * Configure the gem with `bundle config build.nokogiri --use-system-libraries`
    * Retry `bundle install`
2. Bundle failing on gem 'pg' or `rake db:migrate` failing
    * Try the database fixes
    * Retry `bundle install`

## To start the frontend API

In `onboarding-frontend`
```
npm run dev
```