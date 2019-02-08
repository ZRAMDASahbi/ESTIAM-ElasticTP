-----------------------------------------------------------------TP - Elastic search E4A-------------------------------------------------------------------------------
1/-docker run -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:6.6.0
-->Pour ce TP, nous allons utiliser un jeu de données contenant des restaurants

2/-curl -s -H "Content-Type: application/json" -XPOST localhost:9200/product/default/_bulk?pretty --data-binary @restaurants.json
-->Pour importer le ?chier JSON , dans une console 

3/-Testerlabase
-->Pour tester l’importationdelabase,il faut ouvrir un navigateur et ouvrirl’URL suivante:http://localhost: 9200/_plugin/head On doit y voir le nom de la base (index sous elasticsearch) : "restaurants". Et la collection (type) : "restaurants"


                                --------------------------------- Questions TP restaurants-------------------------------------
1/- Liste des restaurant japonais ?
curl -H "content-type: application/json" -XGET "localhost:9200/newlist/_search?pretty" -d"
{
  "query": {
    "match": {
      "cuisine": "japonese" 
    } 
  },
  "size": 0
}
------------------------------------------------------------------------------------------

2/-Liste des restaurants du 8 avenue ?
curl -H "content-type: application/json" -XGET "localhost:9200/newlist/_search?pretty" -d"
{
    "query": {
        "match": {
            "address.street": {      
                "query":    "8 Avenue",
                "operator": "and"
            }
        }
    }
}
---------------------------------------------------------------------------------------------------------
3/-Liste des restaurants italien de Manathan ?

curl -H "content-type: application/json" -XGET "localhost:9200/newlist/_search?pretty" -d"
"{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "borough": "Manhattan" 
          }
        },
        {
          "match": {
            "cuisine": "Italian" 
          }
        }
      ]             
 }    
 }
}                       
  }                                 
}"
---------------------------------------------------------------------------------------------------------
4/-
---------------------------------------------------------------------------------------------------------
5/-
---------------------------------------------------------------------------------------------------------
6/-curl -H "content-type: application/json' -XPUT "localhost:9200/newlist/_mapping/personal" -d"
{
  "properties": {
    "borough": { 
      "type":     "text",
      "fielddata": true
    }
  }
}""
curl -H "content-type: application/json" -XGET "localhost:9200/newlist/_search?pretty" -d"
{
    "aggs" : {
        "genres" : {
            "terms" : { "field" : "borough" }
        }
    }

}"
---------------------------------------------------------------------------------------------------------
7/-
 curl -H "content-type: application/json" -XPUT "localhost:9200/newlist/_mapping/personal" -d"
{
  "properties": {
    "cuisine": { 
      "type":     "text",
      "fielddata": true
    }
  }
}"

curl -H "content-type: application/json" -XGET "localhost:9200/newlist/_search?pretty" -d"
{
	  "aggs" : {
      "genres" : {
          "terms" : { "field" : "cuisine" }
      }
  },
      
  "query": {
    "bool": {
      "must": 
        {
          "match": {
            "borough": "Brooklyn"
                   }
        }
            }
          }}                       
  }                                 
}"

---------------------------------------------------------------------------------------------------------
8/-curl -H "content-type: application/json" -XGET "localhost:9200/newlist/_search?pretty" -d"
{
  "sort" : [
      {"grades.score" : {"order" : "desc"}}
   ],

  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "borough": "Manhattan" 
          }
        },
        {
          "match": {
            "cuisine": "Italian" 
          }
        }
      ]             
 }    
 },
 "size" : 1
}                       
  }        
}
"
---------------------------------------------------------------------------------------------------------
9/-
---------------------------------------------------------------------------------------------------------

En cas de l'utilisation de POSTMAN on utlise que l'URL ce qui est entre les accolades , choir "Post" ou "Get",spécifier le type du fichier,dans notre cas c'est fichier JSon, et puis Exécuter la requète.
------------------------------------------------------------------------------------------------------------