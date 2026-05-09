
# what happens if no arguments are provided?

Here we assume all necessary packages are on the host

```bash
python3 completed_script/produce_output.py
    usage: produce_output.py [-h] nodes_in_graph
    produce_output.py: error: the following arguments are required: nodes_in_graph
```

If you have compiled the Docker image, then

```bash
docker run -it -v `pwd`:/scratch --rm interface_demo python3 /scratch/completed_script/produce_output.py
    usage: produce_output.py [-h] nodes_in_graph
    produce_output.py: error: the following arguments are required: nodes_in_graph
```

For the rest of these examples you can prefix the command with `docker run -it -v `pwd`:/scratch --rm interface_demo /scratch` as needed.

# run as script

```
python3 produce_output.py 4
    (3, 0)
    (3, 2)
    (3, 1)
```

# help message

```bash
python3 produce_output.py --help
    usage: produce_output.py [-h] nodes_in_graph

    generate a graph

    positional arguments:
      nodes_in_graph  an integer number of nodes

    optional arguments:
      -h, --help      show this help message and exit
      --seed random_seed  random seed used by Python
```
# write to file on disk

    python3 produce_output.py 4 > file.dat

# write to stdout

    python3 produce_output.py 4 | grep 3
    (3, 2)
    (3, 3)
    (3, 0)

# interactive help

    python3
    >>> import produce_output
    >>> help(produce_output.create_random_graph)

    Help on function create_random_graph in module produce_output:
    create_random_graph(number_of_nodes: int) -> dict
       generate a graph based on user input and return a dictionary
       primary data structure of interest
       Args:
           number_of_nodes: how many nodes in the graph
       Returns:
           the_graph: a dictionary where each key is a non-negative integer and
           the value is a list of integers corresponding to nearest-neighbor nodes
           {'2': [1, 3],
            '1': [2],
            '3': [2]}

       >>> create_random_graph(4)


# use as a library, return full dictionary

    python3
    >>> import produce_output
    >>> produce_output.create_random_graph(4)
    {0: [], 1: [3, 1], 2: [], 3: [2, 3]}

# use as a library, return generator

    python3
    >>> import produce_output
    >>> for edge_tuple in produce_output.next_edge_from_graph_of_size(4):
    ...     print(edge_tuple)
    ...
    (0, 1)
    (1, 0)
    (1, 2)
    (1, 3)
    (2, 2)
    (3, 2)




In addition to the Python cleanliness tools in the Makefile, there are two additional test script written specifically to evaluate `produce_output.py`

```bash
docker run -it -v `pwd`:/scratch --rm interface_demo /bin/sh -c "python3 produce_output.py 4 | python3 validate_graph.py --stdin"
    key=node ID; value = degree
    {0: 3, 2: 2, 1: 3, 3: 2}
    2 nodes have degree 2
    2 nodes have degree 3
```



```bash
docker run -it -v `pwd`:/scratch --rm interface_demo python3 produce_output.py 4 --json > my_output.json

docker run -it -v `pwd`:/scratch --rm interface_demo python3 validate_json_schema.py /scratch/my_output.json 
    Successfully validated file against schema
```


# doctest

```bash
make doctest
    python3 -m doctest -v produce_output.py
    3 items had no tests:
        produce_output
        produce_output.create_random_graph
        produce_output.next_edge_in_graph
    0 tests in 3 items.
    0 passed and 0 failed.
    Test passed.
```

# mypy

```bash
make mypy
    mypy --check-untyped-defs produce_output.py
    Success: no issues found in 1 source file
```

# black

```bash
make black
    black produce_output.py
    All done! ✨ 🍰 ✨
    1 file left unchanged.
```

# mccabe

```bash
make mccabe
python3 -m mccabe produce_output.py
If 43 2
108:0: 'create_random_graph' 3
153:0: 'next_edge_in_graph' 3
If 176 7
```

EOF
