===============================================
ice40_opt - iCE40: perform simple optimizations
===============================================

.. raw:: latex

    \begin{comment}

.. cmd:def:: ice40_opt
    :title: iCE40: perform simple optimizations



    .. code:: yoscrypt

        ice40_opt [options] [selection]

    ::

        This command executes the following script:

            do
                <ice40 specific optimizations>
                opt_expr -mux_undef -undriven [-full]
                opt_merge
                opt_dff
                opt_clean
            while <changed design>

.. raw:: latex

    \end{comment}

.. only:: latex

    ::

        
            ice40_opt [options] [selection]
        
        This command executes the following script:
        
            do
                <ice40 specific optimizations>
                opt_expr -mux_undef -undriven [-full]
                opt_merge
                opt_dff
                opt_clean
            while <changed design>
        
