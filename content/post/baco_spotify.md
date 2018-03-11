---
title: "Baco e o rap brasileiro no Spotify"
date: 2018-03-11T13:29:49-03:00
draft: false
---

<script src="https://d3js.org/d3.v4.min.js"></script>

# Baco Exu do Blues e o rap brasileiro

<link href="css/bootstrap.min.css" rel="stylesheet">
<link href="css/main.css" rel="stylesheet">
<link href="css/fonts.css" rel="stylesheet">



<div id="chart"></div>

### O que estamos vendo

O grafo acima tem como ponto de referência o rapper Baco Exu do Blues e sua influência na música brasileira segundo dados do Spotify. O grafo apenas mostra apenas os artistas mais tocados e que tem alguma relação com Baco. Além disso o tamanho de cada vértice (nó) do grafo representa as diferentes popularidades desses artistas.

<style>
      .node {
          fill: #ccc;
          stroke: #fff;
          stroke-width: 2px;
      }

      .link {
          stroke: #999;
          stroke-opacity: 0.3;
      }
</style>


<!-- scripts -->

<script>
		var
		    width = 1000,
		    height = 1000;

		var svg = d3.select("#chart")
				.append("svg")
				.attr('version', '1.1')
				.attr('viewBox', '0 0 '+width+' '+height)
				.attr('width', '100%');

		var color = d3.scaleOrdinal(d3.schemeCategory20);

		var simulation = d3.forceSimulation()
		    .force("link", d3.forceLink().id(function(d) { return d.id; }))
		    .force("charge", d3.forceManyBody())
		    .force("center", d3.forceCenter(width / 2, height / 8));

		d3.json("../dados/artistas-semelhantes.json", function(error, graph) {
		  if (error) throw error;


			var superior_nodes = graph.nodes.filter(d => +d.size >= 60);

			var superior_nodes_ids = [];

			superior_nodes.forEach(function(p) {
				return superior_nodes_ids.push(p.id);
			});

			var superior_edges = graph.edges.filter(d => superior_nodes_ids.includes(d.target) &&
																									superior_nodes_ids.includes(d.source));

			console.log(superior_edges);
			console.dir(graph.edges);
			console.dir(graph.nodes);

		  var link = svg.append("g")
		      .attr("class", "link")
		    .selectAll("line")
		    	.data(superior_edges)
		    .enter().append("line");

		  var node = svg.append("g")
		      .attr("class", "nodes")
		    .selectAll("circle")
		    	.data(superior_nodes)
		    .enter().append("circle")
		      .attr("r", function(d) { return d.size/6; })
		      .attr("fill", function(d) { if(d.label == "Baco Exu do Blues") return "Black";
																			return color(d.group);
																		})
		      .call(d3.drag()
		          .on("start", dragstarted)
		          .on("drag", dragged)
		          .on("end", dragended));

		  node.append("title")
		      .text(function(d) { return d.label; });

		  simulation
		      .nodes(graph.nodes)
		      .on("tick", ticked);

		  simulation.force("link")
		      .links(graph.edges);

		  function ticked() {
		    link
		        .attr("x1", function(d) { return d.source.x; })
		        .attr("y1", function(d) { return d.source.y; })
		        .attr("x2", function(d) { return d.target.x; })
		        .attr("y2", function(d) { return d.target.y; });

		    node
		        .attr("cx", function(d) { return d.x; })
		        .attr("cy", function(d) { return d.y; });
		  }
		});

		function dragstarted(d) {
		  if (!d3.event.active) simulation.alphaTarget(0.2).restart();
		  d.fx = d.x;
		  d.fy = d.y;
		}

		function dragged(d) {
		  d.fx = d3.event.x;
		  d.fy = d3.event.y;
		}

		function dragended(d) {
		  if (!d3.event.active) simulation.alphaTarget(0);
		  d.fx = null;
		  d.fy = null;
		}

</script>
