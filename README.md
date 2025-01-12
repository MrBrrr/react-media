# How to run

Start server: 
```
npm run start:server
```

Start app in separate window:
```
npm start
```

## Tailwind 
Following the setup instructions: https://tailwindcss.com/docs/guides/create-react-app 

That migh change over the time, but current setup looks like here:

tailwind.config.js
```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./src/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

src/index.css
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

And usage:
src/App.js
```javascript
function App() {
    return (
      <h1 className="text-3xl font-bold underline">
        Hello world!
      </h1>  
    )
}
export default App;
```

## API Server Setup
Required changes for API server setup:
db.json
```json
{
    "users": [],
    "albums": [],
    "photos": []
}
```
pckages.json
```json
...
  "scripts": {
    "start": "react-scripts start",
    "start:server": "json-server --watch db.json --port 3005",
```
Two terminal windows are needed to run
`npm start` and `npm run start:server`

The second commands shos somethng like this, that describes where I can make requests to: 
```
Endpoints:
http://localhost:3005/users
http://localhost:3005/albums
http://localhost:3005/photos
```

## Data structure

### Denormalized form

```js
[
    {
        id: 50,
        name: "Myra",
        albums: [
            {id: 1, title: "Album #1"},
            {id: 2, title: "Album #2"}
        ]
    }
    {
        id: 62,
        name: "Ervin",
        albums: [
            {id: 3, title: "Album #3"},
            {id: 4, title: "Album #4"}
        ]
    }
]
```

Embedding of records inside of another:  
- array of objects,  
- the top level one represents a user (with id, name and albums properties),  
- album is an array of objects again (with id and name properties)

+ easier to use if data is already structured
+ good if project has rock-solid requirements that won't change

Listing users seems to be easy, but listing albums not really

### Normalized form

```js
albums = [
    {id: 1, title: "Album #1", userId: 50},
    {id: 2, title: "Album #2", userId: 50},
    {id: 3, title: "Album #3", userId: 62},
    {id: 4, title: "Album #4", userId: 62}
]
users = [
    {id: 50, name: "Myra"},
    {id: 62, name: "Ervin",}
]
```

+ more flexible 
+ more code to write

Listing albums assigned to the user:
```
const getAlbumsOfUser(albums, user) {
  return albums.filter(album => album.userId === uses.id)
}
```
### In this application

Both JSON server and app will use `Normalized Format` 
