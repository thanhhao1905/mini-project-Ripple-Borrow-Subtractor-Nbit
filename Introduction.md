Ripple Borrow Subtractor is an N-bit subtractor built by cascading multiple full subtractors. Each stage subtracts corresponding bits of the operands along with the borrow-in from the previous stage. 
The borrow-out of each stage ripples to the next, forming a chain — hence the name Ripple Borrow.
This structure is straightforward to implement but incurs increased propagation delay as the number of bits increases, due to sequential borrow propagation.
