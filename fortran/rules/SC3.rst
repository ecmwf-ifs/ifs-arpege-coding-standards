SC3 : function calls from inside KPROMA loops
*********************************************

Inside tight horizontal loops of type DO ``JL=KIDIA,KFDIA``, calls should be restricted to intrinsics.

If a function construct must be used inside the loop, the function shall be pure elemental or defined in a statement function, so as not
to inhibit compiler vectorization of the loop. 

