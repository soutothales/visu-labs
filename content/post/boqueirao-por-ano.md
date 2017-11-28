---
title: "Boqueirao Por Ano"
date: 2017-11-28T09:34:05-03:00
draft: true
---

<div class="container">
    <div class="row">
      <h2>Visualização de boqueirão</h2>
    </div>
    <div class="row mychart" id="chart">
    </div>
  </div>

  <style>
    /*.mychart circle {
      fill: steelblue;
    }

    .mychart circle:hover {
      fill: goldenrod;
    }*/

  </style>

  <script type="text/javascript">
    "use strict"

    var alturaSVG = 500,
        larguraSVG = 1750;


    function desenhaVis(dados) {
          var grafico = d3.select('#chart') // cria elemento <svg> com um <g> dentro
                            .append('svg')
                              .attr('width', larguraSVG)
                              .attr('height', alturaSVG);



          /*
           * As marcas
           */



           var grupo = grafico.selectAll('g')
                   .data(dados)
                   .enter()
                   .append('g');


         grupo.append('circle')
            .attr("cx", function(d) { return (d.mes) * 125; })
            .attr("cy", 250)
            .attr("r", function(d) { return d.noventa_percentil * 0.60; })
            .style("fill", "goldenrod");

          grupo.append('circle')
             .attr("cx", function(d) { return (d.mes) * 125; })
             .attr("cy", 250)
             .attr("r", function(d) { return d.dez_percentil; })
             .style("fill", "steelblue");

        grupo.append('text')
             .text(function(d){
                return d.mes;
             })
             .attr("x", function(d) { return (d.mes) * 125 - 8; }) //Localização de onde o número ficará na barrano eixo X
             .attr("y", 250);





    }

    d3.csv('dados/boqueirao-por-mes.csv', function(dados) {
      console.log("provavelmente acontece depois");
      desenhaVis(dados);
    });

    console.log("provavelmente acontece primeiro") // me apague quando entender.

  </script>
