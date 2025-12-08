Using ggdiceplot: The Recommended R Implementation
===================================================

.. important::
   **ggdiceplot is now the preferred R package for creating dice plots and domino plots.**
   
   It offers full ggplot2 integration, more flexibility, and additional features compared to the original DicePlot package.

Overview
--------

**ggdiceplot** is a modern, ggplot2-native R package for visualizing high-dimensional categorical data. It provides a fully integrated ggplot2 workflow, making it easy to create, customize, and extend dice plots and domino plots using familiar ggplot2 syntax.

The package is available on GitHub: `https://github.com/maflot/ggdiceplot <https://github.com/maflot/ggdiceplot>`_

Why ggdiceplot Should Be Preferred Over DicePlot
-------------------------------------------------

ggdiceplot offers several advantages over the original DicePlot package:

1. **Full ggplot2 Integration**: Built as a native ggplot2 extension using the ``geom_dice()`` layer, allowing seamless integration with the ggplot2 ecosystem.

2. **More Flexible Aesthetics**: Supports advanced ggplot2 customization options, themes, and faceting.

3. **Color Overloading**: Enhanced support for mapping multiple variables to color aesthetics within dice faces.

4. **Better Aspect Ratio Control**: Improved handling of plot dimensions and aspect ratios for publication-quality figures.

5. **Active Development**: Actively maintained with modern R best practices and continuous improvements.

6. **Consistent API**: Uses familiar ggplot2 syntax (``+`` operator for layers) instead of custom function arguments.

7. **Extensibility**: Easy to combine with other ggplot2 geoms and extensions.

Key Differences Between DicePlot and ggdiceplot
------------------------------------------------

.. list-table::
   :header-rows: 1
   :widths: 30 35 35

   * - Feature
     - DicePlot (Legacy)
     - ggdiceplot (Recommended)
   * - Integration
     - Custom plotting function
     - Native ggplot2 geom
   * - Syntax
     - Function-based
     - Layer-based (ggplot2 style)
   * - Customization
     - Limited to function parameters
     - Full ggplot2 customization
   * - Extensibility
     - Standalone plots
     - Easily combined with other geoms
   * - Maintenance
     - Legacy support
     - Active development

Installation
------------

Install ggdiceplot directly from GitHub using devtools or remotes:

.. code-block:: r

   # Install devtools if you haven't already
   install.packages("devtools")
   
   # Install ggdiceplot from GitHub
   devtools::install_github("maflot/ggdiceplot")

Or using remotes:

.. code-block:: r

   # Install remotes if you haven't already
   install.packages("remotes")
   
   # Install ggdiceplot from GitHub
   remotes::install_github("maflot/ggdiceplot")

Load the Package
~~~~~~~~~~~~~~~~

After installation, load ggdiceplot into your R session:

.. code-block:: r

   library(ggdiceplot)
   library(ggplot2)  # ggdiceplot extends ggplot2
   library(dplyr)    # useful for data manipulation

Basic Example Using geom_dice()
--------------------------------

Here's a simple example demonstrating the ggplot2-native workflow with ``geom_dice()``:

.. code-block:: r

   library(ggdiceplot)
   library(ggplot2)
   library(dplyr)

   # Create sample data
   data <- expand.grid(
     x = LETTERS[1:5],
     y = 1:5
   ) %>%
     mutate(
       category1 = sample(c("Type1", "Type2", "Type3"), n(), replace = TRUE),
       category2 = sample(c("A", "B", "C"), n(), replace = TRUE),
       value = runif(n())
     )

   # Create a dice plot using ggplot2 syntax
   ggplot(data, aes(x = x, y = y)) +
     geom_dice(aes(fill = category1, color = category2), 
               dice_size = 0.8) +
     scale_fill_viridis_d() +
     theme_minimal() +
     labs(title = "Simple Dice Plot with ggdiceplot",
          x = "X Category",
          y = "Y Category")

Advanced Example: Gene Expression Patterns
-------------------------------------------

This example demonstrates how to visualize complex gene expression data across multiple cell types and conditions:

.. code-block:: r

   library(ggdiceplot)
   library(ggplot2)
   library(dplyr)
   library(tidyr)

   # Simulate gene expression data
   set.seed(123)
   gene_data <- expand.grid(
     gene = paste0("Gene", 1:10),
     cell_type = c("Neuron", "Astrocyte", "Microglia", "Oligodendrocyte"),
     condition = c("Control", "Treatment")
   ) %>%
     mutate(
       expression = rnorm(n(), mean = 5, sd = 2),
       significance = sample(c("NS", "p<0.05", "p<0.01"), n(), replace = TRUE,
                            prob = c(0.6, 0.3, 0.1))
     )

   # Create dice plot with faceting
   ggplot(gene_data, aes(x = gene, y = cell_type)) +
     geom_dice(aes(fill = expression, size = significance),
               dice_size = 0.9) +
     facet_wrap(~condition) +
     scale_fill_gradient2(low = "blue", mid = "white", high = "red",
                         midpoint = 5, name = "Expression") +
     scale_size_manual(values = c("NS" = 1, "p<0.05" = 2, "p<0.01" = 3),
                      name = "Significance") +
     theme_bw() +
     theme(axis.text.x = element_text(angle = 45, hjust = 1)) +
     labs(title = "Gene Expression Across Cell Types",
          subtitle = "Comparison of Control vs Treatment conditions",
          x = "Gene", y = "Cell Type")

Domino Plot Example Using geom_dice()
--------------------------------------

Domino plots are useful for visualizing differential expression data. Here's how to create them with ggdiceplot:

.. code-block:: r

   library(ggdiceplot)
   library(ggplot2)
   library(dplyr)

   # Simulate differential expression data
   set.seed(456)
   de_data <- expand.grid(
     gene = paste0("Gene", 1:15),
     cell_type = c("T cell", "B cell", "NK cell", "Monocyte")
   ) %>%
     mutate(
       log_fc = rnorm(n(), mean = 0, sd = 2),
       pvalue = runif(n()),
       significant = pvalue < 0.05,
       direction = ifelse(log_fc > 0, "Up", "Down")
     ) %>%
     filter(significant)  # Only show significant results

   # Create domino plot
   ggplot(de_data, aes(x = gene, y = cell_type)) +
     geom_dice(aes(fill = log_fc, size = -log10(pvalue)),
               dice_shape = "domino") +
     scale_fill_gradient2(low = "blue", mid = "white", high = "red",
                         midpoint = 0, 
                         name = "Log2 Fold Change") +
     scale_size_continuous(range = c(2, 6), name = "-log10(p-value)") +
     theme_classic() +
     theme(axis.text.x = element_text(angle = 45, hjust = 1),
           panel.grid.major = element_line(color = "grey90")) +
     labs(title = "Differential Expression Domino Plot",
          subtitle = "Showing only significant genes (p < 0.05)",
          x = "Gene", y = "Cell Type")

Working with Real Data: miRNA Example
--------------------------------------

This example shows how to visualize microRNA expression patterns:

.. code-block:: r

   library(ggdiceplot)
   library(ggplot2)
   library(dplyr)
   library(RColorBrewer)

   # Load your miRNA data (example structure)
   # mirna_data <- read.csv("your_mirna_data.csv")
   
   # For demonstration, create sample data
   set.seed(789)
   mirna_data <- expand.grid(
     mirna = paste0("miR-", sample(100:999, 12)),
     tissue = c("Brain", "Liver", "Heart", "Kidney"),
     disease = c("Healthy", "Disease")
   ) %>%
     mutate(
       expression_level = sample(c("Low", "Medium", "High"), n(), replace = TRUE),
       fold_change = rnorm(n(), mean = 0, sd = 1.5)
     )

   # Create the plot
   ggplot(mirna_data, aes(x = mirna, y = tissue)) +
     geom_dice(aes(fill = fold_change, shape = expression_level),
               dice_size = 0.85) +
     facet_grid(disease ~ .) +
     scale_fill_distiller(palette = "RdYlBu", direction = -1,
                         name = "Fold Change") +
     theme_minimal() +
     theme(
       axis.text.x = element_text(angle = 45, hjust = 1, size = 9),
       strip.background = element_rect(fill = "grey90"),
       strip.text = element_text(face = "bold")
     ) +
     labs(title = "miRNA Expression Patterns",
          x = "miRNA", y = "Tissue Type")

Taxonomy Example: Visualizing Hierarchical Data
------------------------------------------------

ggdiceplot can be used to visualize taxonomic or hierarchical categorical data:

.. code-block:: r

   library(ggdiceplot)
   library(ggplot2)
   library(dplyr)

   # Create taxonomic abundance data
   set.seed(101)
   taxa_data <- expand.grid(
     sample = paste("Sample", 1:8),
     phylum = c("Proteobacteria", "Firmicutes", "Bacteroidetes", "Actinobacteria")
   ) %>%
     mutate(
       class = paste0("Class_", sample(LETTERS[1:3], n(), replace = TRUE)),
       abundance = runif(n(), 0, 100),
       prevalence = sample(c("Rare", "Common", "Abundant"), n(), replace = TRUE)
     )

   # Create dice plot for taxonomy
   ggplot(taxa_data, aes(x = sample, y = phylum)) +
     geom_dice(aes(fill = abundance, alpha = prevalence),
               dice_size = 0.9) +
     scale_fill_viridis_c(option = "plasma", name = "Abundance (%)") +
     scale_alpha_manual(values = c("Rare" = 0.3, "Common" = 0.6, "Abundant" = 1.0),
                       name = "Prevalence") +
     theme_light() +
     theme(axis.text.x = element_text(angle = 45, hjust = 1)) +
     labs(title = "Microbial Community Structure",
          subtitle = "Taxonomic abundance across samples",
          x = "Sample ID", y = "Phylum")

Migration Guide: From DicePlot to ggdiceplot
---------------------------------------------

If you're transitioning from DicePlot to ggdiceplot, here are the key changes:

1. **Function to Geom**: Replace ``dice_plot()`` function with ``ggplot() + geom_dice()``

**Old DicePlot syntax:**

.. code-block:: r

   # DicePlot (old)
   library(diceplot)
   
   p <- dice_plot(
     data = my_data,
     x = "category1",
     y = "category2",
     z = "category3",
     z_colors = my_colors,
     title = "My Plot"
   )
   print(p)

**New ggdiceplot syntax:**

.. code-block:: r

   # ggdiceplot (new)
   library(ggdiceplot)
   library(ggplot2)
   
   ggplot(my_data, aes(x = category1, y = category2)) +
     geom_dice(aes(fill = category3)) +
     scale_fill_manual(values = my_colors) +
     labs(title = "My Plot") +
     theme_minimal()

2. **Parameter Mapping**

.. list-table::
   :header-rows: 1
   :widths: 40 60

   * - DicePlot Parameter
     - ggdiceplot Equivalent
   * - ``x = "var1"``
     - ``aes(x = var1)`` in ggplot()
   * - ``y = "var2"``
     - ``aes(y = var2)`` in ggplot()
   * - ``z = "var3"``
     - ``aes(fill = var3)`` in geom_dice()
   * - ``z_colors = colors``
     - ``scale_fill_manual(values = colors)``
   * - ``title = "text"``
     - ``labs(title = "text")``
   * - ``custom_theme = theme_*()``
     - Add ``+ theme_*()`` as layer
   * - ``min_dot_size``, ``max_dot_size``
     - ``aes(size = var)`` + ``scale_size_continuous()``

3. **Combining Multiple Customizations**

With ggdiceplot, you can easily stack multiple customizations:

.. code-block:: r

   ggplot(data, aes(x = x, y = y)) +
     geom_dice(aes(fill = var1, color = var2, size = var3)) +
     scale_fill_brewer(palette = "Set1") +
     scale_color_manual(values = c("red", "blue")) +
     scale_size_continuous(range = c(2, 6)) +
     facet_wrap(~group) +
     theme_minimal() +
     theme(axis.text.x = element_text(angle = 45, hjust = 1)) +
     labs(title = "Complex Dice Plot", 
          x = "X Label", y = "Y Label")

4. **Domino Plots**

**Old DicePlot syntax:**

.. code-block:: r

   # DicePlot domino_plot function
   p <- domino_plot(
     data = de_data,
     gene_list = genes,
     var_id = "contrast",
     x = "gene",
     y = "cell_type",
     log_fc = "logFC",
     p_val = "FDR"
   )

**New ggdiceplot syntax:**

.. code-block:: r

   # ggdiceplot with geom_dice
   ggplot(de_data, aes(x = gene, y = cell_type)) +
     geom_dice(aes(fill = logFC, size = -log10(FDR)),
               dice_shape = "domino") +
     scale_fill_gradient2(low = "blue", mid = "white", high = "red") +
     facet_wrap(~contrast) +
     theme_bw()

Tips for Migration
~~~~~~~~~~~~~~~~~~~

1. **Start with the basics**: Convert simple plots first to understand the new syntax
2. **Use the ggplot2 cheat sheet**: Familiar ggplot2 patterns all work with ggdiceplot
3. **Leverage faceting**: Use ``facet_wrap()`` or ``facet_grid()`` instead of creating multiple separate plots
4. **Combine with other geoms**: Add ``geom_text()``, ``geom_hline()``, etc. as needed
5. **Save plots with ggsave()**: Use ggplot2's ``ggsave()`` function for consistent output

Additional Resources
--------------------

- **GitHub Repository**: `https://github.com/maflot/ggdiceplot <https://github.com/maflot/ggdiceplot>`_
- **Issue Tracker**: Report bugs or request features on the `GitHub Issues page <https://github.com/maflot/ggdiceplot/issues>`_
- **Examples and Demos**: Check the ``demo_output/`` folder in the repository for more examples
- **ggplot2 Documentation**: `https://ggplot2.tidyverse.org/ <https://ggplot2.tidyverse.org/>`_

Comparison with Legacy DicePlot
--------------------------------

While DicePlot remains available on CRAN for backwards compatibility, we strongly recommend new projects use ggdiceplot for the following reasons:

**Advantages of ggdiceplot:**

- Native ggplot2 integration allows using the full power of the ggplot2 ecosystem
- More intuitive syntax for users already familiar with ggplot2
- Better support for complex multi-panel figures through faceting
- Easier to combine with other visualization layers
- Active development and regular updates
- Better documentation and examples

**When to still use DicePlot:**

- Maintaining existing codebases that use DicePlot
- Quick prototyping with the original function-based interface
- Projects that cannot be updated to ggdiceplot

Getting Help
------------

If you encounter issues or have questions about ggdiceplot:

1. Check the `GitHub repository <https://github.com/maflot/ggdiceplot>`_ for examples and documentation
2. Search existing `GitHub Issues <https://github.com/maflot/ggdiceplot/issues>`_ for similar problems
3. Open a new issue on GitHub with a reproducible example
4. For general ggplot2 questions, consult the `ggplot2 documentation <https://ggplot2.tidyverse.org/>`_

Contributing
------------

We welcome contributions to ggdiceplot! If you'd like to contribute:

1. Fork the repository on GitHub
2. Create a new branch for your feature or bug fix
3. Write tests for your changes
4. Submit a pull request with a clear description

Citation
--------

If you use ggdiceplot in your research, please cite:

**M. Flotho, P. Flotho, A. Keller, "Diceplot: A package for high dimensional categorical data visualization," arXiv, 2024.**
*doi:10.48550/arXiv.2410.23897*  
`https://doi.org/10.48550/arXiv.2410.23897 <https://doi.org/10.48550/arXiv.2410.23897>`_

.. code-block:: bibtex

    @article{flotho2024,
        author = {Flotho, M. and Flotho, P. and Keller, A.},
        title = {Diceplot: A package for high dimensional categorical data visualization},
        year = {2024},
        journal = {arXiv preprint},
        doi = {https://doi.org/10.48550/arXiv.2410.23897}
    }
