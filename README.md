# localportalv2

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production(**Not confirmed**)
```
npm run build
```

### Run your tests(**Not confirmed**)
```
npm run test
```

### Lints and fixes files(**Not confirmed**)
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

## json-server setup

localportalv2 needs json-server. Setup procedure is follows.

### Install
```
npm install -g json-server
```

### Run

Prepare following db.json.

```
{
  "sites": []
}
```

After that, run json-server.

```
json-server --watch db.json
```

### Confirm

Access to http://localhost:3000/sites, and confirm [] is displayed.

Finally access to http://localhost:8080/
