DicePlot: a package for high dimensional categorical data visualization
========================================================================

.. image:: https://www.r-pkg.org/badges/version/diceplot
    :target: https://CRAN.R-project.org/package=diceplot
    :alt: CRAN Status Badge

.. image:: https://badge.fury.io/py/pydiceplot.svg
    :target: https://pypi.org/project/pydiceplot/
    :alt: PyPI Status Badge

.. important::
   **Note for R users:** We recommend using the newer `ggdiceplot <https://github.com/maflot/ggdiceplot>`_ package for R. 
   ggdiceplot is fully ggplot2-native, offering more flexibility, additional features, and better integration with the ggplot2 ecosystem. 
   DicePlot remains available for compatibility but is no longer the primary R interface. 
   See the :doc:`ggdiceplot` chapter for details.

.. note::
   This project is under active development.

.. figure:: ./Diceplot-graphical-abstract.png
   :alt: DicePlot aims to bridge the gap between high- and low-level visualizations of your data.

Displaying multidimensional categorical data often poses a challenge in life sciences to get a comprehensive overview of
the underlying data. This is not limited to but holds in particular for pathway analysis across multiple conditions. Here
we developed a visualization concept to create easy to understand and intuitive representation of such data. We provide
the implementation as python as well as R package to ensure easy access and application.


Features
~~~~~~~~~~~~~~~~~~~

- **Visualize Complex Data:** Easily create plots for datasets with multiple categorical variables.
- **DicePlot:** Create DicePlots for datasets with more than two categorical variables.
- **DominoPlot:** Visualize gene expression data for different cell types and contrasts.
- **R and python**: Implementations in both R and python to ensure easy access and application.
- **Customization:** Customize plots with titles, labels, and themes.
- **Integration with ggplot2:** Leverages the power of ``ggplot2`` for advanced plotting capabilities.
- **Interactive Plots:** Create interactive plots for easy exploration of your data using the plotly backend.



ggdiceplot (Recommended for R)
--------------------------------
The modern, ggplot2-native implementation for R users. You can find the `ggdiceplot Source Code <https://github.com/maflot/ggdiceplot>`_ on github.

.. toctree::
   ggdiceplot

DicePlot (R - Legacy)
---------------------
The original R implementation, maintained for compatibility. You can find the `R Source Code <https://github.com/maflot/DicePlot>`_ on github.

.. toctree::
   R

pyDicePlot
----------
You can find the `python Source Code <https://github.com/maflot/pyDicePlot/tree/main>`_ on github.

.. toctree::
   python



Contributing
~~~~~~~~~~~~~~~~~~~

We welcome contributions from the community! If you'd like to contribute:

1. Fork the repository on GitHub.
2. Create a new branch for your feature or bug fix.
3. Submit a pull request with a detailed description of your changes.

Contact
~~~~~~~~~~~~~~~~~~~

If you have any questions, suggestions, or issues, please open an issue on GitHub.


Citation
~~~~~~~~~~~~~~~~~~~

If you use this code or the R and Python packages for your own work, please cite DicePlot as:

**M. Flotho, P. Flotho, A. Keller, "Diceplot: A package for high dimensional categorical data visualization," arxiv, 2024.**
*doi:10.48550/arXiv.2410.23897*  
<https://doi.org/10.48550/arXiv.2410.23897>

BibTeX entry:

.. code-block:: bibtex

    @article{flotea2024,
        author = {Flotho, M. and Flotho, P. and Keller, A.},
        title = {Diceplot: A package for high dimensional categorical data visualization},
        year = {2024},
        journal = {arXiv preprint},
        doi = {https://doi.org/10.48550/arXiv.2410.23897}
    }
