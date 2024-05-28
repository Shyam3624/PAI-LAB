#ipl score prediction
from constraint import Problem

# Define the problem
problem = Problem()

# Variables and domains
colors = ["Red", "Green", "Blue"]
vertices = ["A", "B", "C", "D"]

for vertex in vertices:
    problem.addVariable(vertex, colors)

# Constraints
problem.addConstraint(lambda a, b: a != b, ("A", "B"))
problem.addConstraint(lambda a, c: a != c, ("A", "C"))
problem.addConstraint(lambda b, c: b != c, ("B", "C"))
problem.addConstraint(lambda c, d: c != d, ("C", "D"))

# Solve the problem
solutions = problem.getSolutions()

# Print solutions
for solution in solutions:
    print(solution)

........
[5/27, 22:03] Anu😍: def is_consistent(country, color, assignment, neighbors):
    # Check if the assignment of the color to the country is consistent with the neighbors
    for neighbor in neighbors[country]:
        if neighbor in assignment and assignment[neighbor] == color:
            return False
    return True

def backtrack_search(assignment, countries, colors, neighbors):
    if len(assignment) == len(countries):
        return assignment
    
    unassigned_countries = [country for country in countries if country not in assignment]
    country = unassigned_countries[0]
    
    for color in colors:
        if is_consistent(country, color, assignment, neighbors):
            assignment[country] = color
            result = backtrack_search(assignment, countries, colors, neighbors)
            if result is not None:
                return result
            assignment.pop(country)
    
    return None

# Example usage
countries = ['USA', 'Canada', 'Mexico']
colors = ['Red', 'Green', 'Blue']
neighbors = {
    'USA': ['Canada', 'Mexico'],
    'Canada': ['USA', 'Mexico'],
    'Mexico': ['USA', 'Canada']
}

assignment = {}
solution = backtrack_search(assignment, countries, colors, neighbors)
print(solution)
[5/27, 22:03] Anu😍: import math






def minimax(values, depth, maximizing_player, alpha, beta):
    """
    Implementation of the Minimax algorithm with Alpha-Beta Pruning for finding the maximum value.
    
    Parameters:
        values (list): The list of numbers to search for the maximum value.
        depth (int): The depth of the search tree.
        maximizing_player (bool): True if the current player is maximizing, False otherwise.
        alpha (int): The best value that the maximizing player currently can guarantee at the current state.
        beta (int): The best value that the minimizing player currently can guarantee at the current state.
        
    Returns:
        int: The maximum value found.
    """
    if depth == 0 or len(values) == 0:
        return values[0] if maximizing_player else -values[0]

    if maximizing_player:
        max_value = -math.inf
        for value in values:
            max_value = max(max_value, minimax(values[1:], depth - 1, False, alpha, beta))
            alpha = max(alpha, max_value)
            if beta <= alpha:
                break
        return max_value
    else:
        min_value = math.inf
        for value in values:
            min_value = min(min_value, minimax(values[1:], depth - 1, True, alpha, beta))
            beta = min(beta, min_value)
            if beta <= alpha:
                break
        return min_value

# Example usage:
values = [3, 5, 2, 9, 12, 5, 23, 23]
depth = 3
max_value = minimax(values, depth, True, -math.inf, math.inf)
print("Maximum value found:", max_value)
[5/27, 22:03] Anu😍:


def water_jug_problem(capacity_A, capacity_B, target):
    state = (0, 0)  # Initial state where both jugs are empty
    visited = set()  # To keep track of visited states

    # Helper function to pour water from one jug to another
    def pour(state, from_jug, to_jug):
        jug_A, jug_B = state
        if from_jug == 'A':
            if to_jug == 'B':
                pour_amount = min(jug_A, capacity_B - jug_B)
                return (jug_A - pour_amount, jug_B + pour_amount)
        elif from_jug == 'B':
            if to_jug == 'A':
                pour_amount = min(jug_B, capacity_A - jug_A)
                return (jug_A + pour_amount, jug_B - pour_amount)
        return state

    # Depth-first search to find the solution
    def dfs(state):
        if state[0] == target or state[1] == target:
            return True
        visited.add(state)
        next_states = [
            pour(state, 'A', 'B'),
            pour(state, 'B', 'A'),
            (capacity_A, state[1]),
            (state[0], capacity_B),
            (0, state[1]),
            (state[0], 0)
        ]
        for next_state in next_states:
            if next_state not in visited and dfs(next_state):
                return True
        return False

    if dfs(state):
        print("The target amount of water can be measured.")
    else:
        print("The target amount of water cannot be measured.")

# Example usage:
water_jug_problem(3, 5, 4)
