SC4 : no horizontal indirection
********************************

Where relevant (Single Column), indirect addressing on the innermost, 
horizontal array index shall not be used.

The Loki tool relies on elements of code style in order to identify loops needing to be manipulated
for architecture specialisation. 

>>PM: some parts of cucalln.F90 rely on indirect addressing, and can nevertheless transformed. I think
this constraint should be relaxed.<<
