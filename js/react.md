Dans main.js, ne plus utiliser displayCommits.js (commenter le require)


npm install react react-dom --save

Copier/coller le composant dans main.js et vous l'affichez.
(rajouter des require('react'), require('react-dom'))

var mountNode = document.body;
setInterval(function(){
    ReactDOM.render(
        React.createElement(
            HelloMessage,
            { name: "David "+Math.random() }
        ),
        mountNode
    );
}, 1000);


* Mettre le composant React dans son propre module
* Faire un composant CommitListItem (<li>) et tester avec le premire commit ([0])


## Pour passer un tableau en props

On ne peut pas. Il faut créer un objet.

React.createElement(
    CommitList,
    {list: commits}
)

// CommitList :
//    ...
//    render(){
//      ...
//      this.props.list.map(...)
//    }
      
    


````
module.exports = React.createClass({
  displayName: "CommitListItem",

  render: function render() {
    console.log('this.props', this.props) // !!
  
  
    return React.createElement(
      "li",
      null,
      // ... this.props ....
    );
  }
});

````



React.createClass({

    render: function(){
    
        return React.createElement('ol', {},
            this.props.list.map(function(commit){
                React.createElement('li', {
                    onClick: function(){
                        console.log('commit', commit)
                    }
                },
                commit.commit.message)
            })
        )
    
    }

})






* Faire composant CommitList (avec tous les commits et qui utilise CommitListItem)

# Jour 3



React.createElement(
    Donut,
    {
        data: _.groupBy(commits, function(commit){
            return moment(commit.author.date).format('dddd')
        })
    }
)


* Faire un composant DonutChart.
http://bl.ocks.org/mbostock/3887193
    * props : {
        data: [
            ['label 1', 23],
            ['label 2', 54],
            ['label 3', 100],
            ['label 4', 5],
            ['label 5', 12]
        ]
    }
    * npm install d3-shape --save

````js
var d3Shape = require('d3-shape');
    
    render: function(){
        var props = this.props;
        
        ...
        
        var pie = d3Shape.pie()
            /*.startAngle(-Math.PI/2)
            .endAngle(Math.PI/2);*/
        var arc = d3Shape.arc()
            .innerRadius(xxx)
            .outerRadius(yyy);
        
        var arcDescs = pie(props.data.map(function(d){ return d[1]}));

        ...
        
        props.data.map(function(d, i){
            var arcDesc = arcDescs[i];

            return React.createElement('g', {className: 'arc', key: i},
                React.createElement('path', {
                    d: arc(arcDesc)
                }),
                React.createElement('text', {
                    transform: 'translate('+arc.centroid(arcDesc)+')',
                }, d[0])
            )
        })    
    }
````

    
* Afficher un DonutChart sur votre application
    * (main.js)
    render(createElement('div', {},
        createElement(CommitList, {commits: commits}),
        createElement(DonutChart, {data: makeDonutData(commits)}),
    ))
    * function(commits){
        var res = {};
        
        commits.forEach(function(c){
            var day = moment(c.commit.author.date).format('dddd');
            
            var dayCommits = res[day]
            if(!dayCommits){
                dayCommits = [];
            }
            dayCommits.push(c);
            
            res[day] = dayCommits
        })
        
        return Object.keys(res).map(function(key){
            return [key, res[key].length];
        })
    }
    * {
        data: [
            ['lundi', 23],
            ['mardi', 54],
            ['mercredi', 32],
            ['jeudi', 8765],
            ['vendredi', 54],
        ]
    }

* Question bonus :
    * RainbowChart




// CSS
.ma-classe{
    text-decoration: line-through;
}

// JS
// maClasse.js
module.exports = {
    textDecoration: "line-through"
}

// JS (React) v1
React.createElement('div', {className: 'ma-classe'})

// JS (React) v2
var maClasse = require('./style/maClasse.js')
React.createElement('div', {style: maClasse})




