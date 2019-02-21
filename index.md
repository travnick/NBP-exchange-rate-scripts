# Fetch rates by date - uses http://api.nbp.pl/
## date format: YYYY-MM-DD
## each date in separate line

<input id="currency" type="text" placeholder="currency code">
<textarea id="dates" width="150" height="350" onpaste="fetchRates()" placeholder="dates: YYYY-MM-DD">
</textarea>
<input type="button" onclick="fetchRates()" value="Fetch rates">

<table>
  <thead>
    <tr>
      <th width="100">Date</th>
      <th>Rate</th>
    </tr>
  </thead>
  <tbody id="rates">
  </tbody>
</table>

<script>
function makeUrl(currency, date)
{
  return `https://api.nbp.pl/api/exchangerates/rates/a/${currency}/${date}?format=json`;
}

function getRate(url) {  
  console.log(url)
  
  var xhttp = new XMLHttpRequest();
  xhttp.open("GET", url, false);
  xhttp.send()
  
  return JSON.parse(xhttp.responseText).rates[0]
}

function getHtmlResult(result)
{
  return ` <tr>
		<td>${result.effectiveDate}</td>
		<td>${result.mid}</td>
	</tr>`;
}

function getHtmlError(date)
{
  return ` <tr>
		<td>${date}</td>
		<td>no result</td>
  	</tr>`;
}

function fetchRates()
{
  var dates = document.getElementById("dates").value.trim().split('\n');
  var currency = document.getElementById("currency").value.trim();
  document.getElementById("rates").innerHTML = ''
  
  if (!dates.length || !dates[0])
  {
  	return
  }

  for (var i in dates)
  {
    var date = dates[i]
    var result
    var url
    try
    {
      url = makeUrl(currency, date)
      result = getRate(url)

      document.getElementById("rates").innerHTML += getHtmlResult(result)
    }
    catch(e)
    {
      document.getElementById("rates").innerHTML += getHtmlError(date)
    }
  }
}
</script>
