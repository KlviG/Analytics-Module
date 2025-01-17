# Analytics-Module

### Dev setup

- Clone [Ruuter](https://github.com/buerokratt/Ruuter)
- Navigate to Ruuter and build the image `docker build -t ruuter .`
- Clone [Resql](https://github.com/buerokratt/Resql)
- Navigate to Resql and build the image `docker build -t resql .`
- Clone [Data Mapper](https://github.com/buerokratt/DataMapper)
- Navigate to Data Mapper and build the image `docker build -t data-mapper .`
- Clone [TIM](https://github.com/buerokratt/TIM)
- Navigate to TIM and build the image `docker build -t tim .`
- Clone [Cron Manager](https://github.com/buerokratt/CronManager.git)
- Navigate to Cron Manager dev branch and build the image `docker build -t cron-manager .`
- Running locally need to set ENV variable REACT_APP_LOCAL to true (default).
- Navigate to current repo and run `docker compose up -d`

- Go to https://localhost:3001

### Database setup

- For setting up the database initially, run
  `docker run --platform linux/amd64 --network=bykstack riaee/byk-users-db:liquibase20220615 --url=jdbc:postgresql://users_db:5432/byk --username=byk --password=01234 --changelog-file=./master.yml update`
- Run migrations added in this repository by running the helper script `./migrate.sh`
- When creating new migrations, use the helper `./create-migration.sh name-of-migration` which will create a timestamped file in the correct directory and add the required headers

Database configuration seed for developers:
Run the following command in your terminal when the users_db container is running, to add a default user and bot configuration

```
docker exec users_db psql byk byk -c "INSERT INTO public."user" (login,password_hash,first_name,last_name,id_code,display_name,status,created) VALUES
         ('EE90009999999','t','t','t','EE90009999999','t',NULL,NULL);
INSERT INTO public."configuration" ("key",value) VALUES
         ('bot_institution_id','botname');"
```

### Data Mapper Changes

- In Server.js add

```
app.post('/hbs/*', (req, res) => {
    res.render(req.params[0], req.body, function(_, response) {
        if (req.get('type') === 'csv') {
            res.json({response});
        } else if (req.get('type') === 'json') {
            res.json(JSON.parse(response));
        }
        res.render(req.params[0], req.body);
    });
});
```

to enable handlebars templates to receive a body and return a json

- When Building a handlebars template make sure to add `layout:false` so that hbs response in the data-mapper will discard the html layout and only return the body data

### Use external components(Header and Main Navigation).

Currently, Header and Main Navigation used as external components, they are defined as dependency in package.json

```
 "@buerokrat-ria/header": "^0.0.1"
 "@buerokrat-ria/menu": "^0.0.1"
 "@buerokrat-ria/styles": "^0.0.1"
```

### TIM

- if you are running `Locally` then you need to curl the login request or run it on postman first to create and store the cookie in TIM and then on the browser create the cookie manully in the browser with name `customJwtCookie` and the value return from the curl
  the curl request is as follows:

```
curl -X POST -H "Content-Type: application/json" -d '{
  "login": "EE30303039914",
  "password": ""
}' http://localhost:8080/login-user
```

 
