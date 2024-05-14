import random
import numpy as np

# Function to calculate distance between two cities
def distance(city1, city2):
    x1, y1 = city1
    x2, y2 = city2
    return np.sqrt((x2 - x1)**2 + (y2 - y1)**2)

# Function to create a random tour
def create_tour(num_cities):
    return random.sample(range(num_cities), num_cities)

# Function to evaluate fitness (total distance of a tour)
def tsp_fitness(tour, cities):
    total_distance = sum(distance(cities[tour[i]], cities[tour[i - 1]]) for i in range(len(tour)))
    return total_distance

# Function for tournament selection
def tournament_selection(population, fitness_values, k=3):
    selected = []
    for _ in range(len(population)):
        contestants = random.choices(list(range(len(population))), k=k)
        selected_index = min(contestants, key=lambda x: fitness_values[x])
        selected.append(population[selected_index])
    return selected

# Function for ordered crossover
def crossover(parent1, parent2):
    point1 = random.randint(0, len(parent1) - 1)
    point2 = random.randint(point1 + 1, len(parent1))
    child = [-1] * len(parent1)
    child[point1:point2] = parent1[point1:point2]
    for i in range(len(parent1)):
        if parent2[i] not in child:
            for j in range(len(parent1)):
                if child[j] == -1:
                    child[j] = parent2[i]
                    break
    return child

# Function for swap mutation
def mutation(tour):
    index1, index2 = random.sample(range(len(tour)), 2)
    tour[index1], tour[index2] = tour[index2], tour[index1]
    return tour

# Get user input for the number of cities and their coordinates
num_cities = int(input("Enter the number of cities: "))
cities = {}
for i in range(num_cities):
    x, y = map(int, input(f"Enter coordinates for city {i+1} (x y): ").split())
    cities[i] = (x, y)

# Genetic Algorithm parameters
population_size = 100
num_generations = 100
crossover_rate = 0.8
mutation_rate = 0.2

# Create initial population
population = [create_tour(num_cities) for _ in range(population_size)]

# Evolution loop
for gen in range(num_generations):
    fitness_values = [tsp_fitness(tour, cities) for tour in population]
    selected = tournament_selection(population, fitness_values)
    offspring = []
    for i in range(0, len(selected), 2):
        if random.random() < crossover_rate:
            child1 = crossover(selected[i], selected[i + 1])
            child2 = crossover(selected[i + 1], selected[i])
            offspring.extend([mutation(child1), mutation(child2)])
        else:
            offspring.extend([selected[i], selected[i + 1]])
    population = offspring

# Get the best individual
best_tour = min(population, key=lambda tour: tsp_fitness(tour, cities))
best_route = [cities[i] for i in best_tour]

print("Best tour:", best_tour)
print("Best route:", best_route)
print("Total distance:", tsp_fitness(best_tour, cities))
