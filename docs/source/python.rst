Python - pyDicePlot
======================

.. image:: https://badge.fury.io/py/pydiceplot.svg
    :target: https://pypi.org/project/pydiceplot/
    :alt: PyPI Status Badge

The pyDicePlot package allows you to create visualizations (dice plots) for datasets with more than two categorical variables and additional continuous variables. This tool is particularly useful for exploring complex categorical data and their relationships with continuous variables.

Requirements and installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Using Conda
-----------

To set up the environment and install dependencies:

.. code-block:: bash

    conda create --name pyDicePlot python=3
    conda activate pyDicePlot
    pip install -r requirements.txt

Installation via pip
--------------------

To install the package via pip, run:

.. code-block:: bash
    pip install -e .kv

from the base directory.

Sample Output

.. raw:: html
    :file: python_plots/dice_plot_4_example.html

:alt: Sample Dice with 3 categories Plot

.. raw:: html
    :file: python_plots/dice_plot_6_example.html
Figure: A sample dice plot generated using the pyDicePlot package.


Diceplot
~~~~~~~~
Here is a simple example of how to use the ``pydiceplot`` package using dummy data.
The example shows a dummy dataframe containing three hypothetical categorical variables: ``CellType``, ``Pathway``, and ``PathologyVariable``.
The ``Group`` variable is used to assign different colors to the pathways.

.. code-block:: python 

    from pydiceplot import dice_plot
    from pydiceplot.plots.backends._dice_utils import (get_diceplot_example_data,
                                                       get_example_group_colors,
                                                       get_example_cat_c_colors)
    import pydiceplot

    #Set the backend for pydiceplot
    pydiceplot.set_backend("matplotlib")
    pydiceplot.set_backend("plotly")
    
    if __name__ == "__main__":
        plot_path = "./plots"
    
        # define colors for the example plot
    
    
        # Generate and save dice plots for different numbers of pathology variables
        for n in [2,3, 4, 5, 6]:
            # Get the data using the utility function
            # load example data
            data_expanded = get_diceplot_example_data(n)
            group_colors = get_example_group_colors()
            cat_c_colors = get_example_cat_c_colors()
            # Define pathology variables and their colors
            # extract pathology variables and select proper color scale
            pathology_vars = data_expanded["PathologyVariable"].unique()
            current_cat_c_colors = {var: cat_c_colors[var] for var in pathology_vars}
    
            # Create the dice plot
            title = f"Dice Plot with {n} Pathology Variables"
            fig = dice_plot(
                data=data_expanded,
                cat_a="CellType",
                cat_b="Pathway",
                cat_c="PathologyVariable",
                group="Group", # default is set to None, it will color the boxes plain white
                switch_axis=False,
                title=title,
                cat_c_colors=current_cat_c_colors,
                group_colors=group_colors,  # Include group colors
                max_dice_sides=6  # Adjust if needed
            )
    
            # Display and save the figure
            fig.show()



Example Usage
-------------
.. note::
   Todo

Dominoplot
~~~~~~~~~~
.. note::
   Todo
Example Usage
-------------
.. note::
   Todo
