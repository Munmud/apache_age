# Install and Run AGE and age-viewer in one command

## Goal of this Repository
- Minimize number of steps to install age and age viewer just for trying out
- Preventing version mismatch issue

## Prerequisite
- docker compose

## Installation Guide
- Clone the repository
- Go to project directory 
- run
```bash
git submodule update --init --recursive
```
- replace age-viewer/Dockerfile with following code
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

### Create a node with Person label
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

### Create a node with Animal label
```sql
SELECT * 
FROM cypher('graph_name', 
$$
    CREATE (n:Animal 
        {
            name:"Lucy", 
            type:"Cat"
        }
    )
$$) as (n agtype);
```

### ADD owner edge
```sql
SELECT * 
FROM cypher('graph_name', $$
    MATCH (a:Person), (b:Animal)
    WHERE a.name = 'Moontasir' AND b.name = 'Lucy'
    CREATE (b)-[e:Owner {
        price: "500USD"
    }]->(a)
    RETURN e
$$) as (e agtype);
```

### Display ALL Nodes
```sql
SELECT * 
FROM cypher('graph_name', $$
MATCH (v)
RETURN v
$$) as (v agtype);
```

### Display ALL relation
```sql
SELECT * 
FROM cypher('graph_name', $$
    MATCH (V)-[R]-(V2)
    RETURN V,R,V2

$$) as (V agtype, R agtype, V2 agtype);
```
### Demo View
![Screenshot from 2023-01-02 07-21-57](https://user-images.githubusercontent.com/53787290/210190026-7bea4a4a-f6c3-4722-a36f-42679898fa0d.png)
