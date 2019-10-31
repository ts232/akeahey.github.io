<meta charset="utf-8">

<style>
.button {
  background-color: #f16900;
  border: none;
  color: white;
  padding: 15px 25px;
  text-align: center;
  font-size: 16px;
  cursor: pointer;
}

.button:hover {
  background-color: DarkOrange;  
}

 .h1 {
  color: black;
  font-family: "Verdana", sans-serif;
}
</style>

<!-- Load d3.js -->
<script src="https://d3js.org/d3.v4.js"></script>

<!-- Color scale -->
<script src="https://d3js.org/d3-scale-chromatic.v1.min.js"></script>

<h1 class = "h1"> Vettel DNF Home vs. Away </h1>

<body> This visualization demonstrates the percentage of home (Germany) vs. away races in which Sebastian Vettel did not finish between 2016-2019. 


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


var color = d3.scaleOrdinal(['#dd0000','#ffa600'])


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
