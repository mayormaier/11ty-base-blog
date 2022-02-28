---
title: Integrating APIs with JavaScript in the Front End
description: Connecting front end applications to multiple APIs 
date: 2022-02-28
tags:
  - javascript
  - webdev
  - frontend
  - api
layout: layouts/post.njk
---

## Fetch and the Power of APIs

`fetch()` is an *asynchronous* JavaScript function that allows client-side web applications to make HTTP Requests to a web endpoint. This is most commonly used to make API calls from the browser.

Asynchronous functions are known as "non-blocking". Rather than taking up a processing thread while waiting for a return value, non-blocking functions allow other operations to execute in the program. This results in much more responsive applications. 

Fetch's asynchronous property is enables it to free the processing thread while waiting for an API response. This is a great way of handing API calls, as responses can vary in speed depending on the destination server and application.

```js
fetch('http://example.com/movies.json')
  .then(response => response.json())
  .then(data => console.log(data));
```
*the above example is courtesy of Mozilla*

The fetch method is relatively simple. In its most basic form, `fetch()` has one parameter, the URL of the HTTP endpoint. Other parameters can be added to send data to an endpoint (i.e., JSON for a HTTP PUT request). This enables developers to fully leverage API requests in their front end applications. 

In the example above, an HTTP GET request was made, which returns data from the server to the client. After the response returns successfully, the `.then()` functions parse the response as JSON, then print it to the console. However, console logging is not the only thing that can be accomplished in this function. 

`.then()` clauses can also be used to pull data from the API response and set it as a variable. For example, in the application presented in this example, the responses from  [freegeoip.app/json](https://www.freegeoip.app/json) are used to identify the location of a user at a specific IP address. The `latitude` `longitude` `city` and `region_name` fields are all variables that the API returns and are tracked by the application. Here's an example of the JSON data returned by the API:

```json
{
"ip": "104.38.28.100",
"country_code": "US",
"country_name": "United States",
"region_code": "PA",
"region_name": "Pennsylvania",
"city": "University Park",
"zip_code": "16802",
"time_zone": "America/New_York",
"latitude": 40,
"longitude": -77,
"metro_code": 574
}
```
*This JSON blob is sample API response from the [freegeoip.app/json](https://www.freegeoip.app/json) API.*

## Element Variables and Rendering

Variable assignment in the `then()` method calls enables stateful updating of the application. Each time that the API is called and returns data successfully, the instance variables are updated, and the DOM is re-painted with the new data. The render() function determines how the page will be displayed each time that the DOM is painted. Not all variables in the application achieve this behavior- only variables defined in the `static get properties()` method trigger the DOM to be re-painted. Note: you can also generate new variables based on the variables that are returned by an API call. For example, I set location equal to `$city, $region_name` which is used many other times in the application.

```js
static get properties() {
    return {
      lat: { type: Number, reflect: true },
      long: { type: Number, reflect: true },
      city: { type: String, reflect: true },
      region: { type: String, reflect: true },
      location: { type: String, reflect: true },
    };
  }
```
*The properties defined in this method trigger the DOM to be re-painted*

Let's discuss the `<location-from-ip>` component in more depth. Firstly, the properties listed above populate the component with the data that it needs to render. The data relies on APIs to populate. The `getGEOIPData()` function includes all of the logic to obtain these data points.

Firstly, a `UserIP` object instance is created to identify the IP address of the user. This relies on an API that returns the IP of the user making the request. This IP addres data is then fed into another API ([freegeoip.app](https://www.freegeoip.app/)) that takes the IP address from the user and returns location data about that IP address. See the example API response above. After the response is retuned, the given variables are updated which triggers a repainting of the DOM. This update feeds those new variables into a number of services defined in the `render()` function:

```js
const url = `https://maps.google.com/maps?q=${this.lat},${this.long}&t=&z=15&ie=UTF8&iwloc=&output=embed`;
    return html`
      <iframe title="Where you are" src="${url}"></iframe>
      <br /><a
        href="https://www.google.com/maps/@${this.lat},${this.long},15z"
        target="_blank"
        >Expand Map to ${this.location}</a
      >
      <br /><br />
      <wikipedia-query search="${this.location}"></wikipedia-query>
      <wikipedia-query search="${this.city}"></wikipedia-query>
      <wikipedia-query search="${this.region}"></wikipedia-query>
    `;
```

1. The `lat` and `long` variables are inserted into an Google Maps embed link that populates an `<iframe>`.
2. The `lat`, `long` and `location` variables are used to populate a hyperlink that opens the location in the full Google Maps site. 
3. The `<wikipedia-query>` web component is leveraged to provide articles about the location determined by the GEOIP API. The compnent relies on a `search` property that defines the wikipedia page to display. There are three `<wikipedia-query>` tags total. One uses the `location` property as the search string, and the other two use `city` and `region`.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/tqtzywaadnkkyumy56ri.png)
*The `<location-from-ip>` element, visually*

Ultimately, the use of APIs within web components can be achieved with ease and is a great way to add responsive elements to a static site.

For more information about the API used in this application, see [freegeoip.app](https://www.freegeoip.app/), [wikipedia element](https://codepen.io/btopro/pen/yLNmVbw), [IPFast IP Address API](https://ip-fast.com/api/ip/)

Check out the [application repository here](https://github.com/mayormaier/API-Project/blob/master/src/LocationFromIP.js)


#### Sources

[General asynchronous programming concepts - MDN Web Docs](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Concepts)
[Using Fetch - MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)

