===============================================
synth_greenpak4 - synthesis for GreenPAK4 FPGAs
===============================================

.. raw:: latex

    \begin{comment}

.. cmd:def:: synth_greenpak4
    :title: synthesis for GreenPAK4 FPGAs



    .. code:: yoscrypt

        synth_greenpak4 [options]

    ::

        This command runs synthesis for GreenPAK4 FPGAs. This work is experimental.
        It is intended to be used with https://github.com/azonenberg/openfpga as the
        place-and-route.


    .. code:: yoscrypt

        -top <module>

    ::

            use the specified module as top module (default='top')


    .. code:: yoscrypt

        -part <part>

    ::

            synthesize for the specified part. Valid values are SLG46140V,
            SLG46620V, and SLG46621V (default).


    .. code:: yoscrypt

        -json <file>

    ::

            write the design to the specified JSON file. writing of an output file
            is omitted if this parameter is not specified.


    .. code:: yoscrypt

        -run <from_label>:<to_label>

    ::

            only run the commands between the labels (see below). an empty
            from label is synonymous to 'begin', and empty to label is
            synonymous to the end of the command list.


    .. code:: yoscrypt

        -noflatten

    ::

            do not flatten design before synthesis


    .. code:: yoscrypt

        -retime

    ::

            run 'abc' with '-dff -D 1' options



    ::

        The following commands are executed by this synthesis command:

            begin:
                read_verilog -lib +/greenpak4/cells_sim.v
                hierarchy -check -top <top>

            flatten:    (unless -noflatten)
                proc
                flatten
                tribuf -logic

            coarse:
                synth -run coarse

            fine:
                extract_counter -pout GP_DCMP,GP_DAC -maxwidth 14
                clean
                opt -fast -mux_undef -undriven -fine
                memory_map
                opt -undriven -fine
                techmap -map +/techmap.v -map +/greenpak4/cells_latch.v
                dfflibmap -prepare -liberty +/greenpak4/gp_dff.lib
                opt -fast -noclkinv -noff
                abc -dff -D 1    (only if -retime)

            map_luts:
                nlutmap -assert -luts 0,6,8,2     (for -part SLG46140V)
                nlutmap -assert -luts 2,8,16,2    (for -part SLG46620V)
                nlutmap -assert -luts 2,8,16,2    (for -part SLG46621V)
                clean

            map_cells:
                shregmap -tech greenpak4
                dfflibmap -liberty +/greenpak4/gp_dff.lib
                dffinit -ff GP_DFF Q INIT
                dffinit -ff GP_DFFR Q INIT
                dffinit -ff GP_DFFS Q INIT
                dffinit -ff GP_DFFSR Q INIT
                iopadmap -bits -inpad GP_IBUF OUT:IN -outpad GP_OBUF IN:OUT -inoutpad GP_OBUF OUT:IN -toutpad GP_OBUFT OE:IN:OUT -tinoutpad GP_IOBUF OE:OUT:IN:IO
                attrmvcp -attr src -attr LOC t:GP_OBUF t:GP_OBUFT t:GP_IOBUF n:*
                attrmvcp -attr src -attr LOC -driven t:GP_IBUF n:*
                techmap -map +/greenpak4/cells_map.v
                greenpak4_dffinv
                clean

            check:
                hierarchy -check
                stat
                check -noinit
                blackbox =A:whitebox

            json:
                write_json <file-name>

.. raw:: latex

    \end{comment}

.. only:: latex

    ::

        
            synth_greenpak4 [options]
        
        This command runs synthesis for GreenPAK4 FPGAs. This work is experimental.
        It is intended to be used with https://github.com/azonenberg/openfpga as the
        place-and-route.
        
            -top <module>
                use the specified module as top module (default='top')
        
            -part <part>
                synthesize for the specified part. Valid values are SLG46140V,
                SLG46620V, and SLG46621V (default).
        
            -json <file>
                write the design to the specified JSON file. writing of an output file
                is omitted if this parameter is not specified.
        
            -run <from_label>:<to_label>
                only run the commands between the labels (see below). an empty
                from label is synonymous to 'begin', and empty to label is
                synonymous to the end of the command list.
        
            -noflatten
                do not flatten design before synthesis
        
            -retime
                run 'abc' with '-dff -D 1' options
        
        
        The following commands are executed by this synthesis command:
        
            begin:
                read_verilog -lib +/greenpak4/cells_sim.v
                hierarchy -check -top <top>
        
            flatten:    (unless -noflatten)
                proc
                flatten
                tribuf -logic
        
            coarse:
                synth -run coarse
        
            fine:
                extract_counter -pout GP_DCMP,GP_DAC -maxwidth 14
                clean
                opt -fast -mux_undef -undriven -fine
                memory_map
                opt -undriven -fine
                techmap -map +/techmap.v -map +/greenpak4/cells_latch.v
                dfflibmap -prepare -liberty +/greenpak4/gp_dff.lib
                opt -fast -noclkinv -noff
                abc -dff -D 1    (only if -retime)
        
            map_luts:
                nlutmap -assert -luts 0,6,8,2     (for -part SLG46140V)
                nlutmap -assert -luts 2,8,16,2    (for -part SLG46620V)
                nlutmap -assert -luts 2,8,16,2    (for -part SLG46621V)
                clean
        
            map_cells:
                shregmap -tech greenpak4
                dfflibmap -liberty +/greenpak4/gp_dff.lib
                dffinit -ff GP_DFF Q INIT
                dffinit -ff GP_DFFR Q INIT
                dffinit -ff GP_DFFS Q INIT
                dffinit -ff GP_DFFSR Q INIT
                iopadmap -bits -inpad GP_IBUF OUT:IN -outpad GP_OBUF IN:OUT -inoutpad GP_OBUF OUT:IN -toutpad GP_OBUFT OE:IN:OUT -tinoutpad GP_IOBUF OE:OUT:IN:IO
                attrmvcp -attr src -attr LOC t:GP_OBUF t:GP_OBUFT t:GP_IOBUF n:*
                attrmvcp -attr src -attr LOC -driven t:GP_IBUF n:*
                techmap -map +/greenpak4/cells_map.v
                greenpak4_dffinv
                clean
        
            check:
                hierarchy -check
                stat
                check -noinit
                blackbox =A:whitebox
        
            json:
                write_json <file-name>
        
