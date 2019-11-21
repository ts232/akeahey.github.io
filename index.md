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
  .title {
  text-align: center;
  color: black;
  font-family: "Verdana", sans-serif;
}

</style>

<!-- Load d3.js -->
<script src="https://d3js.org/d3.v4.js"></script>

<!-- Color scale -->
<script src="https://d3js.org/d3-scale-chromatic.v1.min.js"></script>

<h1 class = "h1"> The Home Race Curse</h1>
  
<p>Everyone loves a good sports curse, but the home race curse in Formula 1 has been sent swirling with multiple failures among drivers in their home races. In order to explore whether the home race curse is a significant phenomenon or a freak coicidence, these visualizations have been created to compare Home and Away races of multiple drivers over the past few seasons.</p>


<h2 class = "h2">2019 Season</h2>
  
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

<p>Each of the 14 drivers drove at home, giving us 14 home races in 10 countries and 126 away races in the same 10 countries. </p>

<h3 class = "title">Finished vs. Unfinished Races</h3>

<!-- Add 2 buttons -->
<button class = "button" onclick="update(data1)">Away</button>
<button class = "button" onclick="update(data2)">Home</button>

<!-- Create a div where the graph will take place -->
<div id="season">


<script>

var width = 450
    height = 450
    margin = 40

var radius = Math.min(width, height) / 2 - margin

var svg = d3.select("#season")
  .append("svg")
    .attr("width", width)
    .attr("height", height)
  .append("g")
    .attr("transform", "translate(" + width / 2 + "," + height / 2 + ")");

var data1 = {Finished: 86, DNF: 14}
var data2 = {Finished: 71, DNF: 29}

var color = d3.scaleOrdinal(['#b22222', '#cd5c5c'])

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

svg.append("circle").attr("cx",140).attr("cy",-210).attr("r", 6).style("fill", "#b22222")
svg.append("circle").attr("cx",140).attr("cy",-180).attr("r", 6).style("fill", "#cd5c5c")
svg.append("text").attr("x", 160).attr("y", -210).text("Finished").style("font-size", "15px").attr("alignment-baseline","middle")
svg.append("text").attr("x", 160).attr("y", -180).text("DNF").style("font-size", "15px").attr("alignment-baseline","middle")

</script>

</div>

<p>One further exploration would be how to quantify the "curse" beyond DNF record, for example as listed in <a href="https://www.reddit.com/r/formula1/comments/cpysq1/the_home_race_curse/">this</a> Reddit post. </p>


<!-- Create a div where the graph will take place -->
<div id="drivers">
  FOO
<script>
var svg = d3.select("drivers"),
            margin = {
                top: 20,
                right: 60,
                bottom: 30,
                left: 60
            },

var width = 450;
    height = 400;
    
g = svg.append("g").attr("transform", "translate(" + margin.left + "," + margin.top + ")");

var y = d3.scaleBand()
    .rangeRound([0, width])
    .padding(0.2)
    .align(0.1);

var x = d3.scaleLinear()
    .rangeRound([height, 0]);

var z = d3.scaleOrdinal()
    .range(['#b22222', '#cd5c5c']);

var stack = d3.stack()
    .offset(d3.stackOffsetExpand);

d3.csv("data.csv", type, function (error, data) {
    if (error) throw error;


y.domain(data.map(function (d) {
    return d.State;
}));
z.domain(data.columns.slice(1));

var serie = g.selectAll(".serie")
    .data(stack.keys(data.columns.slice(1))(data))
    .enter().append("g")
    .attr("class", "serie")
    .attr("fill", function (d) {
        return z(d.key);
    });

var bar = serie.selectAll("rect")
    .data(function (d) {
        return d;
    })
    .enter().append("rect")
    .attr("y", function (d) {
        return y(d.data.State);
    })
    .attr("x", function (d) {
        return x(d[1]);
    })
    .attr("width", function (d) {
        return x(d[0]) - x(d[1]);
    })
    .attr("height", y.bandwidth());

bar.append("text")
    .attr("x", function (d) {
        return x(d[1]);
    })
    .attr("dy", "1.35em")
    .text(function (d) { return d; });


g.append("g")
    .attr("class", "axis axis--y")
    .call(d3.axisLeft(y));

var legend = serie.append("g")
    .attr("class", "legend")
    .attr("transform", function (d) {
        var d = d[0];
        return "translate(" + ((x(d[0]) + x(d[1])) / 2) + ", " + (y(d.data.State) - y.bandwidth()) + ")";
    });

});

function type(d, i, columns) {
    var t;
    for (i = 1, t = 0; i < columns.length; ++i) t += d[columns[i]] = +d[columns[i]];
    d.total = t;
    return d;
}

</script>


</div>
