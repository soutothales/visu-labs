---
title: "Esse é o Ciclismo Interativo"
date: 2017-12-19T09:12:24-03:00
draft: false
---

<h1 align="center"> Esse é o ciclismo interativo! </h1>

<script src="https://d3js.org/d3.v4.min.js"></script>

<div class="container">
    <style>
     #chart rect {
        fill: orange;
    }
    #chart rect:hover {
        fill: orangered;
    }
    div.tooltip {
      position: absolute;
      text-align: center;
      width: 50px;
      height: 14px;
      padding: 2px;
      font: 12px sans-serif;
      background: lightsteelblue;
      border: 0px;
      border-radius: 8px;
    }
    </style>
    <div class="row">
      <h4 align="center">O gráfico mostra a quantidade de ciclistas durante horários recomendados por especialistas para tomar sol ao redor do Açude Velho</h4>
    </div>
    <div class="row mychart" id="chart">
</div>
</div>

<script type="text/javascript">
    var alturaSVG = 400,
        larguraSVG = 800;

    var	margin = {top: 10, right: 20, bottom:30, left: 60},
          larguraVis = larguraSVG - margin.left - margin.right,
          alturaVis = alturaSVG - margin.top - margin.bottom;


    function desenhaVis(dados) {
        var dataFiltrada = {}
        //criando o "container"/"esqueleto" do gráfico
        var grafico = d3.select('#chart')
            .append("svg")
                .attr('width', larguraVis + margin.left + margin.right)
                .attr('height', alturaVis + margin.top + margin.bottom)
            .append('g')
                .attr('transform', 'translate(' +  margin.left + ',' + margin.top + ')');
        //escalas
       var teste = dados.reduce(function(result, current) {
           if ((current.horario_inicial >= "16:30" && current.horario_inicial <= "18:00")||
            (current.horario_inicial <= "09:30")) {
                  result[current.horario_inicial] = result[current.horario_inicial] || [];
                  result[current.horario_inicial].push(current);
            }
        return result;
        }, {})

       var abc = []

       for (var i in teste) {
           var sum = 0;
           for(var j in teste[i]){
               sum += parseInt(teste[i][j].total_ciclistas);
           }
           abc.push({"total_ciclistas": sum, "horario_inicial": teste[i][j].horario_inicial})
        }

        var tooltip = d3.select("body").append("div")
        .attr("class", "tooltip")				
        .style("opacity", 0);   
        var x = d3.scaleBand().domain(abc.map((data, indice) => data.horario_inicial))
            .range([0, larguraVis]).padding(0.1);

        var y = d3.scaleLinear().domain([0, d3.max(abc, (d, i) => d.total_ciclistas * 2)]).range([alturaVis, 0]);
        //dados mostrados
        grafico.selectAll('g')
            .data(abc)
            .enter()
            .append('rect')
                .attr('x', d => x(d.horario_inicial))   
                .attr('width', x.bandwidth())
                .attr('y', d => y(d.total_ciclistas))
                .attr('height', (d) => alturaVis - y(d.total_ciclistas))
                .call(d3.zoom().on("zoom", function () {
                    grafico.attr("transform", d3.event.transform)
            }))
            .on("mouseover", function(d) {
                tooltip.transition().duration(200).style("opacity", .9);

        		tooltip.html(d.total_ciclistas)
                .style("left", (d3.event.pageX + 20) + "px")		
                .style("top", (d3.event.pageY - 10) + "px");
            })					
        .on("mouseout", function(d) {		
            tooltip.transition()		
                .duration(500)		
                .style("opacity", 0)
      			});	 
        //eixos
        grafico.append("g")
            .attr("class", "x axis")
            .attr("transform", "translate(0," + alturaVis + ")")
            .call(d3.axisBottom(x));
        grafico.append('g')
            .attr('transform', 'translate(0,0)')
            .call(d3.axisLeft(y));  
        grafico.append("text")
            .attr("transform", "translate(-35," + (alturaVis + margin.top)/2 + ") rotate(-90)")
            .text("Quantidade de ciclistas");

      }

    d3.csv('../dados/dados.csv', function(data) {
        desenhaVis(data);
    });
 </script>

</div>
