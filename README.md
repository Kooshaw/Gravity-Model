<span style="font-family:Times New Roman; font-size:14pt;">
<h1 align="center"><b>Gravity Model Python Implementation: Predictive Transportation Planning</b></h2>
</span>

<span style="font-family: Times New Roman; font-size: 13pt;">
The Gravity Model, introduced by Casey in 1955, remains the most widely used synthetic distribution model in transportation planning. Inspired by Newton’s law of gravitation, it assumes that the interaction between different zones is directly proportional to their activity levels—comparable to mass in physics—and inversely proportional to the impedance of travel, such as distance, time, or cost. This model provides a robust framework for estimating trip distribution patterns by predicting how many trips will be generated and attracted between different areas based on these factors.

One of the key advantages of the Gravity Model is its ability to construct Origin-Destination (O-D) matrices without requiring historical travel data, making it especially useful in cases where empirical trip records are limited or unavailable. This predictive capability allows planners to assess the impact of new developments, infrastructure changes, or policy shifts on travel behavior. The model is widely applied in urban and regional transportation planning, offering valuable insights for optimizing traffic flow, improving public transit networks, and supporting sustainable mobility solutions.

Furthermore, variations of the Gravity Model have been developed to enhance its accuracy, incorporating additional parameters such as socioeconomic characteristics, land use patterns, and behavioral factors influencing trip-making decisions. As transportation networks evolve with emerging technologies and shifting urban dynamics, the Gravity Model continues to be a fundamental tool for shaping efficient, data-driven mobility strategies.
</span>

## Overview of the Method

The Furness method offers an algorithmic approach to address the intricacies of the doubly constrained growth-factor problem in transportation planning. The predicted number of trips from zone $i$ to zone $j$, denoted as $T_{ij}$, is given by the formula:

$$ T_{ij} = \alpha \times \frac{P_i \times P_j}{d_{ij}^2} \quad \text{(Similar to the gravitational force formula: } F = G \times \frac{m1 \times m2}{r^2}\text{)} $$

Here, $\alpha$ is a proportionality factor, also known as the calibration parameter. $P_i$ and $P_j$ represent the populations of the origin and destination towns, respectively, while $d_{ij}$ is the distance between these zones. This formula draws a parallel to the gravitational force equation in physics, where the attraction between two objects is proportional to their masses and inversely proportional to the square of the distance between them.

However, the Gravity Model is refined to better reflect urban planning realities. It considers that the number of trips between an origin and a destination zone is influenced by the production capacity of the origin zone, the attractiveness of the destination zone, and the travel costs between them. These factors serve as ideal replacements for the population and distance parameters in the original formula. As a result, the modified formula is presented as follows:

$$ T_{ij} = \rho \times O_i \times D_j \times f(c_{ij}) $$

In this equation, $\rho$ replaces the calibration variable, representing the average trip intensity. $O_i$ is the number of trips originating from zone $i$, indicating its production potential, while $D_j$ is the number of trips destined for zone $j$, reflecting its attraction potential. The function $f(c_{ij})$ represents the accessibility of zone $j$ from $i$, which is a generalized travel cost function. This function, known as the impedance or deterrence function, describes the relative "willingness" to make a trip as a function of travel costs.

Employing principles from physics and adapting them to the nuances of urban travel behavior, the Gravity Model serves as an indispensable tool in predicting and analyzing trip distribution in transportation networks. Its ability to simulate travel patterns in various scenarios, particularly where historical data is lacking, makes it a cornerstone in the field of transportation planning.

In the advanced application of the Gravity Model for urban transportation planning, the formula is enhanced by introducing balancing factors $A_i$ and $B_j$. This modification allows for a more accurate representation of trip distribution, acknowledging the interdependencies between various zones. The revised formula is given by:

$$ T_{ij} = A_i \times O_i \times B_j \times D_j \times f(c_{ij}) $$

Here, $A_i$ and $B_j$ are the balancing factors for the origin and destination zones, respectively, while $O_i$ and $D_j$ represent the trip production and attraction potentials. The function $f(c_{ij})$ continues to be the impedance or deterrence function, indicating the relative ease or difficulty of travel between zones.

To derive the values of $A_i$ and $B_j$, we consider the following relationships: The sum of trips from all origins to a destination $j$ ($\sum_i T_{ij}$) is equal to the sum of the product of the balancing factors, production, and deterrence function for all origins to destination $j$. Since $B_j$ and $D_j$ are constants for each destination, they can be factored out of the summation:

$$ D_j = B_j \times D_j \times \sum_i (A_i \times O_i \times f(c_{ij})) $$

Therefore, the balancing factor $B_j$ is:

$$ B_j = 1 / \sum_i (A_i \times O_i \times f(c_{ij})) $$

Similarly, for the origin $i$, the balancing factor $A_i$ is:

$$ A_i = 1 / \sum_j (B_j \times D_j \times f(c_{ij})) $$

These balancing factors are interdependent, indicating that the calculation of one set requires the values of the other. This necessitates an iterative process, outlined as follows:

1. Initially set all $B_j = 1$.
2. With the current values of $B_j$, compute the balancing factors $A_i$ to satisfy the trip generation constraint:

$$ \sum_j T_{ij} = O_i $$

3. Update $B_j$ using the newly calculated $A_i$ to meet the trip attraction constraint:

$$ \sum_i T_{ij} = D_j $$

4. Calculate $T_{ij}$, update the Origin-Destination (OD) matrix, and check for the error percentage ($E\%$). The error is calculated as: (where $T$ is the total number of trips, equal to the sum of either all $O_i$ or all $D_j$.)

$$ \text{error} = \frac{\sum_i |\sum_j T_{ij} - O_i| + \sum_j |\sum_i T_{ij} - D_j|}{T} $$

5. Repeat steps 2 to 4 until the error percentage is below a predetermined threshold, indicating convergence.

This iterative process ensures the Gravity Model accurately reflects the complex dynamics of trip distribution, making it a powerful tool for urban transportation planning. Its ability to adapt to various scenarios, particularly in the absence of historical data, highlights its importance in developing efficient and responsive transportation systems.

## Code Script

In the implementation of the Gravity Model, a Python script has been developed, providing a computational realization of the algorithm. The script includes a function named `gravity_model`, designed to generate an Origin-Destination (OD) matrix based on given inputs.

The `gravity_model` function accepts the following inputs:
- `O`: Origin matrix, representing the number of trips originating from each zone.
- `D`: Destination matrix, indicating the number of trips destined for each zone.
- `cost_matrix`: A matrix representing the travel costs between zones.
- `deterrence_matrix`: A matrix that accounts for the deterrence effect of travel costs between zones.
- `error_threshold`: A threshold value to determine the stopping condition based on the error percentage (default is 0.01).
- `improvement_threshold`: A threshold to assess the improvement between iterations (default is 1e-4).

The function's core algorithm involves an iterative process to balance the trip distribution based on the production and attraction potential of each zone, adjusted by the cost and deterrence factors. It iteratively calculates the balancing factors $A_i$ and $B_j$ for each zone and updates the OD matrix. The process continues until the change in the error percentage between iterations falls below the defined `improvement_threshold`, or the error percentage is less than the `error_threshold`.

Key features of the script include:
- Format and print functions for easy visualization of matrices.
- Normalization of the Origin and Destination matrices to ensure their sums are equal.
- Calculation of the OD matrix $T_{ij}$ using the updated balancing factors and deterrence matrix.
- Calculation of error percentage to monitor the convergence of the model.
- An iterative approach to update balancing factors and minimize error.

This script serves as a practical tool for applying the Gravity Model, enabling users to implement the algorithm with ease and flexibility while addressing the specific constraints associated with the transportation problem at hand.

### Deterrence Matrix Calculation

In the Gravity Model, deterrence functions play a crucial role in defining the impedance or reluctance to travel between zones. Various functional forms can be used to represent the deterrence effect based on travel cost (`cij`). Below are some common deterrence functions:

1. **Exponential Function:** This function models the deterrence effect as an exponentially decreasing function of the travel cost.
   
$$ f(c_{ij}) = \alpha \times \exp(-\beta \times c_{ij}) $$

2. **Power Function:** In this formulation, the deterrence decreases as a power function of the travel cost.

$$ f(c_{ij}) = \alpha \times c_{ij}^{-\beta} $$
   
3. **Combined Function:** This combined form uses both power and exponential components to model deterrence.

$$ f(c_{ij}) = \alpha \times c_{ij}^{\beta} \times \exp(-\gamma \times c_{ij}) $$
  
4. **Lognormal Function:** The lognormal function applies a squared logarithmic transformation to the travel cost.

$$ f(c_{ij}) = \alpha \times \exp(-\beta \times \ln^2(c_{ij} + 1)) $$
   
5. **Top-Lognormal Function:** This function modifies the lognormal form by adjusting the travel cost with a factor `gamma`.

$$ f(c_{ij}) = \alpha \times \exp(\beta \times \ln^2(c_{ij} / \gamma)) $$
