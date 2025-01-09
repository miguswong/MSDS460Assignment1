# MSDS460Assignment1
Utilizing PuLP Python Linear Programming package to create a personalized version of Stigler's Diet Problem

## Assignment Details
This assignment has five graded components, with each component worth 30 points:

**Part 1.** Provide documentation for the five packaged food items you have selected for the assignment. Photographs of the Nutrition Facts labels are sufficient. Show your price calculations for serving sizes. [If you cook meals for yourself and prefer not to use packaged food items in this assignment, see the Assignment Addendum below.]

**Part 2.** Specify the linear programming problem in standard form, showing decision variables, objective function with cost coefficients, and weekly nutritional constraints. Describe the problem in plain English. Implement the linear programming problem using Python PuLP or AMPL. Provide the program code and output/listing as plain text files, posting within a GitHub repository dedicated to this assignment. 

**Part 3.** Solve the linear programming problem using your Python PuLP or AMPL code. Describe the solution, showing units (serving sizes) for each of the five food items. What is the minimum cost solution? How much would you need to spend on food each week? Include the text file (listing) of your solution in the GitHub repository.

**Part 4.** Solutions to the diet problem are notorious for their lack of variety. Suppose you require at least one serving of each food item or meal during the week, setting constraints in the diet problem accordingly. Solve the revised linear programming problem. How do these additional constraints change the solution? How much more would you have to spend on food each week? Include the text file (listing) of your solution of this revised problem in the GitHub repository. Suppose this diet solution continues to lack variety. Describe how could you add further variety to your diet by making additional revisions to the model specification.

**Part 5.** Large language models (LLMs), as implemented with ChatGPT, Gemini 2.0 Flash, or other generative AI systems, are being used to solve many problems in data science. Select an LLM-based service or agent with which to work. Pick a free plan or one that is freely available to Northwestern students. (Do not pay for services in completing this assignment.) Indicate which service you have selected, providing its web address or uniform resource locator (URL). See if you can get your selected LLM agent to specify a model for The Diet Problem. See to what extent you can get the agent to develop computer code for The Diet Problem. Report on your success or failure. Provide a step-by-step review of your work. What prompts did you use? How did the conversation with the LLM agent go? Were you able to build on the conversation or to tailor it to your specific needs? As part of the GitHub repository for this assignment, include a plain text file of the complete conversation that you had with the LLM agent. Comment on your experience. Could an LLM agent be used to complete this assignment?


## Part 1: Chosen Foods
The following foods were chosen for the diet problem and are staple ingredients in my daily diet. The following values listed in the table are not per serving, but per 100 grams (which in some cases is equal to 1 serving).

**Food** | **Sodium (mg)** | **Energy (kcal)** | **Protein (g)** | **Vitamin D (mcg)** | **Calcium (mg)** | **Iron (mg)** | **Potassium (mg)** | **Cost ($)**
--- | --- | --- | --- | --- | --- | --- | --- | ---
[White Rice](https://fdc.nal.usda.gov/food-details/169757/nutrients) | 1 | 130 | 2.69 | 0 | 10 | 0.2 | 35 | [0.12](https://www.walmart.com/ip/Great-Value-Long-Grain-Enriched-Rice-20-Lb/10315883?wmlspartner=wlpa&selectedSellerId=0)
[Ground Beef (88/12)](https://www.carbmanager.com/food-detail/md:1747daabb2d1b0d84c13d33d8c4dac75/88-lean-ground-beef) | 58 | 187.5 | 19.6 | 0 | 12.5 | 2.7 | 0 | [1.05](https://www.costcobusinessdelivery.com/kirkland-signature-ground-beef%2C-88%25-lean%2C-10-lb-avg-wt-.product.100082499.html)
[Spinach](https://fdc.nal.usda.gov/food-details/168462/nutrients) | 79 | 23 | 2.86 | 0 | 99 | 2.71 | 558 | [0.70](https://www.walmart.com/ip/Marketside-Fresh-Spinach-10-oz-Bag-Fresh/13893738?classType=REGULAR&athbdg=L1200&from=/search)
[Large Egg](https://www.nutritionix.com/food/large-egg) | 142 | 144 | 12.6 | 2 | 56 | 1.8 | 138 | [0.76](https://www.walmart.com/ip/Great-Value-Large-White-Eggs-12-Count/145051970?wl13=3857&selectedSellerId=0&wmlspartner=wlpa&gStoreCode=3857&gQT=1)
[Banana](https://fdc.nal.usda.gov/food-details/173944/nutrients) | 1 | 89 | 1 | 0 | 5 | 0.26 | 358 | [0.11](https://www.walmart.com/ip/Fresh-Banana-Fruit-Each/44390948?classType=REGULAR&athbdg=L1200&from=/search)

## Linear Programming Problem Variable Specifications

### Plain English Explanation
In the following problem, we are trying to determine the most ideal diet that will meet daily/ weekly nutritional standards while keeping cost as low as possible. The possible foods that can be included in the diet are White Rice, Ground Beef, Spinach, Large Eggs, and Bananas.

### Decision Variables
The following table represents all variables that can be modified to find the most optimal solution. Each variable identifier is arbitrary and is just meant to reduce the length of the other displayed equations.

| **Variable** | **Description**       |
|--------------|-----------------------|
| <sub>x<sub>1</sub></sub> | White Rice        |
| <sub>x<sub>2</sub></sub> | Ground Beef       |
| <sub>x<sub>3</sub></sub> | Spinach           |
| <sub>x<sub>4</sub></sub> | Large Egg         |
| <sub>x<sub>5</sub></sub> | Banana            |




### Objective function with cost coefficients
$\text{Minimize} \: Cost = 0.12x_1 + 1.05x_2 + 0.70x_3 + 0.76x_4 + 0.11x_5$


### Daily nutritional constraints
Each equation would simply need to be multiplied by 7 to look at weekly constraints.
| **Nutrient**  | **Equation**                                                                                      | **Operator** | **Value** |
|---------------|---------------------------------------------------------------------------------------------------|--------------|-----------|
| Sodium (mg)   | 1x<sub>1</sub> + 58x<sub>2</sub> + 79x<sub>3</sub> + 142x<sub>4</sub> + 1x<sub>5</sub>            | ≤            | 5000      |
| Energy (kcal) | 130x<sub>1</sub> + 187.5x<sub>2</sub> + 23x<sub>3</sub> + 144x<sub>4</sub> + 89x<sub>5</sub>      | ≥            | 2000      |
| Protein (g)   | 2.69x<sub>1</sub> + 19.6x<sub>2</sub> + 2.86x<sub>3</sub> + 12.6x<sub>4</sub> + 1x<sub>5</sub>    | ≥            | 50        |
| Vitamin D (mcg) | 0x<sub>1</sub> + 0x<sub>2</sub> + 0x<sub>3</sub> + 2x<sub>4</sub> + 0x<sub>5</sub>              | ≥            | 20        |
| Calcium (mg)  | 10x<sub>1</sub> + 12.5x<sub>2</sub> + 99x<sub>3</sub> + 56x<sub>4</sub> + 5x<sub>5</sub>          | ≥            | 1300      |
| Iron (mg)     | 0.2x<sub>1</sub> + 2.7x<sub>2</sub> + 2.71x<sub>3</sub> + 1.8x<sub>4</sub> + 0.26x<sub>5</sub>    | ≥            | 18        |
| Potassium (mg)| 35x<sub>1</sub> + 0x<sub>2</sub> + 558x<sub>3</sub> + 138x<sub>4</sub> + 358x<sub>5</sub>        | ≥            | 4700      |

### Solving the problem in Python using PuLP
````Python
#Define the variables
WhiteRice = LpVariable("WhiteRice", 0, None)
GroundBeef = LpVariable("GroundBeef", 0, None)
Spinach = LpVariable("Spinach", 0, None)
LargeEgg = LpVariable("LargeEgg", 0, None)
Banana = LpVariable("Banana", 0, None)

#Define the problem (In this case, we want to minimize cost)
prob = LpProblem("Problem", LpMinimize)

#Define the Contraints
prob += 1 * WhiteRice    + 58 * GroundBeef    + 79 *  Spinach  + 142 * LargeEgg   + 1 * Banana <= 5000 #Sodium
prob += 130 * WhiteRice  + 187.5 * GroundBeef + 23 * Spinach   + 144 * LargeEgg   + 89 * Banana >= 2000 #Energy
prob += 2.69 * WhiteRice + 19.6 * GroundBeef  + 2.86 *Spinach  + 12.6 * LargeEgg + 1 * Banana >= 50 #Protein
prob += 0 * WhiteRice    + 0 * GroundBeef     + 0 * Spinach    +  2 * LargeEgg    + 0 * Banana >= 20 #Vitamin D 
prob += 10 * WhiteRice   + 12.5 * GroundBeef  + 99 * Spinach   +  56 * LargeEgg   + 5 * Banana >= 1300 # Calcium
prob += 0.2 * WhiteRice  + 2.7 * GroundBeef   + 2.71 * Spinach +  1.8 * LargeEgg + 0.26 * Banana >= 18 #Iron
prob += 35 * WhiteRice   + 0 * GroundBeef     + 558 * Spinach +  138 * LargeEgg  + 358 * Banana >= 4700 #Potassium

#define the opjective function
prob += 0.12 * WhiteRice + 1.05 * GroundBeef    + 0.7 *  Spinach  + 0.76 * LargeEgg   + 0.11 * Banana


#Solve the problem
status = prob.solve()
print(f"Diet Problem")
print(f"Optimization Status={LpStatus[status]}\n")

# print the results
for variable in prob.variables():
    print(f"{variable.name} = {variable.varValue *100:.1f} grams")
    
print(f"\nDaily Cost = ${value(prob.objective):.2f}")
print(f"Weekly Cost = ${7 * value(prob.objective):.2f}")
print(f"")
````
