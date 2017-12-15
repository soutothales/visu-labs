---
title: "Esse é o ciclismo"
date: 2017-12-11T10:48:02-03:00
draft: false
---

# Esse é o ciclismo!

<script src="https://d3js.org/d3.v4.min.js"></script>

Um grupo de pesquisadores ao coletar dados em pontos distintos ao redor do Açude Velho decidiram estudar se mulheres e homens costumam andar de bicicleta em igual proporção.

Tarefa 1: Qual sexo costuma andar mais de bicicleta ao redor do Açude Velho?
-Multiple Line Chart
--Eixo X: Horários
--Eixo Y: Número de ciclistas
--Linha Azul/Cinza: Homens
--Linha Laranja/Vermelha: Mulheres

<div id="graficoDiferenca">
  <style>

  .axis--x path {
    display: none;
  }

  .line {
    fill: none;
    stroke: steelblue;
    stroke-width: 1.5px;
  }

  </style>

  <script>

    function generateLines(data) {

      var svg = d3.select("svg"),
          margin = {top: 20, right: 80, bottom: 30, left: 50},
          width = 700,
          height = 450,
          g = svg.append("g").attr("transform", "translate(" + margin.left + "," + margin.top + ")");


      var x = d3.scaleTime().range([0, width]),
          y = d3.scaleLinear().range([height, 0]),
          z = d3.scaleOrdinal(d3.schemeCategory10);

      var line = d3.line()
        .curve(d3.curveBasis)
        .x(function(d) { return x(d.horario_final); })
        .y(function(d) { return y(d.total_ciclistas); });

      x.domain(d3.extent(data, function(d) { return d.horario_final; }));

    }

  </script>

</div>



Ao observar essa grande diferença os pesquisadores então decidiram averiguar a proporção de mulheres PEDESTRES (em porcentagem).

<div id="graficoPedestre">
    <style>

    .arc text {
    font: 12px sans-serif;
    text-anchor: middle;
    }

    .arc path {
    stroke: #fff;
    }

    </style>

    <script type="text/javascript">

      function generatePie(data) {

        var width = 700,
            height = 250,
            radius = Math.min(width, height) / 2;

        var color = ["#98abc5", "#db8941"];

        var svg = d3.select("#graficoPedestre").append("svg")
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

          g.append("text")
              .attr("transform", function(d) { return "translate(" + labelArc.centroid(d) + ")"; })
              .attr("dy", ".35em")
              .text(function(d) { return d.data + "%"; });


        }

        d3.csv("../dados/dados.csv", function(d) {
          var por_homens = Math.round((d3.sum(d, function(d){return parseFloat(d.homens_pedestres);})) /
                             (d3.sum(d, function(d){return parseFloat(d.total_pedestres);})) * 100);

           var por_mulheres = Math.round((d3.sum(d, function(d){return parseFloat(d.mulheres_pedestres);})) /
                            (d3.sum(d, function(d){return parseFloat(d.total_pedestres);})) * 100);

          var dados = [ por_homens, por_mulheres ];
          generatePie(dados);
        });


    </script>

</div>

Após isso vamos averiguar a proporção de mulheres ciclistas (em porcentagem):

<div id="graficoCiclista">
    <style>

    .arc text {
    font: 12px sans-serif;
    text-anchor: middle;
    }

    .arc path {
    stroke: #fff;
    }

    </style>

    <script type="text/javascript">

      function generatePie2(data) {

        var width = 700,
            height = 250,
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
          generatePie2(dados);
        });


    </script>

</div>




Como foi observado pelos pesquisadores, homens costumam andar mais de bicicleta ao redor do açude velho do que mulheres. Que tal então averiguar qual dos 3 pontos observados costumam haver mais ciclistas (de ambos os sexos)?


<div id="barrasPontos">

  <style>

    text {
      font: 12px sans-serif;
      text-anchor: left;
    }
  </style>

  <script>

  var alturaSVG = 400, larguraSVG = 700;
  var	margin = {top: 10, right: 20, bottom:30, left: 45}, // para descolar a vis das bordas do grafico
      larguraVis = larguraSVG - margin.left - margin.right,
      alturaVis = alturaSVG - margin.top - margin.bottom;



    function desenhaBarras(dados) {

      var color = ["#98abc5", "#db8941", "#58c481"];


      var grafico = d3.select('#barrasPontos') // cria elemento <svg> com um <g> dentro
        .append('svg')
          .attr('width', larguraVis + margin.left + margin.right)
          .attr('height', alturaVis + margin.top + margin.bottom)
        .append('g') // para entender o <g> vá em x03-detalhes-svg.html
          .attr('transform', 'translate(' +  margin.left + ',' + margin.top + ')');


      var x = d3.scaleBand()
                .domain(dados.map((dados, indice) => dados.local))
                .range([0, larguraVis]); // Configure essa escala com domain, range e padding

      var y = d3.scaleLinear()
                .domain([0, 1200])
                .range([alturaVis, 0]);



      grafico.selectAll('g')
              .data(dados)
              .enter()
                .append('rect')
                  .attr('x', d => x(d.local) + 80)   // usando a escala definida acima
                  .attr('width', 50) // largura da barra via escala
                  .attr('y', d => y(d.valor))
                  .attr('height', (d) => alturaVis - y(d.valor)) // de cabeca para baixo
                  .style("fill", function(d, i) { return color[i]; });


      grafico.append("g")
              .attr("class", "x axis")
              .attr("transform", "translate(0," + alturaVis + ")")
              .call(d3.axisBottom(x)); // magica do d3: gera eixo a partir da escala

      grafico.append('g')
              .attr('transform', 'translate(0,0)')
              .call(d3.axisLeft(y));  // gera eixo a partir da escala

      grafico.append("text")
        .attr("transform", "translate(-30," + (alturaVis + margin.top)/2 + ") rotate(-90)")
        .text("Total de Ciclistas");



    }

    d3.csv("../dados/dados.csv", function(data) {
      var dados = []

      dados.push({"local": "Frente do Bob's", "valor": d3.sum(data.filter((d) => d.local === "bobs"), (d, i) => d.total_ciclistas) })
      dados.push({"local": "Monumento do Sesquicentenário", "valor": d3.sum(data.filter((d) => d.local === "burrinhos"), (d, i) => d.total_ciclistas) })
      dados.push({"local": "Jackson do Pandeiro", "valor": d3.sum(data.filter((d) => d.local === "jackson"), (d, i) => d.total_ciclistas) })

      desenhaBarras(dados);
    });

  </script>

</div>

Como podemos ver, nos 3 pontos observados a quantidade de ciclistas é bem parecida, porém o ponto que se observa um maior número de ciclistas é em frente ao Bob's.
