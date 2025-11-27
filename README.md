# Exp-No-10: Implementation of Classical Planning Algorithm 

### NAME: SUBHA SHREE U
### REG NO: 2305002025

## AIM: 
To implement classical planning algorithm for practical real-life situations

## Classical Planning Algorithm:

Classical Planning is a fundamental concept in Artificial Intelligence (AI) that focuses on finding an optimal sequence of actions to reach a desired goal from an initial state.
It assumes a deterministic, fully observable and static environment where the agent knows exactly how each action affects the world. 

It contains:

**State:** Represents the configuration of the world at a specific time i.e all facts that are currently true.

**Action:** Defines transitions between states. Each action has Preconditions (what must be true before it executes.) and Effect (show the action changes the world.).

**Goal:** The desired end-state that satisfies the planner’s objective.

**Plan:** A sequence of actions that transitions the system from the initial state to the goal while satisfying all constraints.

## ALGORITHM:

**Step-1:** Define the initial state

**Step-2:** Define the goal state

**Step-3:** Define the actions

**Step-4:** Find a plan to reach the goal state

**Step-5:** Print the plan

### Example - 1

This program is a simple AI planner that starts from an initial state, checks which actions are possible, applies them, and finds a sequence of steps that reaches the goal state. It basically figures out the right actions automatically using state-space search.

**Program:**
```python

initial_state = {'A': 'Table', 'B': 'Table'}
goal_state = {'A': 'B', 'B': 'Table'}

actions = {
    'move_A_to_B': {'precondition': {'A': 'Table', 'B': 'Table'}, 'effect': {'A': 'B'}},
    'move_B_to_Table': {'precondition': {'A': 'Table', 'B': 'B'}, 'effect': {'B': 'Table'}}
}

plan = find_plan(initial_state, goal_state, actions)
print(plan)

```

**Output:**

```python
['move_A_to_B']
```

### Example - 2
This program uses an AI planning approach to find a sequence of valid actions that transforms the initial arrangement of blocks into the goal arrangement. It checks action preconditions, applies their effects, and uses search to automatically produce the correct plan.

**Program:**

```python
initial_state = {'A': 'Table', 'B': 'Table', 'C': 'Table'}
goal_state = {'A': 'B', 'B': 'C', 'C': 'Table'}

actions = {
    'move_A_to_B': {'precondition': {'A': 'Table', 'B': 'Table'}, 'effect': {'A': 'B'}},
    'move_B_to_C': {'precondition': {'A': 'B', 'B': 'Table', 'C': 'Table'}, 'effect': {'B': 'C'}},
    'move_C_to_Table': {'precondition': {'A': 'B', 'B': 'C', 'C': 'C'}, 'effect': {'C': 'Table'}}
}

plan = find_plan(initial_state, goal_state, actions)
print(plan)
```

**Output:**
```python
['move_A_to_B', 'move_B_to_C']
```

### SAMPLE PROGRAM:

```python

def is_goal_state(current_state, goal_state):
    return all(current_state.get(k) == v for k, v in goal_state.items())


def apply_action(current_state, effect):
    new_state = current_state.copy()
    new_state.update(effect)
    return new_state


def is_applicable(current_state, precondition):
    return all(current_state.get(k) == v for k, v in precondition.items())


def find_plan(initial_state, goal_state, actions):
    queue = [(initial_state, [])]
    visited = set()

    while queue:
        current_state, plan = queue.pop(0)

        # Check goal
        if is_goal_state(current_state, goal_state):
            return plan

        # Mark visited
        state_tuple = tuple(sorted(current_state.items()))
        if state_tuple in visited:
            continue
        visited.add(state_tuple)

        # Expand actions
        for action_name, action_data in actions.items():
            if is_applicable(current_state, action_data['precondition']):
                next_state = apply_action(current_state, action_data['effect'])
                queue.append((next_state, plan + [action_name]))

    return None


# ---------------------------------------------------------
# TEST CASE 1
# ---------------------------------------------------------

initial_state = {'A': 'Table', 'B': 'Table'}

goal_state = {'A': 'B', 'B': 'Table'}

actions = {
    'move_A_to_B': {
        'precondition': {'A': 'Table', 'B': 'Table'},
        'effect': {'A': 'B'}
    }
}

print("PLAN 1:", find_plan(initial_state, goal_state, actions))


# ---------------------------------------------------------
# TEST CASE 2
# ---------------------------------------------------------

initial_state = {'A': 'Table', 'B': 'Table', 'C': 'Table'}

goal_state = {'A': 'B', 'B': 'C', 'C': 'Table'}

actions = {
    'move_A_to_B': {
        'precondition': {'A': 'Table', 'B': 'Table'},
        'effect': {'A': 'B'}
    },
    'move_B_to_C': {
        'precondition': {'A': 'B', 'B': 'Table', 'C': 'Table'},
        'effect': {'B': 'C'}
    }
}

print("PLAN 2:", find_plan(initial_state, goal_state, actions))
```

### SAMPLE OUTPUT:
<img width="802" height="79" alt="image" src="https://github.com/user-attachments/assets/e5cfc784-6eed-4e6d-b366-b1fd7c8cef8a" />

## PROBLEM STATEMENT:

Air Cargo Transport Planning (STRIPS Benchmark Problem)

**Description:**

Air cargo planning: load/unload cargo, fly planes, track state transitions.

**Concepts:**

Multi-step planning

Constraints handling

BFS or DFS planning

## PROGRAM:

```python
# -----------------------------
# AIR CARGO PLANNING (STRIPS)
# -----------------------------

from collections import deque

# Check if goal is satisfied
def is_goal_state(current_state, goal_state):
    return all(current_state.get(k) == v for k, v in goal_state.items())

# Apply action effects
def apply_action(current_state, effect):
    new_state = current_state.copy()
    new_state.update(effect)
    return new_state

# Check if action is valid
def is_applicable(current_state, preconditions):
    return all(current_state.get(k) == v for k, v in preconditions.items())

# BFS Planner
def find_plan(initial_state, goal_state, actions):
    queue = deque([(initial_state, [])])
    visited = set()

    while queue:
        state, plan = queue.popleft()

        # Goal reached?
        if is_goal_state(state, goal_state):
            return plan

        state_tuple = tuple(sorted(state.items()))
        if state_tuple in visited:
            continue
        visited.add(state_tuple)

        # Try each action
        for action_name, action_rules in actions.items():
            if is_applicable(state, action_rules["precondition"]):
                next_state = apply_action(state, action_rules["effect"])
                queue.append((next_state, plan + [action_name]))

    return None


# -------------------------------------
# AIR CARGO PROBLEM - DOMAIN & ACTIONS
# -------------------------------------

initial_state = {
    'Cargo1': 'Airport_A',
    'Cargo2': 'Airport_B',
    'Plane1': 'Airport_A',
    'Plane2': 'Airport_B'
}

goal_state = {
    'Cargo1': 'Airport_B',
    'Cargo2': 'Airport_A'
}

actions = {
    'Load_C1_P1': {
        'precondition': {'Cargo1': 'Airport_A', 'Plane1': 'Airport_A'},
        'effect': {'Cargo1': 'Plane1'}
    },

    'Unload_C1_P1': {
        'precondition': {'Cargo1': 'Plane1', 'Plane1': 'Airport_B'},
        'effect': {'Cargo1': 'Airport_B'}
    },

    'Fly_P1_A_to_B': {
        'precondition': {'Plane1': 'Airport_A'},
        'effect': {'Plane1': 'Airport_B'}
    },

    'Load_C2_P2': {
        'precondition': {'Cargo2': 'Airport_B', 'Plane2': 'Airport_B'},
        'effect': {'Cargo2': 'Plane2'}
    },

    'Unload_C2_P2': {
        'precondition': {'Cargo2': 'Plane2', 'Plane2': 'Airport_A'},
        'effect': {'Cargo2': 'Airport_A'}
    },

    'Fly_P2_B_to_A': {
        'precondition': {'Plane2': 'Airport_B'},
        'effect': {'Plane2': 'Airport_A'}
    }
}

# Run planner
plan = find_plan(initial_state, goal_state, actions)

print("Plan found:")
print(plan)

```

## OUTPUT:

<img width="853" height="104" alt="image" src="https://github.com/user-attachments/assets/3cf3d427-45ab-4bbd-91e1-6f97691dd0f9" />

## RESULT:
Thus, the program to implement Classical Planning Algorithm has been executed successfully.

