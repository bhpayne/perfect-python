## How to get a Python script with a single function to be production quality?

This repository serves as a demonstration and boilerplate for Python best practices. 
A script (`completed/script/produce_output.py`) generates a random directed graph. 
Everything surrounding that is an example of best practices.

Here's the "trivial task" in 12 lines of Python 3:
```python
#!/usr/bin/env python3
import sys  # command-line arguments
import random  # for graph construction
number_of_nodes = int(sys.argv[1])
the_graph = {}
for node_id in range(number_of_nodes):
    edge_list = random.sample(
        range(number_of_nodes), random.choice(range(number_of_nodes)))
    if node_id in edge_list:
        edge_list.remove(node_id)
    the_graph[node_id] = edge_list
print(the_graph)
````
The specific example above isn't meant to be efficient or even relevant for how one should generate graphs. The purpose is to have code with which to show best practices. For example, 
* wrap the code snippet in a function and then have `main` call the function allows other developers to use that same capability elsewhere by importing the function. See `tutorial_on_how_to_evolve_from_research-grade_to_production/input_files/step01_produce_output.py`
* document the function with a comment block that enables `help`. See `tutorial_on_how_to_evolve_from_research-grade_to_production/input_files/step02_produce_output.py`
* provide command-line help when calling the .py file
* implement logging capability
* wrapping the snippet in a function implies testing the function
* fuzz testing
  * <https://www.fuzzingbook.org/html/Fuzzer.html>
  * <https://fuzzing.readthedocs.io/en/latest/tutorial.html>
  * <https://github.com/google/atheris> and <https://opensource.googleblog.com/2020/12/announcing-atheris-python-fuzzer.html>
* behavioral tests
* regression tests
* performance characterization: scaling tests of memory, wall-clock time
* does the code enact the required capability? 
* create sphinx documentation
* containerize dependencies
* specify version for python dependencies. Better: specify hash for Python dependencies. See <https://github.com/pypa/packaging.python.org/issues/564#issuecomment-428011603> and <https://lil.law.harvard.edu/blog/2019/05/20/improving-pip-compile-generate-hashes/>
* cache the python dependencies offline (rather than relying on pypi over the network)
* build python dependencies from source (rather than relying on wheels)
* use github actions; <https://docs.github.com/en/actions/creating-actions/creating-a-docker-container-action>

# Getting Started

## Read the content

There are four ways to explore this project:
* THe quickest is to look in the folder `completed_script` for the result.
* The tutorial folder contains three ways to assess the progression of adding the capabilities to the trivial task.
  * as a sequence of `.py` files; see `tutorial_on_how_to_evolve_from_research-grade_to_production/input_files/`
  * as a README containing the progression as in-line diffs: `tutorial_on_how_to_evolve_from_research-grade_to_production/README_inline_diff.md`
  * as a README containing the progression as side-by-side diffs: `tutorial_on_how_to_evolve_from_research-grade_to_production/README_diff_side-by-side.md`

## Run the content requires building Docker image

All dependencies, including LaTeX, Sphinx, Doxygen, and Python linters, are containerized using Docker. 

```bash
make docker
```

For how to use the Docker image see `completed_script/README.md`.

### TODO

Not yet enacted

- [ ] write a set of formal requirements against which to evaluate the implementation
- [ ] test for structural consistency against defined specifications.
- [ ] Validate that the Python functions are consistent with API definition
   - [ ] no extra return values
   - [ ] data type correct
   - [ ] correct types in keys and values
   - [ ] valid ranges per variable (int >= 0)
