---
layout: page
permalink: /about/
title: About
---
This site is built with [fastpages](https://github.com/fastai/fastpages)

## Key Links
- GitHub Repos:  <a href="https://github.com/nighthawkcoders">github.com/nighthawkcoders</a>, <a href="https://github.com/nighthawkcoders/spring_portfolio/generate">Spring Portfolio Template</a>
- AWS Deployments: <a href="https://csa.nighthawkcodingsociety.com/">csa.nighthawkcodingsociety.com</a>
- Slack: <a href="https://join.slack.com/t/cs-a-hq/shared_invite/zt-uasn6lmf-_ij463qEW71hsvI2S9nDsg">Join Link</a>
- 2021-2022 Archives: <a href="https://padlet.com/jmortensen7/csa2022tri1">Fall</a>, <a href="https://padlet.com/jmortensen7/csa2022tri2">Early Winter</a>, <a href="https://csacoders.nighthawkcodingsociety.com/">Late Winter, Spring</a>
![QR Code]({{site.baseurl}}/images/qr_code.png)

## Students
<!-- HTML table fragment for page -->
<table>
  <thead>
  <tr>
    <th>Name</th>
    <th>ID</th>
    <th>Age</th>
  </tr>
  </thead>
  <tbody id="result">
    <!-- javascript generated data -->
  </tbody>
</table>

<!-- Script is layed out in a sequence (no function) and will execute when page is loaded -->
<script>
  // prepare HTML result container for new output
  const resultContainer = document.getElementById("result");

  // prepare fetch options
  const url = "https://csa.nighthawkcodingsociety.com/api/person/all";
  const options = {
    method: 'GET', // *GET, POST, PUT, DELETE, etc.
    mode: 'cors', // no-cors, *cors, same-origin
    cache: 'default', // *default, no-cache, reload, force-cache, only-if-cached
    credentials: 'omit', // include, *same-origin, omit
    headers: {
      'Content-Type': 'application/json'
      // 'Content-Type': 'application/x-www-form-urlencoded',
    },
  };

  // fetch the API
  fetch(url, options)
    // response is a RESTful "promise" on any successful fetch
    .then(response => {
      // check for response errors
      if (response.status !== 200) {
          const errorMsg = 'Database response error: ' + response.status;
          console.log(errorMsg);
          const tr = document.createElement("tr");
          const td = document.createElement("td");
          td.innerHTML = errorMsg;
          tr.appendChild(td);
          resultContainer.appendChild(tr);
          return;
      }
      // valid response will have json data
      response.json().then(data => {
          console.log(data);
          for (const row of data) {
            // tr and td build out for each row
            const tr = document.createElement("tr");
            const name = document.createElement("td");
            const id = document.createElement("td");
            const age = document.createElement("td");
            // data is specific to the API
            name.innerHTML = row.name; 
            id.innerHTML = row.email; 
            age.innerHTML = row.age; 
            // this build td's into tr
            tr.appendChild(name);
            tr.appendChild(id);
            tr.appendChild(age);
            // add HTML to container
            resultContainer.appendChild(tr);
          }
      })
  })
  // catch fetch errors (ie ACCESS to server blocked)
  .catch(err => {
    console.error(err);
    const tr = document.createElement("tr");
    const td = document.createElement("td");
    td.innerHTML = err;
    tr.appendChild(td);
    resultContainer.appendChild(tr);
  });
</script>
