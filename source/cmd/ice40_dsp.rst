==================================
ice40_dsp - iCE40: map multipliers
==================================

.. raw:: latex

    \begin{comment}

.. cmd:def:: ice40_dsp
    :title: iCE40: map multipliers



    .. code:: yoscrypt

        ice40_dsp [options] [selection]

    ::

        Map multipliers ($mul/SB_MAC16) and multiply-accumulate ($mul/SB_MAC16 + $add)
        cells into iCE40 DSP resources.
        Currently, only the 16x16 multiply mode is supported and not the 2 x 8x8 mode.

        Pack input registers (A, B, {C,D}; with optional hold), pipeline registers
        ({F,J,K,G}, H), output registers (O -- full 32-bits or lower 16-bits only; with
        optional hold), and post-adder into into the SB_MAC16 resource.

        Multiply-accumulate operations using the post-adder with feedback on the {C,D}
        input will be folded into the DSP. In this scenario only, resetting the
        the accumulator to an arbitrary value can be inferred to use the {C,D} input.

.. raw:: latex

    \end{comment}

.. only:: latex

    ::

        
            ice40_dsp [options] [selection]
        
        Map multipliers ($mul/SB_MAC16) and multiply-accumulate ($mul/SB_MAC16 + $add)
        cells into iCE40 DSP resources.
        Currently, only the 16x16 multiply mode is supported and not the 2 x 8x8 mode.
        
        Pack input registers (A, B, {C,D}; with optional hold), pipeline registers
        ({F,J,K,G}, H), output registers (O -- full 32-bits or lower 16-bits only; with
        optional hold), and post-adder into into the SB_MAC16 resource.
        
        Multiply-accumulate operations using the post-adder with feedback on the {C,D}
        input will be folded into the DSP. In this scenario only, resetting the
        the accumulator to an arbitrary value can be inferred to use the {C,D} input.
        
