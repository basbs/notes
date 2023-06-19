# Postgres 101


## Installation in Ubuntu

```BASH
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo apt-get update
sudo apt-get -y install postgresql

```

## Terminologies
After the installation, below are the commands available in your `$PATH`.

### psql
`psql` is a terminal-based front-end to PostgreSQL. It enables you to type in queries interactively, issue them to PostgreSQL, and see the query results.

### postgres
`postgres` is the PostgreSQL database server. In order for a client application to access a database it connects (over a network or locally) to a running postgres instance. The postgres instance then starts a separate server process to handle the connection.

## Get Started
- After the installation, the default username to access the database is `postgres` 
    ```
    sudo -u postgres psql template1
    ```
>Above command changes the current user to `postgres` and runs the `psql` command. As per the `psql man page` If an argument is found that does not belong to any option it will be interpreted as the database name.

- To set the password for user `postgres`
  ```
  ALTER USER postgres with encrypted password 'your_password';
  ```

## Usage
Once the password is set. Login to the instance using
```
psql -U sampleuser -h localhost
```
> Most of the postgres command shortcuts can be found in [references](#references)

## Troubleshoot

**Q:** `sudo -u postgres psql template1` works, whereas `sudo psql -U postgres template1` doesn't work. Why?
**A:** First command works because, the psql took the default values for `-h`(hostname) `-p`(password) and `-U`(username) in this case it is `postgres`. The second command did not work because, it lacked the default value for the necessary arguments. To fix this `sudo psql -h localhost -U postgres template1`

**Q:** change user to `postgres` and execute `ddl` commands.

**A:** `sudo -i -u postgres`


## References
[Install in ubuntu](https://www.postgresql.org/download/linux/ubuntu/)

[Dummy guide](https://tomcam.github.io/postgres/)



