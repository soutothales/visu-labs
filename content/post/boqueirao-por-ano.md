---
title: "Boqueirao Por Ano"
date: 2017-11-28T09:34:05-03:00
draft: false
---

<script src="https://d3js.org/d3.v4.min.js"></script>

<div class="container">
    <div class="row">
      <h2>Visualização final de boqueirão</h2>
    </div>
    <div class="row mychart" id="chart">
    </div>
  </div>

<script type="text/javascript">
    "use strict"

    var alturaSVG = 400, larguraSVG = 900;
    var	margin = {top: 10, right: 20, bottom:30, left: 45}, // para descolar a vis das bordas do grafico
        larguraVis = larguraSVG - margin.left - margin.right,
        alturaVis = alturaSVG - margin.top - margin.bottom;



  function desenhaCirculos(dados) {

    var grafico = d3.select('#chart') // cria elemento <svg> com um <g> dentro
      .append('svg')
        .attr('width', larguraVis + margin.left + margin.right)
        .attr('height', alturaVis + margin.top + margin.bottom)
      .append('g') // para entender o <g> vá em x03-detalhes-svg.html
        .attr('transform', 'translate(' +  margin.left + ',' + margin.top + ')');


    var x = d3.scaleBand()
              .domain(dados.map((dados, indice) => dados.mes))
              .range([0, larguraVis]);

    var y = d3.scaleLinear()
              .domain([0, 100])
              .range([alturaVis, 0]);





    grafico.selectAll('g')
            .data(dados)
            .enter()
              .append('circle')
                .attr("cx", d => x(d.mes) + 40)
                .attr("cy", d => y(d.mediana))
                .attr("r", d => d.noventa_percentil * 0.38)
                .style("fill", "goldenrod");


    grafico.selectAll('g')
           .data(dados)
           .enter()
            .append('circle')
               .attr("cx", d => x(d.mes) + 40)
               .attr("cy", d => y(d.mediana))
               .attr("cz", d => -1)
               .attr("r", d => d.dez_percentil * 0.50)
               .style("fill", "steelblue");


    grafico.append("g")
            .attr("class", "x axis")
            .attr("transform", "translate(0," + alturaVis + ")")
            .call(d3.axisBottom(x)); // magica do d3: gera eixo a partir da escala

    grafico.append('g')
            .attr('transform', 'translate(0,0)')
            .call(d3.axisLeft(y));  // gera eixo a partir da escala



  }

  d3.csv('../dados/boqueirao-por-mes.csv', function(dados) {
    desenhaCirculos(dados);
  });

  </script>

</div>
