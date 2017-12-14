---
title: "Esse é o ciclismo"
date: 2017-12-11T10:48:02-03:00
draft: false
---

# Esse é o ciclismo!

<svg width="960" height="500"></svg>


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


<script src="https://d3js.org/d3.v4.min.js">

var svg = d3.select("svg"),
    width = +svg.attr("width"),
    height = +svg.attr("height"),
    radius = Math.min(width, height) / 2,
    g = svg.append("g").attr("transform", "translate(" + width / 2 + "," + height / 2 + ")");


function desenhaGrafico(data) {
  var color = ["#98abc5", "#8a89a6", "#7b6888", "#6b486b", "#a05d56", "#d0743c", "#ff8c00"];

  var pie = d3.pie()
      .sort(null)
      .value(function(d) { return d; });

  var path = d3.arc()
      .outerRadius(radius - 10)
      .innerRadius(0);

  var label = d3.arc()
      .outerRadius(radius - 40)
      .innerRadius(radius - 40);

  D = {
      slice1:d3.sum(data, function(d){return parseFloat(d.mulheres_pedestres);}),
      slice2:d3.sum(data, function(d){return parseFloat(d.homens_pedestres);})
  }


}



d3.csv("../dados/dados.csv", function(data) {
    desenhaGrafico(data);
});

</script>



Após isso vamos averiguar a proporção de mulheres ciclistas (em porcentagem):

Tarefa 3: Saber se existe relação entre a proporção de mulheres pedestres e a proporção de mulheres ciclistas.
-Chart Pie
--Porcentagem mulheres ciclistas
--Porcentagem homens ciclistas


Como foi observado pelos pesquisadores, homens costumam andar mais de bicicleta ao redor do açude velho do que mulheres. Que tal então averiguar qual dos 3 pontos observados costumam haver mais ciclistas (de ambos os sexos)?
Tarefa 4: Qual dos 3 pontos no entorno do açude foi observado maior número de ciclistas?
-Bar Chart

#dados.push({"local": "Frente do Bob's", "valor": d3.sum(data.filter((d) => d.local === "bobs"), (d, i) => d.total_motorizados) })
#dados.push({"local": "Monumento do Sesquicentenário", "valor": d3.sum(data.filter((d) => d.local === "burrinhos"), (d, i) => d.total_motorizados) })
#dados.push({"local": "Jackson do Pandeiro", "valor": d3.sum(data.filter((d) => d.local === "jackson"), (d, i) => d.total_motorizados) })
