
import math

# -----------------------------
# 1. Warehouse locations
# -----------------------------
warehouses = {
    "W1": [34, 29],
    "W2": [95, 4],
    "W3": [86, 21],
    "W4": [32, 5],
    "W5": [14, 12]
}

# -----------------------------
# 2. Delivery agent locations
# -----------------------------
agents = {
    "A1": [89, 16],
    "A2": [52, 21],
    "A3": [17, 17],
    "A4": [99, 83]
}

# -----------------------------
# 3. Package information
# -----------------------------
packages = [
    {"id": "P1", "warehouse": "W5", "destination": [12, 7]},
    {"id": "P2", "warehouse": "W2", "destination": [100, 1]},
    {"id": "P3", "warehouse": "W5", "destination": [24, 17]},
    {"id": "P4", "warehouse": "W2", "destination": [87, 14]},
    {"id": "P5", "warehouse": "W5", "destination": [6, 2]},
    {"id": "P6", "warehouse": "W3", "destination": [83, 19]},
    {"id": "P7", "warehouse": "W5", "destination": [10, 2]},
    {"id": "P8", "warehouse": "W4", "destination": [37, 13]},
    {"id": "P9", "warehouse": "W1", "destination": [44, 35]},
    {"id": "P10", "warehouse": "W2", "destination": [102, 0]},
    {"id": "P11", "warehouse": "W5", "destination": [7, 22]},
    {"id": "P12", "warehouse": "W4", "destination": [40, 8]}
]

# -----------------------------
# 4. Calculate Euclidean distance
# -----------------------------
def distance(point1, point2):
    x1, y1 = point1
    x2, y2 = point2

    return math.sqrt(
        (x2 - x1) ** 2 + (y2 - y1) ** 2
    )

# -----------------------------
# 5. Assign packages to agents
# -----------------------------
assignments = []
agent_totals = {agent: 0 for agent in agents}

for package in packages:

    package_id = package["id"]
    warehouse_id = package["warehouse"]

    warehouse_location = warehouses[warehouse_id]
    destination = package["destination"]

    # Distance from warehouse to destination
    delivery_distance = distance(
        warehouse_location,
        destination
    )

    # Find the nearest agent to the warehouse
    nearest_agent = min(
        agents,
        key=lambda agent: distance(
            agents[agent],
            warehouse_location
        )
    )

    # Agent's distance to the warehouse
    agent_to_warehouse = distance(
        agents[nearest_agent],
        warehouse_location
    )

    # Total distance for this package
    total_distance = (
        agent_to_warehouse + delivery_distance
    )

    # Save assignment
    assignments.append({
        "package": package_id,
        "warehouse": warehouse_id,
        "agent": nearest_agent,
        "agent_to_warehouse": agent_to_warehouse,
        "delivery_distance": delivery_distance,
        "total_distance": total_distance
    })

    agent_totals[nearest_agent] += total_distance

# -----------------------------
# 6. Display results
# -----------------------------
print("\nPACKAGE ASSIGNMENTS")
print("-" * 90)

print(
    f"{'Package':<10}"
    f"{'Warehouse':<12}"
    f"{'Agent':<10}"
    f"{'Agent->WH':<15}"
    f"{'WH->Dest':<15}"
    f"{'Total':<12}"
)

print("-" * 90)

for item in assignments:
    print(
        f"{item['package']:<10}"
        f"{item['warehouse']:<12}"
        f"{item['agent']:<10}"
        f"{item['agent_to_warehouse']:<15.2f}"
        f"{item['delivery_distance']:<15.2f}"
        f"{item['total_distance']:<12.2f}"
    )

# -----------------------------
# 7. Total distance per agent
# -----------------------------
print("\nTOTAL DISTANCE PER AGENT")
print("-" * 35)

for agent, total in agent_totals.items():
    print(f"{agent}: {total:.2f} units")

# -----------------------------
# 8. Overall distance
# -----------------------------
overall_distance = sum(agent_totals.values())

print("\nOverall total distance:",
      round(overall_distance, 2), "units")
