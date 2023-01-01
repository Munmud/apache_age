# apache_age

## Prerequisite
- docker compose

## Installation Guide
- Clone the repository
- Go to project directory 
- run
```bash
git submodule update --recursive
```
- replace age-viewer/Dockerfile
```dockerfile 
FROM node:14-alpine3.16
RUN npm install pm2
WORKDIR /src
COPY . .
RUN npm run setup
EXPOSE 3000
```
- run 
```bash
docker compose up
```
- Visit URL 
```url
localhost:3000
```
- Fill the form as below :
    - Connect URL : `age`
    - Connect Port : `5432`
    - Database Name : `postgresDB`
    - User Name : `postgresUser`
    - Password : `postgresPW`

## AGE Viewer command
### Create Graph
```sql
SELECT create_graph('graph_name');
```

### Create a node with Person lebel
```sql
SELECT * 
FROM cypher('graph_name', 
$$
    CREATE (n:Person 
        {
            name:"Moontasir", 
            bornIn:"Chittagong"
        }
    )
$$) as (n agtype);
```

### Display Everything
```sql
SELECT * 
FROM cypher('graph_name', $$
MATCH (v)
RETURN v
$$) as (v agtype);
```