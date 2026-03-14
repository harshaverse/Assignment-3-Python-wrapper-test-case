# Assignment 3: SU2 Python Wrapper Test Case – Heated Flat Plate

## Author

**Boddu Harshavardhan**
B.Tech Mechanical Engineering
GVP College of Engineering

---

# Objective

The objective of this assignment is to test the **SU2 Python wrapper interface** by running a CFD simulation using a custom Python script.

The case studied is a **heated flat plate with unsteady wall temperature**, which demonstrates how Python can dynamically control boundary conditions during a simulation.

---

# Problem Description

The simulation models **turbulent flow over a heated flat plate**.

Key characteristics of the problem:

* 2D compressible flow
* Low Mach number flow
* Turbulent boundary layer development
* Unsteady wall temperature condition
* RANS turbulence modeling

The simulation is useful for studying **conjugate heat transfer (CHT)** and boundary layer heat transfer behavior.

---

# Governing Equations

The simulation solves the **Reynolds-Averaged Navier–Stokes (RANS) equations** for compressible flow:

Continuity equation:

ρ_t + ∇·(ρu) = 0

Momentum equation:

∂(ρu)/∂t + ∇·(ρuu + pI − τ) = 0

Energy equation:

∂(ρE)/∂t + ∇·((ρE + p)u − τu + q) = 0

---

# Turbulence Model

The **Shear Stress Transport (SST)** turbulence model is used.

Advantages:

* Accurate near-wall modeling
* Good performance for adverse pressure gradients
* Widely used in aerospace CFD simulations

Configuration:

```
SOLVER = RANS
KIND_TURB_MODEL = SST
```

---

# Flow Conditions

| Parameter       | Value     |
| --------------- | --------- |
| Mach number     | 0.03059   |
| Pressure        | 101325 Pa |
| Temperature     | 293.15 K  |
| Reynolds number | 24407     |

These conditions represent **low-speed turbulent flow over a flat plate**.

---

# Boundary Conditions

### Wall (Flat Plate)

The wall temperature is **time dependent** and defined inside the Python script.

Temperature function:

T_wall = 293 + 57 sin(2πt)

This creates an **oscillating wall temperature** to study unsteady heat transfer behavior.

---

### Farfield

The outer boundary uses a farfield boundary condition representing free-stream flow.

```
MARKER_FAR = ( farfield )
```

---

# Python Wrapper Script

The Python script uses the **pysu2 interface** to control SU2.

Key features of the script:

* Initializes the SU2 solver
* Accesses the flat plate boundary marker
* Applies a time-varying wall temperature
* Runs the solver for each time iteration
* Updates boundary conditions dynamically

Example temperature update inside Python:

```
WallTemp = 293.0 + 57.0*sin(2*pi*time)
```

This value is applied to all nodes on the plate surface.

---

# Running the Simulation

The simulation can be launched using:

```
python launch_unsteady_CHT_FlatPlate.py -f config.cfg
```

For parallel execution:

```
mpirun -np 4 python launch_unsteady_CHT_FlatPlate.py -f config.cfg --parallel
```

---

# Output Files

During simulation SU2 generates several output files.

| File             | Description         |
| ---------------- | ------------------- |
| history.csv      | Convergence history |
| flow.vtu         | Flow field solution |
| surface_flow.csv | Surface quantities  |
| restart_flow.dat | Restart data        |

These outputs can be visualized using **ParaView**.

---

# Visualization

Results were analyzed using **ParaView**.

Typical quantities visualized:

* Velocity contours
* Pressure distribution
* Boundary layer development
* Surface temperature variation

These results demonstrate the effect of **unsteady wall heating on the turbulent boundary layer**.

---

# Repository Structure

```
flatplate_python_wrapper

│
├── config.cfg
├── launch_unsteady_CHT_FlatPlate.py
├── 2D_FlatPlate_Rounded.su2
├── history.csv
```

---

# Learning Outcomes

Through this assignment the following concepts were learned:

* Using the **SU2 Python wrapper**
* Applying **custom boundary conditions**
* Running **unsteady CFD simulations**
* Understanding **turbulent boundary layer flow**
* Coupling Python scripting with CFD solvers

---

# References

SU2 Official Repository
https://github.com/su2code/SU2

SU2 Documentation
https://su2code.github.io

Anderson, J.D.
Modern Compressible Flow
