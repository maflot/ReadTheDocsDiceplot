ggdiceplot: The Recommended R Implementation
===================================================

.. image:: https://www.r-pkg.org/badges/version/ggdiceplot
    :target: https://CRAN.R-project.org/package=ggdiceplot
    :alt: CRAN Status Badge

.. image:: https://cranlogs.r-pkg.org/badges/grand-total/ggdiceplot
    :target: https://CRAN.R-project.org/package=ggdiceplot
    :alt: CRAN Downloads

.. important::
   **ggdiceplot is now the preferred R package for creating dice plots and domino plots.**
   
   It offers full ggplot2 integration, more flexibility, and additional features compared to the original DicePlot package.

Overview
--------

**ggdiceplot** is a modern, ggplot2-native R package for visualizing high-dimensional categorical data. It provides a fully integrated ggplot2 workflow, making it easy to create, customize, and extend dice plots and domino plots using familiar ggplot2 syntax.

The package is available on GitHub: `https://github.com/maflot/ggdiceplot <https://github.com/maflot/ggdiceplot>`_

What's New in v1.2.0
--------------------

.. note::
   ggdiceplot **1.2.0** is the latest release. It includes important bug fixes for ggplot2 >= 4.0 compatibility.

**Bug fixes**

- **Fixed dice not rendering with ggplot2 >= 4.0** — The ``drawDetails.DiceGrob`` S3 method was not registered in the package NAMESPACE, so grid never dispatched to the custom drawing code. Neither tiles nor pips were drawn. Fixed by adding ``S3method(grid::drawDetails, DiceGrob)`` to NAMESPACE via ``@exportS3Method``.

- **Fixed invisible pips when** ``fill`` **is not mapped** — When the ``fill`` aesthetic was not mapped (default ``NA``), pip colour was also set to ``NA``, making pips invisible. Pips now default to black when ``fill`` is unmapped.

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

Install ggdiceplot from CRAN:

.. code-block:: r

   install.packages("ggdiceplot")

Or install the development version directly from GitHub using devtools or remotes:

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

Key Parameters
--------------

``geom_dice()`` accepts the following key arguments:

.. list-table::
   :header-rows: 1
   :widths: 20 15 65

   * - Parameter
     - Default
     - Description
   * - ``dots``
     - (required)
     - Aesthetic mapping — the categorical variable whose levels occupy dice pip positions (1–6).
   * - ``fill``
     - ``NA``
     - Aesthetic mapping — fill colour for pips. Defaults to black when unmapped.
   * - ``size``
     - constant
     - Aesthetic mapping — pip size. When mapped to a variable, pips scale between 25% and ``pip_scale`` of the maximum pip diameter.
   * - ``width``, ``height``
     - ``0.9``
     - Aesthetics controlling the tile dimensions (passed inside ``aes()``).
   * - ``ndots``
     - ``NULL``
     - Integer (1–6): number of pip positions shown per die. Should equal ``length(unique(data$dots_var))``.
   * - ``x_length``
     - ``NULL``
     - Number of unique x categories (used for aspect ratio and coord scaling).
   * - ``y_length``
     - ``NULL``
     - Number of unique y categories (used for aspect ratio and coord scaling).
   * - ``pip_scale``
     - ``0.75``
     - Pip diameter as a fraction (0–1) of the maximum available space inside each tile. Set to ``NULL`` to disable auto-scaling and use the raw ``size`` aesthetic (legacy behaviour).
   * - ``na.rm``
     - ``FALSE``
     - If ``TRUE``, silently remove rows with missing ``size`` or ``fill`` values.

Key Functions
-------------

- ``geom_dice()``: Main geom for creating dice plots with automatic 1:1 aspect ratio.
- ``theme_dice()``: Minimal theme optimised for dice plots.
- ``create_dice_positions()``: Generate standard dice dot position layouts (used internally for legends).
- ``make_offsets()``: Calculate pip positions for rendering.

Built-in Datasets
-----------------

ggdiceplot ships four sample datasets:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Dataset
     - Description
   * - ``sample_dice_data1``
     - 160 rows (8 taxa × 4 diseases × 5 specimens). Contains ``lfc`` and ``q`` columns that may be ``NA``.
   * - ``sample_dice_data2``
     - 160 rows; same structure as ``sample_dice_data1`` without a ``replicate`` column.
   * - ``sample_dice_miRNA``
     - ~90 rows of miRNA dysregulation data (miRNA × Compound × Organ) with a ``direction`` column (Up / Down / Unchanged).
   * - ``sample_dice_large``
     - 480 rows (60 taxa) for demonstrating high-density dice plots.

Basic Example Using geom_dice()
--------------------------------

Here's a simple example demonstrating the ggplot2-native workflow with ``geom_dice()``:

.. code-block:: r

   library(ggdiceplot)
   library(ggplot2)

   df <- data.frame(
     x    = 1:3,
     y    = 1,
     dots = c("A,B", "A,C,E", "F")
   )

   ggplot(df, aes(x, y, dots = dots)) +
     geom_dice(ndots = 6, x_length = 3, y_length = 1) +
     labs(title = "Minimal Dice Plot")

Taxonomy Example
----------------

This example uses the built-in ``sample_dice_data2`` dataset:

.. code-block:: r

   library(ggplot2)
   library(ggdiceplot)

   data("sample_dice_data2", package = "ggdiceplot")
   toy_data <- sample_dice_data2

   lo      <- floor(min(toy_data$lfc, na.rm = TRUE))
   up      <- ceiling(max(toy_data$lfc, na.rm = TRUE))
   mid     <- (lo + up) / 2
   minsize <- floor(min(-log10(toy_data$q), na.rm = TRUE))
   maxsize <- ceiling(max(-log10(toy_data$q), na.rm = TRUE))
   midsize <- ceiling(quantile(-log10(toy_data$q), 0.5, na.rm = TRUE))

   ggplot(toy_data, aes(x = specimen, y = taxon)) +
     geom_dice(
       aes(dots = disease, fill = lfc, size = -log10(q),
           width = 0.9, height = 0.9),
       na.rm       = TRUE,
       show.legend = TRUE,
       pip_scale   = 0.9,
       ndots       = length(unique(toy_data$disease)),
       x_length    = length(unique(toy_data$specimen)),
       y_length    = length(unique(toy_data$taxon))
     ) +
     scale_fill_gradient2(
       low = "#40004B", high = "#00441B", mid = "white",
       na.value = "white", limit = c(lo, up), midpoint = mid,
       name = "Log2FC"
     ) +
     scale_size_continuous(
       range  = c(2, 8),
       limits = c(minsize, maxsize),
       breaks = c(minsize, midsize, maxsize),
       labels = c(10^minsize, 10^-midsize, 10^-maxsize),
       name   = "q-value"
     )

miRNA Dysregulation Example
----------------------------

This example uses the built-in ``sample_dice_miRNA`` dataset:

.. code-block:: r

   library(ggplot2)
   library(ggdiceplot)

   data("sample_dice_miRNA", package = "ggdiceplot")
   df_dice <- sample_dice_miRNA

   direction_colors <- c(Down = "#2166ac", Unchanged = "grey80", Up = "#b2182b")

   ggplot(df_dice, aes(x = miRNA, y = Compound)) +
     geom_dice(
       aes(dots = Organ, fill = direction, width = 0.8, height = 0.8),
       show.legend = TRUE,
       pip_scale   = 1.0,
       ndots       = length(levels(df_dice$Organ)),
       x_length    = length(levels(df_dice$miRNA)),
       y_length    = length(levels(df_dice$Compound))
     ) +
     scale_fill_manual(values = direction_colors, name = "Regulation") +
     theme_dice() +
     theme(
       axis.text.x = element_text(angle = 0, hjust = 0.5, vjust = 0.5),
       axis.text.y = element_text(hjust = 1),
       panel.grid  = element_blank()
     ) +
     labs(
       title = "DicePlot: log2FC direction per miRNA, compound and organ",
       x     = "miRNA",
       y     = "Compound"
     )

Domino Plot Example (Gene Expression)
--------------------------------------

Domino plots visualise differential expression data across multiple conditions and cell types. Use ``geom_dice()`` with a ``dots`` aesthetic mapped to the contrast variable:

.. code-block:: r

   library(ggplot2)
   library(ggdiceplot)
   library(dplyr)
   library(tidyr)

   zebra.df <- read.csv("legacy examples/data/ZEBRA_sex_degs_set.csv")
   genes <- c("SPP1", "APOE", "SERPINA1", "PINK1", "ANGPT1",
              "ANGPT2", "APP", "CLU", "ABCA7")

   zebra.df <- zebra.df %>%
     filter(gene %in% genes) %>%
     filter(contrast %in% c("MS-CT", "AD-CT", "ASD-CT", "FTD-CT", "HD-CT")) %>%
     mutate(
       cell_type = factor(cell_type, levels = sort(unique(cell_type))),
       contrast  = factor(contrast,
                          levels = c("MS-CT", "AD-CT", "ASD-CT", "FTD-CT", "HD-CT")),
       gene      = factor(gene, levels = genes)
     ) %>%
     filter(PValue < 0.05) %>%
     group_by(gene, cell_type, contrast) %>%
     summarise(logFC = mean(logFC, na.rm = TRUE),
               FDR   = min(FDR,   na.rm = TRUE), .groups = "drop") %>%
     complete(gene, cell_type, contrast,
              fill = list(logFC = NA_real_, FDR = NA_real_))

   lo      <- floor(min(zebra.df$logFC, na.rm = TRUE))
   up      <- ceiling(max(zebra.df$logFC, na.rm = TRUE))
   mid     <- (lo + up) / 2
   minsize <- floor(min(-log10(zebra.df$FDR), na.rm = TRUE))
   maxsize <- ceiling(max(-log10(zebra.df$FDR), na.rm = TRUE))
   midsize <- ceiling(quantile(-log10(zebra.df$FDR), 0.5, na.rm = TRUE))

   ggplot(zebra.df, aes(x = gene, y = cell_type)) +
     geom_dice(
       aes(dots = contrast, fill = logFC, size = -log10(FDR)),
       na.rm       = TRUE,
       show.legend = TRUE,
       ndots       = 5,
       x_length    = length(genes),
       y_length    = length(unique(zebra.df$cell_type))
     ) +
     scale_fill_gradient2(
       low = "#40004B", high = "#00441B", mid = "white",
       na.value = "white", limit = c(lo, up), midpoint = mid,
       name = "Log2FC"
     ) +
     scale_size_continuous(
       limits = c(minsize, maxsize),
       breaks = c(minsize, midsize, maxsize),
       labels = c(10^minsize, 10^-midsize, 10^-maxsize),
       name   = "FDR"
     ) +
     theme_minimal() +
     theme(
       axis.text.x     = element_text(angle = 45, hjust = 1, size = 12),
       axis.text.y     = element_text(size = 12),
       legend.text     = element_text(size = 12),
       legend.title    = element_text(size = 12),
       legend.key      = element_blank(),
       legend.key.size = unit(0.8, "cm")
     ) +
     labs(x = "Gene", y = "Cell Type", title = "ZEBRA Sex DEGs Domino Plot")

Migration Guide: From DicePlot to ggdiceplot
---------------------------------------------

If you're transitioning from DicePlot to ggdiceplot, here are the key changes:

1. **Function to Geom**: Replace ``dice_plot()`` / ``domino_plot()`` functions with ``ggplot() + geom_dice()``

**Old DicePlot syntax:**

.. code-block:: r

   # DicePlot (old)
   library(diceplot)

   p <- dice_plot(
     data     = my_data,
     x        = "category1",
     y        = "category2",
     z        = "category3",
     z_colors = my_colors,
     title    = "My Plot"
   )
   print(p)

**New ggdiceplot syntax:**

.. code-block:: r

   # ggdiceplot (new)
   library(ggdiceplot)
   library(ggplot2)

   ggplot(my_data, aes(x = category1, y = category2)) +
     geom_dice(
       aes(dots = category3),
       ndots    = length(unique(my_data$category3)),
       x_length = length(unique(my_data$category1)),
       y_length = length(unique(my_data$category2))
     ) +
     scale_fill_manual(values = my_colors) +
     labs(title = "My Plot") +
     theme_dice()

2. **Parameter Mapping**

.. list-table::
   :header-rows: 1
   :widths: 40 60

   * - DicePlot Parameter
     - ggdiceplot Equivalent
   * - ``x = "var1"``
     - ``aes(x = var1)`` in ``ggplot()``
   * - ``y = "var2"``
     - ``aes(y = var2)`` in ``ggplot()``
   * - ``z = "var3"``
     - ``aes(dots = var3)`` in ``geom_dice()``
   * - ``z_colors = colors``
     - ``scale_fill_manual(values = colors)``
   * - ``title = "text"``
     - ``labs(title = "text")``
   * - ``custom_theme = theme_*()``
     - Add ``+ theme_*()`` as a layer, or use ``theme_dice()``
   * - ``min_dot_size``, ``max_dot_size``
     - ``aes(size = var)`` + ``scale_size_continuous()``
   * - n/a
     - ``pip_scale`` — controls pip diameter (0–1, default ``0.75``)

3. **Domino Plots**

**Old DicePlot syntax:**

.. code-block:: r

   # DicePlot domino_plot function
   p <- domino_plot(
     data      = de_data,
     gene_list = genes,
     var_id    = "contrast",
     x         = "gene",
     y         = "cell_type",
     log_fc    = "logFC",
     p_val     = "FDR"
   )

**New ggdiceplot syntax:**

.. code-block:: r

   # ggdiceplot with geom_dice
   ggplot(de_data, aes(x = gene, y = cell_type)) +
     geom_dice(
       aes(dots = contrast, fill = logFC, size = -log10(FDR)),
       na.rm    = TRUE,
       ndots    = length(unique(de_data$contrast)),
       x_length = length(unique(de_data$gene)),
       y_length = length(unique(de_data$cell_type))
     ) +
     scale_fill_gradient2(low = "blue", mid = "white", high = "red") +
     theme_dice()

Tips for Migration
~~~~~~~~~~~~~~~~~~~

1. **Start with the basics**: Convert simple plots first to understand the new syntax.
2. **Use the ggplot2 cheat sheet**: Familiar ggplot2 patterns all work with ggdiceplot.
3. **Leverage faceting**: Use ``facet_wrap()`` or ``facet_grid()`` instead of creating multiple separate plots.
4. **Combine with other geoms**: Add ``geom_text()``, ``geom_hline()``, etc. as needed.
5. **Save plots with ggsave()**: Use ggplot2's ``ggsave()`` function for consistent output.
6. **pip_scale migration**: If your existing ggdiceplot v1.0.0 plots used the raw ``size`` aesthetic, add ``pip_scale = NULL`` to ``geom_dice()`` to restore exact v1.0.0 pip sizes.

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
