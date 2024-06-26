===================================
techmap - generic technology mapper
===================================

.. raw:: latex

    \begin{comment}

.. cmd:def:: techmap
    :title: generic technology mapper



    .. code:: yoscrypt

        techmap [-map filename] [selection]

    ::

        This pass implements a very simple technology mapper that replaces cells in
        the design with implementations given in form of a Verilog or RTLIL source
        file.


    .. code:: yoscrypt

        -map filename

    ::

            the library of cell implementations to be used.
            without this parameter a builtin library is used that
            transforms the internal RTL cells to the internal gate
            library.


    .. code:: yoscrypt

        -map %<design-name>

    ::

            like -map above, but with an in-memory design instead of a file.


    .. code:: yoscrypt

        -extern

    ::

            load the cell implementations as separate modules into the design
            instead of inlining them.


    .. code:: yoscrypt

        -max_iter <number>

    ::

            only run the specified number of iterations on each module.
            default: unlimited


    .. code:: yoscrypt

        -recursive

    ::

            instead of the iterative breadth-first algorithm use a recursive
            depth-first algorithm. both methods should yield equivalent results,
            but may differ in performance.


    .. code:: yoscrypt

        -autoproc

    ::

            Automatically call "proc" on implementations that contain processes.


    .. code:: yoscrypt

        -wb

    ::

            Ignore the 'whitebox' attribute on cell implementations.


    .. code:: yoscrypt

        -assert

    ::

            this option will cause techmap to exit with an error if it can't map
            a selected cell. only cell types that end on an underscore are accepted
            as final cell types by this mode.


    .. code:: yoscrypt

        -D <define>, -I <incdir>

    ::

            this options are passed as-is to the Verilog frontend for loading the
            map file. Note that the Verilog frontend is also called with the
            '-nooverwrite' option set.


    ::

        When a module in the map file has the 'techmap_celltype' attribute set, it will
        match cells with a type that match the text value of this attribute. Otherwise
        the module name will be used to match the cell.  Multiple space-separated cell
        types can be listed, and wildcards using [] will be expanded (ie.
        "$_DFF_[PN]_" is the same as "$_DFF_P_ $_DFF_N_").

        When a module in the map file has the 'techmap_simplemap' attribute set, techmap
        will use 'simplemap' (see 'help simplemap') to map cells matching the module.

        When a module in the map file has the 'techmap_maccmap' attribute set, techmap
        will use 'maccmap' (see 'help maccmap') to map cells matching the module.

        When a module in the map file has the 'techmap_wrap' attribute set, techmap
        will create a wrapper for the cell and then run the command string that the
        attribute is set to on the wrapper module.

        When a port on a module in the map file has the 'techmap_autopurge' attribute
        set, and that port is not connected in the instantiation that is mapped, then
        a cell port connected only to such wires will be omitted in the mapped version
        of the circuit.

        All wires in the modules from the map file matching the pattern _TECHMAP_*
        or *._TECHMAP_* are special wires that are used to pass instructions from
        the mapping module to the techmap command. At the moment the following special
        wires are supported:

            _TECHMAP_FAIL_
                When this wire is set to a non-zero constant value, techmap will not
                use this module and instead try the next module with a matching
                'techmap_celltype' attribute.

                When such a wire exists but does not have a constant value after all
                _TECHMAP_DO_* commands have been executed, an error is generated.

            _TECHMAP_DO_*
                This wires are evaluated in alphabetical order. The constant text value
                of this wire is a yosys command (or sequence of commands) that is run
                by techmap on the module. A common use case is to run 'proc' on modules
                that are written using always-statements.

                When such a wire has a non-constant value at the time it is to be
                evaluated, an error is produced. That means it is possible for such a
                wire to start out as non-constant and evaluate to a constant value
                during processing of other _TECHMAP_DO_* commands.

                A _TECHMAP_DO_* command may start with the special token 'CONSTMAP; '.
                in this case techmap will create a copy for each distinct configuration
                of constant inputs and shorted inputs at this point and import the
                constant and connected bits into the map module. All further commands
                are executed in this copy. This is a very convenient way of creating
                optimized specializations of techmap modules without using the special
                parameters described below.

                A _TECHMAP_DO_* command may start with the special token 'RECURSION; '.
                then techmap will recursively replace the cells in the module with their
                implementation. This is not affected by the -max_iter option.

                It is possible to combine both prefixes to 'RECURSION; CONSTMAP; '.

            _TECHMAP_REMOVEINIT_<port-name>_
                When this wire is set to a constant value, the init attribute of the
                wire(s) connected to this port will be consumed.  This wire must have
                the same width as the given port, and for every bit that is set to 1 in
                the value, the corresponding init attribute bit will be changed to 1'bx.
                If all bits of an init attribute are left as x, it will be removed.

        In addition to this special wires, techmap also supports special parameters in
        modules in the map file:

            _TECHMAP_CELLTYPE_
                When a parameter with this name exists, it will be set to the type name
                of the cell that matches the module.

            _TECHMAP_CELLNAME_
                When a parameter with this name exists, it will be set to the name
                of the cell that matches the module.

            _TECHMAP_CONSTMSK_<port-name>_
            _TECHMAP_CONSTVAL_<port-name>_
                When this pair of parameters is available in a module for a port, then
                former has a 1-bit for each constant input bit and the latter has the
                value for this bit. The unused bits of the latter are set to undef (x).

            _TECHMAP_WIREINIT_<port-name>_
                When a parameter with this name exists, it will be set to the initial
                value of the wire(s) connected to the given port, as specified by the
                init attribute. If the attribute doesn't exist, x will be filled for the
                missing bits.  To remove the init attribute bits used, use the
                _TECHMAP_REMOVEINIT_*_ wires.

            _TECHMAP_BITS_CONNMAP_
            _TECHMAP_CONNMAP_<port-name>_
                For an N-bit port, the _TECHMAP_CONNMAP_<port-name>_ parameter, if it
                exists, will be set to an N*_TECHMAP_BITS_CONNMAP_ bit vector containing
                N words (of _TECHMAP_BITS_CONNMAP_ bits each) that assign each single
                bit driver a unique id. The values 0-3 are reserved for 0, 1, x, and z.
                This can be used to detect shorted inputs.

        When a module in the map file has a parameter where the according cell in the
        design has a port, the module from the map file is only used if the port in
        the design is connected to a constant value. The parameter is then set to the
        constant value.

        A cell with the name _TECHMAP_REPLACE_ in the map file will inherit the name
        and attributes of the cell that is being replaced.
        A cell with a name of the form `_TECHMAP_REPLACE_.<suffix>` in the map file will
        be named thus but with the `_TECHMAP_REPLACE_' prefix substituted with the name
        of the cell being replaced.
        Similarly, a wire named in the form `_TECHMAP_REPLACE_.<suffix>` will cause a
        new wire alias to be created and named as above but with the `_TECHMAP_REPLACE_'
        prefix also substituted.

        See 'help extract' for a pass that does the opposite thing.

        See 'help flatten' for a pass that does flatten the design (which is
        essentially techmap but using the design itself as map library).

.. raw:: latex

    \end{comment}

.. only:: latex

    ::

        
            techmap [-map filename] [selection]
        
        This pass implements a very simple technology mapper that replaces cells in
        the design with implementations given in form of a Verilog or RTLIL source
        file.
        
            -map filename
                the library of cell implementations to be used.
                without this parameter a builtin library is used that
                transforms the internal RTL cells to the internal gate
                library.
        
            -map %<design-name>
                like -map above, but with an in-memory design instead of a file.
        
            -extern
                load the cell implementations as separate modules into the design
                instead of inlining them.
        
            -max_iter <number>
                only run the specified number of iterations on each module.
                default: unlimited
        
            -recursive
                instead of the iterative breadth-first algorithm use a recursive
                depth-first algorithm. both methods should yield equivalent results,
                but may differ in performance.
        
            -autoproc
                Automatically call "proc" on implementations that contain processes.
        
            -wb
                Ignore the 'whitebox' attribute on cell implementations.
        
            -assert
                this option will cause techmap to exit with an error if it can't map
                a selected cell. only cell types that end on an underscore are accepted
                as final cell types by this mode.
        
            -D <define>, -I <incdir>
                this options are passed as-is to the Verilog frontend for loading the
                map file. Note that the Verilog frontend is also called with the
                '-nooverwrite' option set.
        
        When a module in the map file has the 'techmap_celltype' attribute set, it will
        match cells with a type that match the text value of this attribute. Otherwise
        the module name will be used to match the cell.  Multiple space-separated cell
        types can be listed, and wildcards using [] will be expanded (ie.
        "$_DFF_[PN]_" is the same as "$_DFF_P_ $_DFF_N_").
        
        When a module in the map file has the 'techmap_simplemap' attribute set, techmap
        will use 'simplemap' (see 'help simplemap') to map cells matching the module.
        
        When a module in the map file has the 'techmap_maccmap' attribute set, techmap
        will use 'maccmap' (see 'help maccmap') to map cells matching the module.
        
        When a module in the map file has the 'techmap_wrap' attribute set, techmap
        will create a wrapper for the cell and then run the command string that the
        attribute is set to on the wrapper module.
        
        When a port on a module in the map file has the 'techmap_autopurge' attribute
        set, and that port is not connected in the instantiation that is mapped, then
        a cell port connected only to such wires will be omitted in the mapped version
        of the circuit.
        
        All wires in the modules from the map file matching the pattern _TECHMAP_*
        or *._TECHMAP_* are special wires that are used to pass instructions from
        the mapping module to the techmap command. At the moment the following special
        wires are supported:
        
            _TECHMAP_FAIL_
                When this wire is set to a non-zero constant value, techmap will not
                use this module and instead try the next module with a matching
                'techmap_celltype' attribute.
        
                When such a wire exists but does not have a constant value after all
                _TECHMAP_DO_* commands have been executed, an error is generated.
        
            _TECHMAP_DO_*
                This wires are evaluated in alphabetical order. The constant text value
                of this wire is a yosys command (or sequence of commands) that is run
                by techmap on the module. A common use case is to run 'proc' on modules
                that are written using always-statements.
        
                When such a wire has a non-constant value at the time it is to be
                evaluated, an error is produced. That means it is possible for such a
                wire to start out as non-constant and evaluate to a constant value
                during processing of other _TECHMAP_DO_* commands.
        
                A _TECHMAP_DO_* command may start with the special token 'CONSTMAP; '.
                in this case techmap will create a copy for each distinct configuration
                of constant inputs and shorted inputs at this point and import the
                constant and connected bits into the map module. All further commands
                are executed in this copy. This is a very convenient way of creating
                optimized specializations of techmap modules without using the special
                parameters described below.
        
                A _TECHMAP_DO_* command may start with the special token 'RECURSION; '.
                then techmap will recursively replace the cells in the module with their
                implementation. This is not affected by the -max_iter option.
        
                It is possible to combine both prefixes to 'RECURSION; CONSTMAP; '.
        
            _TECHMAP_REMOVEINIT_<port-name>_
                When this wire is set to a constant value, the init attribute of the
                wire(s) connected to this port will be consumed.  This wire must have
                the same width as the given port, and for every bit that is set to 1 in
                the value, the corresponding init attribute bit will be changed to 1'bx.
                If all bits of an init attribute are left as x, it will be removed.
        
        In addition to this special wires, techmap also supports special parameters in
        modules in the map file:
        
            _TECHMAP_CELLTYPE_
                When a parameter with this name exists, it will be set to the type name
                of the cell that matches the module.
        
            _TECHMAP_CELLNAME_
                When a parameter with this name exists, it will be set to the name
                of the cell that matches the module.
        
            _TECHMAP_CONSTMSK_<port-name>_
            _TECHMAP_CONSTVAL_<port-name>_
                When this pair of parameters is available in a module for a port, then
                former has a 1-bit for each constant input bit and the latter has the
                value for this bit. The unused bits of the latter are set to undef (x).
        
            _TECHMAP_WIREINIT_<port-name>_
                When a parameter with this name exists, it will be set to the initial
                value of the wire(s) connected to the given port, as specified by the
                init attribute. If the attribute doesn't exist, x will be filled for the
                missing bits.  To remove the init attribute bits used, use the
                _TECHMAP_REMOVEINIT_*_ wires.
        
            _TECHMAP_BITS_CONNMAP_
            _TECHMAP_CONNMAP_<port-name>_
                For an N-bit port, the _TECHMAP_CONNMAP_<port-name>_ parameter, if it
                exists, will be set to an N*_TECHMAP_BITS_CONNMAP_ bit vector containing
                N words (of _TECHMAP_BITS_CONNMAP_ bits each) that assign each single
                bit driver a unique id. The values 0-3 are reserved for 0, 1, x, and z.
                This can be used to detect shorted inputs.
        
        When a module in the map file has a parameter where the according cell in the
        design has a port, the module from the map file is only used if the port in
        the design is connected to a constant value. The parameter is then set to the
        constant value.
        
        A cell with the name _TECHMAP_REPLACE_ in the map file will inherit the name
        and attributes of the cell that is being replaced.
        A cell with a name of the form `_TECHMAP_REPLACE_.<suffix>` in the map file will
        be named thus but with the `_TECHMAP_REPLACE_' prefix substituted with the name
        of the cell being replaced.
        Similarly, a wire named in the form `_TECHMAP_REPLACE_.<suffix>` will cause a
        new wire alias to be created and named as above but with the `_TECHMAP_REPLACE_'
        prefix also substituted.
        
        See 'help extract' for a pass that does the opposite thing.
        
        See 'help flatten' for a pass that does flatten the design (which is
        essentially techmap but using the design itself as map library).
        
