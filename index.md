<meta charset="utf-8">

<style>
.button {
  background-color: #f5f5f5;
  border-color: #dcdcdc;
  color: black;
  padding: 10px 20px;
  text-align: center;
  font-size: 16px;
  cursor: pointer;
}

.button:hover {
  background-color: whitesmoke;  
}

 .h1 {
  color: black;
  font-family: "Verdana", sans-serif;
}
  .h2 {
  color: black;
  font-family: "Verdana", sans-serif;
}
</style>

<!-- Load d3.js -->
<script src="https://d3js.org/d3.v4.js"></script>

<!-- Color scale -->
<script src="https://d3js.org/d3-scale-chromatic.v1.min.js"></script>

<h1 class = "h1"> The Home Race Curse</h1>

<body>
  
<p>Everyone loves a good sports curse, but the home race curse in Formula 1 has been sent swirling with multiple failures among drivers in their home races. In order to explore whether the home race curse is a significant phenomenon or a freak coicidence, these visualizations have been created to compare Home and Away races of multiple drivers over the past few seasons.</p>

<h2 class = "h2">Vettel Home vs. Away DNF</h2>
  
<p>This visualization demonstrates the percentage of home (Germany) vs. away races in which Sebastian Vettel did not finish between 2016-2019.</p>

</body>

<!-- Add 2 buttons -->
<button class = "button" onclick="update(data1)">Away</button>
<button class = "button" onclick="update(data2)">Home</button>

<!-- Create a div where the graph will take place -->
<div id="vettel"></div>


<script>


var width = 500
    height = 450
    margin = 40


var radius = Math.min(width, height) / 2 - margin


var svg = d3.select("#vettel")
  .append("svg")
    .attr("width", width)
    .attr("height", height)
  .append("g")
    .attr("transform", "translate(" + width / 2 + "," + height / 2 + ")");

// HAND ENTERED VETTEL DATA
var data1 = {a: 92, b: 8}
var data2 = {a: 67, b: 34}


var color = d3.scaleOrdinal(['#dd0000','#ffa07a'])


function update(data) {


  var pie = d3.pie()
    .value(function(d) {return d.value; })
    .sort(function(a, b) { console.log(a) ; return d3.ascending(a.key, b.key);} ) // This make sure that group order remains the same in the pie chart
  var data_ready = pie(d3.entries(data))


  var u = svg.selectAll("path")
    .data(data_ready)

  u
    .enter()
    .append('path')
    .merge(u)
    .transition()
    .duration(1000)
    .attr('d', d3.arc()
      .innerRadius(0)
      .outerRadius(radius)
    )
    .attr('fill', function(d){ return(color(d.data.key)) })
    .attr("stroke", "white")
    .style("stroke-width", "2px")
    .style("opacity", 1)


  u
    .exit()
    .remove()

}


update(data1)

</script>


<h2 class = "h2">2019 </h2>
  
<p>This visualization demonstrates the percentage of home races among 14 drivers that raced at home that ended in DNF for home drivers and the percentage of away races for the same 14 drivers that raced away that ended in DNF.</p>
<p>The 14 drivers that raced at home in 2019 were: <br>
Vettel and Hulkenberg (Germany)<br>
Hamilton, Norris, and Russell (Great Britain)<br>
Gasly and Grosjean (France)<br> 
Leclerc (Monaco)<br>
Sainz (Spain)<br>
Perez (Mexico)<br>
Ricciardo (Australia)<br>
Kvyat (Russia)<br>
Stroll (Canada)<br>
Giovinazzi (Italy)</p>

<!-- Add 2 buttons -->
<button class = "button" onclick="update(data1)">Away</button>
<button class = "button" onclick="update(data2)">Home</button>

<!-- Create a div where the graph will take place -->
<div id="2019"></div>


<script>


var width = 500
    height = 450
    margin = 40


var radius = Math.min(width, height) / 2 - margin


var svg = d3.select("#2019")
  .append("svg")
    .attr("width", width)
    .attr("height", height)
  .append("g")
    .attr("transform", "translate(" + width / 2 + "," + height / 2 + ")");

// HAND ENTERED 2019 DATA
var data3 = {Finished: 86, DNF: 14}
var data4 = {Finished: 71, DNF: 29}


var color = d3.scaleOrdinal(['#dd0000','#ffa07a'])


function update(data) {


  var pie = d3.pie()
    .value(function(d) {return d.value; })
    .sort(function(a, b) { console.log(a) ; return d3.ascending(a.key, b.key);} ) // This make sure that group order remains the same in the pie chart
  var data_ready = pie(d3.entries(data))


  var u = svg.selectAll("path")
    .data(data_ready)

  u
    .enter()
    .append('path')
    .merge(u)
    .transition()
    .duration(1000)
    .attr('d', d3.arc()
      .innerRadius(0)
      .outerRadius(radius)
    )
    .attr('fill', function(d){ return(color(d.data.key)) })
    .attr("stroke", "white")
    .style("stroke-width", "2px")
    .style("opacity", 1)


  u
    .exit()
    .remove()

}


update(data3)

</script>
