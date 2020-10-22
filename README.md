# Take Home
This project is to help people who are next to return from their holidays and want to find the best office of their company based on the best weather or cheapest flight.

Built with:

  - Vue.js

  - Tailwind

### Demo

[Take Home](https://takehomeapp.000webhostapp.com/)

## Project setup
```
npm install
```

#### Compiles for development
```
npm run serve
```

#### Compiles and minifies for production
```
npm run build
```

## APIs

  - Flight tickets and Autocomplete: [Kiwi](https://docs.kiwi.com/)
  - Current Weather: [Open Weather Map](https://openweathermap.org/current)

## Features

  - Autocomplete to search current location
  - Validations for current location
  - Filter by: Best weather | Cheapest flight | Both options
  - Empty state (initial state)
  - Loading
  - Display weather icon

## Future code improvements

  - Implement unit testing
  - Vuex to hold shared data like the offices list
  - Improve validations
  - Hold API Key in .env variable
  - Proxy API calls in server to prevent expose the API Key
  - Add support for image in retina "@2x"
  - Style `<select>` and `<datalist>`
  - Prevent native zoom in mobile devices
  
## Features: New ideas to implement

  - Add field "When": Date input to select desired date to travel
  - Option to display temperature in: Celcius | Farenheit
  - Option to change flight price currency
  - Filter by current season
  - Add offices image for retina devices
  - Find a better way to hold cities that doesn't have an airport / are very small cities.
