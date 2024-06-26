==========================================================
dfflegalize - convert FFs to types supported by the target
==========================================================

.. raw:: latex

    \begin{comment}

.. cmd:def:: dfflegalize
    :title: convert FFs to types supported by the target



    .. code:: yoscrypt

        dfflegalize [options] [selection]

    ::

        Converts FFs to types supported by the target.


    .. code:: yoscrypt

        -cell <cell_type_pattern> <init_values>

    ::

            specifies a supported group of FF cells.  <cell_type_pattern>
            is a yosys internal fine cell name, where ? characters can be
            as a wildcard matching any character.  <init_values> specifies
            which initialization values these FF cells can support, and can
            be one of:

            - x (no init value supported)
            - 0
            - 1
            - r (init value has to match reset value, only for some FF types)
            - 01 (both 0 and 1 supported).


    .. code:: yoscrypt

        -mince <num>

    ::

            specifies a minimum number of FFs that should be using any given
            clock enable signal.  If a clock enable signal doesn't meet this
            threshold, it is unmapped into soft logic.


    .. code:: yoscrypt

        -minsrst <num>

    ::

            specifies a minimum number of FFs that should be using any given
            sync set/reset signal.  If a sync set/reset signal doesn't meet this
            threshold, it is unmapped into soft logic.


    ::

        The following cells are supported by this pass (ie. will be ingested,
        and can be specified as allowed targets):

        - $_DFF_[NP]_
        - $_DFFE_[NP][NP]_
        - $_DFF_[NP][NP][01]_
        - $_DFFE_[NP][NP][01][NP]_
        - $_ALDFF_[NP][NP]_
        - $_ALDFFE_[NP][NP][NP]_
        - $_DFFSR_[NP][NP][NP]_
        - $_DFFSRE_[NP][NP][NP][NP]_
        - $_SDFF_[NP][NP][01]_
        - $_SDFFE_[NP][NP][01][NP]_
        - $_SDFFCE_[NP][NP][01][NP]_
        - $_SR_[NP][NP]_
        - $_DLATCH_[NP]_
        - $_DLATCH_[NP][NP][01]_
        - $_DLATCHSR_[NP][NP][NP]_

        The following transformations are performed by this pass:

        - upconversion from a less capable cell to a more capable cell, if the less
          capable cell is not supported (eg. dff -> dffe, or adff -> dffsr)
        - unmapping FFs with clock enable (due to unsupported cell type or -mince)
        - unmapping FFs with sync reset (due to unsupported cell type or -minsrst)
        - adding inverters on the control pins (due to unsupported polarity)
        - adding inverters on the D and Q pins and inverting the init/reset values
          (due to unsupported init or reset value)
        - converting sr into adlatch (by tying D to 1 and using E as set input)
        - emulating unsupported dffsr cell by adff + adff + sr + mux
        - emulating unsupported dlatchsr cell by adlatch + adlatch + sr + mux
        - emulating adff when the (reset, init) value combination is unsupported by
          dff + adff + dlatch + mux
        - emulating adlatch when the (reset, init) value combination is unsupported by
        - dlatch + adlatch + dlatch + mux
        If the pass is unable to realize a given cell type (eg. adff when only plain dff
        is available), an error is raised.
.. raw:: latex

    \end{comment}

.. only:: latex

    ::

        
            dfflegalize [options] [selection]
        
        Converts FFs to types supported by the target.
        
            -cell <cell_type_pattern> <init_values>
                specifies a supported group of FF cells.  <cell_type_pattern>
                is a yosys internal fine cell name, where ? characters can be
                as a wildcard matching any character.  <init_values> specifies
                which initialization values these FF cells can support, and can
                be one of:
        
                - x (no init value supported)
                - 0
                - 1
                - r (init value has to match reset value, only for some FF types)
                - 01 (both 0 and 1 supported).
        
            -mince <num>
                specifies a minimum number of FFs that should be using any given
                clock enable signal.  If a clock enable signal doesn't meet this
                threshold, it is unmapped into soft logic.
        
            -minsrst <num>
                specifies a minimum number of FFs that should be using any given
                sync set/reset signal.  If a sync set/reset signal doesn't meet this
                threshold, it is unmapped into soft logic.
        
        The following cells are supported by this pass (ie. will be ingested,
        and can be specified as allowed targets):
        
        - $_DFF_[NP]_
        - $_DFFE_[NP][NP]_
        - $_DFF_[NP][NP][01]_
        - $_DFFE_[NP][NP][01][NP]_
        - $_ALDFF_[NP][NP]_
        - $_ALDFFE_[NP][NP][NP]_
        - $_DFFSR_[NP][NP][NP]_
        - $_DFFSRE_[NP][NP][NP][NP]_
        - $_SDFF_[NP][NP][01]_
        - $_SDFFE_[NP][NP][01][NP]_
        - $_SDFFCE_[NP][NP][01][NP]_
        - $_SR_[NP][NP]_
        - $_DLATCH_[NP]_
        - $_DLATCH_[NP][NP][01]_
        - $_DLATCHSR_[NP][NP][NP]_
        
        The following transformations are performed by this pass:
        
        - upconversion from a less capable cell to a more capable cell, if the less
          capable cell is not supported (eg. dff -> dffe, or adff -> dffsr)
        - unmapping FFs with clock enable (due to unsupported cell type or -mince)
        - unmapping FFs with sync reset (due to unsupported cell type or -minsrst)
        - adding inverters on the control pins (due to unsupported polarity)
        - adding inverters on the D and Q pins and inverting the init/reset values
          (due to unsupported init or reset value)
        - converting sr into adlatch (by tying D to 1 and using E as set input)
        - emulating unsupported dffsr cell by adff + adff + sr + mux
        - emulating unsupported dlatchsr cell by adlatch + adlatch + sr + mux
        - emulating adff when the (reset, init) value combination is unsupported by
          dff + adff + dlatch + mux
        - emulating adlatch when the (reset, init) value combination is unsupported by
        - dlatch + adlatch + dlatch + mux
        If the pass is unable to realize a given cell type (eg. adff when only plain dff
        is available), an error is raised.
