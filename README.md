# Akıllı Kuş ([Demo](http://OrhanAbuska.github.io/Akillikus/))

Program that learns to play Flappy Bird by machine learning ([Neuroevolution](http://www.scholarpedia.org/article/Neuroevolution))

![alt tag](https://github.com/OrhanAbuska/Akillikus/blob/gh-pages/img/flappy.png?raw=true)

### [NeuroEvolution.js](http://github.com/OrhanAbuska/Akillikus/Neuroevolution.js) : Utilization
```javascript
// Initialize
var ne = new Neuroevolution({options});

//Default options values
var options = {
    network:[1, [1], 1],    // Perceptron structure
    population:50,          // Population by generation
    elitism:0.2,            // Best networks kepts unchanged for the next generation (rate)
    randomBehaviour:0.2,    // New random networks for the next generation (rate)
    mutationRate:0.1,       // Mutation rate on the weights of synapses
    mutationRange:0.5,      // Interval of the mutation changes on the synapse weight
    historic:0,             // Latest generations saved
    lowHistoric:false,      // Only save score (not the network)
    scoreSort:-1,           // Sort order (-1 = desc, 1 = asc)
    nbChild:1               // number of child by breeding
}

//Update options at any time
ne.set({options});

// Generate first or next generation
var generation = ne.nextGeneration();

//When an network is over -> save this score
ne.networkScore(generation[x], <score = 0>);
```

You can see the NeuroEvolution integration in Flappy Bird in [Game.js](http://github.com/OrhanAbuska/Akillikus/blob/gh-pages/game.js).

How does it work?


Neural Network Architecture
To play the game, each unit (bird) has its own neural network consisted of the next 3 layers:

an input layer with 2 neurons presenting what a bird sees:

1) horizontal distance between the bird and the closest gap
2) height difference between the bird and the closest gap
a hidden layer with 6 neurons

an output layer with 1 neuron used to provide an action as follows:

if output > 0.5 then flap else do nothing

There is used Synaptic Neural Network library to implement entire artificial neural network instead of making a new one from the scratch.

The Main Concept of Machine Learning
The main concept of machine learning implemented in this program is based on the neuro-evolution form. It uses evolutionary algorithms such as a genetic algorithm to train artificial neural networks. Here are the main steps:

create a new population of 10 units (birds) with a random neural network

let all units play the game simultaneously by using their own neural networks

for each unit calculate its fitness function to measure its quality as:

fitness = total travelled distance - distance to the closest gap

when all units are killed, evaluate the current population to the next one using genetic algorithm operators (selection, crossover and mutation) as follows:

1. sort the units of the current population in decreasing order by their fitness ranking
2. select the top 4 units and mark them as the winners of the current population
3. the 4 winners are directly passed on to the next population
4. to fill the rest of the next population, create 6 offsprings as follows:
    - 1 offspring is made by a crossover of two best winners
    - 3 offsprings are made by a crossover of two random winners
    - 2 offsprings are direct copy of two random winners
5. to add some variations, apply random mutations on each offspring.
go back to the step 2