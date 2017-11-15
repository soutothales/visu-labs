---
title: "Visualizando a Crise"
date: 2017-11-15T17:31:26-03:00
draft: false
---

# Visualizando nossa crise hídrica antes e depois da transposição

## I. Visualizando o geral 

Antes de começarmos a analisar o antes e depois da transposição, vamos observar o comportamento do volume de nosso reservatório de boqueirão desde 1990.

<div id="visTotal" width=300></div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/vega/3.0.7/vega.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/vega-lite/2.0.1/vega-lite.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/vega-embed/3.0.0-rc7/vega-embed.js"></script>

<script>
    const specTotal = {
    "$schema": "https://vega.github.io/schema/vega-lite/v2.json",
    "data": {
        "url": "https://api.insa.gov.br/reservatorios/12172/monitoramento",
        "format": {
        "type": "json",
        "property": "volumes",
        "parse": {
            "DataInformacao": "utc:'%d/%m/%Y'"
                }
            }
        },

    "width": 500,
    "height": 120,

    "mark": {
        "type": "line",
        "interpolate": "monotone"
    },
    "selection": {
      "brush": {"type": "interval", "encodings": ["x"]}
    },
    "encoding": {
      "x": {
        "timeUnit" : "monthyear",
        "field": "DataInformacao",
        "type": "temporal",
        "axis": {"format": "%Y", "title" : "Volume percentual ao longo dos anos"}
       },
      "y": {
        "field": "VolumePercentual",
        "type": "quantitative",
        "axis": {"tickCount": 30, "grid": false, "title": "Volume percentual"}
         }
       }
     };
     
  	vegaEmbed('#visTotal', specTotal).catch(console.warn);
</script>

Nota-se que a cada 5~6 anos o nosso reservatório alterna em tempos de seca e tempos de cheia.

## II. O açude chegou a sangrar...

É importante para nós observar que em 2011, tivemos a maior média de volume.

<div id="visMaior" width=300>
<script>
const specMaior = {
        "$schema": "https://vega.github.io/schema/vega-lite/v2.json",  
  "data": {
        "url": "https://api.insa.gov.br/reservatorios/12172/monitoramento",
        "format": {
        "type": "json",
        "property": "volumes",
        "parse": {
            "DataInformacao": "utc:'%d/%m/%Y'"
                }
            }   
        },
        
    "width": 500,
    "height": 120,

    "transform": [
          {"filter": {"timeUnit": "year", "field": "DataInformacao", "range": [2011, 2011] }}
    ],
            
    "mark": "line",    
    "encoding": {
        "x": {
            "timeUnit": "yearmonth",
            "field": "DataInformacao",
            "type": "ordinal"
        },
        "y": {
            "aggregate": "mean",
            "field": "VolumePercentual",
            "type": "quantitative"
        
    }
    
  }
    };
    
    vegaEmbed('#visMaior', specMaior).catch(console.warn);

</script>
</div>

Seria muito bom se fosse sempre assim!

## III. Quando a coisa desandou?

Desde o início de 2015 o volume de boqueirão andava caindo bruscamente, esses dados estavam tentando alertar a todos nós o que estava por vir, e não era coisa boa...

<div id="visActual" width=300>
<script>
const specActual = {
        "$schema": "https://vega.github.io/schema/vega-lite/v2.json",  
        "data": {
            "url": "https://api.insa.gov.br/reservatorios/12172/monitoramento",
            "format": {
            "type": "json",
            "property": "volumes",
            "parse": {
                "DataInformacao": "utc:'%d/%m/%Y'"
                    }
                }   
            },

        "transform": [
              {"filter": {"timeUnit": "year", "field": "DataInformacao", "range": [2015, 2017] }}
        ],

        "mark": "line",    
        "encoding": {
            "x": {
                "timeUnit": "yearmonth",
                "field": "DataInformacao",
                "type": "ordinal"
            },
            "y": {
                "aggregate": "mean",
                "field": "VolumePercentual",
                "type": "quantitative"

        }

      }
    };
    
    vegaEmbed('#visActual', specActual).catch(console.warn);

</script>
</div>

... e foi caindo, caindo... até que chegamos no volume morto, onde a qualidade da água não era tão apropriada. Isso gerou discussões entre profissionais da área de saúde, uma parte afirmava que a água do volume morto era apropriada para o consumo humano e a outra parte afirmava o contrário, porém, era a única escolha se quiséssemos cidades recebendo água "normalmente".


## IV. A tão esperada transposição

Desde a segunda metade de 2016 o governador do estado da PB estava projetando trazer a transposição do rio São Francisco para o açude de Boqueirão. A demora só deixava os moradores das cidades abastecidas pelo reservatório mais aflitos. O desespero aumentou quando chegou a 2,5% do volume no açude.

<div id="visTransp" width=300>
<script>
const specTransp = {
        "$schema": "https://vega.github.io/schema/vega-lite/v2.json",  
  "data": {
        "url": "https://api.insa.gov.br/reservatorios/12172/monitoramento",
        "format": {
        "type": "json",
        "property": "volumes",
        "parse": {
            "DataInformacao": "utc:'%d/%m/%Y'"
                }
            }   
        },
        
    "width": 500,
    "height": 120,

    "transform": [
          {"filter": {"timeUnit": "year", "field": "DataInformacao", "range": [2017, 2017] }}
    ],
            
    "mark": "line",    
    "encoding": {
        "x": {
            "timeUnit": "yearmonth",
            "field": "DataInformacao",
            "type": "ordinal"
        },
        "y": {
            "aggregate": "mean",
            "field": "VolumePercentual",
            "type": "quantitative"
        
    }
    
  }
    };
    
    vegaEmbed('#visTransp', specTransp).catch(console.warn);

</script>
</div>

Foi então que em Abril de 2017 o açude começou a receber água da transposição e foi possível sair do volume morto a partir de Outubro/2017 e continua subindo. Mesmo com a transposição que ajudou muito, devemos ficar atentos para tomar outras medidas e não enfrentar outra crise assim, já que, não temos controle sobre a chuva.

