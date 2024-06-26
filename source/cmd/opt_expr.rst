================================================================
opt_expr - perform const folding and simple expression rewriting
================================================================

.. raw:: latex

    \begin{comment}

.. cmd:def:: opt_expr
    :title: perform const folding and simple expression rewriting



    .. code:: yoscrypt

        opt_expr [options] [selection]

    ::

        This pass performs const folding on internal cell types with constant inputs.
        It also performs some simple expression rewriting.


    .. code:: yoscrypt

        -mux_undef

    ::

            remove 'undef' inputs from $mux, $pmux and $_MUX_ cells


    .. code:: yoscrypt

        -mux_bool

    ::

            replace $mux cells with inverters or buffers when possible


    .. code:: yoscrypt

        -undriven

    ::

            replace undriven nets with undef (x) constants


    .. code:: yoscrypt

        -noclkinv

    ::

            do not optimize clock inverters by changing FF types


    .. code:: yoscrypt

        -fine

    ::

            perform fine-grain optimizations


    .. code:: yoscrypt

        -full

    ::

            alias for -mux_undef -mux_bool -undriven -fine


    .. code:: yoscrypt

        -keepdc

    ::

            some optimizations change the behavior of the circuit with respect to
            don't-care bits. for example in 'a+0' a single x-bit in 'a' will cause
            all result bits to be set to x. this behavior changes when 'a+0' is
            replaced by 'a'. the -keepdc option disables all such optimizations.

.. raw:: latex

    \end{comment}

.. only:: latex

    ::

        
            opt_expr [options] [selection]
        
        This pass performs const folding on internal cell types with constant inputs.
        It also performs some simple expression rewriting.
        
            -mux_undef
                remove 'undef' inputs from $mux, $pmux and $_MUX_ cells
        
            -mux_bool
                replace $mux cells with inverters or buffers when possible
        
            -undriven
                replace undriven nets with undef (x) constants
        
            -noclkinv
                do not optimize clock inverters by changing FF types
        
            -fine
                perform fine-grain optimizations
        
            -full
                alias for -mux_undef -mux_bool -undriven -fine
        
            -keepdc
                some optimizations change the behavior of the circuit with respect to
                don't-care bits. for example in 'a+0' a single x-bit in 'a' will cause
                all result bits to be set to x. this behavior changes when 'a+0' is
                replaced by 'a'. the -keepdc option disables all such optimizations.
        
