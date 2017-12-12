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







<script src="https://d3js.org/d3.v4.min.js"></script>

<div class="container">

    <script type="text/javascript">

        d3.csv('https://github.com/luizaugustomm/pessoas-no-acude/blob/master/dados/processados/dados.csv', function(dados) {
          desenhaCirculos(dados);
        });

    </script>

</div>
