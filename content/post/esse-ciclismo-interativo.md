---
title: "Esse é o Ciclismo Interativo"
date: 2017-12-19T09:12:24-03:00
draft: true
---

# Esse é o ciclismo interativo!

<script src="https://d3js.org/d3.v4.min.js"></script>
<script src="https://d3js.org/colorbrewer.v1.min.js"></script>


<div id="tooltip" class="hidden">
         <p id="titulo_tooltip"></p>
			   <p><b id="value">0</b></p>
</div>

<style>

  .arc text {
    font: 12px sans-serif;
    text-anchor: middle;
  }

  .arc path {
    stroke: #fff;
  }

  #tooltip {
    position: absolute;
    width: auto;
    height: auto;
    padding: 10px;
    background-color: #e8a7e5;
    border-radius: 10px;
  }

  #tooltip.hidden {
    display: none;
  }

  #tooltip p {
    margin: 0;
    font-family: sans-serif;
    font-size: 12px;
    line-height: 20px;
  }

</style>

<div id="graficoCiclista">

    <script type="text/javascript">

      function generatePie(data) {

          var width = 900,
              height = 400,
              radius = Math.min(width, height) / 2;

          var color = ["#98abc5", "#db8941"];

          var svg = d3.select("#graficoCiclista").append("svg")
              .attr("width", width)
              .attr("height", height)
            .append("g")
              .attr("transform", "translate(" + width / 2 + "," + height / 2 + ")");


          var arc = d3.arc()
              .outerRadius(radius - 10)
              .innerRadius(0);

          var labelArc = d3.arc()
              .outerRadius(radius - 40)
              .innerRadius(radius - 40);

          var pie = d3.pie()
              .sort(null)
              .value(function(d) { return d; });

          var g = svg.selectAll(".arc")
              .data(pie(data))
            .enter().append("g")
              .attr("class", "arc");

          g.append("path")
              .attr("d", arc)
              .style("fill", function(d, i) { return color[i]; });

          g.selectAll("arc").on("mouseover", mouseDentro);
          function mouseDentro(d){
            d3.selectAll("text")
              .style("font-weight",
                      p => p.nome_curto === d.nome_curto ? "bold" : "normal");

            d3.select("#tooltip")
  						.style("left", (d3.event.pageX - 50) + "px")
  						.style("top", d3.event.pageY + "px")
  						.select("#value")
  						.text();
            d3.select("#tooltip #titulo_tooltip")
              .text()

  					d3.select("#tooltip").classed("hidden", false);
          }

          g.selectAll("arc").on("mouseout", mouseSaiu);
          function mouseSaiu(d){
            d3.selectAll("text")
              .style("font-weight", "normal");


            d3.select("#tooltip").classed("hidden", true);
          }

          g.append("text")
              .attr("transform", function(d) { return "translate(" + labelArc.centroid(d) + ")"; })
              .attr("dy", ".35em")
              .text(function(d) { return d.data + "%"; });


      }

        d3.csv("../dados/dados.csv", function(d) {
          var por_homens = Math.round((d3.sum(d, function(d){return parseFloat(d.homens_ciclistas);})) /
                             (d3.sum(d, function(d){return parseFloat(d.total_ciclistas);})) * 100);

           var por_mulheres = Math.round((d3.sum(d, function(d){return parseFloat(d.mulheres_ciclistas);})) /
                            (d3.sum(d, function(d){return parseFloat(d.total_ciclistas);})) * 100);

          var dados = [ por_homens, por_mulheres ];
          generatePie(dados);
        });


    </script>

</div>
