---
title: "Esse Ciclismo"
date: 2017-12-11T10:48:02-03:00
draft: false
---

# Esse é o ciclismo!


Um grupo de pesquisadores ao coletar dados em pontos distintos ao redor do Açude Velho decidiram estudar se mulheres e homens costumam andar de bicicleta em igual proporção.

Tarefa 1: Qual sexo costuma andar mais de bicicleta ao redor do Açude Velho?
-Multiple Line Chart
--Eixo X: Horários
--Eixo Y: Número de ciclistas
--Linha Azul: Homens
--Linha Laranja/Vermelha: Mulheres



Ao observar essa grande diferença os pesquisadores então decidiram averiguar a proporção de mulheres PEDESTRES (em porcentagem).

Tarefa 2: Saber a proporção de mulheres pedestres para o total de pedestres.
-Chart Pie
--Porcentagem mulheres pedestres
--Porcentagem homens pedestres



Após isso vamos averiguar a proporção de mulheres ciclistas (em porcentagem):

Tarefa 3: Saber se existe relação entre a proporção de mulheres pedestres e a proporção de mulheres ciclistas.
-Chart Pie
--Porcentagem mulheres ciclistas
--Porcentagem homens ciclistas


Como foi observado pelos pesquisadores, homens costumam andar mais de bicicleta ao redor do açude velho do que mulheres. Que tal então averiguar qual dos 3 pontos observados costumam haver mais ciclistas (de ambos os sexos)?
Tarefa 4: Qual dos 3 pontos no entorno do açude foi observado maior número de ciclistas?
-Bar Chart



<div class="container">
    <style>
     #chart rect {
        fill: #6C44FF;
    }
    </style>
    <div class="row">
      <h2>Ciclistas em cada ponto observado no Açude Velho</h2>
    </div>
    <div class="row mychart" id="chart">
</div>
</div>

  <script type="text/javascript">
  ipt type="text/javascript">
  var alturaSVG = 400,
      larguraSVG = 600;

  var	margin = {top: 10, right: 20, bottom:30, left: 60},
        larguraVis = larguraSVG - margin.left - margin.right,
        alturaVis = alturaSVG - margin.top - margin.bottom;


  function desenhaVis(data) {

      var grafico = d3.select('#chart')
          .append("svg")
              .attr('width', larguraVis + margin.left + margin.right)
              .attr('height', alturaVis + margin.top + margin.bottom)
          .append('g')
              .attr('transform', 'translate(' +  margin.left + ',' + margin.top + ')');


      var x = d3.scaleBand().domain(data.filter((d) =>
          (d.horario_inicial <= "13:00" && d.horario_inicial >= "11:30")||
          (d.horario_inicial <= "19:00" && d.horario_inicial >="17:30"))
          .map((data, indice) => data.horario_inicial))
          .range([0, larguraVis]).padding(0.1);

      var y = d3.scaleLinear().domain([0, 1500]).range([alturaVis, 0]);


      grafico.selectAll('g')
          .data(data)
          .enter()
          .append('rect')
              .attr('x', d => x(d.horario_inicial))   
              .attr('width', x.bandwidth())
              .attr('y', d => y(d.total_motorizados))
              .attr('height', (d) => alturaVis - y(d.total_motorizados));



      grafico.append("g")
          .attr("class", "x axis")
          .attr("transform", "translate(0," + alturaVis + ")")
          .call(d3.axisBottom(x));
      grafico.append('g')
          .attr('transform', 'translate(0,0)')
          .call(d3.axisLeft(y));  
      grafico.append("text")
          .attr("transform", "translate(-35," + (alturaVis + margin.top)/2 + ") rotate(-90)")
          .text("Quantidade de carros");

    }

    d3.csv('dados/boqueirao-por-mes.csv', function(dados) {
      console.log("https://github.com/luizaugustomm/pessoas-no-acude/blob/master/dados/processados/dados.csv");
      desenhaVis(dados);
    });

    console.log("provavelmente acontece primeiro") // me apague quando entender.

  </script>
