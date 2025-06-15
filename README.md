# 🧬 Heredity AI

Predict gene and trait probabilities across family trees using Bayesian networks and inference by enumeration.

## 🚀 Overview
Given a CSV input of individuals (with parent relationships and known trait occurrences), the AI:

* Computes **joint probabilities** for all gene/trait configurations.
* **Updates** per-person probability distributions.
* **Normalizes** the results to ensure valid probability distributions (summing to 1).

The output displays, for each person, a breakdown of probabilities for having 0, 1, or 2 gene copies, and whether they exhibit the trait

## 🧪 Example Joint Probability
To illustrate how joint probabilities are computed in this project, consider the following data structure:
```
people = {
  'Harry': {'name': 'Harry', 'mother': 'Lily', 'father': 'James', 'trait': None},
  'James': {'name': 'James', 'mother': None, 'father': None, 'trait': True},
  'Lily': {'name': 'Lily', 'mother': None, 'father': None, 'trait': False}
}
```
Suppose we call the function:
```
joint_probability(people, {"Harry"}, {"James"}, {"James"})
```
This configuration indicates:

* ```one_gene = {"Harry"}```: Harry has 1 copy of the gene.
* ```two_genes = {"James"}```: James has 2 copies of the gene.
* ```have_trait = {"James"}```: James exhibits the trait.

We compute the joint probability for:

* Lily: 0 copies, no trait
<br>```→ 0.96 (gene) * 0.99 (trait) = 0.9504```
* James: 2 copies, has trait
<br>```→ 0.01 (gene) * 0.65 (trait) = 0.0065```
* Harry: 1 copy, no trait<br>
→ Gene from mother (0 genes, mutation) = 0.01
<br>Gene from father (2 genes, not mutated) = 0.99
<br> ```→ 0.99 * 0.99 + 0.01 * 0.01 = 0.9802```
<br>```→ 0.9802 * 0.44 (no trait) = 0.431288```

Finally, multiplying them all gives the joint probability:
```
0.9504 * 0.0065 * 0.431288 ≈ 0.002664
```
This example demonstrates how the model factors in inheritance, mutation, and trait expression to compute probabilities over multiple generations.

## 📂 Core Components
* ```heredity.py``` – Implements joint_probability, update, and normalize functions.
* ```data/``` – Contains sample CSV test files with family relationships and trait data.

**Output Example**
```
$ python heredity.py data/family0.csv
Harry:
  Gene:
    2: 0.0092
    1: 0.4557
    0: 0.5351
  Trait:
    True: 0.2665
    False: 0.7335
James:
  Gene:
    2: 0.1976
    1: 0.5106
    0: 0.2918
  Trait:
    True: 1.0000
    False: 0.0000
Lily:
  Gene:
    2: 0.0036
    1: 0.0136
    0: 0.9827
  Trait:
    True: 0.0000
    False: 1.0000
```

## 🛠️ Usage
Clone the project and navigate inside:
```
git clone https://github.com/yourusername/heredity-ai.git
cd heredity-ai
```
Run the program with a dataset:
```
python heredity.py data/family0.csv
```
Results display per-person probability distributions for gene count and trait presence.