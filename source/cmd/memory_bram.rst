========================================
memory_bram - map memories to block rams
========================================

.. raw:: latex

    \begin{comment}

.. cmd:def:: memory_bram
    :title: map memories to block rams



    .. code:: yoscrypt

        memory_bram -rules <rule_file> [selection]

    ::

        This pass converts the multi-port $mem memory cells into block ram instances.
        The given rules file describes the available resources and how they should be
        used.

        The rules file contains configuration options, a set of block ram description
        and a sequence of match rules.

        The option 'attr_icase' configures how attribute values are matched. The value 0
        means case-sensitive, 1 means case-insensitive.

        A block ram description looks like this:

            bram RAMB1024X32     # name of BRAM cell
              init 1             # set to '1' if BRAM can be initialized
              abits 10           # number of address bits
              dbits 32           # number of data bits
              groups 2           # number of port groups
              ports  1 1         # number of ports in each group
              wrmode 1 0         # set to '1' if this groups is write ports
              enable 4 1         # number of enable bits
              transp 0 2         # transparent (for read ports)
              clocks 1 2         # clock configuration
              clkpol 2 2         # clock polarity configuration
            endbram

        For the option 'transp' the value 0 means non-transparent, 1 means transparent
        and a value greater than 1 means configurable. All groups with the same
        value greater than 1 share the same configuration bit.

        For the option 'clocks' the value 0 means non-clocked, and a value greater
        than 0 means clocked. All groups with the same value share the same clock
        signal.

        For the option 'clkpol' the value 0 means negative edge, 1 means positive edge
        and a value greater than 1 means configurable. All groups with the same value
        greater than 1 share the same configuration bit.

        Using the same bram name in different bram blocks will create different variants
        of the bram. Verilog configuration parameters for the bram are created as
        needed.

        It is also possible to create variants by repeating statements in the bram block
        and appending '@<label>' to the individual statements.

        A match rule looks like this:

            match RAMB1024X32
              max waste 16384    # only use this bram if <= 16k ram bits are unused
              min efficiency 80  # only use this bram if efficiency is at least 80%
            endmatch

        It is possible to match against the following values with min/max rules:

            words  ........  number of words in memory in design
            abits  ........  number of address bits on memory in design
            dbits  ........  number of data bits on memory in design
            wports  .......  number of write ports on memory in design
            rports  .......  number of read ports on memory in design
            ports  ........  number of ports on memory in design
            bits  .........  number of bits in memory in design
            dups ..........  number of duplications for more read ports

            awaste  .......  number of unused address slots for this match
            dwaste  .......  number of unused data bits for this match
            bwaste  .......  number of unused bram bits for this match
            waste  ........  total number of unused bram bits (bwaste*dups)
            efficiency  ...  total percentage of used and non-duplicated bits

            acells  .......  number of cells in 'address-direction'
            dcells  .......  number of cells in 'data-direction'
            cells  ........  total number of cells (acells*dcells*dups)

        A match containing the command 'attribute' followed by a list of space
        separated 'name[=string_value]' values requires that the memory contains any
        one of the given attribute name and string values (where specified), or name
        and integer 1 value (if no string_value given, since Verilog will interpret
        '(* attr *)' as '(* attr=1 *)').
        A name prefixed with '!' indicates that the attribute must not exist.

        The interface for the created bram instances is derived from the bram
        description. Use 'techmap' to convert the created bram instances into
        instances of the actual bram cells of your target architecture.

        A match containing the command 'or_next_if_better' is only used if it
        has a higher efficiency than the next match (and the one after that if
        the next also has 'or_next_if_better' set, and so forth).

        A match containing the command 'make_transp' will add external circuitry
        to simulate 'transparent read', if necessary.

        A match containing the command 'make_outreg' will add external flip-flops
        to implement synchronous read ports, if necessary.

        A match containing the command 'shuffle_enable A' will re-organize
        the data bits to accommodate the enable pattern of port A.

.. raw:: latex

    \end{comment}

.. only:: latex

    ::

        
            memory_bram -rules <rule_file> [selection]
        
        This pass converts the multi-port $mem memory cells into block ram instances.
        The given rules file describes the available resources and how they should be
        used.
        
        The rules file contains configuration options, a set of block ram description
        and a sequence of match rules.
        
        The option 'attr_icase' configures how attribute values are matched. The value 0
        means case-sensitive, 1 means case-insensitive.
        
        A block ram description looks like this:
        
            bram RAMB1024X32     # name of BRAM cell
              init 1             # set to '1' if BRAM can be initialized
              abits 10           # number of address bits
              dbits 32           # number of data bits
              groups 2           # number of port groups
              ports  1 1         # number of ports in each group
              wrmode 1 0         # set to '1' if this groups is write ports
              enable 4 1         # number of enable bits
              transp 0 2         # transparent (for read ports)
              clocks 1 2         # clock configuration
              clkpol 2 2         # clock polarity configuration
            endbram
        
        For the option 'transp' the value 0 means non-transparent, 1 means transparent
        and a value greater than 1 means configurable. All groups with the same
        value greater than 1 share the same configuration bit.
        
        For the option 'clocks' the value 0 means non-clocked, and a value greater
        than 0 means clocked. All groups with the same value share the same clock
        signal.
        
        For the option 'clkpol' the value 0 means negative edge, 1 means positive edge
        and a value greater than 1 means configurable. All groups with the same value
        greater than 1 share the same configuration bit.
        
        Using the same bram name in different bram blocks will create different variants
        of the bram. Verilog configuration parameters for the bram are created as
        needed.
        
        It is also possible to create variants by repeating statements in the bram block
        and appending '@<label>' to the individual statements.
        
        A match rule looks like this:
        
            match RAMB1024X32
              max waste 16384    # only use this bram if <= 16k ram bits are unused
              min efficiency 80  # only use this bram if efficiency is at least 80%
            endmatch
        
        It is possible to match against the following values with min/max rules:
        
            words  ........  number of words in memory in design
            abits  ........  number of address bits on memory in design
            dbits  ........  number of data bits on memory in design
            wports  .......  number of write ports on memory in design
            rports  .......  number of read ports on memory in design
            ports  ........  number of ports on memory in design
            bits  .........  number of bits in memory in design
            dups ..........  number of duplications for more read ports
        
            awaste  .......  number of unused address slots for this match
            dwaste  .......  number of unused data bits for this match
            bwaste  .......  number of unused bram bits for this match
            waste  ........  total number of unused bram bits (bwaste*dups)
            efficiency  ...  total percentage of used and non-duplicated bits
        
            acells  .......  number of cells in 'address-direction'
            dcells  .......  number of cells in 'data-direction'
            cells  ........  total number of cells (acells*dcells*dups)
        
        A match containing the command 'attribute' followed by a list of space
        separated 'name[=string_value]' values requires that the memory contains any
        one of the given attribute name and string values (where specified), or name
        and integer 1 value (if no string_value given, since Verilog will interpret
        '(* attr *)' as '(* attr=1 *)').
        A name prefixed with '!' indicates that the attribute must not exist.
        
        The interface for the created bram instances is derived from the bram
        description. Use 'techmap' to convert the created bram instances into
        instances of the actual bram cells of your target architecture.
        
        A match containing the command 'or_next_if_better' is only used if it
        has a higher efficiency than the next match (and the one after that if
        the next also has 'or_next_if_better' set, and so forth).
        
        A match containing the command 'make_transp' will add external circuitry
        to simulate 'transparent read', if necessary.
        
        A match containing the command 'make_outreg' will add external flip-flops
        to implement synchronous read ports, if necessary.
        
        A match containing the command 'shuffle_enable A' will re-organize
        the data bits to accommodate the enable pattern of port A.
        
