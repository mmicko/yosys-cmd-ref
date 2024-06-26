=============================================
xilinx_dsp - Xilinx: pack resources into DSPs
=============================================

.. raw:: latex

    \begin{comment}

.. cmd:def:: xilinx_dsp
    :title: Xilinx: pack resources into DSPs



    .. code:: yoscrypt

        xilinx_dsp [options] [selection]

    ::

        Pack input registers (A2, A1, B2, B1, C, D, AD; with optional enable/reset),
        pipeline registers (M; with optional enable/reset), output registers (P; with
        optional enable/reset), pre-adder and/or post-adder into Xilinx DSP resources.

        Multiply-accumulate operations using the post-adder with feedback on the 'C'
        input will be folded into the DSP. In this scenario only, the 'C' input can be
        used to override the current accumulation result with a new value, which will
        be added to the multiplier result to form the next accumulation result.

        Use of the dedicated 'PCOUT' -> 'PCIN' cascade path is detected for 'P' -> 'C'
        connections (optionally, where 'P' is right-shifted by 17-bits and used as an
        input to the post-adder -- a pattern common for summing partial products to
        implement wide multipliers). Limited support also exists for similar cascading
        for A and B using '[AB]COUT' -> '[AB]CIN'. Currently, cascade chains are limited
        to a maximum length of 20 cells, corresponding to the smallest Xilinx 7 Series
        device.

        This pass is a no-op if the scratchpad variable 'xilinx_dsp.multonly' is set
        to 1.


        Experimental feature: addition/subtractions less than 12 or 24 bits with the
        '(* use_dsp="simd" *)' attribute attached to the output wire or attached to
        the add/subtract operator will cause those operations to be implemented using
        the 'SIMD' feature of DSPs.

        Experimental feature: the presence of a `$ge' cell attached to the registered
        P output implementing the operation "(P >= <power-of-2>)" will be transformed
        into using the DSP48E1's pattern detector feature for overflow detection.


    .. code:: yoscrypt

        -family {xcup|xcu|xc7|xc6v|xc5v|xc4v|xc6s|xc3sda}

    ::

            select the family to target
            default: xc7

.. raw:: latex

    \end{comment}

.. only:: latex

    ::

        
            xilinx_dsp [options] [selection]
        
        Pack input registers (A2, A1, B2, B1, C, D, AD; with optional enable/reset),
        pipeline registers (M; with optional enable/reset), output registers (P; with
        optional enable/reset), pre-adder and/or post-adder into Xilinx DSP resources.
        
        Multiply-accumulate operations using the post-adder with feedback on the 'C'
        input will be folded into the DSP. In this scenario only, the 'C' input can be
        used to override the current accumulation result with a new value, which will
        be added to the multiplier result to form the next accumulation result.
        
        Use of the dedicated 'PCOUT' -> 'PCIN' cascade path is detected for 'P' -> 'C'
        connections (optionally, where 'P' is right-shifted by 17-bits and used as an
        input to the post-adder -- a pattern common for summing partial products to
        implement wide multipliers). Limited support also exists for similar cascading
        for A and B using '[AB]COUT' -> '[AB]CIN'. Currently, cascade chains are limited
        to a maximum length of 20 cells, corresponding to the smallest Xilinx 7 Series
        device.
        
        This pass is a no-op if the scratchpad variable 'xilinx_dsp.multonly' is set
        to 1.
        
        
        Experimental feature: addition/subtractions less than 12 or 24 bits with the
        '(* use_dsp="simd" *)' attribute attached to the output wire or attached to
        the add/subtract operator will cause those operations to be implemented using
        the 'SIMD' feature of DSPs.
        
        Experimental feature: the presence of a `$ge' cell attached to the registered
        P output implementing the operation "(P >= <power-of-2>)" will be transformed
        into using the DSP48E1's pattern detector feature for overflow detection.
        
            -family {xcup|xcu|xc7|xc6v|xc5v|xc4v|xc6s|xc3sda}
                select the family to target
                default: xc7
        
