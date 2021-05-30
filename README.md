# Gas station finder with geolocation
This project uses geolocation to display gas stations around the user (only works in Germany).

The user is able to filter and sort the data so he can find a cheap gas station in his distance.

# Setup
## API Key
This project uses the free api from Tankerkoenig (https://creativecommons.tankerkoenig.de/). To build this repository you need to get an api key for their service and create a .env file in the root directory of this project with the following contents
```
VUE_APP_TOTALLY_NOT_AN_API_KEY=YOURAPIKEY
```
## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

