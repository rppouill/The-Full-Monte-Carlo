# The Full Monte Carlo 🎲

> *You can leave your hat on. Everything else gets randomly sampled.*

Some people strip down to the bare essentials on stage. We strip problems down to the bare essentials too: **throw random points at them and count**. No calculus degree, no clever tricks, just a lot of dice and the law of large numbers doing the heavy lifting.

This repo goes the full Monte Carlo on three problems, from "obviously solvable" to "please don't make me integrate that".

## The lineup

| Notebook | Question | How we cheat |
|---|---|---|
| [`PI.ipynb`](PI.ipynb) | What is π? | Throw darts at a square and count how many land in the quarter circle. |
| [`lake.ipynb`](lake.ipynb) | How big is this weirdly shaped lake? | Drop random pebbles on the map and count the splashes. |
| [`CurveAliasing.ipynb`](CurveAliasing.ipynb) | How much do two Gaussians overlap? | Integrate `min(f₁, f₂)` by averaging it at random points. |

Every notebook has two acts:

1. **The show:** the idea, the maths in plain words, and a first estimate with a nice picture.
2. **Benchmark Time:** hundreds or thousands of independent runs to check that the error really shrinks like $1/\sqrt{n}$, as the theory promises.

## The one trick (it is always the same trick)

If you pick points uniformly at random, the fraction that lands in a region is close to the fraction of space that region occupies:

$$\frac{\text{hits}}{\text{throws}} \approx \frac{\text{area of the target}}{\text{area of the box}}$$

The catch: the error only decreases like $1/\sqrt{n}$. Want **one more correct digit**? Throw **100 times more points**. Monte Carlo is honest, but it is not in a hurry.

## Spoilers

| Problem | Estimate | Truth |
|---|---|---|
| π (100 runs × 10⁷ points) | 3.141603 ± 0.000055 | 3.141593... |
| Gaussian overlap (100 runs × 10⁷ points) | 0.273645 ± 0.000014 | 0.273639 (analytic) |
| Lake area (10 000 random lakes) | see the plot | depends on the lake, obviously |

## Running it

Requires Python 3.10+.

```bash
python -m venv .venv
source .venv/bin/activate
pip install numpy scipy matplotlib numba tqdm jupyter
jupyter lab
```

The benchmarks use [Numba](https://numba.pydata.org/) and all your CPU cores. Your laptop fan is part of the performance.

## Known quirks

- The lake ends up about 4% bigger than requested (holes get filled, edges get smoothed), and the lake benchmark compares against the requested area. So its error curve flattens out around 4% instead of going all the way to zero. The dice are fine; the reference value is wrong.
- Results change every run. That is not a bug, that is the whole point.
