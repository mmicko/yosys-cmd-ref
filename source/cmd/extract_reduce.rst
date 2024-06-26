==========================================================
extract_reduce - converts gate chains into $reduce_* cells
==========================================================

.. raw:: latex

    \begin{comment}

.. cmd:def:: extract_reduce
    :title: converts gate chains into $reduce_* cells



    .. code:: yoscrypt

        extract_reduce [options] [selection]

    ::

        converts gate chains into $reduce_* cells

        This command finds chains of $_AND_, $_OR_, and $_XOR_ cells and replaces them
        with their corresponding $reduce_* cells. Because this command only operates on
        these cell types, it is recommended to map the design to only these cell types
        using the `abc -g` command. Note that, in some cases, it may be more effective
        to map the design to only $_AND_ cells, run extract_reduce, map the remaining
        parts of the design to AND/OR/XOR cells, and run extract_reduce a second time.


    .. code:: yoscrypt

        -allow-off-chain

    ::

            Allows matching of cells that have loads outside the chain. These cells
            will be replicated and folded into the $reduce_* cell, but the original
            cell will remain, driving its original loads.

.. raw:: latex

    \end{comment}

.. only:: latex

    ::

        
            extract_reduce [options] [selection]
        
        converts gate chains into $reduce_* cells
        
        This command finds chains of $_AND_, $_OR_, and $_XOR_ cells and replaces them
        with their corresponding $reduce_* cells. Because this command only operates on
        these cell types, it is recommended to map the design to only these cell types
        using the `abc -g` command. Note that, in some cases, it may be more effective
        to map the design to only $_AND_ cells, run extract_reduce, map the remaining
        parts of the design to AND/OR/XOR cells, and run extract_reduce a second time.
        
            -allow-off-chain
                Allows matching of cells that have loads outside the chain. These cells
                will be replicated and folded into the $reduce_* cell, but the original
                cell will remain, driving its original loads.
        
