# Describe the use case you have chosen.

The project's main purpose is to create a cost estimate for the whole construction. To explain, the aim is to develop a program that can extract the necessary information from the IFC, allowing someone to make a cost estimate. For instance, it can find data like the amount of concrete needed and from the price per cubic meter, enabling a cost estimate to be determined.

---

# Who is the use case for?

This use case benefits both contractors and consultants. It aids consultants in compiling a materials list for the project. For contractors, it results in significant time and cost savings by providing a standardized tool to determine material lengths, volumes, and prices for each project.

---

# What disciplinary (non BIM) expertise did you use to solve the use case?

We used our collective knowledge from the construction industry, where two of us works as consultants at COWI and AFRY, and one working as a contractor at MT Højgaard. As we all dedicate considerable time to preparing bids, we aimed to streamline this process. The goal is to avoid the manual task of, for example, searching for specific concrete volumes and prices for each project. Instead, we aimed to develop a script that automates much of this work.

---

# What IFC concepts did you use in your script (or would you use in the rest of the tool)?

We used Anaconda, which simplifies script creation but presents challenges in extracting data from the IFC file. In Anaconda, directly obtaining specific outputs like volume was not feasible. Therefore, we had to retrieve the coordinates of each material and derive the volume from there. Since IFC is structured around entities such as IFC_Wall, which has multiple attributes like dimension, utilizing object-oriented extraction has been very useful for this project. In the project, there has been a focus on using IFC’s object-oriented structure, there has been estimated a price for each structure type IFC_Wall, IFC_Beam, IFC_Column, and IFC_Slabs. Furthermore, also estimated a price for every entity type, for example, a specific wall type. This can be useful to see how the total price is divided.

---

# What disciplinary analysis does it require?

The disciplinary analysis required for this project would be high construction knowledge, expertise in project cost as well as BIM to navigate and utilize IFC data efficiently.

---

# What building elements are you interested in?

In the future, we are interested in all the materials used in the entire project. In this part to narrow it down we only focused on the structural parts (beams, columns, slabs, and walls), as this is what we are most familiar with.

---

# What (use cases) need to be done before you can start your use case?

Before we can start our use case the different materials need to be determined. For instance, we can currently determine the volume of a slab but lack data regarding concrete type, rebar volume, and strength.

---

# What is the input data for your use case?

The input data for our use case is:

- The length of the beams and columns retrieved by the IFC file.
- The coordinates from the slabs and walls, which then finds the volume.
- A self-made price list which can give the different prices for different beams and columns depending on the length, and a price based on the volume of concrete. For this project, this is done by making an excel price list.

---

# What other use cases are waiting for your use case to complete?

The next step involves obtaining information about all materials, including insulation, and then proceeding to gather data on elements not included in 3D models, such as scaffolding and man-hours needed to complete the project. This will give us a more accurate cost estimate for the entire project.
