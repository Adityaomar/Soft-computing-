import random

Define items (value, weight)



items = [(10, 5), (40, 4), (30, 6), (50, 3)]
max_weight = 10
population_size = 10
generations = 50
mutation_rate = 0.1

Initialize population randomly



def initialize_population():
    return [[random.choice([0, 1]) for _ in items] for _ in range(population_size)]

Calculate fitness (total value if weight <= max_weight)



def fitness(individual):
    value = sum(ind * itm[0] for ind, itm in zip(individual, items))
    weight = sum(ind * itm[1] for ind, itm in zip(individual, items))
    return value if weight <= max_weight else 0  # Penalize overweight

Selection (roulette wheel method based on fitness)



def select(population):
    weights = [fitness(ind) for ind in population]
    return random.choices(population, weights=weights, k=2)

Crossover (single-point)



def crossover(parent1, parent2):
    point = random.randint(1, len(parent1) - 1)
    return parent1[:point] + parent2[point:], parent2[:point] + parent1[point:]

Mutation (flip a bit with some probability)



def mutate(individual):
    return [1 - gene if random.random() < mutation_rate else gene for gene in individual]

GA main loop



population = initialize_population()

for generation in range(generations):
    new_population = []
    for _ in range(population_size // 2):
        # Selection
        parent1, parent2 = select(population)
        
        # Crossover
        offspring1, offspring2 = crossover(parent1, parent2)
        
        # Mutation
        offspring1 = mutate(offspring1)
        offspring2 = mutate(offspring2)
        
        # Add to new population
        new_population.extend([offspring1, offspring2])
    
    # Replace old population
    population = new_population

Best solution



